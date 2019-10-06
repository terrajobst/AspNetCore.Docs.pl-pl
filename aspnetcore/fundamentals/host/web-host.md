---
title: ASP.NET Core hosta sieci Web
author: rick-anderson
description: Dowiedz się więcej o hoście sieci Web w ASP.NET Core, który jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: 977c1df67c2775870d630f3a1085d5e19cef58f5
ms.sourcegitcommit: 4115bf0e850c13d4e655beb5ab5e8ff431173cb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981897"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="f1b05-103">ASP.NET Core hosta sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1b05-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="f1b05-104">ASP.NET Core aplikacje konfigurują i uruchamiają *hosta*.</span><span class="sxs-lookup"><span data-stu-id="f1b05-104">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="f1b05-105">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f1b05-106">Host konfiguruje co najmniej serwer i potok przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="f1b05-106">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="f1b05-107">Na hoście można także skonfigurować rejestrowanie, iniekcję zależności i konfigurację.</span><span class="sxs-lookup"><span data-stu-id="f1b05-107">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f1b05-108">W tym artykule omówiono hosta sieci Web, który jest dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="f1b05-108">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="f1b05-109">[Host ogólny](xref:fundamentals/host/generic-host) jest zalecany dla wszystkich typów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-109">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="f1b05-110">W tym artykule omówiono hosta sieci Web, który służy do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1b05-110">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="f1b05-111">W przypadku innych rodzajów aplikacji należy użyć [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="f1b05-111">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="f1b05-112">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="f1b05-112">Set up a host</span></span>

