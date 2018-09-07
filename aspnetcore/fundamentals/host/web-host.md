---
title: Host sieci Web platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat hosta sieci web w programie ASP.NET Core, który jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 09/01/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7440ab26534840b190a346614f645860fc2b7d78
ms.sourcegitcommit: 7211ae2dd702f67d36365831c490d6178c9a46c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44089902"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="0565c-103">Host sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0565c-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="0565c-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0565c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0565c-105">Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*.</span><span class="sxs-lookup"><span data-stu-id="0565c-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="0565c-106">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0565c-107">Jako minimum host konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="0565c-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="0565c-108">W tym temacie omówiono hosta sieci Web platformy ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), co jest przydatne do hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0565c-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="0565c-109">Pokrycia hosta ogólnego .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="0565c-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="0565c-110">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="0565c-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0565c-111">Tworzenie hosta przy użyciu wystąpienia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0565c-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="0565c-112">To jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="0565c-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0565c-113">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="0565c-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="0565c-114">Typowa *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="0565c-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="0565c-115">`CreateDefaultBuilder` wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="0565c-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="0565c-116">Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci web i konfiguruje serwer przy użyciu dostawcy usług konfiguracji hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="0565c-117">Kestrel opcjach domyślny, zobacz temat <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="0565c-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="0565c-118">Ustawia zawartość katalogu głównego ścieżka zwrócona przez element [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="0565c-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="0565c-119">Ładunki [konfiguracji hosta](#host-configuration-values) od:</span><span class="sxs-lookup"><span data-stu-id="0565c-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="0565c-120">Zmienne środowiskowe prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="0565c-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="0565c-121">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0565c-121">Command-line arguments.</span></span>
* <span data-ttu-id="0565c-122">Konfiguracja aplikacji obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="0565c-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="0565c-123">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0565c-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="0565c-124">*appSettings. {Środowiska} .json*.</span><span class="sxs-lookup"><span data-stu-id="0565c-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="0565c-125">[Wpisami tajnymi użytkowników](xref:security/app-secrets) uruchamiania aplikacji `Development` środowisko przy użyciu zestawu wpisu.</span><span class="sxs-lookup"><span data-stu-id="0565c-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="0565c-126">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="0565c-126">Environment variables.</span></span>
  * <span data-ttu-id="0565c-127">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0565c-127">Command-line arguments.</span></span>
* <span data-ttu-id="0565c-128">Konfiguruje [rejestrowania](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="0565c-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="0565c-129">Rejestrowanie powoduje umieszczenie [filtrowanie dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji Konfiguracja rejestrowania *appsettings.json* lub *appsettings. { Środowisko} .json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0565c-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="0565c-130">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="0565c-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="0565c-131">Umożliwia skonfigurowanie ścieżki podstawowej i portów serwer nasłuchuje na w przypadku korzystania z [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0565c-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0565c-132">Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0565c-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="0565c-133">Konfiguruje również aplikacji na [przechwytywania błędów uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="0565c-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="0565c-134">Opcjach domyślny usług IIS, zobacz temat <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="0565c-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="0565c-135">Zestawy [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="0565c-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="0565c-136">Aby uzyskać więcej informacji, zobacz [zakresu walidacji](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="0565c-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="0565c-137">Konfiguracja zdefiniowane przez `CreateDefaultBuilder` zastąpienia i wzmacnia [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)i inne metody, jak i metody rozszerzenia [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0565c-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="0565c-138">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="0565c-138">A few examples follow:</span></span>

* <span data-ttu-id="0565c-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) służy do określania dodatkowych `IConfiguration` dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="0565c-140">Następujące `ConfigureAppConfiguration` wywołanie dodaje delegata w celu uwzględnienia konfiguracji aplikacji w *appsettings.xml* pliku.</span><span class="sxs-lookup"><span data-stu-id="0565c-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="0565c-141">`ConfigureAppConfiguration` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="0565c-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="0565c-142">Należy pamiętać, że ta konfiguracja nie ma zastosowania do hosta (na przykład adresy URL serwera lub środowiska).</span><span class="sxs-lookup"><span data-stu-id="0565c-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="0565c-143">Zobacz [hosta wartości konfiguracji](#host-configuration-values) sekcji.</span><span class="sxs-lookup"><span data-stu-id="0565c-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="0565c-144">Następujące `ConfigureLogging` wywołanie dodaje delegata, aby skonfigurować poziom rejestrowania minimalny ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) do [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="0565c-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="0565c-145">Ustawienie to zastępuje ustawienia w *appsettings. Development.JSON* (`LogLevel.Debug`) i *appsettings. Production.JSON* (`LogLevel.Error`) skonfigurowane przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0565c-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0565c-146">`ConfigureLogging` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="0565c-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="0565c-147">Następujące wywołanie do `ConfigureKestrel` zastępuje to domyślne [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtów nawiązane, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0565c-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="0565c-148">Następujące wywołanie do [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) zastępuje to domyślne [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtów nawiązane, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0565c-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0565c-149">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="0565c-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0565c-150">Po uruchomieniu aplikacji z folderu głównego projektu, folder główny projektu jest używany jako katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="0565c-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0565c-151">Jest to opcja domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0565c-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0565c-152">Aby uzyskać więcej informacji na temat konfiguracji aplikacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0565c-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="0565c-153">Jako alternatywa dla użycia statycznego `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) metoda przy użyciu obsługiwanych za pomocą programu ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="0565c-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="0565c-154">Aby uzyskać więcej informacji zobacz kartę 1.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0565c-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0565c-155">Tworzenie hosta przy użyciu wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0565c-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="0565c-156">Tworzenie hosta jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="0565c-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="0565c-157">W szablonach projektu `Main` znajduje się w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0565c-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="0565c-158">`WebHostBuilder` wymaga [serwera, który implementuje IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="0565c-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="0565c-159">Wbudowane serwery są [Kestrel](xref:fundamentals/servers/kestrel) i [HTTP.sys](xref:fundamentals/servers/httpsys) (przed wydaniem programu ASP.NET Core 2.0, sterownik HTTP.sys została wywołana [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="0565c-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="0565c-160">W tym przykładzie [— metoda rozszerzenia UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Określa serwer Kestrel.</span><span class="sxs-lookup"><span data-stu-id="0565c-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="0565c-161">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="0565c-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="0565c-162">Domyślny katalog główny zawartości zostaje uzyskana dla `UseContentRoot` przez [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="0565c-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="0565c-163">Po uruchomieniu aplikacji z folderu głównego projektu, folder główny projektu jest używany jako katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="0565c-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="0565c-164">Jest to opcja domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0565c-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="0565c-165">Aby używać usług IIS jako zwrotny serwer proxy, należy wywołać [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) w ramach tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="0565c-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="0565c-166">`UseIISIntegration` nie Konfiguruj *serwera*, takiej jak [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) jest.</span><span class="sxs-lookup"><span data-stu-id="0565c-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="0565c-167">`UseIISIntegration` Umożliwia skonfigurowanie ścieżki podstawowej i portów serwer nasłuchuje na w przypadku korzystania z [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) utworzyć zwrotny serwer proxy między Kestrel i usług IIS.</span><span class="sxs-lookup"><span data-stu-id="0565c-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="0565c-168">Usługi IIS za pomocą platformy ASP.NET Core `UseKestrel` i `UseIISIntegration` musi być określona.</span><span class="sxs-lookup"><span data-stu-id="0565c-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="0565c-169">`UseIISIntegration` aktywuje tylko podczas uruchamiania usług IIS lub IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0565c-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="0565c-170">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/aspnet-core-module> i <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="0565c-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="0565c-171">Minimalny implementację, która umożliwia skonfigurowanie hosta (i aplikacji ASP.NET Core) obejmuje określenie serwerze i konfiguracji Potok żądań aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0565c-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

::: moniker-end

<span data-ttu-id="0565c-172">Podczas konfigurowania hostów [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) można przedstawić metody.</span><span class="sxs-lookup"><span data-stu-id="0565c-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="0565c-173">Jeśli `Startup` klasy jest określony, należy ją zdefiniować `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="0565c-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="0565c-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="0565c-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="0565c-175">Wiele wywołań `ConfigureServices` dołączenia do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="0565c-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="0565c-176">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpienia poprzednich ustawień.</span><span class="sxs-lookup"><span data-stu-id="0565c-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="0565c-177">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="0565c-177">Host configuration values</span></span>

<span data-ttu-id="0565c-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) opiera się na następujących podejść można ustawić wartości konfiguracji hostów:</span><span class="sxs-lookup"><span data-stu-id="0565c-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="0565c-179">Konfiguracja Konstruktora hosta, który zawiera zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="0565c-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="0565c-180">Na przykład `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="0565c-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="0565c-181">Rozszerzenia, takie jak [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) i [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (zobacz [zastępczą konfigurację](#override-configuration) sekcji).</span><span class="sxs-lookup"><span data-stu-id="0565c-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="0565c-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="0565c-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="0565c-183">Podczas ustawiania wartości z `UseSetting`, wartość jest ustawiana jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="0565c-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="0565c-184">Host używa jednego z tych opcji ustawia wartość ostatniego.</span><span class="sxs-lookup"><span data-stu-id="0565c-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="0565c-185">Aby uzyskać więcej informacji, zobacz [zastępczą konfigurację](#override-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0565c-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="0565c-186">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="0565c-186">Application Key (Name)</span></span>

<span data-ttu-id="0565c-187">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) właściwość ma wartość automatycznie, gdy [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) lub [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) jest wywoływana podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="0565c-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="0565c-188">Wartość jest równa Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="0565c-189">Aby jawnie ustawić wartość, użyj [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="0565c-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="0565c-190">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="0565c-190">**Key**: applicationName</span></span>  
<span data-ttu-id="0565c-191">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-191">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-192">**Domyślne**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="0565c-193">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0565c-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0565c-194">**Zmienna środowiskowa**: `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="0565c-194">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="0565c-195">Przechwytywania błędów uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0565c-195">Capture Startup Errors</span></span>

<span data-ttu-id="0565c-196">To ustawienie steruje przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0565c-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="0565c-197">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="0565c-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="0565c-198">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="0565c-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0565c-199">**Domyślne**: wartość domyślna to `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, w którym domyślnie są `true`.</span><span class="sxs-lookup"><span data-stu-id="0565c-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="0565c-200">**Można ustawić przy użyciu**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="0565c-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="0565c-201">**Zmienna środowiskowa**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="0565c-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="0565c-202">Gdy `false`, błędy podczas uruchamiania wynik na hoście, kończenie.</span><span class="sxs-lookup"><span data-stu-id="0565c-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0565c-203">Gdy `true`, host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="0565c-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="0565c-204">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="0565c-204">Content Root</span></span>

<span data-ttu-id="0565c-205">To ustawienie określa, gdzie platformy ASP.NET Core rozpoczyna się wyszukiwanie plików zawartości, takich jak widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="0565c-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="0565c-206">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="0565c-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="0565c-207">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-207">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-208">**Domyślne**: wartość domyślna to folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="0565c-209">**Można ustawić przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="0565c-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="0565c-210">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="0565c-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="0565c-211">Główny zawartości jest również używane jako podstawową ścieżkę dla [ustawienie katalog główny sieci Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="0565c-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="0565c-212">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="0565c-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="0565c-213">Szczegółowe informacje o błędach</span><span class="sxs-lookup"><span data-stu-id="0565c-213">Detailed Errors</span></span>

<span data-ttu-id="0565c-214">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="0565c-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="0565c-215">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="0565c-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="0565c-216">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="0565c-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0565c-217">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="0565c-217">**Default**: false</span></span>  
<span data-ttu-id="0565c-218">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0565c-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0565c-219">**Zmienna środowiskowa**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="0565c-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="0565c-220">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ustawiono `Development`), aplikacja przechwytuje szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="0565c-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="0565c-221">Środowisko</span><span class="sxs-lookup"><span data-stu-id="0565c-221">Environment</span></span>

<span data-ttu-id="0565c-222">Ustawia środowiska Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-222">Sets the app's environment.</span></span>

<span data-ttu-id="0565c-223">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="0565c-223">**Key**: environment</span></span>  
<span data-ttu-id="0565c-224">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-224">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-225">**Domyślne**: produkcji</span><span class="sxs-lookup"><span data-stu-id="0565c-225">**Default**: Production</span></span>  
<span data-ttu-id="0565c-226">**Można ustawić przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="0565c-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="0565c-227">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="0565c-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="0565c-228">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="0565c-228">The environment can be set to any value.</span></span> <span data-ttu-id="0565c-229">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="0565c-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0565c-230">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="0565c-230">Values aren't case sensitive.</span></span> <span data-ttu-id="0565c-231">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="0565c-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="0565c-232">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0565c-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="0565c-233">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0565c-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="0565c-234">Hosting zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="0565c-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="0565c-235">Ustawia hostingu zestawy uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="0565c-236">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="0565c-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="0565c-237">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-237">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-238">**Domyślne**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="0565c-238">**Default**: Empty string</span></span>  
<span data-ttu-id="0565c-239">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0565c-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0565c-240">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0565c-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="0565c-241">Rozdzielana średnikami ciąg hostingu zestawy startowe załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0565c-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="0565c-242">Mimo że konfiguracja ma domyślnie wartość pustego ciągu, hostingu zestawy startowe zawsze zawierać zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="0565c-243">Hostingu zestawy startowe są udostępniane, są dodawane do zestawu aplikacji dotyczące ładowania, gdy aplikacja tworzy swoich usług wspólne podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0565c-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="0565c-244">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="0565c-244">HTTPS Port</span></span>

<span data-ttu-id="0565c-245">Ustawienie HTTPS przekierowania portu.</span><span class="sxs-lookup"><span data-stu-id="0565c-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="0565c-246">Używane w [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="0565c-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="0565c-247">**Klucz**: https_port **typu**: *ciąg*
**domyślne**: nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="0565c-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="0565c-248">**Można ustawić przy użyciu**: `UseSetting` 
 **zmiennej środowiskowej**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="0565c-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="0565c-249">Hosting zestawy wykluczania uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0565c-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="0565c-250">OPIS ELEMENTU</span><span class="sxs-lookup"><span data-stu-id="0565c-250">DESCRIPTION</span></span>

<span data-ttu-id="0565c-251">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="0565c-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="0565c-252">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-252">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-253">**Domyślne**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="0565c-253">**Default**: Empty string</span></span>  
<span data-ttu-id="0565c-254">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0565c-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0565c-255">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0565c-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="0565c-256">Rozdzielana średnikami ciąg hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0565c-256">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="0565c-257">Preferuj hostingu adresów URL</span><span class="sxs-lookup"><span data-stu-id="0565c-257">Prefer Hosting URLs</span></span>

<span data-ttu-id="0565c-258">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast konfigurowane przy użyciu `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-258">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="0565c-259">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="0565c-259">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="0565c-260">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="0565c-260">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0565c-261">**Domyślne**: true</span><span class="sxs-lookup"><span data-stu-id="0565c-261">**Default**: true</span></span>  
<span data-ttu-id="0565c-262">**Można ustawić przy użyciu**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="0565c-262">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="0565c-263">**Zmienna środowiskowa**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="0565c-263">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="0565c-264">Zapobiegaj hostingu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="0565c-264">Prevent Hosting Startup</span></span>

<span data-ttu-id="0565c-265">Uniemożliwia automatyczne ładowanie obsługi zestawów uruchamiania, w tym hosting zestawy startowe skonfigurowany przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-265">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="0565c-266">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0565c-266">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="0565c-267">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0565c-267">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="0565c-268">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="0565c-268">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="0565c-269">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="0565c-269">**Default**: false</span></span>  
<span data-ttu-id="0565c-270">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="0565c-270">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="0565c-271">**Zmienna środowiskowa**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="0565c-271">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="0565c-272">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="0565c-272">Server URLs</span></span>

<span data-ttu-id="0565c-273">Wskazuje adresy IP lub adresów hosta, portów i protokołów, które serwer powinien nasłuchiwać żądań protokołu.</span><span class="sxs-lookup"><span data-stu-id="0565c-273">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="0565c-274">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="0565c-274">**Key**: urls</span></span>  
<span data-ttu-id="0565c-275">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-275">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-276">**Domyślne**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="0565c-276">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="0565c-277">**Można ustawić przy użyciu**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="0565c-277">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="0565c-278">**Zmienna środowiskowa**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="0565c-278">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="0565c-279">Ustaw rozdzielone średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="0565c-279">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="0565c-280">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="0565c-280">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0565c-281">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="0565c-281">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="0565c-282">Protokół (`http://` lub `https://`) muszą być dołączone do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="0565c-282">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0565c-283">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="0565c-283">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="0565c-284">Kestrel ma swój własny konfiguracji punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="0565c-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="0565c-285">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0565c-285">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="0565c-286">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="0565c-286">Shutdown Timeout</span></span>

<span data-ttu-id="0565c-287">Określa ilość czasu oczekiwania dla hosta sieci web zamknąć.</span><span class="sxs-lookup"><span data-stu-id="0565c-287">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="0565c-288">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="0565c-288">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="0565c-289">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="0565c-289">**Type**: *int*</span></span>  
<span data-ttu-id="0565c-290">**Domyślne**: 5</span><span class="sxs-lookup"><span data-stu-id="0565c-290">**Default**: 5</span></span>  
<span data-ttu-id="0565c-291">**Można ustawić przy użyciu**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="0565c-291">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="0565c-292">**Zmienna środowiskowa**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="0565c-292">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="0565c-293">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) — metoda rozszerzenia ma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="0565c-293">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="0565c-294">Podczas limitu czasu, hostingu:</span><span class="sxs-lookup"><span data-stu-id="0565c-294">During the timeout period, hosting:</span></span>

* <span data-ttu-id="0565c-295">Wyzwalacze [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="0565c-295">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="0565c-296">Podejmuje próby zatrzymania usług hostowanych, rejestrowanie błędów, których nie można zatrzymać usługi.</span><span class="sxs-lookup"><span data-stu-id="0565c-296">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="0565c-297">Jeśli upłynie limit czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostaną zatrzymane podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-297">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="0565c-298">Usługi zatrzymania nawet wtedy, gdy jeszcze nie zakończyło się przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="0565c-298">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="0565c-299">Jeśli usługi wymagają dodatkowego czasu, aby zatrzymać, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="0565c-299">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="0565c-300">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="0565c-300">Startup Assembly</span></span>

<span data-ttu-id="0565c-301">Określa zestaw, aby wyszukać `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="0565c-301">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="0565c-302">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="0565c-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="0565c-303">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-303">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-304">**Domyślne**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="0565c-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="0565c-305">**Można ustawić przy użyciu**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="0565c-305">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="0565c-306">**Zmienna środowiskowa**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="0565c-306">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="0565c-307">Zestawu według nazwy (`string`) lub wpisz (`TStartup`) mogą być przywoływane.</span><span class="sxs-lookup"><span data-stu-id="0565c-307">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="0565c-308">Jeśli wiele `UseStartup` metody są wywoływane, ostatni z nich ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="0565c-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="0565c-309">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="0565c-309">Web Root</span></span>

<span data-ttu-id="0565c-310">Ustawia ścieżkę względną do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-310">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="0565c-311">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="0565c-311">**Key**: webroot</span></span>  
<span data-ttu-id="0565c-312">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="0565c-312">**Type**: *string*</span></span>  
<span data-ttu-id="0565c-313">**Domyślne**: Jeśli nie zostanie określony, wartość domyślna to "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="0565c-313">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="0565c-314">Jeśli ścieżka nie istnieje, jest używany dostawca pliku pusta.</span><span class="sxs-lookup"><span data-stu-id="0565c-314">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="0565c-315">**Można ustawić przy użyciu**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="0565c-315">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="0565c-316">**Zmienna środowiskowa**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="0565c-316">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="0565c-317">Zastąp konfigurację</span><span class="sxs-lookup"><span data-stu-id="0565c-317">Override configuration</span></span>

<span data-ttu-id="0565c-318">Użyj [konfiguracji](xref:fundamentals/configuration/index) skonfigurować hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="0565c-318">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="0565c-319">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określony w *hostsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="0565c-319">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="0565c-320">Dowolna Konfiguracja ładowane z *hostsettings.json* pliku może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="0565c-320">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="0565c-321">Konfiguracja wbudowanych (w `config`) służy do konfigurowania hosta z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0565c-321">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="0565c-322">`IWebHostBuilder` Konfiguracja zostanie dodany do konfiguracji aplikacji, ale prawdą nie dotyczy&mdash; `ConfigureAppConfiguration` nie ma wpływu na `IWebHostBuilder` konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0565c-322">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0565c-323">Zastępowanie konfiguracji, jaką zapewnia `UseUrls` z *hostsettings.json* config argument po pierwsze, wiersza polecenia konfiguracji drugiego:</span><span class="sxs-lookup"><span data-stu-id="0565c-323">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="0565c-324">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="0565c-324">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0565c-325">Zastępowanie konfiguracji, jaką zapewnia `UseUrls` z *hostsettings.json* config argument po pierwsze, wiersza polecenia konfiguracji drugiego:</span><span class="sxs-lookup"><span data-stu-id="0565c-325">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
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

<span data-ttu-id="0565c-326">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="0565c-326">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0565c-327">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metody rozszerzenia nie jest obecnie zdolne do analizowania sekcji konfiguracji, zwracany przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="0565c-327">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="0565c-328">`GetSection` Metoda klucze konfiguracji do sekcji żądane filtry, ale klucze nie powoduje usunięcia nazwy sekcji (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="0565c-328">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="0565c-329">`UseConfiguration` Metoda oczekuje kluczy, aby dopasować `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="0565c-329">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="0565c-330">Obecność nazwa sekcji na kluczach zapobiega wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="0565c-330">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="0565c-331">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="0565c-331">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="0565c-332">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie sekcję konfiguracji do WebHostBuilder.UseConfiguration korzysta z kluczami pełną](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="0565c-332">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="0565c-333">`UseConfiguration` tylko kopiuje kluczy z dostarczonego `IConfiguration` konfiguracji konstruktora hosta.</span><span class="sxs-lookup"><span data-stu-id="0565c-333">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="0565c-334">W związku z tym, ustawienie `reloadOnChange: true` plików ustawień JSON, INI i XML nie ma wpływu.</span><span class="sxs-lookup"><span data-stu-id="0565c-334">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="0565c-335">Aby określić hosta, uruchom na określony adres URL, żądaną wartość w można przekazać polecenie w wierszu polecenia podczas wykonywania [dotnet, uruchom](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="0565c-335">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="0565c-336">Argument wiersza polecenia zastępuje `urls` wartość z *hostsettings.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="0565c-336">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="0565c-337">Zarządzać hosta</span><span class="sxs-lookup"><span data-stu-id="0565c-337">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0565c-338">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="0565c-338">**Run**</span></span>

<span data-ttu-id="0565c-339">`Run` Metoda uruchamia aplikację sieci web i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="0565c-339">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0565c-340">**Start**</span><span class="sxs-lookup"><span data-stu-id="0565c-340">**Start**</span></span>

<span data-ttu-id="0565c-341">Uruchamiane w sposób nieblokującej na poziomie hosta, przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="0565c-341">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0565c-342">Jeśli lista adresów URL jest przekazywany do `Start` metody nasłuchuje on na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="0565c-342">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="0565c-343">Aplikacja może inicjuje i uruchamia nowy host, przy użyciu wstępnie skonfigurowanych ustawień domyślnych z `CreateDefaultBuilder` za pomocą innej metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="0565c-343">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="0565c-344">Te metody uruchomić serwer bez danych wyjściowych konsoli i przy [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podziału (Ctrl-C/SIGINT lub SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="0565c-344">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="0565c-345">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="0565c-345">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="0565c-346">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="0565c-346">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0565c-347">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="0565c-347">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0565c-348">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0565c-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0565c-349">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="0565c-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0565c-350">**Uruchom (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="0565c-350">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="0565c-351">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="0565c-351">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0565c-352">Daje ten sam wynik jako **Start (aplikacja RequestDelegate)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="0565c-352">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="0565c-353">**Uruchom (Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0565c-353">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="0565c-354">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) używać oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="0565c-354">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="0565c-355">W przykładzie, należy użyć następujących żądań przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="0565c-355">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="0565c-356">Żądanie</span><span class="sxs-lookup"><span data-stu-id="0565c-356">Request</span></span>                                    | <span data-ttu-id="0565c-357">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="0565c-357">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="0565c-358">Witaj, Martin!</span><span class="sxs-lookup"><span data-stu-id="0565c-358">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="0565c-359">Buenos diasem, Catrina!</span><span class="sxs-lookup"><span data-stu-id="0565c-359">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="0565c-360">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="0565c-360">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="0565c-361">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="0565c-361">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="0565c-362">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="0565c-362">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="0565c-363">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="0565c-363">Hello World!</span></span>                             |

<span data-ttu-id="0565c-364">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0565c-364">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0565c-365">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="0565c-365">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0565c-366">**Uruchom (ciąg adresu url, Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="0565c-366">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="0565c-367">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0565c-367">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0565c-368">Daje ten sam wynik jako **Start (Akcja&lt;IRouteBuilder&gt; routeBuilder)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="0565c-368">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="0565c-369">**StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="0565c-369">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="0565c-370">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0565c-370">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0565c-371">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="0565c-371">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="0565c-372">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0565c-372">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="0565c-373">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="0565c-373">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="0565c-374">**StartWith (ciąg adresu url, Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="0565c-374">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="0565c-375">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0565c-375">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="0565c-376">Daje ten sam wynik jako **StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="0565c-376">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0565c-377">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="0565c-377">**Run**</span></span>

<span data-ttu-id="0565c-378">`Run` Metoda uruchamia aplikację sieci web i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="0565c-378">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0565c-379">**Start**</span><span class="sxs-lookup"><span data-stu-id="0565c-379">**Start**</span></span>

<span data-ttu-id="0565c-380">Uruchamiane w sposób nieblokującej na poziomie hosta, przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="0565c-380">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0565c-381">Jeśli lista adresów URL jest przekazywany do `Start` metody nasłuchuje on na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="0565c-381">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="0565c-382">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="0565c-382">IHostingEnvironment interface</span></span>

<span data-ttu-id="0565c-383">[Interfejsu IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje dotyczące aplikacji sieci web, środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="0565c-383">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="0565c-384">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać `IHostingEnvironment` aby można było używać jej właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="0565c-384">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="0565c-385">A [podejścia opartego na Konwencji](xref:fundamentals/environments#environment-based-startup-class-and-methods) mogą być używane do konfigurowania aplikacji przy uruchamianiu opartych na środowisku.</span><span class="sxs-lookup"><span data-stu-id="0565c-385">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="0565c-386">Alternatywnie wstrzyknąć `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0565c-386">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="0565c-387">Oprócz `IsDevelopment` metody rozszerzenia `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="0565c-387">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="0565c-388">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0565c-388">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="0565c-389">`IHostingEnvironment` Usługi może także wstrzyknięte bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="0565c-389">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="0565c-390">`IHostingEnvironment` może być wstrzykuje `Invoke` metody podczas tworzenia niestandardowych [oprogramowania pośredniczącego](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="0565c-390">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="0565c-391">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0565c-391">IApplicationLifetime interface</span></span>

<span data-ttu-id="0565c-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działań po uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="0565c-392">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="0565c-393">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="0565c-393">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="0565c-394">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="0565c-394">Cancellation Token</span></span>    | <span data-ttu-id="0565c-395">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="0565c-395">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="0565c-396">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="0565c-396">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="0565c-397">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="0565c-397">The host has fully started.</span></span> |
| [<span data-ttu-id="0565c-398">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="0565c-398">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="0565c-399">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="0565c-399">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="0565c-400">Wszystkie żądania powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="0565c-400">All requests should be processed.</span></span> <span data-ttu-id="0565c-401">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="0565c-401">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="0565c-402">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="0565c-402">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="0565c-403">Wykonuje łagodne zamykanie hosta.</span><span class="sxs-lookup"><span data-stu-id="0565c-403">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="0565c-404">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="0565c-404">Requests may still be processing.</span></span> <span data-ttu-id="0565c-405">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="0565c-405">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="0565c-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-406">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="0565c-407">Następujące klasy używa `StopApplication` bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="0565c-407">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="0565c-408">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="0565c-408">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0565c-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="0565c-409">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="0565c-410">Gdy `ValidateScopes` ustawiono `true`, domyślny dostawca usługi sprawdza upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="0565c-410">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="0565c-411">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="0565c-411">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="0565c-412">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="0565c-412">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="0565c-413">Dostawcy usług główny jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0565c-413">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="0565c-414">Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0565c-414">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="0565c-415">Usługi o określonym zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="0565c-415">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="0565c-416">Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="0565c-416">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="0565c-417">Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0565c-417">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="0565c-418">Aby zawsze weryfikowały zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) w Konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="0565c-418">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="0565c-419">Rozwiązywanie problemów z System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="0565c-419">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="0565c-420">**Następujące ma zastosowanie tylko do aplikacji ASP.NET Core 2.0, gdy aplikacja nie wywołać `UseStartup` lub `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="0565c-420">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="0565c-421">Host mogły zostać utworzone przez iniekcję `IStartup` bezpośrednio do kontenera iniekcji zależności zamiast wywoływania `UseStartup` lub `Configure`:</span><span class="sxs-lookup"><span data-stu-id="0565c-421">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="0565c-422">Jeśli host jest wbudowana w ten sposób, może wystąpić następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="0565c-422">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="0565c-423">Dzieje się tak dlatego, nazwę aplikacji (nazwa bieżącego zestawu) jest wymagany do skanowania w poszukiwaniu `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="0565c-423">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="0565c-424">Jeśli aplikacja ręcznie wprowadza `IStartup` do kontenera iniekcji zależności, Dodaj następujące wywołanie do `WebHostBuilder` o podanej nazwie zestawu:</span><span class="sxs-lookup"><span data-stu-id="0565c-424">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="0565c-425">Możesz też dodać dummy `Configure` do `WebHostBuilder`, który automatycznie ustawia nazwę aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0565c-425">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="0565c-426">Aby uzyskać więcej informacji, zobacz [anonsów: Microsoft.Extensions.PlatformAbstractions został usunięty (komentarz)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) i [przykładowe StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="0565c-426">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0565c-427">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="0565c-427">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
