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
ms.openlocfilehash: 7deccf135ddd21729206ebed58ddc8aca52c1deb
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="d5561-104">Hosting w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5561-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="d5561-105">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d5561-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d5561-106">Aplikacje platformy ASP.NET Core skonfigurować i uruchomić *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d5561-107">Co najmniej hosta konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="d5561-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="d5561-108">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="d5561-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-109">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d5561-110">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d5561-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d5561-111">Jest to najczęściej wykonywane w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="d5561-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="d5561-112">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d5561-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d5561-113">Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="d5561-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="d5561-114">`CreateDefaultBuilder`wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="d5561-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d5561-115">Konfiguruje [Kestrel](servers/kestrel.md) jako serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="d5561-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="d5561-116">Aby Kestrel domyślnych opcji, zobacz [Kestrel opcje sekcji Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="d5561-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="d5561-117">Ustawia zawartości głównego [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d5561-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d5561-118">Konfiguracja opcjonalna obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="d5561-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="d5561-119">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="d5561-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="d5561-120">*appSettings. {Środowiska} JSON*.</span><span class="sxs-lookup"><span data-stu-id="d5561-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d5561-121">[Klucze tajne użytkownika](xref:security/app-secrets) po uruchomieniu aplikacji `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="d5561-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="d5561-122">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="d5561-122">Environment variables.</span></span>
  * <span data-ttu-id="d5561-123">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d5561-123">Command-line arguments.</span></span>
* <span data-ttu-id="d5561-124">Konfiguruje [rejestrowanie](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania z [filtrowania dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji konfiguracji rejestrowania *appsettings.json* lub *appsettings. {Środowiska} JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="d5561-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d5561-125">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:publishing/iis) przez skonfigurowanie ścieżki podstawowej i port serwera powinien nasłuchiwać przy użyciu [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d5561-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d5561-126">Moduł tworzy serwer proxy wstecznego między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d5561-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d5561-127">Konfiguruje również aplikacji na [przechwytywania błędy uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d5561-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d5561-128">Dla opcji domyślnych usług IIS, zobacz [IIS opcje sekcji hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="d5561-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="d5561-129">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="d5561-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d5561-130">Domyślny element zawartości jest [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d5561-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="d5561-131">Powoduje to przy użyciu folderu głównego projektu sieci web jako główny zawartości po uruchomieniu aplikacji z folderu głównego (na przykład wywołanie elementu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z folderu projektu).</span><span class="sxs-lookup"><span data-stu-id="d5561-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="d5561-132">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d5561-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d5561-133">Zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index) uzyskać więcej informacji o konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="d5561-134">Alternatywą wobec przy użyciu statycznych `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) jest to obsługiwane podejście z platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d5561-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d5561-135">Zobacz kartę 1.x platformy ASP.NET Core, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-136">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5561-137">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d5561-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d5561-138">Jest to najczęściej wykonywane w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="d5561-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="d5561-139">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d5561-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d5561-140">Następujące *Program.cs* przedstawiono sposób użycia `WebHostBuilder` tworzenie hosta:</span><span class="sxs-lookup"><span data-stu-id="d5561-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="d5561-141">`WebHostBuilder`wymaga [serwera, który implementuje IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="d5561-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="d5561-142">Wbudowane serwery są [Kestrel](servers/kestrel.md) i [HTTP.sys](servers/httpsys.md) (przed wydaniem programu ASP.NET 2.0 Core została wywołana HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d5561-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d5561-143">W tym przykładzie [— metoda rozszerzenia UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Określa serwer Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d5561-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d5561-144">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="d5561-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d5561-145">Domyślny element zawartości dostarczony do `UseContentRoot` jest [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d5561-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d5561-146">Powoduje to przy użyciu folderu głównego projektu sieci web jako główny zawartości po uruchomieniu aplikacji z folderu głównego (na przykład wywołanie elementu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z folderu projektu).</span><span class="sxs-lookup"><span data-stu-id="d5561-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="d5561-147">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d5561-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d5561-148">Do używania serwera IIS jako zwrotny serwer proxy, należy wywołać [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) w ramach tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="d5561-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d5561-149">`UseIISIntegration`nie Konfiguruj *serwera*, takiej jak [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) jest.</span><span class="sxs-lookup"><span data-stu-id="d5561-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d5561-150">`UseIISIntegration`Konfiguruje serwer powinien nasłuchiwać przy użyciu portu i ścieżki bazowej [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) utworzyć proxy wstecznego między Kestrel i IIS.</span><span class="sxs-lookup"><span data-stu-id="d5561-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d5561-151">Aby używać usług IIS z platformy ASP.NET Core, należy określić zarówno `UseKestrel` i `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="d5561-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="d5561-152">`UseIISIntegration`aktywuje tylko podczas uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d5561-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d5561-153">Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d5561-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="d5561-154">Minimalny wdrożenia, który konfiguruje hosta (a aplikacji platformy ASP.NET Core) obejmuje określenie serwera i konfiguracji Potok żądań aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d5561-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="d5561-155">Podczas konfigurowania hosta, możesz podać [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody.</span><span class="sxs-lookup"><span data-stu-id="d5561-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="d5561-156">Jeśli określisz `Startup` klasy, należy zdefiniować `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="d5561-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="d5561-157">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji w ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="d5561-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="d5561-158">Wiele wywołań `ConfigureServices` dołączyć do siebie.</span><span class="sxs-lookup"><span data-stu-id="d5561-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d5561-159">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpić poprzednie ustawienia.</span><span class="sxs-lookup"><span data-stu-id="d5561-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d5561-160">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="d5561-160">Host configuration values</span></span>

<span data-ttu-id="d5561-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) udostępnia metody do ustawiania większość wartości konfiguracji dostępne dla hosta, który można również ustawić bezpośrednio z [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="d5561-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d5561-162">Podczas ustawiania wartości z `UseSetting`, ma wartość ciągu (w cudzysłowy) niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="d5561-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="d5561-163">Przechwyć błędy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="d5561-163">Capture Startup Errors</span></span>

<span data-ttu-id="d5561-164">To ustawienie określa przechwytywania błędy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d5561-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d5561-165">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d5561-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d5561-166">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d5561-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5561-167">**Domyślne**: Domyślnie `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="d5561-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d5561-168">**Ustawić za pomocą**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d5561-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="d5561-169">Gdy `false`, błędy podczas wynik uruchomienia na hoście został zakończony.</span><span class="sxs-lookup"><span data-stu-id="d5561-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d5561-170">Gdy `true`, host przechwytuje wyjątków podczas uruchamiania i podejmie próbę uruchomienia serwera.</span><span class="sxs-lookup"><span data-stu-id="d5561-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-171">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-172">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="d5561-173">Główny zawartości</span><span class="sxs-lookup"><span data-stu-id="d5561-173">Content Root</span></span>

<span data-ttu-id="d5561-174">To ustawienie określa, gdzie platformy ASP.NET Core rozpocznie się wyszukiwanie plików zawartości, np. widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="d5561-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d5561-175">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d5561-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="d5561-176">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d5561-176">**Type**: *string*</span></span>  
<span data-ttu-id="d5561-177">**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d5561-178">**Ustawić za pomocą**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d5561-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="d5561-179">Główny zawartości jest również używany jako podstawowa ścieżka dla [ustawienia sieci Web głównego](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d5561-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d5561-180">Jeśli ścieżka nie istnieje, host nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="d5561-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-181">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-182">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="d5561-183">Błędy szczegółowe</span><span class="sxs-lookup"><span data-stu-id="d5561-183">Detailed Errors</span></span>

<span data-ttu-id="d5561-184">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="d5561-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d5561-185">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d5561-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d5561-186">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d5561-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5561-187">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="d5561-187">**Default**: false</span></span>  
<span data-ttu-id="d5561-188">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d5561-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="d5561-189">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ma ustawioną wartość `Development`), aplikacji znajdują się szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="d5561-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-190">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-191">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="d5561-192">Środowisko</span><span class="sxs-lookup"><span data-stu-id="d5561-192">Environment</span></span>

<span data-ttu-id="d5561-193">Ustawia środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-193">Sets the app's environment.</span></span>

<span data-ttu-id="d5561-194">**Klucz**: środowiska</span><span class="sxs-lookup"><span data-stu-id="d5561-194">**Key**: environment</span></span>  
<span data-ttu-id="d5561-195">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d5561-195">**Type**: *string*</span></span>  
<span data-ttu-id="d5561-196">**Domyślna**: produkcji</span><span class="sxs-lookup"><span data-stu-id="d5561-196">**Default**: Production</span></span>  
<span data-ttu-id="d5561-197">**Ustawić za pomocą**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d5561-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="d5561-198">Można ustawić *środowiska* dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="d5561-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="d5561-199">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="d5561-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d5561-200">Wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="d5561-200">Values aren't case sensitive.</span></span> <span data-ttu-id="d5561-201">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="d5561-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d5561-202">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d5561-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d5561-203">Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d5561-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-204">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-205">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d5561-206">Zestawy uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="d5561-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d5561-207">Ustawia hostingu zestawy uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d5561-208">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d5561-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d5561-209">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d5561-209">**Type**: *string*</span></span>  
<span data-ttu-id="d5561-210">**Domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="d5561-210">**Default**: Empty string</span></span>  
<span data-ttu-id="d5561-211">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d5561-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="d5561-212">Ciąg rozdzielany średnikami obsługi zestawów uruchamiania załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d5561-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="d5561-213">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="d5561-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="d5561-214">Mimo że konfiguracja domyślnie przyjmowana jest wartość pustego ciągu, hostingu zestawy uruchamiania zawsze należy uwzględniać zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d5561-215">Podając hostingu zestawy uruchamiania, są one dodane do zestawu aplikacji ładowania, gdy aplikacja tworzy jego wspólne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d5561-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-216">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-217">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5561-218">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5561-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d5561-219">Preferowane jest Hosting adresy URL</span><span class="sxs-lookup"><span data-stu-id="d5561-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="d5561-220">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast ustawień skonfigurowanych z `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d5561-221">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d5561-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d5561-222">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d5561-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5561-223">**Domyślna**: true</span><span class="sxs-lookup"><span data-stu-id="d5561-223">**Default**: true</span></span>  
<span data-ttu-id="d5561-224">**Ustawić za pomocą**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d5561-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="d5561-225">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="d5561-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-226">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-227">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5561-228">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5561-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d5561-229">Zapobiegaj Hosting uruchamiania</span><span class="sxs-lookup"><span data-stu-id="d5561-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="d5561-230">Uniemożliwia automatyczne ładowanie hosting zestawy uruchomienia, w tym zestawie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-230">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="d5561-231">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d5561-231">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d5561-232">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d5561-232">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5561-233">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="d5561-233">**Default**: false</span></span>  
<span data-ttu-id="d5561-234">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d5561-234">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="d5561-235">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="d5561-235">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-236">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-236">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-237">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-237">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5561-238">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5561-238">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="d5561-239">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="d5561-239">Server URLs</span></span>

<span data-ttu-id="d5561-240">Wskazuje adresy IP lub adresy hostów, porty i protokoły, które serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="d5561-240">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d5561-241">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="d5561-241">**Key**: urls</span></span>  
<span data-ttu-id="d5561-242">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d5561-242">**Type**: *string*</span></span>  
<span data-ttu-id="d5561-243">**Domyślna**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="d5561-243">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d5561-244">**Ustawić za pomocą**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d5561-244">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="d5561-245">Ustaw rozdzielonych średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="d5561-245">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d5561-246">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="d5561-246">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d5561-247">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d5561-247">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d5561-248">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d5561-248">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d5561-249">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="d5561-249">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-250">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="d5561-251">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d5561-251">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d5561-252">Aby uzyskać więcej informacji, zobacz [Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="d5561-252">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-253">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="d5561-254">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="d5561-254">Shutdown Timeout</span></span>

<span data-ttu-id="d5561-255">Określa czas oczekiwania na zamknięcie hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="d5561-255">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="d5561-256">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d5561-256">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d5561-257">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="d5561-257">**Type**: *int*</span></span>  
<span data-ttu-id="d5561-258">**Domyślna**: 5</span><span class="sxs-lookup"><span data-stu-id="d5561-258">**Default**: 5</span></span>  
<span data-ttu-id="d5561-259">**Ustawić za pomocą**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d5561-259">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="d5561-260">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` przyjmuje — metoda rozszerzenia `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="d5561-260">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="d5561-261">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="d5561-261">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-262">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-262">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-263">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-263">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5561-264">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5561-264">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="d5561-265">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="d5561-265">Startup Assembly</span></span>

<span data-ttu-id="d5561-266">Określa zestaw do wyszukania `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="d5561-266">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d5561-267">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d5561-267">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d5561-268">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d5561-268">**Type**: *string*</span></span>  
<span data-ttu-id="d5561-269">**Domyślna**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="d5561-269">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d5561-270">**Ustawić za pomocą**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d5561-270">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="d5561-271">Nazwę można odwoływać się do zestawu (`string`) lub typu (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="d5561-271">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="d5561-272">Jeśli wiele `UseStartup` metody są nazywane, pierwszeństwo ma ostatnią.</span><span class="sxs-lookup"><span data-stu-id="d5561-272">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-273">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-273">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-274">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-274">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="d5561-275">Główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="d5561-275">Web Root</span></span>

<span data-ttu-id="d5561-276">Ustawia ścieżkę względną do statycznego zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-276">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d5561-277">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="d5561-277">**Key**: webroot</span></span>  
<span data-ttu-id="d5561-278">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d5561-278">**Type**: *string*</span></span>  
<span data-ttu-id="d5561-279">**Domyślne**: Jeśli nie jest określony, wartością domyślną jest "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="d5561-279">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d5561-280">Jeśli ścieżka nie istnieje, jest używany dostawca pliku zerowa.</span><span class="sxs-lookup"><span data-stu-id="d5561-280">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d5561-281">**Ustawić za pomocą**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d5561-281">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-282">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-282">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-283">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-283">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="d5561-284">Zastępowanie konfiguracji</span><span class="sxs-lookup"><span data-stu-id="d5561-284">Overriding configuration</span></span>

<span data-ttu-id="d5561-285">Użyj [konfiguracji](xref:fundamentals/configuration/index) Aby skonfigurować hosta.</span><span class="sxs-lookup"><span data-stu-id="d5561-285">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="d5561-286">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określić w *hosting.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d5561-286">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="d5561-287">Wszelkie konfiguracja została załadowana z *hosting.json* plik może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d5561-287">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d5561-288">Zbudowany konfiguracji (w `config`) służy do konfigurowania hosta z `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="d5561-288">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-289">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-289">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d5561-290">*hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d5561-290">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d5561-291">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="d5561-291">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-292">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5561-293">*hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d5561-293">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d5561-294">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="d5561-294">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="d5561-295">`UseConfiguration` — Metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d5561-295">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d5561-296">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d5561-296">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d5561-297">`UseConfiguration` Metoda oczekuje klucze do dopasowania `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d5561-297">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d5561-298">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="d5561-298">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d5561-299">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="d5561-299">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d5561-300">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d5561-300">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="d5561-301">Aby określić hosta, uruchom na określony adres URL, możesz przesłać żądaną wartość z wiersza polecenia podczas wykonywania `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d5561-301">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="d5561-302">Argument wiersza polecenia zastępuje `urls` wartość z *hosting.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="d5561-302">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="d5561-303">Znaczenie kolejności</span><span class="sxs-lookup"><span data-stu-id="d5561-303">Ordering importance</span></span>

<span data-ttu-id="d5561-304">Niektóre `WebHostBuilder` ustawienia najpierw są odczytywane z zmiennych środowiskowych, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="d5561-304">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="d5561-305">Te zmienne środowiskowe, użyj formatu `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="d5561-305">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d5561-306">Aby ustawić adresów URL, które serwer nasłuchuje na domyślne, należy ustawić `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="d5561-306">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="d5561-307">Można zastąpić jedną z tych wartości zmiennych środowiskowych, określając konfiguracji (przy użyciu `UseConfiguration`) lub przez jawne ustawienie wartości (przy użyciu `UseSetting` lub jednej z metod rozszerzenia jawne, takich jak `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="d5561-307">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="d5561-308">Host używa jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="d5561-308">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="d5561-309">Jeśli chcesz programowo ustawiony domyślny adres URL na jedną wartość, ale zezwolenie na należy zastąpić konfiguracji, można użyć wiersza polecenia konfigurację po ustawieniu adres URL.</span><span class="sxs-lookup"><span data-stu-id="d5561-309">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="d5561-310">Zobacz [konfiguracji zastąpiona](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="d5561-310">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="d5561-311">Uruchamianie hosta</span><span class="sxs-lookup"><span data-stu-id="d5561-311">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5561-312">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5561-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d5561-313">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="d5561-313">**Run**</span></span>

<span data-ttu-id="d5561-314">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący do czasu zamknięcia hosta:</span><span class="sxs-lookup"><span data-stu-id="d5561-314">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d5561-315">**Start**</span><span class="sxs-lookup"><span data-stu-id="d5561-315">**Start**</span></span>

<span data-ttu-id="d5561-316">Można uruchomić hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="d5561-316">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d5561-317">W przypadku przekazania listę adresów URL do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d5561-317">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d5561-318">Można zainicjować i rozpocząć nowego hosta za pomocą wstępnie skonfigurowane ustawienia domyślne `CreateDefaultBuilder` przy użyciu metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="d5561-318">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d5561-319">Te metody uruchomienia serwera bez dane wyjściowe konsoli i z [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podział (Ctrl-C/sigint — lub sigterm —):</span><span class="sxs-lookup"><span data-stu-id="d5561-319">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d5561-320">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d5561-320">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d5561-321">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d5561-321">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d5561-322">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d5561-322">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d5561-323">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="d5561-323">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d5561-324">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="d5561-324">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d5561-325">**Start (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d5561-325">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d5561-326">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d5561-326">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d5561-327">Tworzy wynik, w postaci **Start (aplikacja RequestDelegate)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d5561-327">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d5561-328">**Start (akcji<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d5561-328">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d5561-329">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) do używania oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="d5561-329">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d5561-330">Użyj następujących żądań przeglądarki z przykładem:</span><span class="sxs-lookup"><span data-stu-id="d5561-330">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d5561-331">Żądanie</span><span class="sxs-lookup"><span data-stu-id="d5561-331">Request</span></span>                                    | <span data-ttu-id="d5561-332">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="d5561-332">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d5561-333">Witaj, pole!</span><span class="sxs-lookup"><span data-stu-id="d5561-333">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d5561-334">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d5561-334">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d5561-335">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="d5561-335">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d5561-336">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="d5561-336">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d5561-337">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="d5561-337">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d5561-338">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="d5561-338">Hello World!</span></span>                             |

<span data-ttu-id="d5561-339">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="d5561-339">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d5561-340">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="d5561-340">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d5561-341">**Start (ciągu adresu url, Akcja<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d5561-341">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d5561-342">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d5561-342">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d5561-343">Tworzy wynik, w postaci **Start (akcji<IRouteBuilder> routeBuilder)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d5561-343">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d5561-344">**StartWith (akcji<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d5561-344">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d5561-345">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d5561-345">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d5561-346">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d5561-346">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d5561-347">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="d5561-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d5561-348">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="d5561-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d5561-349">**StartWith (ciągu adresu url, Akcja<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d5561-349">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d5561-350">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d5561-350">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d5561-351">Tworzy wynik, w postaci **StartWith (akcji<IApplicationBuilder> aplikacji)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d5561-351">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5561-352">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5561-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5561-353">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="d5561-353">**Run**</span></span>

<span data-ttu-id="d5561-354">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący do czasu zamknięcia hosta:</span><span class="sxs-lookup"><span data-stu-id="d5561-354">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d5561-355">**Start**</span><span class="sxs-lookup"><span data-stu-id="d5561-355">**Start**</span></span>

<span data-ttu-id="d5561-356">Można uruchomić hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="d5561-356">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d5561-357">W przypadku przekazania listę adresów URL do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d5561-357">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d5561-358">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d5561-358">IHostingEnvironment interface</span></span>

<span data-ttu-id="d5561-359">[Interfejsu IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje o aplikacji sieci web środowiska macierzystego.</span><span class="sxs-lookup"><span data-stu-id="d5561-359">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d5561-360">Można użyć [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="d5561-360">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d5561-361">Można użyć [podejście oparte na Konwencji](xref:fundamentals/environments#startup-conventions) Aby skonfigurować aplikację podczas uruchamiania na podstawie środowiska.</span><span class="sxs-lookup"><span data-stu-id="d5561-361">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="d5561-362">Alternatywnie można wstrzyknąć `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d5561-362">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d5561-363">Oprócz `IsDevelopment` — metoda rozszerzenia, `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="d5561-363">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d5561-364">Zobacz [Praca w środowiskach wielu](xref:fundamentals/environments) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="d5561-364">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="d5561-365">`IHostingEnvironment` Usługi także mogą zostać dodane bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania sieci:</span><span class="sxs-lookup"><span data-stu-id="d5561-365">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="d5561-366">Można wstrzyknąć `IHostingEnvironment` do `Invoke` metody podczas tworzenia niestandardowego [oprogramowanie pośredniczące](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="d5561-366">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d5561-367">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d5561-367">IApplicationLifetime interface</span></span>

<span data-ttu-id="d5561-368">[Interfejsu IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia wykonywanie czynności po uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="d5561-368">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="d5561-369">Trzy właściwości w interfejsie są anulowanie tokenów, które można zarejestrować z `Action` metod, aby określić zdarzenia uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="d5561-369">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="d5561-370">Istnieje również `StopApplication` metody.</span><span class="sxs-lookup"><span data-stu-id="d5561-370">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="d5561-371">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="d5561-371">Cancellation Token</span></span>    | <span data-ttu-id="d5561-372">Wyzwalane podczas &#8230;</span><span class="sxs-lookup"><span data-stu-id="d5561-372">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="d5561-373">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d5561-373">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="d5561-374">Host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="d5561-374">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d5561-375">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="d5561-375">Requests may still be processing.</span></span> <span data-ttu-id="d5561-376">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d5561-376">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="d5561-377">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="d5561-377">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d5561-378">Wszystkie żądania należy całkowicie przetworzone.</span><span class="sxs-lookup"><span data-stu-id="d5561-378">All requests should be completely processed.</span></span> <span data-ttu-id="d5561-379">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d5561-379">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="d5561-380">Metoda</span><span class="sxs-lookup"><span data-stu-id="d5561-380">Method</span></span>            | <span data-ttu-id="d5561-381">Akcja</span><span class="sxs-lookup"><span data-stu-id="d5561-381">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="d5561-382">Zakończenie żądania bieżącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5561-382">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d5561-383">Rozwiązywanie problemów z System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="d5561-383">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d5561-384">**Dotyczy tylko platformy ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="d5561-384">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="d5561-385">W przypadku tworzenia hosta przez wstrzykiwanie `IStartup` bezpośrednio do kontenera iniekcji zależności zamiast wywoływania `UseStartup` lub `Configure`, może wystąpić następujący błąd: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="d5561-385">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="d5561-386">Dzieje się tak dlatego [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (bieżącego zestawu) jest wymagane do skanowania pod kątem `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d5561-386">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d5561-387">Jeśli ręcznie wprowadzić `IStartup` do kontenera iniekcji zależności, Dodaj następujące wywołanie do Twojej `WebHostBuilder` o podanej nazwie zestawu:</span><span class="sxs-lookup"><span data-stu-id="d5561-387">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="d5561-388">Możesz też dodać manekina `Configure` do Twojej `WebHostBuilder`, który określa `applicationName`(`ApplicationKey`) automatycznie:</span><span class="sxs-lookup"><span data-stu-id="d5561-388">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="d5561-389">**Uwaga**: jest to tylko wymagane wraz z wydaniem programu ASP.NET 2.0 rdzeni i tylko wtedy gdy zgłaszasz nie `UseStartup` lub `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d5561-389">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="d5561-390">Aby uzyskać więcej informacji, zobacz [anonsów: Microsoft.Extensions.PlatformAbstractions została usunięta (komentarz)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) i [próbki StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d5561-390">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5561-391">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d5561-391">Additional resources</span></span>

* [<span data-ttu-id="d5561-392">Publikowanie do systemu Windows za pomocą usług IIS</span><span class="sxs-lookup"><span data-stu-id="d5561-392">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="d5561-393">Publikowanie w systemie Linux przy użyciu Nginx</span><span class="sxs-lookup"><span data-stu-id="d5561-393">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="d5561-394">Publikowanie w systemie Linux przy użyciu Apache</span><span class="sxs-lookup"><span data-stu-id="d5561-394">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="d5561-395">Host usługi systemu Windows</span><span class="sxs-lookup"><span data-stu-id="d5561-395">Host in a Windows Service</span></span>](xref:hosting/windows-service)