<span data-ttu-id="f1b05-113">Utwórz hosta przy użyciu wystąpienia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f1b05-113">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="f1b05-114">Jest to zwykle wykonywane w punkcie wejścia aplikacji, Metoda `Main`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-114">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="f1b05-115">W szablonach projektu `Main` znajduje się w *program.cs*.</span><span class="sxs-lookup"><span data-stu-id="f1b05-115">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="f1b05-116">Typowe wywołania aplikacji [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="f1b05-116">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="f1b05-117">Kod, który wywołuje `CreateDefaultBuilder`, znajduje się w metodzie o nazwie `CreateWebHostBuilder`, która oddziela ją od kodu w `Main`, który wywołuje `Run` w obiekcie konstruktora.</span><span class="sxs-lookup"><span data-stu-id="f1b05-117">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="f1b05-118">Ta separacja jest wymagana, jeśli używasz [narzędzi Entity Framework Core](/ef/core/miscellaneous/cli/).</span><span class="sxs-lookup"><span data-stu-id="f1b05-118">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="f1b05-119">Narzędzia oczekują na znalezienie metody `CreateWebHostBuilder`, która może być wywoływana w czasie projektowania, aby skonfigurować hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-119">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="f1b05-120">Alternatywą jest wdrożenie `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-120">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="f1b05-121">Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="f1b05-121">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="f1b05-122">`CreateDefaultBuilder` wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="f1b05-122">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="f1b05-123">Konfiguruje serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web przy użyciu dostawców konfiguracji hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-123">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="f1b05-124">Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-124">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="f1b05-125">Ustawia katalog główny zawartości dla ścieżki zwróconej przez [katalog. GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="f1b05-125">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="f1b05-126">Ładuje [konfigurację hosta](#host-configuration-values) z:</span><span class="sxs-lookup"><span data-stu-id="f1b05-126">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="f1b05-127">Zmienne środowiskowe poprzedzone prefiksem `ASPNETCORE_` (na przykład `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="f1b05-127">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="f1b05-128">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-128">Command-line arguments.</span></span>
* <span data-ttu-id="f1b05-129">Ładuje konfigurację aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="f1b05-129">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="f1b05-130">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f1b05-130">*appsettings.json*.</span></span>
  * <span data-ttu-id="f1b05-131">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f1b05-131">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="f1b05-132">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development` przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="f1b05-132">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="f1b05-133">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="f1b05-133">Environment variables.</span></span>
  * <span data-ttu-id="f1b05-134">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-134">Command-line arguments.</span></span>
* <span data-ttu-id="f1b05-135">Konfiguruje [Rejestrowanie](xref:fundamentals/logging/index) dla konsoli i danych wyjściowych debugowania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-135">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="f1b05-136">Rejestrowanie obejmuje reguły [filtrowania dzienników](xref:fundamentals/logging/index#log-filtering) określone w sekcji konfiguracja rejestrowania w pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* .</span><span class="sxs-lookup"><span data-stu-id="f1b05-136">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="f1b05-137">Po uruchomieniu usług IIS za pomocą [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` włącza [integrację usług IIS](xref:host-and-deploy/iis/index), która konfiguruje podstawowy adres i port aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-137">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="f1b05-138">Integracja usług IIS konfiguruje również aplikację do [przechwytywania błędów uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="f1b05-138">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="f1b05-139">Aby poznać domyślne opcje usług IIS, zobacz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-139">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="f1b05-140">Ustawia [ServiceProviderOptions. ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) na `true`, jeśli środowisko aplikacji jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="f1b05-140">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="f1b05-141">Aby uzyskać więcej informacji, zobacz [Walidacja zakresu](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="f1b05-141">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="f1b05-142">Konfigurację zdefiniowaną za pomocą `CreateDefaultBuilder` można zastąpić i rozszerzyć przez [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)i inne metody i metody rozszerzające [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f1b05-142">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="f1b05-143">Poniżej przedstawiono kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="f1b05-143">A few examples follow:</span></span>

* <span data-ttu-id="f1b05-144">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) jest używany do określania dodatkowych `IConfiguration` dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-144">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="f1b05-145">Następujące wywołanie `ConfigureAppConfiguration` dodaje delegata w celu uwzględnienia konfiguracji aplikacji w pliku *appSettings. XML* .</span><span class="sxs-lookup"><span data-stu-id="f1b05-145">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="f1b05-146">`ConfigureAppConfiguration` może być wywoływana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="f1b05-146">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="f1b05-147">Należy zauważyć, że ta konfiguracja nie ma zastosowania do hosta (na przykład adresów URL serwera lub środowiska).</span><span class="sxs-lookup"><span data-stu-id="f1b05-147">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="f1b05-148">Zapoznaj się z sekcją [wartości konfiguracji hosta](#host-configuration-values) .</span><span class="sxs-lookup"><span data-stu-id="f1b05-148">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="f1b05-149">Następujące wywołanie `ConfigureLogging` dodaje delegata w celu skonfigurowania minimalnego poziomu rejestrowania ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) na wartość [LogLevel. Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="f1b05-149">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="f1b05-150">To ustawienie zastępuje ustawienia w *appSettings. Development. JSON* (`LogLevel.Debug`) i *appSettings. Production. JSON* (`LogLevel.Error`) skonfigurowanym przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-150">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f1b05-151">`ConfigureLogging` może być wywoływana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="f1b05-151">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f1b05-152">Następujące wywołanie `ConfigureKestrel` zastępuje domyślne [limity. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30 000 000 bajtów ustanowiono, gdy Kestrel został skonfigurowany przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-152">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f1b05-153">Następujące wywołanie [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) zastępuje domyślne [limity. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30 000 000 bajtów ustanowionych, gdy Kestrel została skonfigurowana przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-153">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="f1b05-154">*Katalog główny zawartości* określa, gdzie host wyszukuje pliki zawartości, takie jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="f1b05-154">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="f1b05-155">Gdy aplikacja jest uruchamiana z folderu głównego projektu, folder główny projektu jest używany jako katalog główny zawartości.</span><span class="sxs-lookup"><span data-stu-id="f1b05-155">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="f1b05-156">Jest to ustawienie domyślne używane w programie [Visual Studio](https://visualstudio.microsoft.com) i w [nowych szablonach dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="f1b05-156">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="f1b05-157">Aby uzyskać więcej informacji na temat konfiguracji aplikacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-157">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="f1b05-158">Alternatywnym rozwiązaniem jest użycie statycznej metody `CreateDefaultBuilder`, ponieważ utworzenie hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) jest obsługiwaną metodą ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="f1b05-158">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="f1b05-159">Podczas konfigurowania hosta można dostarczyć metody [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) .</span><span class="sxs-lookup"><span data-stu-id="f1b05-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="f1b05-160">Jeśli określono klasę `Startup`, należy zdefiniować metodę `Configure`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="f1b05-161">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="f1b05-162">Wiele wywołań `ConfigureServices` dołącza do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="f1b05-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="f1b05-163">Wiele wywołań `Configure` lub `UseStartup` w `WebHostBuilder` Zastąp poprzednie ustawienia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="f1b05-164">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="f1b05-164">Host configuration values</span></span>

<span data-ttu-id="f1b05-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) opiera się na następujących podejściach do ustawiania wartości konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="f1b05-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="f1b05-166">Konfiguracja konstruktora hostów, która zawiera zmienne środowiskowe o formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="f1b05-167">Na przykład `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="f1b05-168">Rozszerzenia, takie jak [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) i [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (zobacz sekcję [Konfiguracja zastąpień](#override-configuration) ).</span><span class="sxs-lookup"><span data-stu-id="f1b05-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="f1b05-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="f1b05-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="f1b05-170">Podczas ustawiania wartości przy użyciu `UseSetting` wartość jest ustawiana jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="f1b05-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="f1b05-171">Host używa dowolnej opcji ustawia wartość Last.</span><span class="sxs-lookup"><span data-stu-id="f1b05-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="f1b05-172">Aby uzyskać więcej informacji, zobacz [Przesłoń konfigurację](#override-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="f1b05-173">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="f1b05-173">Application Key (Name)</span></span>

<span data-ttu-id="f1b05-174">Właściwość [IHostingEnvironment. ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) jest ustawiana automatycznie, gdy [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) lub [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) jest wywoływana podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="f1b05-174">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="f1b05-175">Wartość jest ustawiana na nazwę zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="f1b05-176">Aby jawnie ustawić wartość, użyj [WebHostDefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="f1b05-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="f1b05-177">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="f1b05-177">**Key**: applicationName</span></span>  
<span data-ttu-id="f1b05-178">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-178">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-179">**Wartość domyślna**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-179">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="f1b05-180">**Ustaw przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f1b05-180">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f1b05-181">**Zmienna środowiskowa**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="f1b05-181">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="f1b05-182">Błędy uruchamiania przechwytywania</span><span class="sxs-lookup"><span data-stu-id="f1b05-182">Capture Startup Errors</span></span>

<span data-ttu-id="f1b05-183">To ustawienie steruje przechwytywaniem błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-183">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="f1b05-184">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="f1b05-184">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="f1b05-185">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="f1b05-185">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f1b05-186">**Wartość domyślna**: Wartość domyślna to `false`, chyba że aplikacja jest uruchamiana z Kestrel za pomocą usług IIS, w której domyślnym ustawieniem jest `true`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-186">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="f1b05-187">**Ustaw przy użyciu**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="f1b05-187">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="f1b05-188">**Zmienna środowiskowa**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="f1b05-188">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="f1b05-189">Gdy `false`, błędy podczas uruchamiania, kończenie działania hosta.</span><span class="sxs-lookup"><span data-stu-id="f1b05-189">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="f1b05-190">Gdy `true`, Host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="f1b05-190">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="f1b05-191">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="f1b05-191">Content Root</span></span>

<span data-ttu-id="f1b05-192">To ustawienie określa, gdzie ASP.NET Core rozpoczyna wyszukiwanie plików zawartości, takich jak widoki MVC.</span><span class="sxs-lookup"><span data-stu-id="f1b05-192">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="f1b05-193">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="f1b05-193">**Key**: contentRoot</span></span>  
<span data-ttu-id="f1b05-194">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-194">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-195">**Wartość domyślna**: Domyślnie znajduje się w folderze, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-195">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="f1b05-196">**Ustaw przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="f1b05-196">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="f1b05-197">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="f1b05-197">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="f1b05-198">Katalog główny zawartości jest również używany jako ścieżka podstawowa dla [Ustawienia katalogu głównego sieci Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="f1b05-198">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="f1b05-199">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="f1b05-199">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="f1b05-200">Szczegóły błędów</span><span class="sxs-lookup"><span data-stu-id="f1b05-200">Detailed Errors</span></span>

<span data-ttu-id="f1b05-201">Określa, czy mają być przechwytywane szczegółowe błędy.</span><span class="sxs-lookup"><span data-stu-id="f1b05-201">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="f1b05-202">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="f1b05-202">**Key**: detailedErrors</span></span>  
<span data-ttu-id="f1b05-203">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="f1b05-203">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f1b05-204">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="f1b05-204">**Default**: false</span></span>  
<span data-ttu-id="f1b05-205">**Ustaw przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f1b05-205">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f1b05-206">**Zmienna środowiskowa**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="f1b05-206">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="f1b05-207">Po włączeniu (lub gdy <a href="#environment">środowisko</a> jest ustawione na `Development`), aplikacja przechwytuje szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="f1b05-207">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="f1b05-208">Środowisko</span><span class="sxs-lookup"><span data-stu-id="f1b05-208">Environment</span></span>

<span data-ttu-id="f1b05-209">Ustawia środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-209">Sets the app's environment.</span></span>

<span data-ttu-id="f1b05-210">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="f1b05-210">**Key**: environment</span></span>  
<span data-ttu-id="f1b05-211">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-211">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-212">**Wartość domyślna**: Narzędzi</span><span class="sxs-lookup"><span data-stu-id="f1b05-212">**Default**: Production</span></span>  
<span data-ttu-id="f1b05-213">**Ustaw przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="f1b05-213">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="f1b05-214">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="f1b05-214">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="f1b05-215">Dla środowiska można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="f1b05-215">The environment can be set to any value.</span></span> <span data-ttu-id="f1b05-216">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging` i `Production`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-216">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="f1b05-217">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f1b05-217">Values aren't case sensitive.</span></span> <span data-ttu-id="f1b05-218">Domyślnie *środowisko* jest odczytywane ze zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-218">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="f1b05-219">W przypadku korzystania z [programu Visual Studio](https://visualstudio.microsoft.com)zmienne środowiskowe można ustawić w pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f1b05-219">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="f1b05-220">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-220">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="f1b05-221">Hostowanie zestawów startowych</span><span class="sxs-lookup"><span data-stu-id="f1b05-221">Hosting Startup Assemblies</span></span>

<span data-ttu-id="f1b05-222">Ustawia zestawy uruchomieniowe obsługujące aplikację.</span><span class="sxs-lookup"><span data-stu-id="f1b05-222">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="f1b05-223">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="f1b05-223">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="f1b05-224">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-224">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-225">**Wartość domyślna**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="f1b05-225">**Default**: Empty string</span></span>  
<span data-ttu-id="f1b05-226">**Ustaw przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f1b05-226">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f1b05-227">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="f1b05-227">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="f1b05-228">Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-228">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="f1b05-229">Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="f1b05-230">W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="f1b05-231">Port HTTPS</span><span class="sxs-lookup"><span data-stu-id="f1b05-231">HTTPS Port</span></span>

<span data-ttu-id="f1b05-232">Ustaw port przekierowania HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f1b05-232">Set the HTTPS redirect port.</span></span> <span data-ttu-id="f1b05-233">Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="f1b05-233">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="f1b05-234">**Key**: https_port **typ**: *ciąg*
**domyślny**: Nie ustawiono wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="f1b05-234">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="f1b05-235">**Ustaw przy użyciu**: `UseSetting` @ no__t-1**zmienna środowiskowa**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="f1b05-235">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="f1b05-236">Hosting — wykluczanie zestawów</span><span class="sxs-lookup"><span data-stu-id="f1b05-236">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="f1b05-237">Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-237">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="f1b05-238">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="f1b05-238">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="f1b05-239">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-239">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-240">**Wartość domyślna**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="f1b05-240">**Default**: Empty string</span></span>  
<span data-ttu-id="f1b05-241">**Ustaw przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f1b05-241">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f1b05-242">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="f1b05-242">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="f1b05-243">Preferuj adresy URL hostingu</span><span class="sxs-lookup"><span data-stu-id="f1b05-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="f1b05-244">Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy użyciu `WebHostBuilder` zamiast dla skonfigurowanych przy użyciu implementacji `IServer`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="f1b05-245">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="f1b05-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="f1b05-246">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="f1b05-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f1b05-247">Wartość **Domyślna**: prawda</span><span class="sxs-lookup"><span data-stu-id="f1b05-247">**Default**: true</span></span>  
<span data-ttu-id="f1b05-248">**Ustaw przy użyciu**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="f1b05-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="f1b05-249">**Zmienna środowiskowa**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="f1b05-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="f1b05-250">Zapobiegaj uruchamianiu hostingu</span><span class="sxs-lookup"><span data-stu-id="f1b05-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="f1b05-251">Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="f1b05-252">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="f1b05-253">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="f1b05-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="f1b05-254">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="f1b05-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="f1b05-255">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="f1b05-255">**Default**: false</span></span>  
<span data-ttu-id="f1b05-256">**Ustaw przy użyciu**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="f1b05-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="f1b05-257">**Zmienna środowiskowa**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="f1b05-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="f1b05-258">Adresy URL serwera</span><span class="sxs-lookup"><span data-stu-id="f1b05-258">Server URLs</span></span>

<span data-ttu-id="f1b05-259">Wskazuje adresy IP lub adresy hosta z portami i protokołami, na których serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="f1b05-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="f1b05-260">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="f1b05-260">**Key**: urls</span></span>  
<span data-ttu-id="f1b05-261">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-261">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-262">**Wartość domyślna**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="f1b05-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="f1b05-263">**Ustaw przy użyciu**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="f1b05-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="f1b05-264">**Zmienna środowiskowa**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="f1b05-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="f1b05-265">Ustaw na rozdzieloną średnikami (;) Lista prefiksów adresów URL, do których serwer powinien reagować.</span><span class="sxs-lookup"><span data-stu-id="f1b05-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="f1b05-266">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="f1b05-267">Użyj opcji "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="f1b05-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="f1b05-268">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f1b05-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="f1b05-269">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="f1b05-269">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="f1b05-270">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="f1b05-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="f1b05-271">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="f1b05-272">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="f1b05-272">Shutdown Timeout</span></span>

<span data-ttu-id="f1b05-273">Określa czas oczekiwania na zamknięcie hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1b05-273">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="f1b05-274">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="f1b05-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="f1b05-275">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="f1b05-275">**Type**: *int*</span></span>  
<span data-ttu-id="f1b05-276">**Wartość domyślna**: 5</span><span class="sxs-lookup"><span data-stu-id="f1b05-276">**Default**: 5</span></span>  
<span data-ttu-id="f1b05-277">**Ustaw przy użyciu**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="f1b05-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="f1b05-278">**Zmienna środowiskowa**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="f1b05-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="f1b05-279">Chociaż klucz akceptuje *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), Metoda rozszerzenia [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) przyjmuje wartość [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="f1b05-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="f1b05-280">Podczas okresu przekroczenia limitu czasu hostowanie:</span><span class="sxs-lookup"><span data-stu-id="f1b05-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="f1b05-281">Wyzwala [IApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="f1b05-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="f1b05-282">Próbuje zatrzymać usługi hostowane, rejestrując błędy dla usług, których nie można zatrzymać.</span><span class="sxs-lookup"><span data-stu-id="f1b05-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="f1b05-283">Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="f1b05-284">Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="f1b05-285">Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="f1b05-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="f1b05-286">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="f1b05-286">Startup Assembly</span></span>

<span data-ttu-id="f1b05-287">Określa zestaw do wyszukania klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="f1b05-288">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="f1b05-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="f1b05-289">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-289">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-290">**Wartość domyślna**: Zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="f1b05-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="f1b05-291">**Ustaw przy użyciu**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="f1b05-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="f1b05-292">**Zmienna środowiskowa**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="f1b05-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="f1b05-293">Można odwołać się do zestawu według nazwy (`string`) lub typu (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="f1b05-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="f1b05-294">W przypadku wywołania wielu metod `UseStartup` ostatni ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="f1b05-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="f1b05-295">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1b05-295">Web Root</span></span>

<span data-ttu-id="f1b05-296">Ustawia ścieżkę względną do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="f1b05-297">**Klucz**: Webroot</span><span class="sxs-lookup"><span data-stu-id="f1b05-297">**Key**: webroot</span></span>  
<span data-ttu-id="f1b05-298">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="f1b05-298">**Type**: *string*</span></span>  
<span data-ttu-id="f1b05-299">**Wartość domyślna**: Jeśli nie zostanie określony, wartością domyślną jest "(katalog zawartości)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="f1b05-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="f1b05-300">Jeśli ścieżka nie istnieje, zostanie użyty dostawca plików No-op.</span><span class="sxs-lookup"><span data-stu-id="f1b05-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="f1b05-301">**Ustaw przy użyciu**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="f1b05-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="f1b05-302">**Zmienna środowiskowa**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="f1b05-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="f1b05-303">Zastąp konfigurację</span><span class="sxs-lookup"><span data-stu-id="f1b05-303">Override configuration</span></span>

<span data-ttu-id="f1b05-304">Skonfiguruj hosta sieci Web za pomocą [konfiguracji](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="f1b05-304">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="f1b05-305">W poniższym przykładzie Konfiguracja hosta jest opcjonalnie określona w pliku *HostSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f1b05-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="f1b05-306">Wszystkie konfiguracje załadowane z pliku *HostSettings. JSON* mogą zostać zastąpione przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="f1b05-307">Utworzona konfiguracja (w `config`) służy do konfigurowania hosta z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="f1b05-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="f1b05-308">Konfiguracja `IWebHostBuilder` jest dodawana do konfiguracji aplikacji, ale nie ma ona wartości true @ no__t-1 @ no__t-2 nie ma wpływu na konfigurację `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="f1b05-309">Zastępowanie konfiguracji dostarczonej przez `UseUrls` z *HostSettings. JSON* config najpierw, argument wiersza polecenia config sekund:</span><span class="sxs-lookup"><span data-stu-id="f1b05-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="f1b05-310">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f1b05-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="f1b05-311">Metoda rozszerzenia [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) nie jest obecnie w stanie analizowania sekcji konfiguracyjnej zwróconej przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-311">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="f1b05-312">Metoda `GetSection` filtruje klucze konfiguracji do żądanej sekcji, ale pozostawia nazwę sekcji w kluczach (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="f1b05-312">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="f1b05-313">Metoda `UseConfiguration` oczekuje, że klucze są zgodne z kluczami `WebHostBuilder` (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="f1b05-313">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="f1b05-314">Obecność nazwy sekcji w kluczach uniemożliwia wartości sekcji od skonfigurowania hosta.</span><span class="sxs-lookup"><span data-stu-id="f1b05-314">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="f1b05-315">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-315">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="f1b05-316">Aby uzyskać więcej informacji i obejść, zobacz [przekazywanie sekcji konfiguracji do WebHostBuilder. UseConfiguration używa pełnych kluczy](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="f1b05-316">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="f1b05-317">`UseConfiguration` kopiuje tylko klucze z podanej `IConfiguration` do konfiguracji konstruktora hostów.</span><span class="sxs-lookup"><span data-stu-id="f1b05-317">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="f1b05-318">W związku z tym ustawienie `reloadOnChange: true` dla plików JSON, INI i XML nie ma żadnego wpływu.</span><span class="sxs-lookup"><span data-stu-id="f1b05-318">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="f1b05-319">Aby określić hosta na określonym adresie URL, wymagana wartość może zostać przeniesiona z wiersza polecenia podczas wykonywania [przebiegu dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="f1b05-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="f1b05-320">Argument wiersza polecenia zastępuje wartość `urls` z pliku *HostSettings. JSON* , a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="f1b05-320">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="f1b05-321">Zarządzanie hostem</span><span class="sxs-lookup"><span data-stu-id="f1b05-321">Manage the host</span></span>

<span data-ttu-id="f1b05-322">**Run**</span><span class="sxs-lookup"><span data-stu-id="f1b05-322">**Run**</span></span>

<span data-ttu-id="f1b05-323">Metoda `Run` uruchamia aplikację sieci Web i blokuje wątek wywołujący do momentu wyłączenia hosta:</span><span class="sxs-lookup"><span data-stu-id="f1b05-323">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="f1b05-324">**Start**</span><span class="sxs-lookup"><span data-stu-id="f1b05-324">**Start**</span></span>

<span data-ttu-id="f1b05-325">Uruchom hosta w sposób bez blokowania, wywołując jego metodę `Start`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-325">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="f1b05-326">Jeśli lista adresów URL jest przenoszona do metody `Start`, nasłuchuje na określonych adresach URL:</span><span class="sxs-lookup"><span data-stu-id="f1b05-326">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="f1b05-327">Aplikacja może inicjować i uruchamiać nowy host przy użyciu wstępnie skonfigurowanych wartości domyślnych `CreateDefaultBuilder` przy użyciu statycznej metody wygodnej.</span><span class="sxs-lookup"><span data-stu-id="f1b05-327">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="f1b05-328">Metody te służą do uruchamiania serwera bez danych wyjściowych konsoli i [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekać na przerwanie (Ctrl-C/SIGINT lub SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="f1b05-328">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="f1b05-329">**Rozpocznij (aplikacja RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="f1b05-329">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="f1b05-330">Zacznij od `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-330">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="f1b05-331">Wykonaj żądanie w przeglądarce, aby `http://localhost:5000`, aby otrzymać odpowiedź "Hello world!"</span><span class="sxs-lookup"><span data-stu-id="f1b05-331">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="f1b05-332">bloki `WaitForShutdown` do momentu wystawienia przerwy (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="f1b05-332">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="f1b05-333">Aplikacja wyświetli komunikat `Console.WriteLine` i czeka na zakończenie naciśnięcia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-333">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="f1b05-334">**Rozpocznij (adres URL ciągu, aplikacja RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="f1b05-334">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="f1b05-335">Zacznij od adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-335">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="f1b05-336">Daje ten sam wynik jako **Start (RequestDelegate App)** , z wyjątkiem tego, że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-336">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="f1b05-337">**Start (Action @ no__t-1IRouteBuilder @ no__t-2 routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="f1b05-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="f1b05-338">Użyj wystąpienia `IRouteBuilder` ([Microsoft. AspNetCore. Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)), aby użyć oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="f1b05-338">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="f1b05-339">Użyj poniższego żądania przeglądarki z przykładem:</span><span class="sxs-lookup"><span data-stu-id="f1b05-339">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="f1b05-340">Żądanie</span><span class="sxs-lookup"><span data-stu-id="f1b05-340">Request</span></span>                                    | <span data-ttu-id="f1b05-341">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="f1b05-341">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="f1b05-342">Witaj, Martin!</span><span class="sxs-lookup"><span data-stu-id="f1b05-342">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="f1b05-343">Buenos Dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="f1b05-343">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="f1b05-344">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="f1b05-344">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="f1b05-345">Zgłasza wyjątek z ciągiem "zapomniano"</span><span class="sxs-lookup"><span data-stu-id="f1b05-345">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="f1b05-346">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="f1b05-346">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="f1b05-347">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="f1b05-347">Hello World!</span></span>                             |

<span data-ttu-id="f1b05-348">bloki `WaitForShutdown` do momentu wystawienia przerwy (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="f1b05-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="f1b05-349">Aplikacja wyświetli komunikat `Console.WriteLine` i czeka na zakończenie naciśnięcia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="f1b05-350">**Start (ciąg URL, Action @ no__t-1IRouteBuilder @ no__t-2 routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="f1b05-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="f1b05-351">Użyj adresu URL i wystąpienia `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-351">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="f1b05-352">Daje ten sam wynik jako **początkowy (Action @ no__t-1IRouteBuilder @ no__t-2 routeBuilder)** , z wyjątkiem tego, że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-352">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="f1b05-353">**StartWith (Action @ no__t-1IApplicationBuilder @ no__t-2 App)**</span><span class="sxs-lookup"><span data-stu-id="f1b05-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="f1b05-354">Podaj delegata, aby skonfigurować `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-354">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="f1b05-355">Wykonaj żądanie w przeglądarce, aby `http://localhost:5000`, aby otrzymać odpowiedź "Hello world!"</span><span class="sxs-lookup"><span data-stu-id="f1b05-355">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="f1b05-356">bloki `WaitForShutdown` do momentu wystawienia przerwy (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="f1b05-356">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="f1b05-357">Aplikacja wyświetli komunikat `Console.WriteLine` i czeka na zakończenie naciśnięcia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-357">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="f1b05-358">**StartWith (adres URL ciągu, Akcja @ no__t-1IApplicationBuilder @ no__t-2)**</span><span class="sxs-lookup"><span data-stu-id="f1b05-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="f1b05-359">Podaj adres URL i delegata, aby skonfigurować `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-359">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="f1b05-360">Daje ten sam wynik jako **StartWith (Action @ no__t-1IApplicationBuilder @ no__t-2 App)** , z tą różnicą, że aplikacja reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-360">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="f1b05-361">IHostingEnvironment, interfejs</span><span class="sxs-lookup"><span data-stu-id="f1b05-361">IHostingEnvironment interface</span></span>

<span data-ttu-id="f1b05-362">[Interfejs IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje o środowisku hostingu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f1b05-362">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="f1b05-363">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) , aby uzyskać `IHostingEnvironment`, aby użyć jego właściwości i metod rozszerzających:</span><span class="sxs-lookup"><span data-stu-id="f1b05-363">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="f1b05-364">[Podejście oparte na Konwencji](xref:fundamentals/environments#environment-based-startup-class-and-methods) może służyć do konfigurowania aplikacji podczas uruchamiania na podstawie środowiska.</span><span class="sxs-lookup"><span data-stu-id="f1b05-364">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="f1b05-365">Alternatywnie można wstrzyknąć `IHostingEnvironment` do konstruktora `Startup` do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f1b05-365">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="f1b05-366">Oprócz metody rozszerzenia `IsDevelopment` `IHostingEnvironment` oferuje `IsStaging`, `IsProduction` i `IsEnvironment(string environmentName)` metod.</span><span class="sxs-lookup"><span data-stu-id="f1b05-366">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="f1b05-367">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="f1b05-367">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="f1b05-368">Usługę `IHostingEnvironment` można również wstrzyknąć bezpośrednio do metody `Configure` w celu skonfigurowania potoku przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="f1b05-368">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
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

<span data-ttu-id="f1b05-369">`IHostingEnvironment` można wstrzyknąć do metody `Invoke` podczas tworzenia niestandardowego [oprogramowania pośredniczącego](xref:fundamentals/middleware/write):</span><span class="sxs-lookup"><span data-stu-id="f1b05-369">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="f1b05-370">IApplicationLifetime, interfejs</span><span class="sxs-lookup"><span data-stu-id="f1b05-370">IApplicationLifetime interface</span></span>

<span data-ttu-id="f1b05-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działanie po uruchomieniu i zamknięciu.</span><span class="sxs-lookup"><span data-stu-id="f1b05-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="f1b05-372">Trzy właściwości interfejsu są tokenami anulowania używanymi do rejestrowania metod `Action`, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="f1b05-372">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="f1b05-373">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="f1b05-373">Cancellation Token</span></span>    | <span data-ttu-id="f1b05-374">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="f1b05-374">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="f1b05-375">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="f1b05-375">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="f1b05-376">Host został w pełni uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="f1b05-376">The host has fully started.</span></span> |
| [<span data-ttu-id="f1b05-377">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="f1b05-377">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="f1b05-378">Host kończy bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="f1b05-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="f1b05-379">Wszystkie żądania powinny być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="f1b05-379">All requests should be processed.</span></span> <span data-ttu-id="f1b05-380">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-380">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="f1b05-381">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="f1b05-381">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="f1b05-382">Host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="f1b05-382">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="f1b05-383">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="f1b05-383">Requests may still be processing.</span></span> <span data-ttu-id="f1b05-384">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="f1b05-384">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="f1b05-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="f1b05-386">Następująca Klasa używa `StopApplication`, aby bezpiecznie zamknąć aplikację w przypadku wywołania metody `Shutdown` klasy:</span><span class="sxs-lookup"><span data-stu-id="f1b05-386">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="f1b05-387">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="f1b05-387">Scope validation</span></span>

<span data-ttu-id="f1b05-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions. ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true`, jeśli środowisko aplikacji jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="f1b05-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="f1b05-389">Gdy `ValidateScopes` jest ustawiona na `true`, domyślny dostawca usług sprawdza, czy:</span><span class="sxs-lookup"><span data-stu-id="f1b05-389">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="f1b05-390">Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.</span><span class="sxs-lookup"><span data-stu-id="f1b05-390">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="f1b05-391">Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="f1b05-391">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="f1b05-392">Główny dostawca usług jest tworzony, gdy wywoływana jest [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) .</span><span class="sxs-lookup"><span data-stu-id="f1b05-392">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="f1b05-393">Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b05-393">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="f1b05-394">Usługi o określonym zakresie są usuwane przez kontener, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="f1b05-394">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="f1b05-395">Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="f1b05-395">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="f1b05-396">Sprawdzanie poprawności zakresów usług przechwytuje te sytuacje, gdy zostanie wywołana `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="f1b05-396">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="f1b05-397">Aby zawsze weryfikować zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) na konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="f1b05-397">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="f1b05-398">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f1b05-398">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
