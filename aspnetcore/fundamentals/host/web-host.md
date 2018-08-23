---
title: Host sieci Web platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat hosta sieci web w programie ASP.NET Core, który jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: dfef2bf21f325f11d147379f75a8d81a8bd05eec
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41886749"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="d16f4-103">Host sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d16f4-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="d16f4-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d16f4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d16f4-105">Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*.</span><span class="sxs-lookup"><span data-stu-id="d16f4-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d16f4-106">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d16f4-107">Jako minimum host konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="d16f4-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="d16f4-108">W tym temacie omówiono hosta sieci Web platformy ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), co jest przydatne do hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="d16f4-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="d16f4-109">Pokrycia hosta ogólnego .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="d16f4-110">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="d16f4-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d16f4-111">Tworzenie hosta przy użyciu wystąpienia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d16f4-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d16f4-112">To jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="d16f4-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d16f4-113">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d16f4-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d16f4-114">Typowa *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="d16f4-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="d16f4-115">`CreateDefaultBuilder` wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="d16f4-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d16f4-116">Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci web i konfiguruje serwer przy użyciu dostawcy usług konfiguracji hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="d16f4-117">Kestrel opcjach domyślny, zobacz temat <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="d16f4-118">Ustawia zawartość katalogu głównego ścieżka zwrócona przez element [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d16f4-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d16f4-119">Ładunki [konfiguracji hosta](#host-configuration-values) od:</span><span class="sxs-lookup"><span data-stu-id="d16f4-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="d16f4-120">Zmienne środowiskowe prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="d16f4-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="d16f4-121">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d16f4-121">Command-line arguments.</span></span>
* <span data-ttu-id="d16f4-122">Konfiguracja aplikacji obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="d16f4-122">Loads app configuration from:</span></span>
  * <span data-ttu-id="d16f4-123">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d16f4-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="d16f4-124">*appSettings. {Środowiska} .json*.</span><span class="sxs-lookup"><span data-stu-id="d16f4-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d16f4-125">[Wpisami tajnymi użytkowników](xref:security/app-secrets) uruchamiania aplikacji `Development` środowisko przy użyciu zestawu wpisu.</span><span class="sxs-lookup"><span data-stu-id="d16f4-125">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d16f4-126">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="d16f4-126">Environment variables.</span></span>
  * <span data-ttu-id="d16f4-127">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d16f4-127">Command-line arguments.</span></span>
* <span data-ttu-id="d16f4-128">Konfiguruje [rejestrowania](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d16f4-129">Rejestrowanie powoduje umieszczenie [filtrowanie dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji Konfiguracja rejestrowania *appsettings.json* lub *appsettings. { Środowisko} .json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d16f4-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d16f4-130">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d16f4-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="d16f4-131">Umożliwia skonfigurowanie ścieżki podstawowej i portów serwer nasłuchuje na w przypadku korzystania z [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d16f4-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d16f4-132">Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d16f4-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d16f4-133">Konfiguruje również aplikacji na [przechwytywania błędów uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d16f4-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d16f4-134">Opcjach domyślny usług IIS, zobacz temat <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="d16f4-135">Zestawy [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="d16f4-136">Aby uzyskać więcej informacji, zobacz [zakresu walidacji](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="d16f4-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="d16f4-137">Konfiguracja zdefiniowane przez `CreateDefaultBuilder` zastąpienia i wzmacnia [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)i inne metody, jak i metody rozszerzenia [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d16f4-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="d16f4-138">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="d16f4-138">A few examples follow:</span></span>

* <span data-ttu-id="d16f4-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) służy do określania dodatkowych `IConfiguration` dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="d16f4-140">Następujące `ConfigureAppConfiguration` wywołanie dodaje delegata w celu uwzględnienia konfiguracji aplikacji w *appsettings.xml* pliku.</span><span class="sxs-lookup"><span data-stu-id="d16f4-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="d16f4-141">`ConfigureAppConfiguration` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="d16f4-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="d16f4-142">Należy pamiętać, że ta konfiguracja nie ma zastosowania do hosta (na przykład adresy URL serwera lub środowiska).</span><span class="sxs-lookup"><span data-stu-id="d16f4-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="d16f4-143">Zobacz [hosta wartości konfiguracji](#host-configuration-values) sekcji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="d16f4-144">Następujące `ConfigureLogging` wywołanie dodaje delegata, aby skonfigurować poziom rejestrowania minimalny ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) do [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="d16f4-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="d16f4-145">Ustawienie to zastępuje ustawienia w *appsettings. Development.JSON* (`LogLevel.Debug`) i *appsettings. Production.JSON* (`LogLevel.Error`) skonfigurowane przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d16f4-146">`ConfigureLogging` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="d16f4-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="d16f4-147">Następujące wywołanie do [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) zastępuje to domyślne [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtów nawiązane, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="d16f4-148">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="d16f4-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d16f4-149">Po uruchomieniu aplikacji z folderu głównego projektu, folder główny projektu jest używany jako katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="d16f4-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d16f4-150">Jest to opcja domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d16f4-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d16f4-151">Aby uzyskać więcej informacji na temat konfiguracji aplikacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="d16f4-152">Jako alternatywa dla użycia statycznego `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) metoda przy użyciu obsługiwanych za pomocą programu ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d16f4-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d16f4-153">Aby uzyskać więcej informacji zobacz kartę 1.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d16f4-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d16f4-154">Tworzenie hosta przy użyciu wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d16f4-154">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d16f4-155">Tworzenie hosta jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="d16f4-155">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d16f4-156">W szablonach projektu `Main` znajduje się w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d16f4-156">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="d16f4-157">`WebHostBuilder` wymaga [serwera, który implementuje IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="d16f4-157">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="d16f4-158">Wbudowane serwery są [Kestrel](xref:fundamentals/servers/kestrel) i [HTTP.sys](xref:fundamentals/servers/httpsys) (przed wydaniem programu ASP.NET Core 2.0, sterownik HTTP.sys została wywołana [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d16f4-158">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d16f4-159">W tym przykładzie [— metoda rozszerzenia UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Określa serwer Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d16f4-159">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d16f4-160">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="d16f4-160">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d16f4-161">Domyślny katalog główny zawartości zostaje uzyskana dla `UseContentRoot` przez [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d16f4-161">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d16f4-162">Po uruchomieniu aplikacji z folderu głównego projektu, folder główny projektu jest używany jako katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="d16f4-162">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d16f4-163">Jest to opcja domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d16f4-163">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d16f4-164">Aby używać usług IIS jako zwrotny serwer proxy, należy wywołać [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) w ramach tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="d16f4-164">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d16f4-165">`UseIISIntegration` nie Konfiguruj *serwera*, takiej jak [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) jest.</span><span class="sxs-lookup"><span data-stu-id="d16f4-165">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d16f4-166">`UseIISIntegration` Umożliwia skonfigurowanie ścieżki podstawowej i portów serwer nasłuchuje na w przypadku korzystania z [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) utworzyć zwrotny serwer proxy między Kestrel i usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d16f4-166">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d16f4-167">Usługi IIS za pomocą platformy ASP.NET Core `UseKestrel` i `UseIISIntegration` musi być określona.</span><span class="sxs-lookup"><span data-stu-id="d16f4-167">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="d16f4-168">`UseIISIntegration` aktywuje tylko podczas uruchamiania usług IIS lub IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d16f4-168">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d16f4-169">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/aspnet-core-module> i <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-169">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="d16f4-170">Minimalny implementację, która umożliwia skonfigurowanie hosta (i aplikacji ASP.NET Core) obejmuje określenie serwerze i konfiguracji Potok żądań aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d16f4-170">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="d16f4-171">Podczas konfigurowania hostów [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) można przedstawić metody.</span><span class="sxs-lookup"><span data-stu-id="d16f4-171">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="d16f4-172">Jeśli `Startup` klasy jest określony, należy ją zdefiniować `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="d16f4-172">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d16f4-173">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-173">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="d16f4-174">Wiele wywołań `ConfigureServices` dołączenia do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="d16f4-174">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d16f4-175">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpienia poprzednich ustawień.</span><span class="sxs-lookup"><span data-stu-id="d16f4-175">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d16f4-176">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="d16f4-176">Host configuration values</span></span>

<span data-ttu-id="d16f4-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) opiera się na następujących podejść można ustawić wartości konfiguracji hostów:</span><span class="sxs-lookup"><span data-stu-id="d16f4-177">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d16f4-178">Konfiguracja Konstruktora hosta, który zawiera zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-178">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d16f4-179">Na przykład `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-179">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="d16f4-180">Rozszerzenia, takie jak [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) i [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (zobacz [zastępczą konfigurację](#override-configuration) sekcji).</span><span class="sxs-lookup"><span data-stu-id="d16f4-180">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="d16f4-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="d16f4-181">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d16f4-182">Podczas ustawiania wartości z `UseSetting`, wartość jest ustawiana jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="d16f4-182">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d16f4-183">Host używa jednego z tych opcji ustawia wartość ostatniego.</span><span class="sxs-lookup"><span data-stu-id="d16f4-183">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d16f4-184">Aby uzyskać więcej informacji, zobacz [zastępczą konfigurację](#override-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-184">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="d16f4-185">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="d16f4-185">Application Key (Name)</span></span>

<span data-ttu-id="d16f4-186">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) właściwość ma wartość automatycznie, gdy [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) lub [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) jest wywoływana podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="d16f4-186">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="d16f4-187">Wartość jest równa Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-187">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="d16f4-188">Aby jawnie ustawić wartość, użyj [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="d16f4-188">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="d16f4-189">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="d16f4-189">**Key**: applicationName</span></span>  
<span data-ttu-id="d16f4-190">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d16f4-190">**Type**: *string*</span></span>  
<span data-ttu-id="d16f4-191">**Domyślne**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-191">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="d16f4-192">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d16f4-192">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d16f4-193">**Zmienna środowiskowa**: `ASPNETCORE_APPLICATIONKEY`</span><span class="sxs-lookup"><span data-stu-id="d16f4-193">**Environment variable**: `ASPNETCORE_APPLICATIONKEY`</span></span>

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

### <a name="capture-startup-errors"></a><span data-ttu-id="d16f4-194">Przechwytywania błędów uruchamiania</span><span class="sxs-lookup"><span data-stu-id="d16f4-194">Capture Startup Errors</span></span>

<span data-ttu-id="d16f4-195">To ustawienie steruje przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-195">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d16f4-196">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d16f4-196">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d16f4-197">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d16f4-197">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d16f4-198">**Domyślne**: wartość domyślna to `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, w którym domyślnie są `true`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-198">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d16f4-199">**Można ustawić przy użyciu**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d16f4-199">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d16f4-200">**Zmienna środowiskowa**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d16f4-200">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d16f4-201">Gdy `false`, błędy podczas uruchamiania wynik na hoście, kończenie.</span><span class="sxs-lookup"><span data-stu-id="d16f4-201">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d16f4-202">Gdy `true`, host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="d16f4-202">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

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

### <a name="content-root"></a><span data-ttu-id="d16f4-203">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="d16f4-203">Content Root</span></span>

<span data-ttu-id="d16f4-204">To ustawienie określa, gdzie platformy ASP.NET Core rozpoczyna się wyszukiwanie plików zawartości, takich jak widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="d16f4-204">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d16f4-205">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d16f4-205">**Key**: contentRoot</span></span>  
<span data-ttu-id="d16f4-206">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d16f4-206">**Type**: *string*</span></span>  
<span data-ttu-id="d16f4-207">**Domyślne**: wartość domyślna to folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-207">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d16f4-208">**Można ustawić przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d16f4-208">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d16f4-209">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d16f4-209">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d16f4-210">Główny zawartości jest również używane jako podstawową ścieżkę dla [ustawienie katalog główny sieci Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d16f4-210">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d16f4-211">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="d16f4-211">If the path doesn't exist, the host fails to start.</span></span>

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

### <a name="detailed-errors"></a><span data-ttu-id="d16f4-212">Szczegółowe informacje o błędach</span><span class="sxs-lookup"><span data-stu-id="d16f4-212">Detailed Errors</span></span>

<span data-ttu-id="d16f4-213">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="d16f4-213">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d16f4-214">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d16f4-214">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d16f4-215">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d16f4-215">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d16f4-216">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="d16f4-216">**Default**: false</span></span>  
<span data-ttu-id="d16f4-217">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d16f4-217">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d16f4-218">**Zmienna środowiskowa**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d16f4-218">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d16f4-219">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ustawiono `Development`), aplikacja przechwytuje szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="d16f4-219">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

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

### <a name="environment"></a><span data-ttu-id="d16f4-220">Środowisko</span><span class="sxs-lookup"><span data-stu-id="d16f4-220">Environment</span></span>

<span data-ttu-id="d16f4-221">Ustawia środowiska Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-221">Sets the app's environment.</span></span>

<span data-ttu-id="d16f4-222">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="d16f4-222">**Key**: environment</span></span>  
<span data-ttu-id="d16f4-223">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d16f4-223">**Type**: *string*</span></span>  
<span data-ttu-id="d16f4-224">**Domyślne**: produkcji</span><span class="sxs-lookup"><span data-stu-id="d16f4-224">**Default**: Production</span></span>  
<span data-ttu-id="d16f4-225">**Można ustawić przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d16f4-225">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d16f4-226">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d16f4-226">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d16f4-227">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="d16f4-227">The environment can be set to any value.</span></span> <span data-ttu-id="d16f4-228">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-228">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d16f4-229">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="d16f4-229">Values aren't case sensitive.</span></span> <span data-ttu-id="d16f4-230">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="d16f4-230">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d16f4-231">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d16f4-231">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d16f4-232">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-232">For more information, see <xref:fundamentals/environments>.</span></span>

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

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d16f4-233">Hosting zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="d16f4-233">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d16f4-234">Ustawia hostingu zestawy uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-234">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d16f4-235">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d16f4-235">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d16f4-236">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d16f4-236">**Type**: *string*</span></span>  
<span data-ttu-id="d16f4-237">**Domyślne**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="d16f4-237">**Default**: Empty string</span></span>  
<span data-ttu-id="d16f4-238">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d16f4-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d16f4-239">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d16f4-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d16f4-240">Rozdzielana średnikami ciąg hostingu zestawy startowe załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-240">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="d16f4-241">Mimo że konfiguracja ma domyślnie wartość pustego ciągu, hostingu zestawy startowe zawsze zawierać zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-241">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d16f4-242">Hostingu zestawy startowe są udostępniane, są dodawane do zestawu aplikacji dotyczące ładowania, gdy aplikacja tworzy swoich usług wspólne podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-242">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="d16f4-243">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="d16f4-243">HTTPS Port</span></span>

<span data-ttu-id="d16f4-244">Ustawienie HTTPS przekierowania portu.</span><span class="sxs-lookup"><span data-stu-id="d16f4-244">Set the HTTPS redirect port.</span></span> <span data-ttu-id="d16f4-245">Używane w [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="d16f4-245">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="d16f4-246">**Klucz**: https_port **typu**: *ciąg*
**domyślne**: nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="d16f4-246">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="d16f4-247">**Można ustawić przy użyciu**: `UseSetting` 
 **zmiennej środowiskowej**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="d16f4-247">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d16f4-248">Preferuj hostingu adresów URL</span><span class="sxs-lookup"><span data-stu-id="d16f4-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="d16f4-249">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast konfigurowane przy użyciu `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d16f4-250">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d16f4-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d16f4-251">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d16f4-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d16f4-252">**Domyślne**: true</span><span class="sxs-lookup"><span data-stu-id="d16f4-252">**Default**: true</span></span>  
<span data-ttu-id="d16f4-253">**Można ustawić przy użyciu**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d16f4-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d16f4-254">**Zmienna środowiskowa**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d16f4-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d16f4-255">Zapobiegaj hostingu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="d16f4-255">Prevent Hosting Startup</span></span>

<span data-ttu-id="d16f4-256">Uniemożliwia automatyczne ładowanie obsługi zestawów uruchamiania, w tym hosting zestawy startowe skonfigurowany przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-256">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d16f4-257">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-257">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="d16f4-258">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d16f4-258">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d16f4-259">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="d16f4-259">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d16f4-260">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="d16f4-260">**Default**: false</span></span>  
<span data-ttu-id="d16f4-261">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d16f4-261">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d16f4-262">**Zmienna środowiskowa**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d16f4-262">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="d16f4-263">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="d16f4-263">Server URLs</span></span>

<span data-ttu-id="d16f4-264">Wskazuje adresy IP lub adresów hosta, portów i protokołów, które serwer powinien nasłuchiwać żądań protokołu.</span><span class="sxs-lookup"><span data-stu-id="d16f4-264">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d16f4-265">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="d16f4-265">**Key**: urls</span></span>  
<span data-ttu-id="d16f4-266">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d16f4-266">**Type**: *string*</span></span>  
<span data-ttu-id="d16f4-267">**Domyślne**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d16f4-267">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d16f4-268">**Można ustawić przy użyciu**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d16f4-268">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d16f4-269">**Zmienna środowiskowa**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d16f4-269">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d16f4-270">Ustaw rozdzielone średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="d16f4-270">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d16f4-271">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-271">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d16f4-272">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d16f4-272">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d16f4-273">Protokół (`http://` lub `https://`) muszą być dołączone do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d16f4-273">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d16f4-274">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="d16f4-274">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="d16f4-275">Kestrel ma swój własny konfiguracji punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d16f4-275">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d16f4-276">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-276">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="d16f4-277">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="d16f4-277">Shutdown Timeout</span></span>

<span data-ttu-id="d16f4-278">Określa ilość czasu oczekiwania dla hosta sieci web zamknąć.</span><span class="sxs-lookup"><span data-stu-id="d16f4-278">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="d16f4-279">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d16f4-279">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d16f4-280">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="d16f4-280">**Type**: *int*</span></span>  
<span data-ttu-id="d16f4-281">**Domyślne**: 5</span><span class="sxs-lookup"><span data-stu-id="d16f4-281">**Default**: 5</span></span>  
<span data-ttu-id="d16f4-282">**Można ustawić przy użyciu**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d16f4-282">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d16f4-283">**Zmienna środowiskowa**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d16f4-283">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d16f4-284">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) — metoda rozszerzenia ma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="d16f4-284">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="d16f4-285">Podczas limitu czasu, hostingu:</span><span class="sxs-lookup"><span data-stu-id="d16f4-285">During the timeout period, hosting:</span></span>

* <span data-ttu-id="d16f4-286">Wyzwalacze [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="d16f4-286">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="d16f4-287">Podejmuje próby zatrzymania usług hostowanych, rejestrowanie błędów, których nie można zatrzymać usługi.</span><span class="sxs-lookup"><span data-stu-id="d16f4-287">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="d16f4-288">Jeśli upłynie limit czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostaną zatrzymane podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-288">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="d16f4-289">Usługi zatrzymania nawet wtedy, gdy jeszcze nie zakończyło się przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="d16f4-289">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="d16f4-290">Jeśli usługi wymagają dodatkowego czasu, aby zatrzymać, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="d16f4-290">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="d16f4-291">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="d16f4-291">Startup Assembly</span></span>

<span data-ttu-id="d16f4-292">Określa zestaw, aby wyszukać `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="d16f4-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d16f4-293">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d16f4-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d16f4-294">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d16f4-294">**Type**: *string*</span></span>  
<span data-ttu-id="d16f4-295">**Domyślne**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="d16f4-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d16f4-296">**Można ustawić przy użyciu**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d16f4-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d16f4-297">**Zmienna środowiskowa**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d16f4-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d16f4-298">Zestawu według nazwy (`string`) lub wpisz (`TStartup`) mogą być przywoływane.</span><span class="sxs-lookup"><span data-stu-id="d16f4-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d16f4-299">Jeśli wiele `UseStartup` metody są wywoływane, ostatni z nich ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="d16f4-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

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

### <a name="web-root"></a><span data-ttu-id="d16f4-300">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="d16f4-300">Web Root</span></span>

<span data-ttu-id="d16f4-301">Ustawia ścieżkę względną do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d16f4-302">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="d16f4-302">**Key**: webroot</span></span>  
<span data-ttu-id="d16f4-303">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="d16f4-303">**Type**: *string*</span></span>  
<span data-ttu-id="d16f4-304">**Domyślne**: Jeśli nie zostanie określony, wartość domyślna to "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="d16f4-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d16f4-305">Jeśli ścieżka nie istnieje, jest używany dostawca pliku pusta.</span><span class="sxs-lookup"><span data-stu-id="d16f4-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d16f4-306">**Można ustawić przy użyciu**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d16f4-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d16f4-307">**Zmienna środowiskowa**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d16f4-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

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

## <a name="override-configuration"></a><span data-ttu-id="d16f4-308">Zastąp konfigurację</span><span class="sxs-lookup"><span data-stu-id="d16f4-308">Override configuration</span></span>

<span data-ttu-id="d16f4-309">Użyj [konfiguracji](xref:fundamentals/configuration/index) skonfigurować hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="d16f4-309">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="d16f4-310">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określony w *hostsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d16f4-310">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="d16f4-311">Dowolna Konfiguracja ładowane z *hostsettings.json* pliku może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d16f4-311">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d16f4-312">Konfiguracja wbudowanych (w `config`) służy do konfigurowania hosta z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="d16f4-312">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="d16f4-313">`IWebHostBuilder` Konfiguracja zostanie dodany do konfiguracji aplikacji, ale prawdą nie dotyczy&mdash; `ConfigureAppConfiguration` nie ma wpływu na `IWebHostBuilder` konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-313">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d16f4-314">Zastępowanie konfiguracji, jaką zapewnia `UseUrls` z *hostsettings.json* config argument po pierwsze, wiersza polecenia konfiguracji drugiego:</span><span class="sxs-lookup"><span data-stu-id="d16f4-314">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d16f4-315">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d16f4-315">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d16f4-316">Zastępowanie konfiguracji, jaką zapewnia `UseUrls` z *hostsettings.json* config argument po pierwsze, wiersza polecenia konfiguracji drugiego:</span><span class="sxs-lookup"><span data-stu-id="d16f4-316">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="d16f4-317">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d16f4-317">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d16f4-318">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metody rozszerzenia nie jest obecnie zdolne do analizowania sekcji konfiguracji, zwracany przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-318">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d16f4-319">`GetSection` Metoda klucze konfiguracji do sekcji żądane filtry, ale klucze nie powoduje usunięcia nazwy sekcji (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d16f4-319">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d16f4-320">`UseConfiguration` Metoda oczekuje kluczy, aby dopasować `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d16f4-320">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d16f4-321">Obecność nazwa sekcji na kluczach zapobiega wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="d16f4-321">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d16f4-322">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-322">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d16f4-323">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie sekcję konfiguracji do WebHostBuilder.UseConfiguration korzysta z kluczami pełną](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d16f4-323">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="d16f4-324">`UseConfiguration` tylko kopiuje kluczy z dostarczonego `IConfiguration` konfiguracji konstruktora hosta.</span><span class="sxs-lookup"><span data-stu-id="d16f4-324">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="d16f4-325">W związku z tym, ustawienie `reloadOnChange: true` plików ustawień JSON, INI i XML nie ma wpływu.</span><span class="sxs-lookup"><span data-stu-id="d16f4-325">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="d16f4-326">Aby określić hosta, uruchom na określony adres URL, żądaną wartość w można przekazać polecenie w wierszu polecenia podczas wykonywania [dotnet, uruchom](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="d16f4-326">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="d16f4-327">Argument wiersza polecenia zastępuje `urls` wartość z *hostsettings.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="d16f4-327">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="d16f4-328">Zarządzać hosta</span><span class="sxs-lookup"><span data-stu-id="d16f4-328">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d16f4-329">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="d16f4-329">**Run**</span></span>

<span data-ttu-id="d16f4-330">`Run` Metoda uruchamia aplikację sieci web i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="d16f4-330">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d16f4-331">**Start**</span><span class="sxs-lookup"><span data-stu-id="d16f4-331">**Start**</span></span>

<span data-ttu-id="d16f4-332">Uruchamiane w sposób nieblokującej na poziomie hosta, przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="d16f4-332">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d16f4-333">Jeśli lista adresów URL jest przekazywany do `Start` metody nasłuchuje on na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d16f4-333">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d16f4-334">Aplikacja może inicjuje i uruchamia nowy host, przy użyciu wstępnie skonfigurowanych ustawień domyślnych z `CreateDefaultBuilder` za pomocą innej metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="d16f4-334">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d16f4-335">Te metody uruchomić serwer bez danych wyjściowych konsoli i przy [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podziału (Ctrl-C/SIGINT lub SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="d16f4-335">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d16f4-336">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d16f4-336">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d16f4-337">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-337">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d16f4-338">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d16f4-338">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d16f4-339">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d16f4-339">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d16f4-340">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="d16f4-340">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d16f4-341">**Uruchom (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d16f4-341">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d16f4-342">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-342">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d16f4-343">Daje ten sam wynik jako **Start (aplikacja RequestDelegate)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-343">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d16f4-344">**Uruchom (Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d16f4-344">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d16f4-345">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) używać oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="d16f4-345">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d16f4-346">W przykładzie, należy użyć następujących żądań przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="d16f4-346">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d16f4-347">Żądanie</span><span class="sxs-lookup"><span data-stu-id="d16f4-347">Request</span></span>                                    | <span data-ttu-id="d16f4-348">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="d16f4-348">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d16f4-349">Witaj, Martin!</span><span class="sxs-lookup"><span data-stu-id="d16f4-349">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d16f4-350">Buenos diasem, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d16f4-350">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d16f4-351">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="d16f4-351">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d16f4-352">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="d16f4-352">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d16f4-353">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="d16f4-353">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d16f4-354">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="d16f4-354">Hello World!</span></span>                             |

<span data-ttu-id="d16f4-355">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d16f4-355">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d16f4-356">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="d16f4-356">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d16f4-357">**Uruchom (ciąg adresu url, Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d16f4-357">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="d16f4-358">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-358">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d16f4-359">Daje ten sam wynik jako **Start (Akcja&lt;IRouteBuilder&gt; routeBuilder)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-359">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d16f4-360">**StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d16f4-360">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d16f4-361">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-361">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d16f4-362">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d16f4-362">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d16f4-363">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d16f4-363">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d16f4-364">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="d16f4-364">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d16f4-365">**StartWith (ciąg adresu url, Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="d16f4-365">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="d16f4-366">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-366">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d16f4-367">Daje ten sam wynik jako **StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-367">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d16f4-368">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="d16f4-368">**Run**</span></span>

<span data-ttu-id="d16f4-369">`Run` Metoda uruchamia aplikację sieci web i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="d16f4-369">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d16f4-370">**Start**</span><span class="sxs-lookup"><span data-stu-id="d16f4-370">**Start**</span></span>

<span data-ttu-id="d16f4-371">Uruchamiane w sposób nieblokującej na poziomie hosta, przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="d16f4-371">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d16f4-372">Jeśli lista adresów URL jest przekazywany do `Start` metody nasłuchuje on na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="d16f4-372">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d16f4-373">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d16f4-373">IHostingEnvironment interface</span></span>

<span data-ttu-id="d16f4-374">[Interfejsu IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje dotyczące aplikacji sieci web, środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="d16f4-374">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d16f4-375">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać `IHostingEnvironment` aby można było używać jej właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="d16f4-375">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d16f4-376">A [podejścia opartego na Konwencji](xref:fundamentals/environments#environment-based-startup-class-and-methods) mogą być używane do konfigurowania aplikacji przy uruchamianiu opartych na środowisku.</span><span class="sxs-lookup"><span data-stu-id="d16f4-376">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d16f4-377">Alternatywnie wstrzyknąć `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-377">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d16f4-378">Oprócz `IsDevelopment` metody rozszerzenia `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="d16f4-378">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d16f4-379">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d16f4-379">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d16f4-380">`IHostingEnvironment` Usługi może także wstrzyknięte bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="d16f4-380">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d16f4-381">`IHostingEnvironment` może być wstrzykuje `Invoke` metody podczas tworzenia niestandardowych [oprogramowania pośredniczącego](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="d16f4-381">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d16f4-382">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d16f4-382">IApplicationLifetime interface</span></span>

<span data-ttu-id="d16f4-383">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działań po uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-383">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d16f4-384">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-384">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d16f4-385">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="d16f4-385">Cancellation Token</span></span>    | <span data-ttu-id="d16f4-386">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="d16f4-386">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="d16f4-387">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d16f4-387">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d16f4-388">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d16f4-388">The host has fully started.</span></span> |
| [<span data-ttu-id="d16f4-389">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="d16f4-389">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d16f4-390">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="d16f4-390">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d16f4-391">Wszystkie żądania powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="d16f4-391">All requests should be processed.</span></span> <span data-ttu-id="d16f4-392">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d16f4-392">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d16f4-393">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="d16f4-393">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d16f4-394">Wykonuje łagodne zamykanie hosta.</span><span class="sxs-lookup"><span data-stu-id="d16f4-394">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d16f4-395">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="d16f4-395">Requests may still be processing.</span></span> <span data-ttu-id="d16f4-396">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="d16f4-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="d16f4-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d16f4-398">Następujące klasy używa `StopApplication` bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="d16f4-398">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="d16f4-399">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="d16f4-399">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d16f4-400">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="d16f4-400">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="d16f4-401">Gdy `ValidateScopes` ustawiono `true`, domyślny dostawca usługi sprawdza upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="d16f4-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="d16f4-402">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="d16f4-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="d16f4-403">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="d16f4-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="d16f4-404">Dostawcy usług główny jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="d16f4-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="d16f4-405">Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d16f4-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="d16f4-406">Usługi o określonym zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="d16f4-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="d16f4-407">Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="d16f4-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="d16f4-408">Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="d16f4-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="d16f4-409">Aby zawsze weryfikowały zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) w Konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="d16f4-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d16f4-410">Rozwiązywanie problemów z System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="d16f4-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d16f4-411">**Następujące ma zastosowanie tylko do aplikacji ASP.NET Core 2.0, gdy aplikacja nie wywołać `UseStartup` lub `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="d16f4-411">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="d16f4-412">Host mogły zostać utworzone przez iniekcję `IStartup` bezpośrednio do kontenera iniekcji zależności zamiast wywoływania `UseStartup` lub `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d16f4-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="d16f4-413">Jeśli host jest wbudowana w ten sposób, może wystąpić następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="d16f4-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="d16f4-414">Dzieje się tak dlatego, nazwę aplikacji (nazwa bieżącego zestawu) jest wymagany do skanowania w poszukiwaniu `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d16f4-414">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d16f4-415">Jeśli aplikacja ręcznie wprowadza `IStartup` do kontenera iniekcji zależności, Dodaj następujące wywołanie do `WebHostBuilder` o podanej nazwie zestawu:</span><span class="sxs-lookup"><span data-stu-id="d16f4-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="d16f4-416">Możesz też dodać dummy `Configure` do `WebHostBuilder`, który automatycznie ustawia nazwę aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d16f4-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="d16f4-417">Aby uzyskać więcej informacji, zobacz [anonsów: Microsoft.Extensions.PlatformAbstractions został usunięty (komentarz)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) i [przykładowe StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d16f4-417">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d16f4-418">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d16f4-418">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
