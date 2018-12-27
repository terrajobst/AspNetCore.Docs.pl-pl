---
title: Host sieci Web platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat hosta sieci web w programie ASP.NET Core, który jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 7215027a083c0ed0bc3b15196e390a31c5dcfc14
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637849"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="39649-103">Host sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39649-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="39649-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="39649-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="39649-105">Dla wersji 1.1 w tym temacie, Pobierz [hosta sieci Web programu ASP.NET Core (w wersji 1.1, plików PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="39649-105">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="39649-106">Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*.</span><span class="sxs-lookup"><span data-stu-id="39649-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="39649-107">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="39649-108">Jako minimum host konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="39649-108">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="39649-109">W tym temacie omówiono hosta sieci Web platformy ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), co jest przydatne do hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="39649-109">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="39649-110">Pokrycia hosta ogólnego .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="39649-110">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="39649-111">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="39649-111">Set up a host</span></span>

<span data-ttu-id="39649-112">Tworzenie hosta przy użyciu wystąpienia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="39649-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="39649-113">To jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="39649-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="39649-114">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="39649-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="39649-115">Typowa *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="39649-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="39649-116">`CreateDefaultBuilder` wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="39649-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="39649-117">Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) serwera jako serwera sieci web przy użyciu aplikacji użytkownika hostingu dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="39649-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="39649-118">Opcjach domyślny serwer Kestrel, zobacz temat <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="39649-118">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="39649-119">Ustawia zawartość katalogu głównego ścieżka zwrócona przez element [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="39649-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="39649-120">Ładunki [konfiguracji hosta](#host-configuration-values) od:</span><span class="sxs-lookup"><span data-stu-id="39649-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="39649-121">Zmienne środowiskowe prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="39649-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="39649-122">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="39649-122">Command-line arguments.</span></span>
* <span data-ttu-id="39649-123">Ładuje konfiguracji aplikacji w kolejności od:</span><span class="sxs-lookup"><span data-stu-id="39649-123">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="39649-124">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="39649-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="39649-125">*appSettings. {Środowiska} .json*.</span><span class="sxs-lookup"><span data-stu-id="39649-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="39649-126">[Klucz tajny Menedżera](xref:security/app-secrets) uruchamiania aplikacji `Development` środowisko przy użyciu zestawu wpisu.</span><span class="sxs-lookup"><span data-stu-id="39649-126">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="39649-127">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="39649-127">Environment variables.</span></span>
  * <span data-ttu-id="39649-128">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="39649-128">Command-line arguments.</span></span>
* <span data-ttu-id="39649-129">Konfiguruje [rejestrowania](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="39649-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="39649-130">Rejestrowanie powoduje umieszczenie [filtrowanie dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji Konfiguracja rejestrowania *appsettings.json* lub *appsettings. { Środowisko} .json* pliku.</span><span class="sxs-lookup"><span data-stu-id="39649-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="39649-131">Gdy działające poza usługą IIS z [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` umożliwia [integracji usług IIS](xref:host-and-deploy/iis/index), który konfiguruje port i adres podstawowy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-131">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="39649-132">Integracja usług IIS umożliwia skonfigurowanie aplikacji [przechwytywania błędów uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="39649-132">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="39649-133">Opcjach domyślny usług IIS, zobacz temat <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="39649-133">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="39649-134">Zestawy [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="39649-134">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="39649-135">Aby uzyskać więcej informacji, zobacz [zakresu walidacji](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="39649-135">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="39649-136">Konfiguracja zdefiniowane przez `CreateDefaultBuilder` zastąpienia i wzmacnia [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)i inne metody, jak i metody rozszerzenia [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="39649-136">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="39649-137">Oto kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="39649-137">A few examples follow:</span></span>

* <span data-ttu-id="39649-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) służy do określania dodatkowych `IConfiguration` dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-138">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="39649-139">Następujące `ConfigureAppConfiguration` wywołanie dodaje delegata w celu uwzględnienia konfiguracji aplikacji w *appsettings.xml* pliku.</span><span class="sxs-lookup"><span data-stu-id="39649-139">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="39649-140">`ConfigureAppConfiguration` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="39649-140">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="39649-141">Należy pamiętać, że ta konfiguracja nie ma zastosowania do hosta (na przykład adresy URL serwera lub środowiska).</span><span class="sxs-lookup"><span data-stu-id="39649-141">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="39649-142">Zobacz [hosta wartości konfiguracji](#host-configuration-values) sekcji.</span><span class="sxs-lookup"><span data-stu-id="39649-142">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="39649-143">Następujące `ConfigureLogging` wywołanie dodaje delegata, aby skonfigurować poziom rejestrowania minimalny ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) do [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="39649-143">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="39649-144">Ustawienie to zastępuje ustawienia w *appsettings. Development.JSON* (`LogLevel.Debug`) i *appsettings. Production.JSON* (`LogLevel.Error`) skonfigurowane przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="39649-144">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="39649-145">`ConfigureLogging` może zostać wywołana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="39649-145">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="39649-146">Następujące wywołanie do `ConfigureKestrel` zastępuje to domyślne [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtów nawiązane, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="39649-146">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="39649-147">Następujące wywołanie do [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) zastępuje to domyślne [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 bajtów nawiązane, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="39649-147">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="39649-148">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="39649-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="39649-149">Po uruchomieniu aplikacji z folderu głównego projektu, folder główny projektu jest używany jako katalogu głównego zawartości.</span><span class="sxs-lookup"><span data-stu-id="39649-149">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="39649-150">Jest to opcja domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="39649-150">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="39649-151">Aby uzyskać więcej informacji na temat konfiguracji aplikacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="39649-151">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="39649-152">Jako alternatywa dla użycia statycznego `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) metoda przy użyciu obsługiwanych za pomocą programu ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="39649-152">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="39649-153">Aby uzyskać więcej informacji zobacz kartę 1.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39649-153">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="39649-154">Podczas konfigurowania hostów [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) można przedstawić metody.</span><span class="sxs-lookup"><span data-stu-id="39649-154">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="39649-155">Jeśli `Startup` klasy jest określony, należy ją zdefiniować `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="39649-155">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="39649-156">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="39649-156">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="39649-157">Wiele wywołań `ConfigureServices` dołączenia do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="39649-157">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="39649-158">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpienia poprzednich ustawień.</span><span class="sxs-lookup"><span data-stu-id="39649-158">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="39649-159">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="39649-159">Host configuration values</span></span>

<span data-ttu-id="39649-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) opiera się na następujących podejść można ustawić wartości konfiguracji hostów:</span><span class="sxs-lookup"><span data-stu-id="39649-160">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="39649-161">Konfiguracja Konstruktora hosta, który zawiera zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="39649-161">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="39649-162">Na przykład `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="39649-162">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="39649-163">Rozszerzenia, takie jak [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) i [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (zobacz [zastępczą konfigurację](#override-configuration) sekcji).</span><span class="sxs-lookup"><span data-stu-id="39649-163">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="39649-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="39649-164">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="39649-165">Podczas ustawiania wartości z `UseSetting`, wartość jest ustawiana jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="39649-165">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="39649-166">Host używa jednego z tych opcji ustawia wartość ostatniego.</span><span class="sxs-lookup"><span data-stu-id="39649-166">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="39649-167">Aby uzyskać więcej informacji, zobacz [zastępczą konfigurację](#override-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="39649-167">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="39649-168">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="39649-168">Application Key (Name)</span></span>

<span data-ttu-id="39649-169">[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) właściwość ma wartość automatycznie, gdy [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) lub [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) jest wywoływana podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="39649-169">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="39649-170">Wartość jest równa Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-170">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="39649-171">Aby jawnie ustawić wartość, użyj [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="39649-171">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="39649-172">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="39649-172">**Key**: applicationName</span></span>  
<span data-ttu-id="39649-173">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-173">**Type**: *string*</span></span>  
<span data-ttu-id="39649-174">**Domyślne**: Nazwa zestawu zawierającego wpis aplikacji punktu.</span><span class="sxs-lookup"><span data-stu-id="39649-174">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="39649-175">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="39649-175">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="39649-176">**Zmienna środowiskowa**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="39649-176">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="39649-177">Przechwytywania błędów uruchamiania</span><span class="sxs-lookup"><span data-stu-id="39649-177">Capture Startup Errors</span></span>

<span data-ttu-id="39649-178">To ustawienie steruje przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="39649-178">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="39649-179">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="39649-179">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="39649-180">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="39649-180">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="39649-181">**Domyślne**: Wartość domyślna to `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, w którym domyślnie są `true`.</span><span class="sxs-lookup"><span data-stu-id="39649-181">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="39649-182">**Można ustawić przy użyciu**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="39649-182">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="39649-183">**Zmienna środowiskowa**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="39649-183">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="39649-184">Gdy `false`, błędy podczas uruchamiania wynik na hoście, kończenie.</span><span class="sxs-lookup"><span data-stu-id="39649-184">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="39649-185">Gdy `true`, host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="39649-185">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="39649-186">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="39649-186">Content Root</span></span>

<span data-ttu-id="39649-187">To ustawienie określa, gdzie platformy ASP.NET Core rozpoczyna się wyszukiwanie plików zawartości, takich jak widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="39649-187">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="39649-188">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="39649-188">**Key**: contentRoot</span></span>  
<span data-ttu-id="39649-189">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-189">**Type**: *string*</span></span>  
<span data-ttu-id="39649-190">**Domyślne**: Wartość domyślna to folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-190">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="39649-191">**Można ustawić przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="39649-191">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="39649-192">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="39649-192">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="39649-193">Główny zawartości jest również używane jako podstawową ścieżkę dla [ustawienie katalog główny sieci Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="39649-193">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="39649-194">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="39649-194">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="39649-195">Szczegółowe informacje o błędach</span><span class="sxs-lookup"><span data-stu-id="39649-195">Detailed Errors</span></span>

<span data-ttu-id="39649-196">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="39649-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="39649-197">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="39649-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="39649-198">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="39649-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="39649-199">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="39649-199">**Default**: false</span></span>  
<span data-ttu-id="39649-200">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="39649-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="39649-201">**Zmienna środowiskowa**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="39649-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="39649-202">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ustawiono `Development`), aplikacja przechwytuje szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="39649-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="39649-203">Środowisko</span><span class="sxs-lookup"><span data-stu-id="39649-203">Environment</span></span>

<span data-ttu-id="39649-204">Ustawia środowiska Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-204">Sets the app's environment.</span></span>

<span data-ttu-id="39649-205">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="39649-205">**Key**: environment</span></span>  
<span data-ttu-id="39649-206">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-206">**Type**: *string*</span></span>  
<span data-ttu-id="39649-207">**Domyślne**: Produkcji</span><span class="sxs-lookup"><span data-stu-id="39649-207">**Default**: Production</span></span>  
<span data-ttu-id="39649-208">**Można ustawić przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="39649-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="39649-209">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="39649-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="39649-210">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="39649-210">The environment can be set to any value.</span></span> <span data-ttu-id="39649-211">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="39649-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="39649-212">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="39649-212">Values aren't case sensitive.</span></span> <span data-ttu-id="39649-213">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="39649-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="39649-214">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="39649-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="39649-215">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="39649-215">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="39649-216">Hosting zestawy startowe</span><span class="sxs-lookup"><span data-stu-id="39649-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="39649-217">Ustawia hostingu zestawy uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="39649-218">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="39649-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="39649-219">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-219">**Type**: *string*</span></span>  
<span data-ttu-id="39649-220">**Domyślne**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="39649-220">**Default**: Empty string</span></span>  
<span data-ttu-id="39649-221">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="39649-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="39649-222">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="39649-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="39649-223">Rozdzielana średnikami ciąg hostingu zestawy startowe załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="39649-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="39649-224">Mimo że konfiguracja ma domyślnie wartość pustego ciągu, hostingu zestawy startowe zawsze zawierać zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-224">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="39649-225">Hostingu zestawy startowe są udostępniane, są dodawane do zestawu aplikacji dotyczące ładowania, gdy aplikacja tworzy swoich usług wspólne podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="39649-225">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="39649-226">HTTPS Port</span><span class="sxs-lookup"><span data-stu-id="39649-226">HTTPS Port</span></span>

<span data-ttu-id="39649-227">Ustawienie HTTPS przekierowania portu.</span><span class="sxs-lookup"><span data-stu-id="39649-227">Set the HTTPS redirect port.</span></span> <span data-ttu-id="39649-228">Używane w [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="39649-228">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="39649-229">**Klucz**: https_port **typu**: *ciąg*
**domyślne**: Nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="39649-229">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="39649-230">**Można ustawić przy użyciu**: `UseSetting`
**Zmienna środowiskowa**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="39649-230">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="39649-231">Hosting zestawy wykluczania uruchamiania</span><span class="sxs-lookup"><span data-stu-id="39649-231">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="39649-232">Rozdzielana średnikami ciąg hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="39649-232">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="39649-233">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="39649-233">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="39649-234">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-234">**Type**: *string*</span></span>  
<span data-ttu-id="39649-235">**Domyślne**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="39649-235">**Default**: Empty string</span></span>  
<span data-ttu-id="39649-236">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="39649-236">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="39649-237">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="39649-237">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="39649-238">Preferuj hostingu adresów URL</span><span class="sxs-lookup"><span data-stu-id="39649-238">Prefer Hosting URLs</span></span>

<span data-ttu-id="39649-239">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast konfigurowane przy użyciu `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="39649-239">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="39649-240">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="39649-240">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="39649-241">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="39649-241">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="39649-242">**Domyślne**: true</span><span class="sxs-lookup"><span data-stu-id="39649-242">**Default**: true</span></span>  
<span data-ttu-id="39649-243">**Można ustawić przy użyciu**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="39649-243">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="39649-244">**Zmienna środowiskowa**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="39649-244">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="39649-245">Zapobiegaj hostingu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="39649-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="39649-246">Uniemożliwia automatyczne ładowanie obsługi zestawów uruchamiania, w tym hosting zestawy startowe skonfigurowany przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="39649-247">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="39649-247">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="39649-248">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="39649-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="39649-249">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="39649-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="39649-250">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="39649-250">**Default**: false</span></span>  
<span data-ttu-id="39649-251">**Można ustawić przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="39649-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="39649-252">**Zmienna środowiskowa**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="39649-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="39649-253">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="39649-253">Server URLs</span></span>

<span data-ttu-id="39649-254">Wskazuje adresy IP lub adresów hosta, portów i protokołów, które serwer powinien nasłuchiwać żądań protokołu.</span><span class="sxs-lookup"><span data-stu-id="39649-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="39649-255">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="39649-255">**Key**: urls</span></span>  
<span data-ttu-id="39649-256">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-256">**Type**: *string*</span></span>  
<span data-ttu-id="39649-257">**Domyślne**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="39649-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="39649-258">**Można ustawić przy użyciu**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="39649-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="39649-259">**Zmienna środowiskowa**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="39649-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="39649-260">Ustaw rozdzielone średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="39649-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="39649-261">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="39649-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="39649-262">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="39649-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="39649-263">Protokół (`http://` lub `https://`) muszą być dołączone do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="39649-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="39649-264">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="39649-264">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="39649-265">Kestrel ma swój własny konfiguracji punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="39649-265">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="39649-266">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="39649-266">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="39649-267">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="39649-267">Shutdown Timeout</span></span>

<span data-ttu-id="39649-268">Określa ilość czasu oczekiwania dla hosta sieci web zamknąć.</span><span class="sxs-lookup"><span data-stu-id="39649-268">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="39649-269">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="39649-269">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="39649-270">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="39649-270">**Type**: *int*</span></span>  
<span data-ttu-id="39649-271">**Domyślne**: 5</span><span class="sxs-lookup"><span data-stu-id="39649-271">**Default**: 5</span></span>  
<span data-ttu-id="39649-272">**Można ustawić przy użyciu**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="39649-272">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="39649-273">**Zmienna środowiskowa**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="39649-273">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="39649-274">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) — metoda rozszerzenia ma [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="39649-274">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="39649-275">Podczas limitu czasu, hostingu:</span><span class="sxs-lookup"><span data-stu-id="39649-275">During the timeout period, hosting:</span></span>

* <span data-ttu-id="39649-276">Wyzwalacze [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="39649-276">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="39649-277">Podejmuje próby zatrzymania usług hostowanych, rejestrowanie błędów, których nie można zatrzymać usługi.</span><span class="sxs-lookup"><span data-stu-id="39649-277">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="39649-278">Jeśli upłynie limit czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostaną zatrzymane podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-278">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="39649-279">Usługi zatrzymania nawet wtedy, gdy jeszcze nie zakończyło się przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="39649-279">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="39649-280">Jeśli usługi wymagają dodatkowego czasu, aby zatrzymać, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="39649-280">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="39649-281">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="39649-281">Startup Assembly</span></span>

<span data-ttu-id="39649-282">Określa zestaw, aby wyszukać `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="39649-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="39649-283">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="39649-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="39649-284">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-284">**Type**: *string*</span></span>  
<span data-ttu-id="39649-285">**Domyślne**: Zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="39649-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="39649-286">**Można ustawić przy użyciu**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="39649-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="39649-287">**Zmienna środowiskowa**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="39649-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="39649-288">Zestawu według nazwy (`string`) lub wpisz (`TStartup`) mogą być przywoływane.</span><span class="sxs-lookup"><span data-stu-id="39649-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="39649-289">Jeśli wiele `UseStartup` metody są wywoływane, ostatni z nich ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="39649-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="39649-290">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="39649-290">Web Root</span></span>

<span data-ttu-id="39649-291">Ustawia ścieżkę względną do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-291">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="39649-292">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="39649-292">**Key**: webroot</span></span>  
<span data-ttu-id="39649-293">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="39649-293">**Type**: *string*</span></span>  
<span data-ttu-id="39649-294">**Domyślne**: Jeśli nie zostanie określony, wartość domyślna to "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="39649-294">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="39649-295">Jeśli ścieżka nie istnieje, jest używany dostawca pliku pusta.</span><span class="sxs-lookup"><span data-stu-id="39649-295">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="39649-296">**Można ustawić przy użyciu**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="39649-296">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="39649-297">**Zmienna środowiskowa**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="39649-297">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="39649-298">Zastąp konfigurację</span><span class="sxs-lookup"><span data-stu-id="39649-298">Override configuration</span></span>

<span data-ttu-id="39649-299">Użyj [konfiguracji](xref:fundamentals/configuration/index) skonfigurować hosta sieci web.</span><span class="sxs-lookup"><span data-stu-id="39649-299">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="39649-300">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określony w *hostsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="39649-300">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="39649-301">Dowolna Konfiguracja ładowane z *hostsettings.json* pliku może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="39649-301">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="39649-302">Konfiguracja wbudowanych (w `config`) służy do konfigurowania hosta z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="39649-302">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="39649-303">`IWebHostBuilder` Konfiguracja zostanie dodany do konfiguracji aplikacji, ale prawdą nie dotyczy&mdash; `ConfigureAppConfiguration` nie ma wpływu na `IWebHostBuilder` konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="39649-303">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="39649-304">Zastępowanie konfiguracji, jaką zapewnia `UseUrls` z *hostsettings.json* config argument po pierwsze, wiersza polecenia konfiguracji drugiego:</span><span class="sxs-lookup"><span data-stu-id="39649-304">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="39649-305">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="39649-305">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="39649-306">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metody rozszerzenia nie jest obecnie zdolne do analizowania sekcji konfiguracji, zwracany przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="39649-306">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="39649-307">`GetSection` Metoda klucze konfiguracji do sekcji żądane filtry, ale klucze nie powoduje usunięcia nazwy sekcji (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="39649-307">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="39649-308">`UseConfiguration` Metoda oczekuje kluczy, aby dopasować `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="39649-308">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="39649-309">Obecność nazwa sekcji na kluczach zapobiega wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="39649-309">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="39649-310">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="39649-310">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="39649-311">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie sekcję konfiguracji do WebHostBuilder.UseConfiguration korzysta z kluczami pełną](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="39649-311">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="39649-312">`UseConfiguration` tylko kopiuje kluczy z dostarczonego `IConfiguration` konfiguracji konstruktora hosta.</span><span class="sxs-lookup"><span data-stu-id="39649-312">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="39649-313">W związku z tym, ustawienie `reloadOnChange: true` plików ustawień JSON, INI i XML nie ma wpływu.</span><span class="sxs-lookup"><span data-stu-id="39649-313">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="39649-314">Aby określić hosta, uruchom na określony adres URL, żądaną wartość w można przekazać polecenie w wierszu polecenia podczas wykonywania [dotnet, uruchom](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="39649-314">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="39649-315">Argument wiersza polecenia zastępuje `urls` wartość z *hostsettings.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="39649-315">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="39649-316">Zarządzać hosta</span><span class="sxs-lookup"><span data-stu-id="39649-316">Manage the host</span></span>

<span data-ttu-id="39649-317">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="39649-317">**Run**</span></span>

<span data-ttu-id="39649-318">`Run` Metoda uruchamia aplikację sieci web i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="39649-318">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="39649-319">**Start**</span><span class="sxs-lookup"><span data-stu-id="39649-319">**Start**</span></span>

<span data-ttu-id="39649-320">Uruchamiane w sposób nieblokującej na poziomie hosta, przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="39649-320">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="39649-321">Jeśli lista adresów URL jest przekazywany do `Start` metody nasłuchuje on na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="39649-321">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="39649-322">Aplikacja może inicjuje i uruchamia nowy host, przy użyciu wstępnie skonfigurowanych ustawień domyślnych z `CreateDefaultBuilder` za pomocą innej metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="39649-322">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="39649-323">Te metody uruchomić serwer bez danych wyjściowych konsoli i przy [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podziału (Ctrl-C/SIGINT lub SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="39649-323">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="39649-324">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="39649-324">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="39649-325">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="39649-325">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="39649-326">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="39649-326">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="39649-327">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="39649-327">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="39649-328">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="39649-328">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="39649-329">**Uruchom (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="39649-329">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="39649-330">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="39649-330">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="39649-331">Daje ten sam wynik jako **Start (aplikacja RequestDelegate)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="39649-331">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="39649-332">**Uruchom (Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="39649-332">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="39649-333">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) używać oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="39649-333">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="39649-334">W przykładzie, należy użyć następujących żądań przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="39649-334">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="39649-335">Żądanie</span><span class="sxs-lookup"><span data-stu-id="39649-335">Request</span></span>                                    | <span data-ttu-id="39649-336">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="39649-336">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="39649-337">Witaj, Martin!</span><span class="sxs-lookup"><span data-stu-id="39649-337">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="39649-338">Buenos diasem, Catrina!</span><span class="sxs-lookup"><span data-stu-id="39649-338">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="39649-339">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="39649-339">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="39649-340">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="39649-340">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="39649-341">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="39649-341">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="39649-342">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="39649-342">Hello World!</span></span>                             |

<span data-ttu-id="39649-343">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="39649-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="39649-344">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="39649-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="39649-345">**Uruchom (ciąg adresu url, Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="39649-345">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="39649-346">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="39649-346">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="39649-347">Daje ten sam wynik jako **Start (Akcja&lt;IRouteBuilder&gt; routeBuilder)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="39649-347">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="39649-348">**StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="39649-348">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="39649-349">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="39649-349">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="39649-350">Wyślij żądanie w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="39649-350">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="39649-351">`WaitForShutdown` blokuje, dopóki nie wystawiono podziału (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="39649-351">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="39649-352">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na naciśnięcie klawisza zakończyć pracę.</span><span class="sxs-lookup"><span data-stu-id="39649-352">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="39649-353">**StartWith (ciąg adresu url, Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="39649-353">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="39649-354">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="39649-354">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="39649-355">Daje ten sam wynik jako **StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**, chyba że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="39649-355">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="39649-356">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="39649-356">IHostingEnvironment interface</span></span>

<span data-ttu-id="39649-357">[Interfejsu IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje dotyczące aplikacji sieci web, środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="39649-357">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="39649-358">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać `IHostingEnvironment` aby można było używać jej właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="39649-358">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="39649-359">A [podejścia opartego na Konwencji](xref:fundamentals/environments#environment-based-startup-class-and-methods) mogą być używane do konfigurowania aplikacji przy uruchamianiu opartych na środowisku.</span><span class="sxs-lookup"><span data-stu-id="39649-359">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="39649-360">Alternatywnie wstrzyknąć `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="39649-360">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="39649-361">Oprócz `IsDevelopment` metody rozszerzenia `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="39649-361">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="39649-362">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="39649-362">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="39649-363">`IHostingEnvironment` Usługi może także wstrzyknięte bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="39649-363">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="39649-364">`IHostingEnvironment` może być wstrzykuje `Invoke` metody podczas tworzenia niestandardowych [oprogramowania pośredniczącego](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="39649-364">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="39649-365">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="39649-365">IApplicationLifetime interface</span></span>

<span data-ttu-id="39649-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działań po uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="39649-366">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="39649-367">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="39649-367">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="39649-368">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="39649-368">Cancellation Token</span></span>    | <span data-ttu-id="39649-369">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="39649-369">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="39649-370">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="39649-370">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="39649-371">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="39649-371">The host has fully started.</span></span> |
| [<span data-ttu-id="39649-372">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="39649-372">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="39649-373">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="39649-373">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="39649-374">Wszystkie żądania powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="39649-374">All requests should be processed.</span></span> <span data-ttu-id="39649-375">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="39649-375">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="39649-376">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="39649-376">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="39649-377">Wykonuje łagodne zamykanie hosta.</span><span class="sxs-lookup"><span data-stu-id="39649-377">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="39649-378">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="39649-378">Requests may still be processing.</span></span> <span data-ttu-id="39649-379">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="39649-379">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="39649-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-380">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="39649-381">Następujące klasy używa `StopApplication` bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="39649-381">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="39649-382">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="39649-382">Scope validation</span></span>

<span data-ttu-id="39649-383">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` w przypadku aplikacji środowiska programowania.</span><span class="sxs-lookup"><span data-stu-id="39649-383">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="39649-384">Gdy `ValidateScopes` ustawiono `true`, domyślny dostawca usługi sprawdza upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="39649-384">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="39649-385">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="39649-385">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="39649-386">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="39649-386">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="39649-387">Dostawcy usług główny jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="39649-387">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="39649-388">Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="39649-388">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="39649-389">Usługi o określonym zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="39649-389">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="39649-390">Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="39649-390">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="39649-391">Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="39649-391">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="39649-392">Aby zawsze weryfikowały zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) w Konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="39649-392">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="39649-393">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="39649-393">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
