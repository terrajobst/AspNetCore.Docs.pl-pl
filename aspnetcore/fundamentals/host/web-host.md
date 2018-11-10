---
title: Host sieci Web platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat hosta sieci web w programie ASP.NET Core, który jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: a3601b71c65321af56644eb87c4527d6290e4378
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505820"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="407c2-103">Host sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="407c2-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="407c2-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="407c2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="407c2-105">Dla wersji 1.1 w tym temacie, Pobierz [hosta sieci Web programu ASP.NET Core (w wersji 1.1, plików PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="407c2-105">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

<span data-ttu-id="407c2-106">Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*.</span><span class="sxs-lookup"><span data-stu-id="407c2-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="407c2-107">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="407c2-108">Jako minimum host konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="407c2-108">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="407c2-109">W tym temacie omówiono hosta sieci Web platformy ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), co jest przydatne do hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="407c2-109">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="407c2-110">Pokrycia hosta ogólnego .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="407c2-110">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="407c2-111">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="407c2-111">Set up a host</span></span>

<span data-ttu-id="407c2-112">Tworzenie hosta przy użyciu wystąpienia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="407c2-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="407c2-113">To jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="407c2-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="407c2-114">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="407c2-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="407c2-115">Typowa *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="407c2-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="407c2-116">`CreateDefaultBuilder` wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="407c2-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="407c2-117">Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci web i konfiguruje serwer przy użyciu dostawcy usług konfiguracji hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="407c2-118">Kestrel opcjach domyślny, zobacz temat <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="407c2-118">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="407c2-119">Ustawia zawartość katalogu głównego ścieżka zwrócona przez element [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="407c2-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="407c2-120">Ładunki [konfiguracji hosta](#host-configuration-values) od:</span><span class="sxs-lookup"><span data-stu-id="407c2-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="407c2-121">Zmienne środowiskowe prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="407c2-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="407c2-122">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="407c2-122">Command-line arguments.</span></span>
* <span data-ttu-id="407c2-123">Ładuje konfiguracji aplikacji w kolejności od:</span><span class="sxs-lookup"><span data-stu-id="407c2-123">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="407c2-124">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="407c2-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="407c2-125">*appSettings. {Środowiska} .json*.</span><span class="sxs-lookup"><span data-stu-id="407c2-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="407c2-126">[Klucz tajny Menedżera](xref:security/app-secrets) uruchamiania aplikacji `Development` środowisko przy użyciu zestawu wpisu.</span><span class="sxs-lookup"><span data-stu-id="407c2-126">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="407c2-127">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="407c2-127">Environment variables.</span></span>
  * <span data-ttu-id="407c2-128">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="407c2-128">Command-line arguments.</span></span>
* <span data-ttu-id="407c2-129">Konfiguruje [rejestrowania](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="407c2-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="407c2-130">Rejestrowanie powoduje umieszczenie [filtrowanie dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji Konfiguracja rejestrowania *appsettings.json* lub *appsettings. { Środowisko} .json* pliku.</span><span class="sxs-lookup"><span data-stu-id="407c2-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="407c2-131">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="407c2-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="407c2-132">Umożliwia skonfigurowanie ścieżki podstawowej i portów serwer nasłuchuje na w przypadku korzystania z [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="407c2-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="407c2-133">Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="407c2-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="407c2-134">Konfiguruje również aplikacji na [przechwytywania błędów uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="407c2-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="407c2-135">Opcjach domyślny usług IIS, zobacz temat <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="407c2-135">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="407c2-136">Zestawy [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="407c2-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="407c2-137">Aby uzyskać więcej informacji, zobacz [zakresu walidacji](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="407c2-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="407c2-138">Konfiguracja zdefiniowane przez `CreateDefaultBuilder` zastąpienia i wzmacnia [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)i inne metody, jak i metody rozszerzenia [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="407c2-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="407c2-139">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="407c2-139">A few examples follow:</span></span>

* <span data-ttu-id="407c2-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) służy do określania dodatkowych `IConfiguration` dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="407c2-141">Następujące `ConfigureAppConfiguration` wywołanie dodaje delegata w celu uwzględnienia konfiguracji aplikacji w *appsettings.xml* pliku.</span><span class="sxs-lookup"><span data-stu-id="407c2-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="407c2-142">`ConfigureAppConfiguration` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="407c2-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="407c2-143">Należy pamiętać, że ta konfiguracja nie ma zastosowania do hosta (na przykład adresy URL serwera lub środowiska).</span><span class="sxs-lookup"><span data-stu-id="407c2-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="407c2-144">Zobacz [hosta wartości konfiguracji](#host-configuration-values) sekcji.</span><span class="sxs-lookup"><span data-stu-id="407c2-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="407c2-145">Następujące `ConfigureLogging` wywołanie dodaje delegata, aby skonfigurować poziom rejestrowania minimalny ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) do [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="407c2-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="407c2-146">Ustawienie to zastępuje ustawienia w *appsettings. Development.JSON* (`LogLevel.Debug`) i *appsettings. Production.JSON* (`LogLevel.Error`) skonfigurowane przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="407c2-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="407c2-147">`ConfigureLogging` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="407c2-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="407c2-148">Następujące wywołanie do `ConfigureKestrel` zastępuje to domyślne [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtów nawiązane, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="407c2-148">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="407c2-149">Następujące wywołanie do [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) zastępuje to domyślne [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtów nawiązane, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="407c2-149">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="407c2-150">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="407c2-150">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="407c2-151">Po uruchomieniu aplikacji z folderu głównego projektu, folder główny projektu jest używany jako katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="407c2-151">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="407c2-152">Jest to opcja domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="407c2-152">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="407c2-153">Aby uzyskać więcej informacji na temat konfiguracji aplikacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="407c2-153">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="407c2-154">Jako alternatywa dla użycia statycznego `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) metoda przy użyciu obsługiwanych za pomocą programu ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="407c2-154">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="407c2-155">Aby uzyskać więcej informacji zobacz kartę 1.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="407c2-155">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="407c2-156">Podczas konfigurowania hostów [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) można przedstawić metody.</span><span class="sxs-lookup"><span data-stu-id="407c2-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="407c2-157">Jeśli `Startup` klasy jest określony, należy ją zdefiniować `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="407c2-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="407c2-158">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="407c2-158">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="407c2-159">Wiele wywołań `ConfigureServices` dołączenia do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="407c2-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="407c2-160">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpienia poprzednich ustawień.</span><span class="sxs-lookup"><span data-stu-id="407c2-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="407c2-161">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="407c2-161">Host configuration values</span></span>

<span data-ttu-id="407c2-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) opiera się na następujących podejść można ustawić wartości konfiguracji hostów:</span><span class="sxs-lookup"><span data-stu-id="407c2-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="407c2-163">Konfiguracja Konstruktora hosta, który zawiera zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="407c2-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="407c2-164">Na przykład `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="407c2-164">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="407c2-165">Rozszerzenia, takie jak [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) i [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (zobacz [zastępczą konfigurację](#override-configuration) sekcji).</span><span class="sxs-lookup"><span data-stu-id="407c2-165">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="407c2-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="407c2-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="407c2-167">Podczas ustawiania wartości z `UseSetting`, wartość jest ustawiana jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="407c2-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="407c2-168">Host używa jednego z tych opcji ustawia wartość ostatniego.</span><span class="sxs-lookup"><span data-stu-id="407c2-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="407c2-169">Aby uzyskać więcej informacji, zobacz [zastępczą konfigurację](#override-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="407c2-169">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="407c2-170">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="407c2-170">Application Key (Name)</span></span>

<span data-ttu-id="407c2-171">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) właściwość ma wartość automatycznie, gdy [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) lub [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) jest wywoływana podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="407c2-171">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="407c2-172">Wartość jest równa Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-172">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="407c2-173">Aby jawnie ustawić wartość, użyj [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="407c2-173">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="407c2-174">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="407c2-174">**Key**: applicationName</span></span>  
<span data-ttu-id="407c2-175">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-175">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-176">**Domyślne**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-176">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="407c2-177">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="407c2-177">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="407c2-178">**Zmienna środowiskowa**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="407c2-178">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="407c2-179">Przechwytywania błędów uruchamiania</span><span class="sxs-lookup"><span data-stu-id="407c2-179">Capture Startup Errors</span></span>

<span data-ttu-id="407c2-180">To ustawienie steruje przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="407c2-180">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="407c2-181">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="407c2-181">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="407c2-182">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="407c2-182">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="407c2-183">**Domyślne**: wartość domyślna to `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, w którym domyślnie są `true`.</span><span class="sxs-lookup"><span data-stu-id="407c2-183">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="407c2-184">**Można ustawić przy użyciu**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="407c2-184">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="407c2-185">**Zmienna środowiskowa**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="407c2-185">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="407c2-186">Gdy `false`, błędy podczas uruchamiania wynik na hoście, kończenie.</span><span class="sxs-lookup"><span data-stu-id="407c2-186">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="407c2-187">Gdy `true`, host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="407c2-187">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="407c2-188">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="407c2-188">Content Root</span></span>

<span data-ttu-id="407c2-189">To ustawienie określa, gdzie platformy ASP.NET Core rozpoczyna się wyszukiwanie plików zawartości, takich jak widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="407c2-189">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="407c2-190">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="407c2-190">**Key**: contentRoot</span></span>  
<span data-ttu-id="407c2-191">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-191">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-192">**Domyślne**: wartość domyślna to folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-192">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="407c2-193">**Można ustawić przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="407c2-193">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="407c2-194">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="407c2-194">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="407c2-195">Główny zawartości jest również używane jako podstawową ścieżkę dla [ustawienie katalog główny sieci Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="407c2-195">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="407c2-196">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="407c2-196">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="407c2-197">Szczegółowe informacje o błędach</span><span class="sxs-lookup"><span data-stu-id="407c2-197">Detailed Errors</span></span>

<span data-ttu-id="407c2-198">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="407c2-198">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="407c2-199">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="407c2-199">**Key**: detailedErrors</span></span>  
<span data-ttu-id="407c2-200">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="407c2-200">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="407c2-201">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="407c2-201">**Default**: false</span></span>  
<span data-ttu-id="407c2-202">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="407c2-202">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="407c2-203">**Zmienna środowiskowa**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="407c2-203">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="407c2-204">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ustawiono `Development`), aplikacja przechwytuje szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="407c2-204">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="407c2-205">Środowisko</span><span class="sxs-lookup"><span data-stu-id="407c2-205">Environment</span></span>

<span data-ttu-id="407c2-206">Ustawia środowiska Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-206">Sets the app's environment.</span></span>

<span data-ttu-id="407c2-207">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="407c2-207">**Key**: environment</span></span>  
<span data-ttu-id="407c2-208">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-208">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-209">**Domyślne**: produkcji</span><span class="sxs-lookup"><span data-stu-id="407c2-209">**Default**: Production</span></span>  
<span data-ttu-id="407c2-210">**Można ustawić przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="407c2-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="407c2-211">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="407c2-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="407c2-212">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="407c2-212">The environment can be set to any value.</span></span> <span data-ttu-id="407c2-213">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="407c2-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="407c2-214">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="407c2-214">Values aren't case sensitive.</span></span> <span data-ttu-id="407c2-215">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="407c2-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="407c2-216">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="407c2-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="407c2-217">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="407c2-217">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="407c2-218">Hosting zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="407c2-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="407c2-219">Ustawia hostingu zestawy uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="407c2-220">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="407c2-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="407c2-221">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-221">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-222">**Domyślne**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="407c2-222">**Default**: Empty string</span></span>  
<span data-ttu-id="407c2-223">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="407c2-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="407c2-224">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="407c2-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="407c2-225">Rozdzielana średnikami ciąg hostingu zestawy startowe załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="407c2-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="407c2-226">Mimo że konfiguracja ma domyślnie wartość pustego ciągu, hostingu zestawy startowe zawsze zawierać zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="407c2-227">Hostingu zestawy startowe są udostępniane, są dodawane do zestawu aplikacji dotyczące ładowania, gdy aplikacja tworzy swoich usług wspólne podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="407c2-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="407c2-228">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="407c2-228">HTTPS Port</span></span>

<span data-ttu-id="407c2-229">Ustawienie HTTPS przekierowania portu.</span><span class="sxs-lookup"><span data-stu-id="407c2-229">Set the HTTPS redirect port.</span></span> <span data-ttu-id="407c2-230">Używane w [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="407c2-230">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="407c2-231">**Klucz**: https_port **typu**: *ciąg*
**domyślne**: nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="407c2-231">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="407c2-232">**Można ustawić przy użyciu**: `UseSetting` 
 **zmiennej środowiskowej**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="407c2-232">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="407c2-233">Hosting zestawy wykluczania uruchamiania</span><span class="sxs-lookup"><span data-stu-id="407c2-233">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="407c2-234">Rozdzielana średnikami ciąg hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="407c2-234">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="407c2-235">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="407c2-235">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="407c2-236">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-236">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-237">**Domyślne**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="407c2-237">**Default**: Empty string</span></span>  
<span data-ttu-id="407c2-238">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="407c2-238">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="407c2-239">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="407c2-239">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="407c2-240">Preferuj hostingu adresów URL</span><span class="sxs-lookup"><span data-stu-id="407c2-240">Prefer Hosting URLs</span></span>

<span data-ttu-id="407c2-241">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast konfigurowane przy użyciu `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-241">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="407c2-242">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="407c2-242">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="407c2-243">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="407c2-243">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="407c2-244">**Domyślne**: true</span><span class="sxs-lookup"><span data-stu-id="407c2-244">**Default**: true</span></span>  
<span data-ttu-id="407c2-245">**Można ustawić przy użyciu**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="407c2-245">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="407c2-246">**Zmienna środowiskowa**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="407c2-246">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="407c2-247">Zapobiegaj hostingu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="407c2-247">Prevent Hosting Startup</span></span>

<span data-ttu-id="407c2-248">Uniemożliwia automatyczne ładowanie obsługi zestawów uruchamiania, w tym hosting zestawy startowe skonfigurowany przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-248">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="407c2-249">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="407c2-249">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="407c2-250">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="407c2-250">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="407c2-251">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="407c2-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="407c2-252">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="407c2-252">**Default**: false</span></span>  
<span data-ttu-id="407c2-253">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="407c2-253">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="407c2-254">**Zmienna środowiskowa**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="407c2-254">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="407c2-255">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="407c2-255">Server URLs</span></span>

<span data-ttu-id="407c2-256">Wskazuje adresy IP lub adresów hosta, portów i protokołów, które serwer powinien nasłuchiwać żądań protokołu.</span><span class="sxs-lookup"><span data-stu-id="407c2-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="407c2-257">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="407c2-257">**Key**: urls</span></span>  
<span data-ttu-id="407c2-258">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-258">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-259">**Domyślne**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="407c2-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="407c2-260">**Można ustawić przy użyciu**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="407c2-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="407c2-261">**Zmienna środowiskowa**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="407c2-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="407c2-262">Ustaw rozdzielone średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="407c2-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="407c2-263">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="407c2-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="407c2-264">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="407c2-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="407c2-265">Protokół (`http://` lub `https://`) muszą być dołączone do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="407c2-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="407c2-266">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="407c2-266">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="407c2-267">Kestrel ma swój własny konfiguracji punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="407c2-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="407c2-268">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="407c2-268">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="407c2-269">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="407c2-269">Shutdown Timeout</span></span>

<span data-ttu-id="407c2-270">Określa ilość czasu oczekiwania dla hosta sieci web zamknąć.</span><span class="sxs-lookup"><span data-stu-id="407c2-270">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="407c2-271">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="407c2-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="407c2-272">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="407c2-272">**Type**: *int*</span></span>  
<span data-ttu-id="407c2-273">**Domyślne**: 5</span><span class="sxs-lookup"><span data-stu-id="407c2-273">**Default**: 5</span></span>  
<span data-ttu-id="407c2-274">**Można ustawić przy użyciu**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="407c2-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="407c2-275">**Zmienna środowiskowa**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="407c2-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="407c2-276">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) — metoda rozszerzenia ma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="407c2-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="407c2-277">Podczas limitu czasu, hostingu:</span><span class="sxs-lookup"><span data-stu-id="407c2-277">During the timeout period, hosting:</span></span>

* <span data-ttu-id="407c2-278">Wyzwalacze [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="407c2-278">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="407c2-279">Podejmuje próby zatrzymania usług hostowanych, rejestrowanie błędów, których nie można zatrzymać usługi.</span><span class="sxs-lookup"><span data-stu-id="407c2-279">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="407c2-280">Jeśli upłynie limit czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostaną zatrzymane podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-280">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="407c2-281">Usługi zatrzymania nawet wtedy, gdy jeszcze nie zakończyło się przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="407c2-281">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="407c2-282">Jeśli usługi wymagają dodatkowego czasu, aby zatrzymać, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="407c2-282">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="407c2-283">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="407c2-283">Startup Assembly</span></span>

<span data-ttu-id="407c2-284">Określa zestaw, aby wyszukać `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="407c2-284">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="407c2-285">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="407c2-285">**Key**: startupAssembly</span></span>  
<span data-ttu-id="407c2-286">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-286">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-287">**Domyślne**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="407c2-287">**Default**: The app's assembly</span></span>  
<span data-ttu-id="407c2-288">**Można ustawić przy użyciu**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="407c2-288">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="407c2-289">**Zmienna środowiskowa**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="407c2-289">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="407c2-290">Zestawu według nazwy (`string`) lub wpisz (`TStartup`) mogą być przywoływane.</span><span class="sxs-lookup"><span data-stu-id="407c2-290">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="407c2-291">Jeśli wiele `UseStartup` metody są wywoływane, ostatni z nich ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="407c2-291">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="407c2-292">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="407c2-292">Web Root</span></span>

<span data-ttu-id="407c2-293">Ustawia ścieżkę względną do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="407c2-294">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="407c2-294">**Key**: webroot</span></span>  
<span data-ttu-id="407c2-295">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="407c2-295">**Type**: *string*</span></span>  
<span data-ttu-id="407c2-296">**Domyślne**: Jeśli nie zostanie określony, wartość domyślna to "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="407c2-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="407c2-297">Jeśli ścieżka nie istnieje, jest używany dostawca pliku pusta.</span><span class="sxs-lookup"><span data-stu-id="407c2-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="407c2-298">**Można ustawić przy użyciu**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="407c2-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="407c2-299">**Zmienna środowiskowa**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="407c2-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="407c2-300">Zastąp konfigurację</span><span class="sxs-lookup"><span data-stu-id="407c2-300">Override configuration</span></span>

<span data-ttu-id="407c2-301">Użyj [konfiguracji](xref:fundamentals/configuration/index) skonfigurować hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="407c2-301">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="407c2-302">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określony w *hostsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="407c2-302">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="407c2-303">Dowolna Konfiguracja ładowane z *hostsettings.json* pliku może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="407c2-303">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="407c2-304">Konfiguracja wbudowanych (w `config`) służy do konfigurowania hosta z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="407c2-304">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="407c2-305">`IWebHostBuilder` Konfiguracja zostanie dodany do konfiguracji aplikacji, ale prawdą nie dotyczy&mdash; `ConfigureAppConfiguration` nie ma wpływu na `IWebHostBuilder` konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="407c2-305">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="407c2-306">Zastępowanie konfiguracji, jaką zapewnia `UseUrls` z *hostsettings.json* config argument po pierwsze, wiersza polecenia konfiguracji drugiego:</span><span class="sxs-lookup"><span data-stu-id="407c2-306">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            });
    }
}
```

<span data-ttu-id="407c2-307">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="407c2-307">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="407c2-308">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metody rozszerzenia nie jest obecnie zdolne do analizowania sekcji konfiguracji, zwracany przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="407c2-308">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="407c2-309">`GetSection` Metoda klucze konfiguracji do sekcji żądane filtry, ale klucze nie powoduje usunięcia nazwy sekcji (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="407c2-309">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="407c2-310">`UseConfiguration` Metoda oczekuje kluczy, aby dopasować `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="407c2-310">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="407c2-311">Obecność nazwa sekcji na kluczach zapobiega wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="407c2-311">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="407c2-312">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="407c2-312">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="407c2-313">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie sekcję konfiguracji do WebHostBuilder.UseConfiguration korzysta z kluczami pełną](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="407c2-313">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="407c2-314">`UseConfiguration` tylko kopiuje kluczy z dostarczonego `IConfiguration` konfiguracji konstruktora hosta.</span><span class="sxs-lookup"><span data-stu-id="407c2-314">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="407c2-315">W związku z tym, ustawienie `reloadOnChange: true` plików ustawień JSON, INI i XML nie ma wpływu.</span><span class="sxs-lookup"><span data-stu-id="407c2-315">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="407c2-316">Aby określić hosta, uruchom na określony adres URL, żądaną wartość w można przekazać polecenie w wierszu polecenia podczas wykonywania [dotnet, uruchom](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="407c2-316">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="407c2-317">Argument wiersza polecenia zastępuje `urls` wartość z *hostsettings.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="407c2-317">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="407c2-318">Zarządzać hosta</span><span class="sxs-lookup"><span data-stu-id="407c2-318">Manage the host</span></span>

<span data-ttu-id="407c2-319">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="407c2-319">**Run**</span></span>

<span data-ttu-id="407c2-320">`Run` Metoda uruchamia aplikację sieci web i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="407c2-320">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="407c2-321">**Start**</span><span class="sxs-lookup"><span data-stu-id="407c2-321">**Start**</span></span>

<span data-ttu-id="407c2-322">Uruchamiane w sposób nieblokującej na poziomie hosta, przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="407c2-322">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="407c2-323">Jeśli lista adresów URL jest przekazywany do `Start` metody nasłuchuje on na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="407c2-323">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="407c2-324">Aplikacja może inicjuje i uruchamia nowy host, przy użyciu wstępnie skonfigurowanych ustawień domyślnych z `CreateDefaultBuilder` za pomocą innej metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="407c2-324">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="407c2-325">Te metody uruchomić serwer bez danych wyjściowych konsoli i przy [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podziału (Ctrl-C/SIGINT lub SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="407c2-325">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="407c2-326">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="407c2-326">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="407c2-327">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="407c2-327">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="407c2-328">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="407c2-328">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="407c2-329">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="407c2-329">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="407c2-330">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="407c2-330">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="407c2-331">**Uruchom (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="407c2-331">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="407c2-332">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="407c2-332">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="407c2-333">Daje ten sam wynik jako **Start (aplikacja RequestDelegate)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="407c2-333">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="407c2-334">**Uruchom (Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="407c2-334">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="407c2-335">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) używać oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="407c2-335">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="407c2-336">W przykładzie, należy użyć następujących żądań przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="407c2-336">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="407c2-337">Żądanie</span><span class="sxs-lookup"><span data-stu-id="407c2-337">Request</span></span>                                    | <span data-ttu-id="407c2-338">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="407c2-338">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="407c2-339">Witaj, Martin!</span><span class="sxs-lookup"><span data-stu-id="407c2-339">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="407c2-340">Buenos diasem, Catrina!</span><span class="sxs-lookup"><span data-stu-id="407c2-340">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="407c2-341">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="407c2-341">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="407c2-342">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="407c2-342">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="407c2-343">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="407c2-343">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="407c2-344">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="407c2-344">Hello World!</span></span>                             |

<span data-ttu-id="407c2-345">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="407c2-345">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="407c2-346">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="407c2-346">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="407c2-347">**Uruchom (ciąg adresu url, Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="407c2-347">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="407c2-348">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="407c2-348">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="407c2-349">Daje ten sam wynik jako **Start (Akcja&lt;IRouteBuilder&gt; routeBuilder)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="407c2-349">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="407c2-350">**StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="407c2-350">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="407c2-351">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="407c2-351">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="407c2-352">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="407c2-352">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="407c2-353">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="407c2-353">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="407c2-354">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="407c2-354">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="407c2-355">**StartWith (ciąg adresu url, Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="407c2-355">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="407c2-356">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="407c2-356">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="407c2-357">Daje ten sam wynik jako **StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="407c2-357">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="407c2-358">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="407c2-358">IHostingEnvironment interface</span></span>

<span data-ttu-id="407c2-359">[Interfejsu IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje dotyczące aplikacji sieci web, środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="407c2-359">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="407c2-360">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać `IHostingEnvironment` aby można było używać jej właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="407c2-360">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="407c2-361">A [podejścia opartego na Konwencji](xref:fundamentals/environments#environment-based-startup-class-and-methods) mogą być używane do konfigurowania aplikacji przy uruchamianiu opartych na środowisku.</span><span class="sxs-lookup"><span data-stu-id="407c2-361">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="407c2-362">Alternatywnie wstrzyknąć `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="407c2-362">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="407c2-363">Oprócz `IsDevelopment` metody rozszerzenia `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="407c2-363">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="407c2-364">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="407c2-364">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="407c2-365">`IHostingEnvironment` Usługi może także wstrzyknięte bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="407c2-365">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="407c2-366">`IHostingEnvironment` może być wstrzykuje `Invoke` metody podczas tworzenia niestandardowych [oprogramowania pośredniczącego](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="407c2-366">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="407c2-367">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="407c2-367">IApplicationLifetime interface</span></span>

<span data-ttu-id="407c2-368">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działań po uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="407c2-368">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="407c2-369">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="407c2-369">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="407c2-370">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="407c2-370">Cancellation Token</span></span>    | <span data-ttu-id="407c2-371">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="407c2-371">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="407c2-372">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="407c2-372">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="407c2-373">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="407c2-373">The host has fully started.</span></span> |
| [<span data-ttu-id="407c2-374">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="407c2-374">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="407c2-375">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="407c2-375">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="407c2-376">Wszystkie żądania powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="407c2-376">All requests should be processed.</span></span> <span data-ttu-id="407c2-377">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="407c2-377">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="407c2-378">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="407c2-378">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="407c2-379">Wykonuje łagodne zamykanie hosta.</span><span class="sxs-lookup"><span data-stu-id="407c2-379">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="407c2-380">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="407c2-380">Requests may still be processing.</span></span> <span data-ttu-id="407c2-381">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="407c2-381">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="407c2-382">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-382">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="407c2-383">Następujące klasy używa `StopApplication` bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="407c2-383">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="407c2-384">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="407c2-384">Scope validation</span></span>

<span data-ttu-id="407c2-385">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="407c2-385">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="407c2-386">Gdy `ValidateScopes` ustawiono `true`, domyślny dostawca usługi sprawdza upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="407c2-386">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="407c2-387">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="407c2-387">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="407c2-388">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="407c2-388">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="407c2-389">Dostawcy usług główny jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="407c2-389">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="407c2-390">Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="407c2-390">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="407c2-391">Usługi o określonym zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="407c2-391">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="407c2-392">Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="407c2-392">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="407c2-393">Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="407c2-393">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="407c2-394">Aby zawsze weryfikowały zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) w Konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="407c2-394">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="407c2-395">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="407c2-395">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
