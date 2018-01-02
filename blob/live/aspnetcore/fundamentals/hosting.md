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
ms.openlocfilehash: 14e48adf5671a41ad6e135caeb4a87fdf7292aa6
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="15a50-104">Hosting w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15a50-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="15a50-105">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="15a50-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="15a50-106">Aplikacje platformy ASP.NET Core skonfigurować i uruchomić *hosta*.</span><span class="sxs-lookup"><span data-stu-id="15a50-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="15a50-107">Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="15a50-108">Co najmniej hosta konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="15a50-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="15a50-109">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="15a50-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-110">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="15a50-111">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="15a50-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="15a50-112">Jest to najczęściej wykonywane w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="15a50-112">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="15a50-113">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="15a50-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="15a50-114">Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="15a50-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="15a50-115">`CreateDefaultBuilder`wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="15a50-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="15a50-116">Konfiguruje [Kestrel](servers/kestrel.md) jako serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="15a50-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="15a50-117">Aby Kestrel domyślnych opcji, zobacz [Kestrel opcje sekcji Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="15a50-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="15a50-118">Ustawia zawartości głównego [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="15a50-118">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="15a50-119">Konfiguracja opcjonalna obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="15a50-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="15a50-120">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="15a50-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="15a50-121">*appSettings. {Środowiska} JSON*.</span><span class="sxs-lookup"><span data-stu-id="15a50-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="15a50-122">[Klucze tajne użytkownika](xref:security/app-secrets) po uruchomieniu aplikacji `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="15a50-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="15a50-123">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="15a50-123">Environment variables.</span></span>
  * <span data-ttu-id="15a50-124">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="15a50-124">Command-line arguments.</span></span>
* <span data-ttu-id="15a50-125">Konfiguruje [rejestrowanie](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="15a50-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="15a50-126">Dostęp do rejestrów uwzględniających [filtrowania dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji konfiguracji rejestrowania *appsettings.json* lub *appsettings. { Środowisko} JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="15a50-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="15a50-127">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:publishing/iis) przez skonfigurowanie ścieżki podstawowej i port serwera powinien nasłuchiwać przy użyciu [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="15a50-127">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="15a50-128">Moduł tworzy serwer proxy wstecznego między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="15a50-128">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="15a50-129">Konfiguruje również aplikacji na [przechwytywania błędy uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="15a50-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="15a50-130">Dla opcji domyślnych usług IIS, zobacz [IIS opcje sekcji hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="15a50-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="15a50-131">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="15a50-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="15a50-132">Domyślny element zawartości jest [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="15a50-132">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="15a50-133">Domyślny element zawartości (`Directory.GetCurrentDirectory`) powoduje przy użyciu folderu głównego projektu sieci web jako główny zawartości po uruchomieniu aplikacji z folderu głównego (na przykład wywołanie elementu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z folderu projektu).</span><span class="sxs-lookup"><span data-stu-id="15a50-133">The default content root (`Directory.GetCurrentDirectory`) results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="15a50-134">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="15a50-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="15a50-135">Aby uzyskać więcej informacji o konfiguracji aplikacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="15a50-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="15a50-136">Alternatywą wobec przy użyciu statycznych `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) jest to obsługiwane podejście z platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="15a50-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="15a50-137">Aby uzyskać więcej informacji zobacz kartę 1.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15a50-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-138">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15a50-139">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="15a50-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="15a50-140">Tworzenie hosta jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="15a50-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="15a50-141">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="15a50-141">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="15a50-142">Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="15a50-142">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="15a50-143">`WebHostBuilder`wymaga [serwera, który implementuje IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="15a50-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="15a50-144">Wbudowane serwery są [Kestrel](servers/kestrel.md) i [HTTP.sys](servers/httpsys.md) (przed wydaniem programu ASP.NET 2.0 Core została wywołana HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="15a50-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="15a50-145">W tym przykładzie [— metoda rozszerzenia UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Określa serwer Kestrel.</span><span class="sxs-lookup"><span data-stu-id="15a50-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="15a50-146">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="15a50-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="15a50-147">Domyślny element zawartości dostarczony do `UseContentRoot` jest [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="15a50-147">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="15a50-148">Powoduje to przy użyciu folderu głównego projektu sieci web jako główny zawartości po uruchomieniu aplikacji z folderu głównego (na przykład wywołanie elementu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z folderu projektu).</span><span class="sxs-lookup"><span data-stu-id="15a50-148">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="15a50-149">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="15a50-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="15a50-150">Do używania serwera IIS jako zwrotny serwer proxy, należy wywołać [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) w ramach tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="15a50-150">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="15a50-151">`UseIISIntegration`nie Konfiguruj *serwera*, takiej jak [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) jest.</span><span class="sxs-lookup"><span data-stu-id="15a50-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="15a50-152">`UseIISIntegration`Konfiguruje serwer powinien nasłuchiwać przy użyciu portu i ścieżki bazowej [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) utworzyć proxy wstecznego między Kestrel i IIS.</span><span class="sxs-lookup"><span data-stu-id="15a50-152">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="15a50-153">Aby używać usług IIS z platformy ASP.NET Core, należy określić zarówno `UseKestrel` i `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="15a50-153">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="15a50-154">`UseIISIntegration`aktywuje tylko podczas uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="15a50-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="15a50-155">Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="15a50-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="15a50-156">Minimalny wdrożenia, który konfiguruje hosta (a aplikacji platformy ASP.NET Core) obejmuje określenie serwera i konfiguracji Potok żądań aplikacji:</span><span class="sxs-lookup"><span data-stu-id="15a50-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="15a50-157">Podczas konfigurowania hosta, możesz podać [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody.</span><span class="sxs-lookup"><span data-stu-id="15a50-157">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="15a50-158">Jeśli określisz `Startup` klasy, należy zdefiniować `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="15a50-158">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="15a50-159">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji w ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="15a50-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="15a50-160">Wiele wywołań `ConfigureServices` dołączyć do siebie.</span><span class="sxs-lookup"><span data-stu-id="15a50-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="15a50-161">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpić poprzednie ustawienia.</span><span class="sxs-lookup"><span data-stu-id="15a50-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="15a50-162">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="15a50-162">Host configuration values</span></span>

<span data-ttu-id="15a50-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) umożliwia ustawienie większość wartości konfiguracji dostępne dla hosta następujących metod:</span><span class="sxs-lookup"><span data-stu-id="15a50-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides the following approaches for setting most of the available configuration values for the host:</span></span>

* <span data-ttu-id="15a50-164">Zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="15a50-164">Environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="15a50-165">Na przykład `ASPNETCORE_DETAILEDERRORS`.</span><span class="sxs-lookup"><span data-stu-id="15a50-165">For example, `ASPNETCORE_DETAILEDERRORS`.</span></span>
* <span data-ttu-id="15a50-166">Jawne metod, takich jak `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="15a50-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="15a50-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="15a50-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="15a50-168">Podczas ustawiania wartości z `UseSetting`, wartość jest ustawiona jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="15a50-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="15a50-169">Przechwyć błędy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="15a50-169">Capture Startup Errors</span></span>

<span data-ttu-id="15a50-170">To ustawienie określa przechwytywania błędy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="15a50-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="15a50-171">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="15a50-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="15a50-172">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="15a50-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="15a50-173">**Domyślne**: Domyślnie `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="15a50-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="15a50-174">**Ustawić za pomocą**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="15a50-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="15a50-175">**Zmienna środowiskowa**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="15a50-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="15a50-176">Gdy `false`, błędy podczas wynik uruchomienia na hoście został zakończony.</span><span class="sxs-lookup"><span data-stu-id="15a50-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="15a50-177">Gdy `true`, host przechwytuje wyjątków podczas uruchamiania i podejmie próbę uruchomienia serwera.</span><span class="sxs-lookup"><span data-stu-id="15a50-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-178">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-179">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="15a50-180">Główny zawartości</span><span class="sxs-lookup"><span data-stu-id="15a50-180">Content Root</span></span>

<span data-ttu-id="15a50-181">To ustawienie określa, gdzie platformy ASP.NET Core rozpocznie się wyszukiwanie plików zawartości, np. widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="15a50-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="15a50-182">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="15a50-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="15a50-183">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="15a50-183">**Type**: *string*</span></span>  
<span data-ttu-id="15a50-184">**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="15a50-185">**Ustawić za pomocą**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="15a50-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="15a50-186">**Zmienna środowiskowa**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="15a50-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="15a50-187">Główny zawartości jest również używany jako podstawowa ścieżka dla [ustawienia sieci Web głównego](#web-root).</span><span class="sxs-lookup"><span data-stu-id="15a50-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="15a50-188">Jeśli ścieżka nie istnieje, host nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="15a50-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-189">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-190">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="15a50-191">Błędy szczegółowe</span><span class="sxs-lookup"><span data-stu-id="15a50-191">Detailed Errors</span></span>

<span data-ttu-id="15a50-192">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="15a50-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="15a50-193">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="15a50-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="15a50-194">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="15a50-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="15a50-195">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="15a50-195">**Default**: false</span></span>  
<span data-ttu-id="15a50-196">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="15a50-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="15a50-197">**Zmienna środowiskowa**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="15a50-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="15a50-198">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ma ustawioną wartość `Development`), aplikacji znajdują się szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="15a50-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-199">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-200">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="15a50-201">Środowisko</span><span class="sxs-lookup"><span data-stu-id="15a50-201">Environment</span></span>

<span data-ttu-id="15a50-202">Ustawia środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-202">Sets the app's environment.</span></span>

<span data-ttu-id="15a50-203">**Klucz**: środowiska</span><span class="sxs-lookup"><span data-stu-id="15a50-203">**Key**: environment</span></span>  
<span data-ttu-id="15a50-204">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="15a50-204">**Type**: *string*</span></span>  
<span data-ttu-id="15a50-205">**Domyślna**: produkcji</span><span class="sxs-lookup"><span data-stu-id="15a50-205">**Default**: Production</span></span>  
<span data-ttu-id="15a50-206">**Ustawić za pomocą**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="15a50-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="15a50-207">**Zmienna środowiskowa**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="15a50-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="15a50-208">Można ustawić *środowiska* dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="15a50-208">You can set the *Environment* to any value.</span></span> <span data-ttu-id="15a50-209">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="15a50-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="15a50-210">Wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="15a50-210">Values aren't case sensitive.</span></span> <span data-ttu-id="15a50-211">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="15a50-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="15a50-212">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="15a50-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="15a50-213">Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="15a50-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-214">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-215">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="15a50-216">Zestawy uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="15a50-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="15a50-217">Ustawia hostingu zestawy uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="15a50-218">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="15a50-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="15a50-219">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="15a50-219">**Type**: *string*</span></span>  
<span data-ttu-id="15a50-220">**Domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="15a50-220">**Default**: Empty string</span></span>  
<span data-ttu-id="15a50-221">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="15a50-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="15a50-222">**Zmienna środowiskowa**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="15a50-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="15a50-223">Ciąg rozdzielany średnikami obsługi zestawów uruchamiania załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="15a50-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="15a50-224">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="15a50-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="15a50-225">Mimo że konfiguracja domyślnie przyjmowana jest wartość pustego ciągu, hostingu zestawy uruchamiania zawsze należy uwzględniać zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="15a50-226">Podając hostingu zestawy uruchamiania, są one dodane do zestawu aplikacji ładowania, gdy aplikacja tworzy jego wspólne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="15a50-226">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-227">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-228">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15a50-229">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="15a50-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="15a50-230">Preferowane jest Hosting adresy URL</span><span class="sxs-lookup"><span data-stu-id="15a50-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="15a50-231">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast ustawień skonfigurowanych z `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="15a50-232">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="15a50-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="15a50-233">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="15a50-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="15a50-234">**Domyślna**: true</span><span class="sxs-lookup"><span data-stu-id="15a50-234">**Default**: true</span></span>  
<span data-ttu-id="15a50-235">**Ustawić za pomocą**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="15a50-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="15a50-236">**Zmienna środowiskowa**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="15a50-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="15a50-237">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="15a50-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-238">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-239">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15a50-240">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="15a50-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="15a50-241">Zapobiegaj Hosting uruchamiania</span><span class="sxs-lookup"><span data-stu-id="15a50-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="15a50-242">Uniemożliwia automatyczne ładowanie hosting zestawy uruchamiania, tym hosting zestawy uruchamiania konfigurowane za pomocą zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="15a50-243">Zobacz [dodać aplikacji funkcje z zewnętrznych zestawu za pomocą IHostingStartup](xref:hosting/ihostingstartup) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-243">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="15a50-244">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="15a50-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="15a50-245">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="15a50-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="15a50-246">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="15a50-246">**Default**: false</span></span>  
<span data-ttu-id="15a50-247">**Ustawić za pomocą**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="15a50-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="15a50-248">**Zmienna środowiskowa**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="15a50-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="15a50-249">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="15a50-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-250">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-251">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15a50-252">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="15a50-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="15a50-253">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="15a50-253">Server URLs</span></span>

<span data-ttu-id="15a50-254">Wskazuje adresy IP lub adresy hostów, porty i protokoły, które serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="15a50-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="15a50-255">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="15a50-255">**Key**: urls</span></span>  
<span data-ttu-id="15a50-256">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="15a50-256">**Type**: *string*</span></span>  
<span data-ttu-id="15a50-257">**Domyślna**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="15a50-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="15a50-258">**Ustawić za pomocą**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="15a50-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="15a50-259">**Zmienna środowiskowa**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="15a50-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="15a50-260">Ustaw rozdzielonych średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="15a50-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="15a50-261">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="15a50-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="15a50-262">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="15a50-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="15a50-263">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="15a50-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="15a50-264">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="15a50-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-265">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="15a50-266">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="15a50-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="15a50-267">Aby uzyskać więcej informacji, zobacz [Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="15a50-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-268">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="15a50-269">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="15a50-269">Shutdown Timeout</span></span>

<span data-ttu-id="15a50-270">Określa czas oczekiwania na zamknięcie hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="15a50-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="15a50-271">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="15a50-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="15a50-272">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="15a50-272">**Type**: *int*</span></span>  
<span data-ttu-id="15a50-273">**Domyślna**: 5</span><span class="sxs-lookup"><span data-stu-id="15a50-273">**Default**: 5</span></span>  
<span data-ttu-id="15a50-274">**Ustawić za pomocą**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="15a50-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="15a50-275">**Zmienna środowiskowa**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="15a50-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="15a50-276">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` przyjmuje — metoda rozszerzenia `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="15a50-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="15a50-277">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="15a50-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-278">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-279">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15a50-280">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="15a50-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="15a50-281">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="15a50-281">Startup Assembly</span></span>

<span data-ttu-id="15a50-282">Określa zestaw do wyszukania `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="15a50-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="15a50-283">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="15a50-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="15a50-284">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="15a50-284">**Type**: *string*</span></span>  
<span data-ttu-id="15a50-285">**Domyślna**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="15a50-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="15a50-286">**Ustawić za pomocą**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="15a50-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="15a50-287">**Zmienna środowiskowa**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="15a50-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="15a50-288">Nazwę można odwoływać się do zestawu (`string`) lub typu (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="15a50-288">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="15a50-289">Jeśli wiele `UseStartup` metody są nazywane, pierwszeństwo ma ostatnią.</span><span class="sxs-lookup"><span data-stu-id="15a50-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-290">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-291">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="15a50-292">Główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="15a50-292">Web Root</span></span>

<span data-ttu-id="15a50-293">Ustawia ścieżkę względną do statycznego zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="15a50-294">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="15a50-294">**Key**: webroot</span></span>  
<span data-ttu-id="15a50-295">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="15a50-295">**Type**: *string*</span></span>  
<span data-ttu-id="15a50-296">**Domyślne**: Jeśli nie jest określony, wartością domyślną jest "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="15a50-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="15a50-297">Jeśli ścieżka nie istnieje, jest używany dostawca pliku zerowa.</span><span class="sxs-lookup"><span data-stu-id="15a50-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="15a50-298">**Ustawić za pomocą**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="15a50-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="15a50-299">**Zmienna środowiskowa**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="15a50-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-300">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-301">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="15a50-302">Zastępowanie konfiguracji</span><span class="sxs-lookup"><span data-stu-id="15a50-302">Overriding configuration</span></span>

<span data-ttu-id="15a50-303">Użyj [konfiguracji](xref:fundamentals/configuration/index) Aby skonfigurować hosta.</span><span class="sxs-lookup"><span data-stu-id="15a50-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="15a50-304">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określić w *hosting.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="15a50-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="15a50-305">Wszelkie konfiguracja została załadowana z *hosting.json* plik może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="15a50-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="15a50-306">Zbudowany konfiguracji (w `config`) służy do konfigurowania hosta z `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="15a50-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-307">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="15a50-308">*hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="15a50-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="15a50-309">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="15a50-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-310">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15a50-311">*hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="15a50-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="15a50-312">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="15a50-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="15a50-313">`UseConfiguration` — Metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="15a50-313">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="15a50-314">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="15a50-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="15a50-315">`UseConfiguration` Metoda oczekuje klucze do dopasowania `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="15a50-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="15a50-316">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="15a50-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="15a50-317">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="15a50-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="15a50-318">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="15a50-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="15a50-319">Aby określić hosta, uruchom na określony adres URL, możesz przesłać żądaną wartość z wiersza polecenia podczas wykonywania `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="15a50-319">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="15a50-320">Argument wiersza polecenia zastępuje `urls` wartość z *hosting.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="15a50-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="15a50-321">Znaczenie kolejności</span><span class="sxs-lookup"><span data-stu-id="15a50-321">Ordering importance</span></span>

<span data-ttu-id="15a50-322">Niektóre `WebHostBuilder` ustawienia najpierw są odczytywane z zmiennych środowiskowych, jeśli ustawiona.</span><span class="sxs-lookup"><span data-stu-id="15a50-322">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="15a50-323">Te zmienne środowiskowe, użyj formatu `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="15a50-323">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="15a50-324">Aby ustawić adresów URL, które serwer nasłuchuje na domyślne, należy ustawić `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="15a50-324">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="15a50-325">Można zastąpić jedną z tych wartości zmiennych środowiskowych, określając konfiguracji (przy użyciu `UseConfiguration`) lub przez jawne ustawienie wartości (przy użyciu `UseSetting` lub jednej z metod rozszerzenia jawne, takich jak `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="15a50-325">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="15a50-326">Host używa jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="15a50-326">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="15a50-327">Jeśli chcesz programowo ustawiony domyślny adres URL na jedną wartość, ale zezwolenie na należy zastąpić konfiguracji, można użyć wiersza polecenia konfigurację po ustawieniu adres URL.</span><span class="sxs-lookup"><span data-stu-id="15a50-327">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="15a50-328">Zobacz [konfiguracji zastąpiona](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="15a50-328">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="15a50-329">Uruchamianie hosta</span><span class="sxs-lookup"><span data-stu-id="15a50-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15a50-330">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15a50-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="15a50-331">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="15a50-331">**Run**</span></span>

<span data-ttu-id="15a50-332">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący do czasu zamknięcia hosta:</span><span class="sxs-lookup"><span data-stu-id="15a50-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="15a50-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="15a50-333">**Start**</span></span>

<span data-ttu-id="15a50-334">Można uruchomić hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="15a50-334">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="15a50-335">W przypadku przekazania listę adresów URL do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="15a50-335">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="15a50-336">Można zainicjować i rozpocząć nowego hosta za pomocą wstępnie skonfigurowane ustawienia domyślne `CreateDefaultBuilder` przy użyciu metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="15a50-336">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="15a50-337">Te metody uruchomienia serwera bez dane wyjściowe konsoli i z [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podział (Ctrl-C/sigint — lub sigterm —):</span><span class="sxs-lookup"><span data-stu-id="15a50-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="15a50-338">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="15a50-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="15a50-339">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="15a50-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="15a50-340">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="15a50-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="15a50-341">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="15a50-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="15a50-342">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="15a50-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="15a50-343">**Start (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="15a50-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="15a50-344">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="15a50-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="15a50-345">Tworzy wynik, w postaci **Start (aplikacja RequestDelegate)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="15a50-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="15a50-346">**Start (akcji<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="15a50-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="15a50-347">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) do używania oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="15a50-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="15a50-348">Użyj następujących żądań przeglądarki z przykładem:</span><span class="sxs-lookup"><span data-stu-id="15a50-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="15a50-349">Żądanie</span><span class="sxs-lookup"><span data-stu-id="15a50-349">Request</span></span>                                    | <span data-ttu-id="15a50-350">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="15a50-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="15a50-351">Witaj, pole!</span><span class="sxs-lookup"><span data-stu-id="15a50-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="15a50-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="15a50-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="15a50-353">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="15a50-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="15a50-354">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="15a50-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="15a50-355">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="15a50-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="15a50-356">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="15a50-356">Hello World!</span></span>                             |

<span data-ttu-id="15a50-357">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="15a50-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="15a50-358">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="15a50-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="15a50-359">**Start (ciągu adresu url, Akcja<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="15a50-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="15a50-360">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="15a50-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="15a50-361">Tworzy wynik, w postaci **Start (akcji<IRouteBuilder> routeBuilder)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="15a50-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="15a50-362">**StartWith (akcji<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="15a50-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="15a50-363">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="15a50-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="15a50-364">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="15a50-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="15a50-365">`WaitForShutdown`bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="15a50-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="15a50-366">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="15a50-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="15a50-367">**StartWith (ciągu adresu url, Akcja<IApplicationBuilder> aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="15a50-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="15a50-368">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="15a50-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="15a50-369">Tworzy wynik, w postaci **StartWith (akcji<IApplicationBuilder> aplikacji)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="15a50-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15a50-370">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15a50-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15a50-371">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="15a50-371">**Run**</span></span>

<span data-ttu-id="15a50-372">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący, dopóki nie zostanie zamknięta hosta:</span><span class="sxs-lookup"><span data-stu-id="15a50-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="15a50-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="15a50-373">**Start**</span></span>

<span data-ttu-id="15a50-374">Można uruchomić hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="15a50-374">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="15a50-375">W przypadku przekazania listę adresów URL do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="15a50-375">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="15a50-376">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="15a50-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="15a50-377">[Interfejsu IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje o aplikacji sieci web środowiska macierzystego.</span><span class="sxs-lookup"><span data-stu-id="15a50-377">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="15a50-378">Można użyć [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="15a50-378">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="15a50-379">Można użyć [podejście oparte na Konwencji](xref:fundamentals/environments#startup-conventions) Aby skonfigurować aplikację podczas uruchamiania na podstawie środowiska.</span><span class="sxs-lookup"><span data-stu-id="15a50-379">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="15a50-380">Alternatywnie można wstrzyknąć `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="15a50-380">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="15a50-381">Oprócz `IsDevelopment` — metoda rozszerzenia, `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="15a50-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="15a50-382">Zobacz [Praca w środowiskach wielu](xref:fundamentals/environments) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="15a50-382">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="15a50-383">`IHostingEnvironment` Usługi także mogą zostać dodane bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania sieci:</span><span class="sxs-lookup"><span data-stu-id="15a50-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="15a50-384">Można wstrzyknąć `IHostingEnvironment` do `Invoke` metody podczas tworzenia niestandardowego [oprogramowanie pośredniczące](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="15a50-384">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="15a50-385">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="15a50-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="15a50-386">[Interfejsu IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia wykonywanie czynności po uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="15a50-386">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="15a50-387">Trzy właściwości w interfejsie są anulowanie tokenów, które można zarejestrować z `Action` metod, aby określić zdarzenia uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="15a50-387">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="15a50-388">Istnieje również `StopApplication` metody.</span><span class="sxs-lookup"><span data-stu-id="15a50-388">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="15a50-389">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="15a50-389">Cancellation Token</span></span>    | <span data-ttu-id="15a50-390">Wyzwalane podczas &#8230;</span><span class="sxs-lookup"><span data-stu-id="15a50-390">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="15a50-391">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="15a50-391">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="15a50-392">Host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="15a50-392">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="15a50-393">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="15a50-393">Requests may still be processing.</span></span> <span data-ttu-id="15a50-394">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="15a50-394">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="15a50-395">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="15a50-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="15a50-396">Wszystkie żądania powinna zostać przetworzona.</span><span class="sxs-lookup"><span data-stu-id="15a50-396">All requests should be processed.</span></span> <span data-ttu-id="15a50-397">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="15a50-397">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="15a50-398">Metoda</span><span class="sxs-lookup"><span data-stu-id="15a50-398">Method</span></span>            | <span data-ttu-id="15a50-399">Akcja</span><span class="sxs-lookup"><span data-stu-id="15a50-399">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="15a50-400">Zakończenie żądania bieżącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15a50-400">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="15a50-401">Rozwiązywanie problemów z System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="15a50-401">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="15a50-402">**Dotyczy tylko platformy ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="15a50-402">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="15a50-403">W przypadku tworzenia hosta przez wstrzykiwanie `IStartup` bezpośrednio do kontenera iniekcji zależności zamiast wywoływania `UseStartup` lub `Configure`, może wystąpić następujący błąd: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="15a50-403">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="15a50-404">Dzieje się tak dlatego [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (bieżącego zestawu) jest wymagane do skanowania pod kątem `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="15a50-404">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="15a50-405">Jeśli ręcznie wprowadzić `IStartup` do kontenera iniekcji zależności, Dodaj następujące wywołanie do Twojej `WebHostBuilder` o podanej nazwie zestawu:</span><span class="sxs-lookup"><span data-stu-id="15a50-405">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="15a50-406">Możesz też dodać manekina `Configure` do Twojej `WebHostBuilder`, który określa `applicationName`(`ApplicationKey`) automatycznie:</span><span class="sxs-lookup"><span data-stu-id="15a50-406">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="15a50-407">**Uwaga**: jest to tylko wymagane wraz z wydaniem programu ASP.NET 2.0 rdzeni i tylko wtedy gdy zgłaszasz nie `UseStartup` lub `Configure`.</span><span class="sxs-lookup"><span data-stu-id="15a50-407">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="15a50-408">Aby uzyskać więcej informacji, zobacz [anonsów: Microsoft.Extensions.PlatformAbstractions została usunięta (komentarz)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) i [próbki StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="15a50-408">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15a50-409">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="15a50-409">Additional resources</span></span>

* [<span data-ttu-id="15a50-410">Publikowanie do systemu Windows za pomocą usług IIS</span><span class="sxs-lookup"><span data-stu-id="15a50-410">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="15a50-411">Publikowanie w systemie Linux przy użyciu Nginx</span><span class="sxs-lookup"><span data-stu-id="15a50-411">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="15a50-412">Publikowanie w systemie Linux przy użyciu Apache</span><span class="sxs-lookup"><span data-stu-id="15a50-412">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="15a50-413">Host usługi systemu Windows</span><span class="sxs-lookup"><span data-stu-id="15a50-413">Host in a Windows Service</span></span>](xref:hosting/windows-service)
