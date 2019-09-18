---
title: ASP.NET Core hosta sieci Web
author: guardrex
description: Dowiedz się więcej o hoście sieci Web w ASP.NET Core, który jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: d387098662cc832cc0e49b6a1636f0ebcc7308de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081686"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="14bc5-103">ASP.NET Core hosta sieci Web</span><span class="sxs-lookup"><span data-stu-id="14bc5-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="14bc5-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="14bc5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="14bc5-105">ASP.NET Core aplikacje konfigurują i uruchamiają *hosta*.</span><span class="sxs-lookup"><span data-stu-id="14bc5-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="14bc5-106">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="14bc5-107">Host konfiguruje co najmniej serwer i potok przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="14bc5-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="14bc5-108">Na hoście można także skonfigurować rejestrowanie, iniekcję zależności i konfigurację.</span><span class="sxs-lookup"><span data-stu-id="14bc5-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="14bc5-109">W tym artykule omówiono hosta sieci Web, który jest dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="14bc5-109">This article covers the Web Host, which remains available only for backward compatibility.</span></span> <span data-ttu-id="14bc5-110">[Host ogólny](xref:fundamentals/host/generic-host) jest zalecany dla wszystkich typów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-110">The [Generic Host](xref:fundamentals/host/generic-host) is recommended for all app types.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="14bc5-111">W tym artykule omówiono hosta sieci Web, który służy do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="14bc5-111">This article covers the Web Host, which is for hosting web apps.</span></span> <span data-ttu-id="14bc5-112">W przypadku innych rodzajów aplikacji należy użyć [hosta ogólnego](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="14bc5-112">For other kinds of apps, use the [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="14bc5-113">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="14bc5-113">Set up a host</span></span>

<span data-ttu-id="14bc5-114">Utwórz hosta przy użyciu wystąpienia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="14bc5-114">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="14bc5-115">Jest to zwykle wykonywane w punkcie wejścia aplikacji, w `Main` metodzie.</span><span class="sxs-lookup"><span data-stu-id="14bc5-115">This is typically performed in the app's entry point, the `Main` method.</span></span>

<span data-ttu-id="14bc5-116">W szablonach `Main` projektu znajduje się w *program.cs*.</span><span class="sxs-lookup"><span data-stu-id="14bc5-116">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="14bc5-117">Typowe wywołania aplikacji [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="14bc5-117">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="14bc5-118">Kod, który wywołuje `CreateDefaultBuilder` , znajduje się w metodzie o nazwie `CreateWebHostBuilder`, która oddziela ją od kodu `Main` w ramach `Run` tego wywołania w obiekcie konstruktora.</span><span class="sxs-lookup"><span data-stu-id="14bc5-118">The code that calls `CreateDefaultBuilder` is in a method named `CreateWebHostBuilder`, which separates it from the code in `Main` that calls `Run` on the builder object.</span></span> <span data-ttu-id="14bc5-119">Ta separacja jest wymagana, jeśli używasz [narzędzi Entity Framework Core](/ef/core/miscellaneous/cli/).</span><span class="sxs-lookup"><span data-stu-id="14bc5-119">This separation is required if you use [Entity Framework Core tools](/ef/core/miscellaneous/cli/).</span></span> <span data-ttu-id="14bc5-120">Narzędzia oczekują na znalezienie `CreateWebHostBuilder` metody, którą mogą wywołać w czasie projektowania, aby skonfigurować hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-120">The tools expect to find a `CreateWebHostBuilder` method that they can call at design time to configure the host without running the app.</span></span> <span data-ttu-id="14bc5-121">Alternatywą jest zaimplementowanie `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="14bc5-121">An alternative is to implement `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="14bc5-122">Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="14bc5-122">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

<span data-ttu-id="14bc5-123">`CreateDefaultBuilder`wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="14bc5-123">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="14bc5-124">Konfiguruje serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web przy użyciu dostawców konfiguracji hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-124">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="14bc5-125">Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="14bc5-125">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="14bc5-126">Ustawia katalog główny zawartości dla ścieżki zwróconej przez [katalog. GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="14bc5-126">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="14bc5-127">Ładuje [konfigurację hosta](#host-configuration-values) z:</span><span class="sxs-lookup"><span data-stu-id="14bc5-127">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="14bc5-128">Zmienne środowiskowe poprzedzone `ASPNETCORE_` prefiksem (na `ASPNETCORE_ENVIRONMENT`przykład).</span><span class="sxs-lookup"><span data-stu-id="14bc5-128">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="14bc5-129">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-129">Command-line arguments.</span></span>
* <span data-ttu-id="14bc5-130">Ładuje konfigurację aplikacji w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="14bc5-130">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="14bc5-131">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="14bc5-131">*appsettings.json*.</span></span>
  * <span data-ttu-id="14bc5-132">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="14bc5-132">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="14bc5-133">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana `Development` w środowisku przy użyciu zestawu wpisów.</span><span class="sxs-lookup"><span data-stu-id="14bc5-133">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="14bc5-134">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="14bc5-134">Environment variables.</span></span>
  * <span data-ttu-id="14bc5-135">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-135">Command-line arguments.</span></span>
* <span data-ttu-id="14bc5-136">Konfiguruje [Rejestrowanie](xref:fundamentals/logging/index) dla konsoli i danych wyjściowych debugowania.</span><span class="sxs-lookup"><span data-stu-id="14bc5-136">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="14bc5-137">Rejestrowanie obejmuje reguły [filtrowania dzienników](xref:fundamentals/logging/index#log-filtering) określone w sekcji konfiguracja rejestrowania w pliku *appSettings. JSON* lub *appSettings. { Environment} plik JSON* .</span><span class="sxs-lookup"><span data-stu-id="14bc5-137">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="14bc5-138">Po uruchomieniu usług IIS za pomocą [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module)program `CreateDefaultBuilder` włącza [integrację usług IIS](xref:host-and-deploy/iis/index), która konfiguruje podstawowy adres i port aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-138">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="14bc5-139">Integracja usług IIS konfiguruje również aplikację do [przechwytywania błędów uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="14bc5-139">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="14bc5-140">Aby poznać domyślne opcje usług IIS, <xref:host-and-deploy/iis/index#iis-options>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="14bc5-140">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="14bc5-141">Ustawia [ServiceProviderOptions. ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) na `true` , jeśli środowisko aplikacji jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="14bc5-141">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="14bc5-142">Aby uzyskać więcej informacji, zobacz [Walidacja zakresu](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="14bc5-142">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="14bc5-143">`CreateDefaultBuilder` Konfiguracja zdefiniowana przez można zastąpić i rozszerzyć przez [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)i inne metody i metody rozszerzające [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="14bc5-143">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="14bc5-144">Poniżej przedstawiono kilka przykładów:</span><span class="sxs-lookup"><span data-stu-id="14bc5-144">A few examples follow:</span></span>

* <span data-ttu-id="14bc5-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) służy do określania dodatkowych `IConfiguration` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-145">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="14bc5-146">Następujące `ConfigureAppConfiguration` wywołanie dodaje delegata w celu uwzględnienia konfiguracji aplikacji w pliku *appSettings. XML* .</span><span class="sxs-lookup"><span data-stu-id="14bc5-146">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="14bc5-147">`ConfigureAppConfiguration`może być wywoływana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="14bc5-147">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="14bc5-148">Należy zauważyć, że ta konfiguracja nie ma zastosowania do hosta (na przykład adresów URL serwera lub środowiska).</span><span class="sxs-lookup"><span data-stu-id="14bc5-148">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="14bc5-149">Zapoznaj się z sekcją [wartości konfiguracji hosta](#host-configuration-values) .</span><span class="sxs-lookup"><span data-stu-id="14bc5-149">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="14bc5-150">Następujące `ConfigureLogging` wywołanie dodaje delegata w celu skonfigurowania minimalnego poziomu rejestrowania ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) na wartość [LogLevel. Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="14bc5-150">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="14bc5-151">To ustawienie zastępuje ustawienia w *appSettings. Development. JSON* (`LogLevel.Debug`) i *appSettings. Production. JSON* (`LogLevel.Error`) skonfigurowany `CreateDefaultBuilder`przez.</span><span class="sxs-lookup"><span data-stu-id="14bc5-151">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="14bc5-152">`ConfigureLogging`może być wywoływana wiele razy.</span><span class="sxs-lookup"><span data-stu-id="14bc5-152">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="14bc5-153">Następujące wywołanie `ConfigureKestrel` przesłania domyślne [limity. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30 000 000 bajtów ustanowionych podczas konfigurowania Kestrel przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-153">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="14bc5-154">Następujące wywołanie [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) zastępuje domyślne [limity. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30 000 000 bajtów ustanowionych podczas konfigurowania Kestrel przez `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-154">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="14bc5-155">*Katalog główny zawartości* określa, gdzie host wyszukuje pliki zawartości, takie jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="14bc5-155">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="14bc5-156">Gdy aplikacja jest uruchamiana z folderu głównego projektu, folder główny projektu jest używany jako katalog główny zawartości.</span><span class="sxs-lookup"><span data-stu-id="14bc5-156">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="14bc5-157">Jest to ustawienie domyślne używane w programie [Visual Studio](https://visualstudio.microsoft.com) i w [nowych szablonach dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="14bc5-157">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="14bc5-158">Aby uzyskać więcej informacji na temat konfiguracji aplikacji <xref:fundamentals/configuration/index>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="14bc5-158">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="14bc5-159">Alternatywą dla użycia metody statycznej `CreateDefaultBuilder` jest utworzenie hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) jest obsługiwane podejście ASP.NET Core 2. x.</span><span class="sxs-lookup"><span data-stu-id="14bc5-159">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span>

<span data-ttu-id="14bc5-160">Podczas konfigurowania hosta można dostarczyć metody [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) .</span><span class="sxs-lookup"><span data-stu-id="14bc5-160">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) methods can be provided.</span></span> <span data-ttu-id="14bc5-161">Jeśli klasa jest określona, musi `Configure` definiować metodę. `Startup`</span><span class="sxs-lookup"><span data-stu-id="14bc5-161">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="14bc5-162">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="14bc5-162">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="14bc5-163">Wiele wywołań do `ConfigureServices` dołączenia do siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="14bc5-163">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="14bc5-164">Wiele wywołań `Configure` lub `UseStartup` Zastąppoprzednieustawienia`WebHostBuilder` .</span><span class="sxs-lookup"><span data-stu-id="14bc5-164">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="14bc5-165">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="14bc5-165">Host configuration values</span></span>

<span data-ttu-id="14bc5-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) opiera się na następujących podejściach do ustawiania wartości konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="14bc5-166">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="14bc5-167">Konfiguracja konstruktora hostów, która zawiera zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="14bc5-167">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="14bc5-168">Na przykład `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="14bc5-168">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="14bc5-169">Rozszerzenia, takie jak [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) i [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (zobacz sekcję [Konfiguracja zastąpień](#override-configuration) ).</span><span class="sxs-lookup"><span data-stu-id="14bc5-169">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="14bc5-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="14bc5-170">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="14bc5-171">Podczas ustawiania wartości za pomocą `UseSetting`, wartość jest ustawiana jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="14bc5-171">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="14bc5-172">Host używa dowolnej opcji ustawia wartość Last.</span><span class="sxs-lookup"><span data-stu-id="14bc5-172">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="14bc5-173">Aby uzyskać więcej informacji, zobacz [Przesłoń konfigurację](#override-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-173">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="14bc5-174">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="14bc5-174">Application Key (Name)</span></span>

<span data-ttu-id="14bc5-175">Właściwość [IHostingEnvironment. ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) jest ustawiana automatycznie, gdy [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) lub [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) jest wywoływana podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="14bc5-175">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="14bc5-176">Wartość jest ustawiana na nazwę zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-176">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="14bc5-177">Aby jawnie ustawić wartość, użyj [WebHostDefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span><span class="sxs-lookup"><span data-stu-id="14bc5-177">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="14bc5-178">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="14bc5-178">**Key**: applicationName</span></span>  
<span data-ttu-id="14bc5-179">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-179">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-180">**Wartość domyślna**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-180">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="14bc5-181">**Ustaw przy użyciu**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="14bc5-181">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="14bc5-182">**Zmienna środowiskowa**:`ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="14bc5-182">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="14bc5-183">Błędy uruchamiania przechwytywania</span><span class="sxs-lookup"><span data-stu-id="14bc5-183">Capture Startup Errors</span></span>

<span data-ttu-id="14bc5-184">To ustawienie steruje przechwytywaniem błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="14bc5-184">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="14bc5-185">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="14bc5-185">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="14bc5-186">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="14bc5-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="14bc5-187">**Wartość domyślna**: Wartość domyślna to, `true` chybażeaplikacjabędziedziałaćzKestrelzausługamiIIS,gdziedomyślniejestto.`false`</span><span class="sxs-lookup"><span data-stu-id="14bc5-187">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="14bc5-188">**Ustaw przy użyciu**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="14bc5-188">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="14bc5-189">**Zmienna środowiskowa**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="14bc5-189">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="14bc5-190">Gdy `false`, błędy podczas uruchamiania, kończy się hostem.</span><span class="sxs-lookup"><span data-stu-id="14bc5-190">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="14bc5-191">Gdy `true`Host przechwytuje wyjątki podczas uruchamiania, a następnie próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="14bc5-191">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="14bc5-192">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="14bc5-192">Content Root</span></span>

<span data-ttu-id="14bc5-193">To ustawienie określa, gdzie ASP.NET Core rozpoczyna wyszukiwanie plików zawartości, takich jak widoki MVC.</span><span class="sxs-lookup"><span data-stu-id="14bc5-193">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="14bc5-194">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="14bc5-194">**Key**: contentRoot</span></span>  
<span data-ttu-id="14bc5-195">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-195">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-196">**Wartość domyślna**: Domyślnie znajduje się w folderze, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-196">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="14bc5-197">**Ustaw przy użyciu**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="14bc5-197">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="14bc5-198">**Zmienna środowiskowa**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="14bc5-198">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="14bc5-199">Katalog główny zawartości jest również używany jako ścieżka podstawowa dla [Ustawienia katalogu głównego sieci Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="14bc5-199">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="14bc5-200">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="14bc5-200">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="14bc5-201">Szczegóły błędów</span><span class="sxs-lookup"><span data-stu-id="14bc5-201">Detailed Errors</span></span>

<span data-ttu-id="14bc5-202">Określa, czy mają być przechwytywane szczegółowe błędy.</span><span class="sxs-lookup"><span data-stu-id="14bc5-202">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="14bc5-203">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="14bc5-203">**Key**: detailedErrors</span></span>  
<span data-ttu-id="14bc5-204">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="14bc5-204">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="14bc5-205">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="14bc5-205">**Default**: false</span></span>  
<span data-ttu-id="14bc5-206">**Ustaw przy użyciu**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="14bc5-206">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="14bc5-207">**Zmienna środowiskowa**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="14bc5-207">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="14bc5-208">Po włączeniu (lub gdy <a href="#environment">środowisko</a> jest ustawione na `Development`), aplikacja przechwytuje szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="14bc5-208">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="14bc5-209">Środowisko</span><span class="sxs-lookup"><span data-stu-id="14bc5-209">Environment</span></span>

<span data-ttu-id="14bc5-210">Ustawia środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-210">Sets the app's environment.</span></span>

<span data-ttu-id="14bc5-211">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="14bc5-211">**Key**: environment</span></span>  
<span data-ttu-id="14bc5-212">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-212">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-213">**Wartość domyślna**: Narzędzi</span><span class="sxs-lookup"><span data-stu-id="14bc5-213">**Default**: Production</span></span>  
<span data-ttu-id="14bc5-214">**Ustaw przy użyciu**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="14bc5-214">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="14bc5-215">**Zmienna środowiskowa**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="14bc5-215">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="14bc5-216">Dla środowiska można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="14bc5-216">The environment can be set to any value.</span></span> <span data-ttu-id="14bc5-217">Wartości zdefiniowane przez platformę `Development`obejmują `Staging`,, `Production`i.</span><span class="sxs-lookup"><span data-stu-id="14bc5-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="14bc5-218">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="14bc5-218">Values aren't case sensitive.</span></span> <span data-ttu-id="14bc5-219">Domyślnie *środowisko* jest odczytywane ze `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="14bc5-219">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="14bc5-220">W przypadku korzystania z [programu Visual Studio](https://visualstudio.microsoft.com)zmienne środowiskowe można ustawić w pliku *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="14bc5-220">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="14bc5-221">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="14bc5-221">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="14bc5-222">Hostowanie zestawów startowych</span><span class="sxs-lookup"><span data-stu-id="14bc5-222">Hosting Startup Assemblies</span></span>

<span data-ttu-id="14bc5-223">Ustawia zestawy uruchomieniowe obsługujące aplikację.</span><span class="sxs-lookup"><span data-stu-id="14bc5-223">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="14bc5-224">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="14bc5-224">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="14bc5-225">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-225">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-226">**Wartość domyślna**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="14bc5-226">**Default**: Empty string</span></span>  
<span data-ttu-id="14bc5-227">**Ustaw przy użyciu**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="14bc5-227">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="14bc5-228">**Zmienna środowiskowa**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="14bc5-228">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="14bc5-229">Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="14bc5-229">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="14bc5-230">Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-230">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="14bc5-231">W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="14bc5-231">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="14bc5-232">Port HTTPS</span><span class="sxs-lookup"><span data-stu-id="14bc5-232">HTTPS Port</span></span>

<span data-ttu-id="14bc5-233">Ustaw port przekierowania HTTPS.</span><span class="sxs-lookup"><span data-stu-id="14bc5-233">Set the HTTPS redirect port.</span></span> <span data-ttu-id="14bc5-234">Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="14bc5-234">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="14bc5-235">**Key**: https_port **Type**:
**wartość domyślna**: Nie ustawiono wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="14bc5-235">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="14bc5-236">**Ustaw przy użyciu**: `UseSetting`
**Zmienna środowiskowa**:`ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="14bc5-236">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="14bc5-237">Hosting — wykluczanie zestawów</span><span class="sxs-lookup"><span data-stu-id="14bc5-237">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="14bc5-238">Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="14bc5-238">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="14bc5-239">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="14bc5-239">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="14bc5-240">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-240">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-241">**Wartość domyślna**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="14bc5-241">**Default**: Empty string</span></span>  
<span data-ttu-id="14bc5-242">**Ustaw przy użyciu**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="14bc5-242">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="14bc5-243">**Zmienna środowiskowa**:`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="14bc5-243">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="14bc5-244">Preferuj adresy URL hostingu</span><span class="sxs-lookup"><span data-stu-id="14bc5-244">Prefer Hosting URLs</span></span>

<span data-ttu-id="14bc5-245">Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy `WebHostBuilder` użyciu zamiast tych skonfigurowanych `IServer` przy użyciu implementacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-245">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="14bc5-246">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="14bc5-246">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="14bc5-247">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="14bc5-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="14bc5-248">Wartość **Domyślna**: prawda</span><span class="sxs-lookup"><span data-stu-id="14bc5-248">**Default**: true</span></span>  
<span data-ttu-id="14bc5-249">**Ustaw przy użyciu**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="14bc5-249">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="14bc5-250">**Zmienna środowiskowa**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="14bc5-250">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="14bc5-251">Zapobiegaj uruchamianiu hostingu</span><span class="sxs-lookup"><span data-stu-id="14bc5-251">Prevent Hosting Startup</span></span>

<span data-ttu-id="14bc5-252">Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-252">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="14bc5-253">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="14bc5-253">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="14bc5-254">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="14bc5-254">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="14bc5-255">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="14bc5-255">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="14bc5-256">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="14bc5-256">**Default**: false</span></span>  
<span data-ttu-id="14bc5-257">**Ustaw przy użyciu**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="14bc5-257">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="14bc5-258">**Zmienna środowiskowa**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="14bc5-258">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="14bc5-259">Adresy URL serwera</span><span class="sxs-lookup"><span data-stu-id="14bc5-259">Server URLs</span></span>

<span data-ttu-id="14bc5-260">Wskazuje adresy IP lub adresy hosta z portami i protokołami, na których serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="14bc5-260">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="14bc5-261">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="14bc5-261">**Key**: urls</span></span>  
<span data-ttu-id="14bc5-262">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-262">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-263">**Wartość domyślna**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="14bc5-263">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="14bc5-264">**Ustaw przy użyciu**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="14bc5-264">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="14bc5-265">**Zmienna środowiskowa**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="14bc5-265">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="14bc5-266">Ustaw na rozdzieloną średnikami (;) Lista prefiksów adresów URL, do których serwer powinien reagować.</span><span class="sxs-lookup"><span data-stu-id="14bc5-266">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="14bc5-267">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="14bc5-267">For example, `http://localhost:123`.</span></span> <span data-ttu-id="14bc5-268">Użyj "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu ( `http://*:5000`na przykład).</span><span class="sxs-lookup"><span data-stu-id="14bc5-268">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="14bc5-269">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="14bc5-269">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="14bc5-270">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="14bc5-270">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="14bc5-271">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="14bc5-271">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="14bc5-272">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="14bc5-272">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="14bc5-273">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="14bc5-273">Shutdown Timeout</span></span>

<span data-ttu-id="14bc5-274">Określa czas oczekiwania na zamknięcie hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="14bc5-274">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="14bc5-275">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="14bc5-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="14bc5-276">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="14bc5-276">**Type**: *int*</span></span>  
<span data-ttu-id="14bc5-277">**Wartość domyślna**: 5</span><span class="sxs-lookup"><span data-stu-id="14bc5-277">**Default**: 5</span></span>  
<span data-ttu-id="14bc5-278">**Ustaw przy użyciu**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="14bc5-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="14bc5-279">**Zmienna środowiskowa**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="14bc5-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="14bc5-280">Chociaż klucz akceptuje *int* z `UseSetting` `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`(na przykład), Metoda rozszerzenia [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) przyjmuje wartość [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="14bc5-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="14bc5-281">Podczas okresu przekroczenia limitu czasu hostowanie:</span><span class="sxs-lookup"><span data-stu-id="14bc5-281">During the timeout period, hosting:</span></span>

* <span data-ttu-id="14bc5-282">Wyzwala [IApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="14bc5-282">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="14bc5-283">Próbuje zatrzymać usługi hostowane, rejestrując błędy dla usług, których nie można zatrzymać.</span><span class="sxs-lookup"><span data-stu-id="14bc5-283">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="14bc5-284">Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-284">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="14bc5-285">Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="14bc5-285">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="14bc5-286">Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="14bc5-286">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="14bc5-287">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="14bc5-287">Startup Assembly</span></span>

<span data-ttu-id="14bc5-288">Określa zestaw, który ma być wyszukiwany dla `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="14bc5-288">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="14bc5-289">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="14bc5-289">**Key**: startupAssembly</span></span>  
<span data-ttu-id="14bc5-290">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-290">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-291">**Wartość domyślna**: Zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="14bc5-291">**Default**: The app's assembly</span></span>  
<span data-ttu-id="14bc5-292">**Ustaw przy użyciu**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="14bc5-292">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="14bc5-293">**Zmienna środowiskowa**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="14bc5-293">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="14bc5-294">Można odwołać się do`string`zestawu według nazwy (`TStartup`) lub typu ().</span><span class="sxs-lookup"><span data-stu-id="14bc5-294">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="14bc5-295">Jeśli wywoływana `UseStartup` jest wiele metod, pierwszeństwo ma Ostatnia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-295">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="14bc5-296">Katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="14bc5-296">Web Root</span></span>

<span data-ttu-id="14bc5-297">Ustawia ścieżkę względną do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-297">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="14bc5-298">**Klucz**: Webroot</span><span class="sxs-lookup"><span data-stu-id="14bc5-298">**Key**: webroot</span></span>  
<span data-ttu-id="14bc5-299">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="14bc5-299">**Type**: *string*</span></span>  
<span data-ttu-id="14bc5-300">**Wartość domyślna**: Jeśli nie zostanie określony, wartością domyślną jest "(katalog zawartości)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="14bc5-300">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="14bc5-301">Jeśli ścieżka nie istnieje, zostanie użyty dostawca plików No-op.</span><span class="sxs-lookup"><span data-stu-id="14bc5-301">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="14bc5-302">**Ustaw przy użyciu**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="14bc5-302">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="14bc5-303">**Zmienna środowiskowa**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="14bc5-303">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="14bc5-304">Zastąp konfigurację</span><span class="sxs-lookup"><span data-stu-id="14bc5-304">Override configuration</span></span>

<span data-ttu-id="14bc5-305">Skonfiguruj hosta sieci Web za pomocą [konfiguracji](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="14bc5-305">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="14bc5-306">W poniższym przykładzie Konfiguracja hosta jest opcjonalnie określona w pliku *HostSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="14bc5-306">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="14bc5-307">Wszystkie konfiguracje załadowane z pliku *HostSettings. JSON* mogą zostać zastąpione przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-307">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="14bc5-308">Utworzona konfiguracja (w programie `config`) służy do konfigurowania hosta z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="14bc5-308">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="14bc5-309">`IWebHostBuilder`Konfiguracja jest dodawana do konfiguracji aplikacji, ale&mdash; `ConfigureAppConfiguration` nie ma ona wpływu `IWebHostBuilder` na konfigurację.</span><span class="sxs-lookup"><span data-stu-id="14bc5-309">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="14bc5-310">Zastępowanie konfiguracji podanej przy `UseUrls` użyciu pliku *HostSettings. JSON* należy najpierw skonfigurować argument wiersza polecenia Second:</span><span class="sxs-lookup"><span data-stu-id="14bc5-310">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="14bc5-311">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="14bc5-311">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="14bc5-312">Metoda rozszerzenia [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) nie jest obecnie w stanie analizowania sekcji konfiguracyjnej zwróconej `GetSection` przez `.UseConfiguration(Configuration.GetSection("section"))`(na przykład.</span><span class="sxs-lookup"><span data-stu-id="14bc5-312">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="14bc5-313">Metoda filtruje klucze konfiguracji do żądanej sekcji `section:urls`, ale pozostawia nazwę sekcji w kluczach (na przykład, `section:environment`). `GetSection`</span><span class="sxs-lookup"><span data-stu-id="14bc5-313">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="14bc5-314">Metoda oczekuje, że klucze są zgodne z `WebHostBuilder` `urls`kluczami (na przykład, `environment`). `UseConfiguration`</span><span class="sxs-lookup"><span data-stu-id="14bc5-314">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="14bc5-315">Obecność nazwy sekcji w kluczach uniemożliwia wartości sekcji od skonfigurowania hosta.</span><span class="sxs-lookup"><span data-stu-id="14bc5-315">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="14bc5-316">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-316">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="14bc5-317">Aby uzyskać więcej informacji i obejść, zobacz [przekazywanie sekcji konfiguracji do WebHostBuilder. UseConfiguration używa pełnych kluczy](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="14bc5-317">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="14bc5-318">`UseConfiguration`tylko kopiuje klucze z dostarczonej `IConfiguration` do konfiguracji konstruktora hostów.</span><span class="sxs-lookup"><span data-stu-id="14bc5-318">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="14bc5-319">W związku z `reloadOnChange: true` tym nie ma żadnego wpływu na ustawienia plików JSON, ini i XML.</span><span class="sxs-lookup"><span data-stu-id="14bc5-319">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="14bc5-320">Aby określić hosta na określonym adresie URL, wymagana wartość może zostać przeniesiona z wiersza polecenia podczas wykonywania [przebiegu dotnet](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="14bc5-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="14bc5-321">Argument wiersza polecenia zastępuje `urls` wartość z pliku *HostSettings. JSON* , a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="14bc5-321">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="14bc5-322">Zarządzanie hostem</span><span class="sxs-lookup"><span data-stu-id="14bc5-322">Manage the host</span></span>

<span data-ttu-id="14bc5-323">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="14bc5-323">**Run**</span></span>

<span data-ttu-id="14bc5-324">`Run` Metoda uruchamia aplikację sieci Web i blokuje wątek wywołujący do momentu wyłączenia hosta:</span><span class="sxs-lookup"><span data-stu-id="14bc5-324">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="14bc5-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="14bc5-325">**Start**</span></span>

<span data-ttu-id="14bc5-326">Uruchom hosta w sposób nieblokowany przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="14bc5-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="14bc5-327">Jeśli lista adresów URL jest przenoszona do `Start` metody, nasłuchuje na określonych adresach URL:</span><span class="sxs-lookup"><span data-stu-id="14bc5-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="14bc5-328">Aplikacja może inicjować i uruchamiać nowy host przy użyciu wstępnie skonfigurowanych ustawień domyślnych `CreateDefaultBuilder` przy użyciu statycznej metody wygodnej.</span><span class="sxs-lookup"><span data-stu-id="14bc5-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="14bc5-329">Metody te służą do uruchamiania serwera bez danych wyjściowych konsoli i [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekać na przerwanie (Ctrl-C/SIGINT lub SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="14bc5-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="14bc5-330">**Rozpocznij (aplikacja RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="14bc5-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="14bc5-331">Zacznij od `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="14bc5-332">Wykonaj żądanie w przeglądarce, aby `http://localhost:5000` otrzymać odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="14bc5-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="14bc5-333">`WaitForShutdown`bloki do momentu wystawienia przerwy (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="14bc5-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="14bc5-334">Aplikacja wyświetla `Console.WriteLine` komunikat i czeka na zamknięcie naciśnięcia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="14bc5-335">**Rozpocznij (adres URL ciągu, aplikacja RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="14bc5-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="14bc5-336">Zacznij od adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="14bc5-337">Daje ten sam wynik jako **Start (RequestDelegate App)** , z wyjątkiem tego, że aplikacja `http://localhost:8080`reaguje na.</span><span class="sxs-lookup"><span data-stu-id="14bc5-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="14bc5-338">**Start (Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="14bc5-338">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="14bc5-339">Użyj wystąpienia programu `IRouteBuilder` ([Microsoft. AspNetCore. Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)), aby użyć oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="14bc5-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="14bc5-340">Użyj poniższego żądania przeglądarki z przykładem:</span><span class="sxs-lookup"><span data-stu-id="14bc5-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="14bc5-341">Request</span><span class="sxs-lookup"><span data-stu-id="14bc5-341">Request</span></span>                                    | <span data-ttu-id="14bc5-342">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="14bc5-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="14bc5-343">Witaj, Martin!</span><span class="sxs-lookup"><span data-stu-id="14bc5-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="14bc5-344">Buenos Dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="14bc5-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="14bc5-345">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="14bc5-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="14bc5-346">Zgłasza wyjątek z ciągiem "zapomniano"</span><span class="sxs-lookup"><span data-stu-id="14bc5-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="14bc5-347">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="14bc5-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="14bc5-348">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="14bc5-348">Hello World!</span></span>                             |

<span data-ttu-id="14bc5-349">`WaitForShutdown`bloki do momentu wystawienia przerwy (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="14bc5-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="14bc5-350">Aplikacja wyświetla `Console.WriteLine` komunikat i czeka na zamknięcie naciśnięcia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="14bc5-351">**Rozpocznij (ciąg URL: Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="14bc5-351">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="14bc5-352">Użyj adresu URL i wystąpienia `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="14bc5-353">Daje ten sam wynik jako **początkowy (Akcja&lt;IRouteBuilder&gt; routeBuilder)** , z wyjątkiem tego, że aplikacja `http://localhost:8080`reaguje na.</span><span class="sxs-lookup"><span data-stu-id="14bc5-353">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="14bc5-354">**StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="14bc5-354">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="14bc5-355">Podaj delegata, aby skonfigurować `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="14bc5-356">Wykonaj żądanie w przeglądarce, aby `http://localhost:5000` otrzymać odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="14bc5-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="14bc5-357">`WaitForShutdown`bloki do momentu wystawienia przerwy (Ctrl-C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="14bc5-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="14bc5-358">Aplikacja wyświetla `Console.WriteLine` komunikat i czeka na zamknięcie naciśnięcia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="14bc5-359">**StartWith (adres URL ciągu,&lt;Akcja&gt; IApplicationBuilder aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="14bc5-359">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="14bc5-360">Podaj adres URL i delegata, aby skonfigurować `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="14bc5-361">Daje ten sam wynik jako **StartWith (Akcja&lt;IApplicationBuilder&gt; aplikacji)** , z wyjątkiem tego, że aplikacja `http://localhost:8080`reaguje na.</span><span class="sxs-lookup"><span data-stu-id="14bc5-361">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="14bc5-362">IHostingEnvironment, interfejs</span><span class="sxs-lookup"><span data-stu-id="14bc5-362">IHostingEnvironment interface</span></span>

<span data-ttu-id="14bc5-363">[Interfejs IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje o środowisku hostingu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="14bc5-363">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="14bc5-364">Użyj [iniekcji konstruktorów](xref:fundamentals/dependency-injection) , `IHostingEnvironment` Aby uzyskać w celu użycia jej właściwości i metod rozszerzających:</span><span class="sxs-lookup"><span data-stu-id="14bc5-364">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="14bc5-365">[Podejście oparte na Konwencji](xref:fundamentals/environments#environment-based-startup-class-and-methods) może służyć do konfigurowania aplikacji podczas uruchamiania na podstawie środowiska.</span><span class="sxs-lookup"><span data-stu-id="14bc5-365">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="14bc5-366">Alternatywnie można wstrzyknąć `IHostingEnvironment` `Startup` do konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="14bc5-366">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="14bc5-367">`IsDevelopment` Oprócz metody rozszerzenia, `IHostingEnvironment` oferty `IsStaging`, `IsProduction`i metody.`IsEnvironment(string environmentName)`</span><span class="sxs-lookup"><span data-stu-id="14bc5-367">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="14bc5-368">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="14bc5-368">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="14bc5-369">Usługę można również wstrzyknąć bezpośrednio `Configure` do metody w celu skonfigurowania potoku przetwarzania: `IHostingEnvironment`</span><span class="sxs-lookup"><span data-stu-id="14bc5-369">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="14bc5-370">`IHostingEnvironment`można wstrzyknąć do `Invoke` metody podczas tworzenia niestandardowego [oprogramowania pośredniczącego](xref:fundamentals/middleware/write):</span><span class="sxs-lookup"><span data-stu-id="14bc5-370">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="14bc5-371">IApplicationLifetime, interfejs</span><span class="sxs-lookup"><span data-stu-id="14bc5-371">IApplicationLifetime interface</span></span>

<span data-ttu-id="14bc5-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działanie po uruchomieniu i zamknięciu.</span><span class="sxs-lookup"><span data-stu-id="14bc5-372">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="14bc5-373">Trzy właściwości interfejsu są tokenami anulowania używanymi do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="14bc5-373">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="14bc5-374">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="14bc5-374">Cancellation Token</span></span>    | <span data-ttu-id="14bc5-375">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="14bc5-375">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="14bc5-376">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="14bc5-376">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="14bc5-377">Host został w pełni uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="14bc5-377">The host has fully started.</span></span> |
| [<span data-ttu-id="14bc5-378">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="14bc5-378">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="14bc5-379">Host kończy bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="14bc5-379">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="14bc5-380">Wszystkie żądania powinny być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="14bc5-380">All requests should be processed.</span></span> <span data-ttu-id="14bc5-381">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-381">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="14bc5-382">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="14bc5-382">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="14bc5-383">Host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="14bc5-383">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="14bc5-384">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="14bc5-384">Requests may still be processing.</span></span> <span data-ttu-id="14bc5-385">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="14bc5-385">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="14bc5-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-386">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="14bc5-387">Następująca Klasa służy `StopApplication` do bezpiecznego zamykania aplikacji w przypadku wywołania `Shutdown` metody klasy:</span><span class="sxs-lookup"><span data-stu-id="14bc5-387">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="14bc5-388">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="14bc5-388">Scope validation</span></span>

<span data-ttu-id="14bc5-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions. ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) na `true` Jeśli środowisko aplikacji jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="14bc5-389">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="14bc5-390">Gdy `ValidateScopes` jest ustawiona na `true`, domyślny dostawca usług sprawdza, czy:</span><span class="sxs-lookup"><span data-stu-id="14bc5-390">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="14bc5-391">Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.</span><span class="sxs-lookup"><span data-stu-id="14bc5-391">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="14bc5-392">Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="14bc5-392">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="14bc5-393">Główny dostawca usług jest tworzony, gdy wywoływana jest [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) .</span><span class="sxs-lookup"><span data-stu-id="14bc5-393">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="14bc5-394">Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14bc5-394">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="14bc5-395">Usługi o określonym zakresie są usuwane przez kontener, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="14bc5-395">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="14bc5-396">Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="14bc5-396">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="14bc5-397">Sprawdzanie poprawności zakresów usług przechwytuje te `BuildServiceProvider` sytuacje, gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="14bc5-397">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="14bc5-398">Aby zawsze weryfikować zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) na konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="14bc5-398">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="14bc5-399">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="14bc5-399">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
