---
title: Host sieci Web platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat hosta sieci web w ASP.NET Core, która jest odpowiedzialna za uruchamianie i okresem istnienia zarządzania aplikacjami.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ced2a766359894b9b83164c12a3ab69aa13c93a0
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="9e026-103">Host sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e026-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="9e026-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9e026-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9e026-105">Aplikacje platformy ASP.NET Core skonfigurować i uruchomić *hosta*.</span><span class="sxs-lookup"><span data-stu-id="9e026-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="9e026-106">Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="9e026-107">Co najmniej hosta konfiguruje serwer i potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="9e026-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="9e026-108">W tym temacie omówiono hosta sieci Web platformy ASP.NET Core ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), co jest przydatne do obsługi aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="9e026-108">This topic covers the ASP.NET Core Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="9e026-109">Pokrycia hosta ogólnego .NET ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), zobacz [rodzajowego hosta](xref:fundamentals/host/generic-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="9e026-109">For coverage of the .NET Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="9e026-110">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="9e026-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9e026-112">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9e026-112">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="9e026-113">Jest to najczęściej wykonywane w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="9e026-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="9e026-114">W szablonach projektu `Main` znajduje się w *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e026-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="9e026-115">Typowe *Program.cs* wywołania [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) do rozpoczęcia konfigurowania hosta:</span><span class="sxs-lookup"><span data-stu-id="9e026-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="9e026-116">`CreateDefaultBuilder` wykonuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="9e026-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="9e026-117">Konfiguruje [Kestrel](xref:fundamentals/servers/kestrel) jako serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="9e026-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server.</span></span> <span data-ttu-id="9e026-118">Aby Kestrel domyślnych opcji, zobacz [Kestrel opcje sekcji Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="9e026-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="9e026-119">Ustawia głównego zawartości w ścieżce zwracanej przez [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="9e026-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="9e026-120">Konfiguracja opcjonalna obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="9e026-120">Loads optional configuration from:</span></span>
  * <span data-ttu-id="9e026-121">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9e026-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="9e026-122">*appSettings. {Środowiska} JSON*.</span><span class="sxs-lookup"><span data-stu-id="9e026-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="9e026-123">[Klucze tajne użytkownika](xref:security/app-secrets) po uruchomieniu aplikacji `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="9e026-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="9e026-124">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="9e026-124">Environment variables.</span></span>
  * <span data-ttu-id="9e026-125">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="9e026-125">Command-line arguments.</span></span>
* <span data-ttu-id="9e026-126">Konfiguruje [rejestrowanie](xref:fundamentals/logging/index) dla danych wyjściowych konsoli i debugowania.</span><span class="sxs-lookup"><span data-stu-id="9e026-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="9e026-127">Dostęp do rejestrów uwzględniających [filtrowania dziennika](xref:fundamentals/logging/index#log-filtering) reguły określone w sekcji konfiguracji rejestrowania *appsettings.json* lub *appsettings. { Środowisko} JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="9e026-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="9e026-128">Podczas uruchamiania za usług IIS, umożliwia [integracji usług IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="9e026-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="9e026-129">Konfiguruje serwer nasłuchuje na przy użyciu portu i ścieżki bazowej [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9e026-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="9e026-130">Moduł tworzy zwrotny serwer proxy między usługami IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9e026-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="9e026-131">Konfiguruje również aplikacji na [przechwytywania błędy uruchamiania](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="9e026-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="9e026-132">Dla opcji domyślnych usług IIS, zobacz [IIS opcje sekcji hosta platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="9e026-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="9e026-133">Ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` Jeśli programowanie jest środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="9e026-134">Aby uzyskać więcej informacji, zobacz [zakres weryfikacji](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="9e026-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="9e026-135">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="9e026-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="9e026-136">Po uruchomieniu aplikacji w folderze głównym projektu folderu głównego projektu jest używany jako główny zawartości.</span><span class="sxs-lookup"><span data-stu-id="9e026-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="9e026-137">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9e026-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="9e026-138">Aby uzyskać więcej informacji o konfiguracji aplikacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9e026-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="9e026-139">Alternatywą wobec przy użyciu statycznych `CreateDefaultBuilder` metody tworzenia hosta z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) jest to obsługiwane podejście z platformy ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="9e026-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="9e026-140">Aby uzyskać więcej informacji zobacz kartę 1.x platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e026-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9e026-142">Utwórz hosta za pomocą wystąpienia [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="9e026-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="9e026-143">Tworzenie hosta jest zwykle wykonywany w punkcie wejścia aplikacji, `Main` metody.</span><span class="sxs-lookup"><span data-stu-id="9e026-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="9e026-144">W szablonach projektu `Main` znajduje się w *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9e026-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="9e026-145">`WebHostBuilder` wymaga [serwera, który implementuje IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="9e026-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="9e026-146">Wbudowane serwery są [Kestrel](xref:fundamentals/servers/kestrel) i [HTTP.sys](xref:fundamentals/servers/httpsys) (przed wydaniem programu ASP.NET 2.0 Core została wywołana HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="9e026-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="9e026-147">W tym przykładzie [— metoda rozszerzenia UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Określa serwer Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9e026-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="9e026-148">*Zawartości głównego* Określa, gdzie hosta wyszukuje pliki zawartości, takich jak pliki widoku MVC.</span><span class="sxs-lookup"><span data-stu-id="9e026-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="9e026-149">Domyślny element zawartości są uzyskiwane dla `UseContentRoot` przez [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="9e026-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="9e026-150">Po uruchomieniu aplikacji w folderze głównym projektu folderu głównego projektu jest używany jako główny zawartości.</span><span class="sxs-lookup"><span data-stu-id="9e026-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="9e026-151">Jest to wartość domyślna używana w [programu Visual Studio](https://www.visualstudio.com/) i [dotnet nowe szablony](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="9e026-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="9e026-152">Do używania serwera IIS jako zwrotny serwer proxy, należy wywołać [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) w ramach tworzenia hosta.</span><span class="sxs-lookup"><span data-stu-id="9e026-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="9e026-153">`UseIISIntegration` nie Konfiguruj *serwera*, takiej jak [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) jest.</span><span class="sxs-lookup"><span data-stu-id="9e026-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="9e026-154">`UseIISIntegration` Konfiguruje serwer nasłuchuje na przy użyciu portu i ścieżki bazowej [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) utworzyć zwrotnego serwera proxy między Kestrel i IIS.</span><span class="sxs-lookup"><span data-stu-id="9e026-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="9e026-155">Aby używać usług IIS z platformy ASP.NET Core `UseKestrel` i `UseIISIntegration` musi być określona.</span><span class="sxs-lookup"><span data-stu-id="9e026-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="9e026-156">`UseIISIntegration` aktywuje tylko podczas uruchamiania usług IIS lub usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9e026-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="9e026-157">Aby uzyskać więcej informacji, zobacz [wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module) i [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9e026-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="9e026-158">Minimalny wdrożenia, który konfiguruje hosta (a aplikacji platformy ASP.NET Core) obejmuje określenie serwera i konfiguracji Potok żądań aplikacji:</span><span class="sxs-lookup"><span data-stu-id="9e026-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="9e026-159">Podczas konfigurowania hosta, [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) można przedstawić metody.</span><span class="sxs-lookup"><span data-stu-id="9e026-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="9e026-160">Jeśli `Startup` jest określona, muszą być zdefiniowane `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="9e026-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="9e026-161">Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji w ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="9e026-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="9e026-162">Wiele wywołań `ConfigureServices` dołączyć do siebie.</span><span class="sxs-lookup"><span data-stu-id="9e026-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="9e026-163">Wiele wywołań `Configure` lub `UseStartup` na `WebHostBuilder` zastąpić poprzednie ustawienia.</span><span class="sxs-lookup"><span data-stu-id="9e026-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="9e026-164">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="9e026-164">Host configuration values</span></span>

<span data-ttu-id="9e026-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) zależy od następujących metod można ustawić wartości konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="9e026-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="9e026-166">Konfiguracja Konstruktora hosta, która zawiera zmienne środowiskowe w formacie `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="9e026-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="9e026-167">Na przykład `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="9e026-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="9e026-168">Jawne metod, takich jak [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span><span class="sxs-lookup"><span data-stu-id="9e026-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="9e026-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) i skojarzony klucz.</span><span class="sxs-lookup"><span data-stu-id="9e026-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="9e026-170">Podczas ustawiania wartości z `UseSetting`, wartość jest ustawiona jako ciąg, niezależnie od typu.</span><span class="sxs-lookup"><span data-stu-id="9e026-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="9e026-171">Host używa jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="9e026-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="9e026-172">Aby uzyskać więcej informacji, zobacz [zastępczą konfigurację](#override-configuration) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="9e026-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="9e026-173">Przechwyć błędy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9e026-173">Capture Startup Errors</span></span>

<span data-ttu-id="9e026-174">To ustawienie określa przechwytywania błędy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9e026-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="9e026-175">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="9e026-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="9e026-176">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="9e026-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9e026-177">**Domyślne**: Domyślnie `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="9e026-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="9e026-178">**Ustawić za pomocą**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="9e026-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="9e026-179">**Zmienna środowiskowa**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="9e026-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="9e026-180">Gdy `false`, błędy podczas wynik uruchomienia na hoście został zakończony.</span><span class="sxs-lookup"><span data-stu-id="9e026-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="9e026-181">Gdy `true`, host przechwytuje wyjątków podczas uruchamiania i podejmie próbę uruchomienia serwera.</span><span class="sxs-lookup"><span data-stu-id="9e026-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="9e026-184">Główny zawartości</span><span class="sxs-lookup"><span data-stu-id="9e026-184">Content Root</span></span>

<span data-ttu-id="9e026-185">To ustawienie określa, gdzie platformy ASP.NET Core rozpocznie się wyszukiwanie plików zawartości, np. widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="9e026-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="9e026-186">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="9e026-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="9e026-187">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="9e026-187">**Type**: *string*</span></span>  
<span data-ttu-id="9e026-188">**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="9e026-189">**Ustawić za pomocą**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="9e026-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="9e026-190">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="9e026-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="9e026-191">Główny zawartości jest również używany jako podstawowa ścieżka dla [ustawienia sieci Web głównego](#web-root).</span><span class="sxs-lookup"><span data-stu-id="9e026-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="9e026-192">Jeśli ścieżka nie istnieje, host nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="9e026-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-193">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="9e026-195">Błędy szczegółowe</span><span class="sxs-lookup"><span data-stu-id="9e026-195">Detailed Errors</span></span>

<span data-ttu-id="9e026-196">Określa, czy szczegółowe błędy, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="9e026-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="9e026-197">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="9e026-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="9e026-198">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="9e026-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9e026-199">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="9e026-199">**Default**: false</span></span>  
<span data-ttu-id="9e026-200">**Ustawić za pomocą**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9e026-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9e026-201">**Zmienna środowiskowa**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="9e026-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="9e026-202">Po włączeniu (lub gdy <a href="#environment">środowiska</a> ma ustawioną wartość `Development`), aplikacji znajdują się szczegółowe wyjątki.</span><span class="sxs-lookup"><span data-stu-id="9e026-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="9e026-205">Środowisko</span><span class="sxs-lookup"><span data-stu-id="9e026-205">Environment</span></span>

<span data-ttu-id="9e026-206">Ustawia środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-206">Sets the app's environment.</span></span>

<span data-ttu-id="9e026-207">**Klucz**: środowiska</span><span class="sxs-lookup"><span data-stu-id="9e026-207">**Key**: environment</span></span>  
<span data-ttu-id="9e026-208">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="9e026-208">**Type**: *string*</span></span>  
<span data-ttu-id="9e026-209">**Domyślna**: produkcji</span><span class="sxs-lookup"><span data-stu-id="9e026-209">**Default**: Production</span></span>  
<span data-ttu-id="9e026-210">**Ustawić za pomocą**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="9e026-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="9e026-211">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="9e026-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="9e026-212">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="9e026-212">The environment can be set to any value.</span></span> <span data-ttu-id="9e026-213">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="9e026-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="9e026-214">Wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="9e026-214">Values aren't case sensitive.</span></span> <span data-ttu-id="9e026-215">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="9e026-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="9e026-216">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="9e026-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="9e026-217">Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="9e026-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="9e026-220">Zestawy uruchomienia hostingu</span><span class="sxs-lookup"><span data-stu-id="9e026-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="9e026-221">Ustawia hostingu zestawy uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="9e026-222">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="9e026-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="9e026-223">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="9e026-223">**Type**: *string*</span></span>  
<span data-ttu-id="9e026-224">**Domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="9e026-224">**Default**: Empty string</span></span>  
<span data-ttu-id="9e026-225">**Ustawić za pomocą**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9e026-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9e026-226">**Zmienna środowiskowa**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="9e026-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="9e026-227">Ciąg rozdzielany średnikami obsługi zestawów uruchamiania załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9e026-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="9e026-228">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="9e026-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="9e026-229">Mimo że konfiguracja domyślnie przyjmowana jest wartość pustego ciągu, hostingu zestawy uruchamiania zawsze należy uwzględniać zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="9e026-230">Jeśli zostały podane zestawy uruchomienia hostingu, są one dodane do zestawu aplikacji ładowania, gdy aplikacja tworzy jego wspólne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9e026-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-231">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e026-233">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9e026-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="9e026-234">Preferowane jest Hosting adresy URL</span><span class="sxs-lookup"><span data-stu-id="9e026-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="9e026-235">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `WebHostBuilder` zamiast ustawień skonfigurowanych z `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="9e026-236">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="9e026-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="9e026-237">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="9e026-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9e026-238">**Domyślna**: true</span><span class="sxs-lookup"><span data-stu-id="9e026-238">**Default**: true</span></span>  
<span data-ttu-id="9e026-239">**Ustawić za pomocą**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="9e026-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="9e026-240">**Zmienna środowiskowa**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="9e026-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="9e026-241">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="9e026-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-242">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e026-244">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9e026-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="9e026-245">Zapobiegaj Hosting uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9e026-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="9e026-246">Uniemożliwia automatyczne ładowanie hosting zestawy uruchamiania, tym hosting zestawy uruchamiania konfigurowane za pomocą zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="9e026-247">Zobacz [ulepszyć aplikację z zewnętrznego zestawu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="9e026-248">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="9e026-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="9e026-249">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="9e026-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="9e026-250">**Domyślna**: false</span><span class="sxs-lookup"><span data-stu-id="9e026-250">**Default**: false</span></span>  
<span data-ttu-id="9e026-251">**Ustawić za pomocą**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="9e026-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="9e026-252">**Zmienna środowiskowa**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="9e026-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="9e026-253">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="9e026-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e026-256">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9e026-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="9e026-257">Adresy URL serwerów</span><span class="sxs-lookup"><span data-stu-id="9e026-257">Server URLs</span></span>

<span data-ttu-id="9e026-258">Wskazuje adresy IP lub adresy hostów, porty i protokoły, które serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="9e026-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="9e026-259">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="9e026-259">**Key**: urls</span></span>  
<span data-ttu-id="9e026-260">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="9e026-260">**Type**: *string*</span></span>  
<span data-ttu-id="9e026-261">**Domyślna**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="9e026-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="9e026-262">**Ustawić za pomocą**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="9e026-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="9e026-263">**Zmienna środowiskowa**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="9e026-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="9e026-264">Ustaw rozdzielonych średnikami (;) lista adresów URL prefiksy powinny odpowiadać serwera.</span><span class="sxs-lookup"><span data-stu-id="9e026-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="9e026-265">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="9e026-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="9e026-266">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="9e026-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="9e026-267">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9e026-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="9e026-268">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="9e026-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-269">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="9e026-270">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="9e026-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="9e026-271">Aby uzyskać więcej informacji, zobacz [Kestrel implementacja serwera sieci web platformy ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="9e026-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="9e026-273">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="9e026-273">Shutdown Timeout</span></span>

<span data-ttu-id="9e026-274">Określa czas oczekiwania na hosta sieci web zamknąć.</span><span class="sxs-lookup"><span data-stu-id="9e026-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="9e026-275">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="9e026-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="9e026-276">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="9e026-276">**Type**: *int*</span></span>  
<span data-ttu-id="9e026-277">**Domyślna**: 5</span><span class="sxs-lookup"><span data-stu-id="9e026-277">**Default**: 5</span></span>  
<span data-ttu-id="9e026-278">**Ustawić za pomocą**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="9e026-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="9e026-279">**Zmienna środowiskowa**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="9e026-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="9e026-280">Mimo że akceptuje klucz *int* z `UseSetting` (na przykład `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) — metoda rozszerzenia przyjmuje [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="9e026-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="9e026-281">Ta funkcja jest nowa w programie ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="9e026-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="9e026-282">Podczas limit czasu, hostingu:</span><span class="sxs-lookup"><span data-stu-id="9e026-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="9e026-283">Wyzwalacze [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="9e026-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="9e026-284">Podejmuje próby zatrzymania usług hostowanych rejestrowania błędów dla usług, których nie udało się zatrzymać.</span><span class="sxs-lookup"><span data-stu-id="9e026-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="9e026-285">Jeśli okres limitu czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostały zatrzymane podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="9e026-286">Usługi zatrzymać, nawet jeśli ich przetwarzanie nie zostało ukończone.</span><span class="sxs-lookup"><span data-stu-id="9e026-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="9e026-287">Jeśli usługi wymagają więcej czasu, aby zatrzymać, należy zwiększyć limit czasu.</span><span class="sxs-lookup"><span data-stu-id="9e026-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-288">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e026-290">Ta funkcja jest niedostępna w ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9e026-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="9e026-291">Zestaw startowy</span><span class="sxs-lookup"><span data-stu-id="9e026-291">Startup Assembly</span></span>

<span data-ttu-id="9e026-292">Określa zestaw do wyszukania `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="9e026-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="9e026-293">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="9e026-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="9e026-294">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="9e026-294">**Type**: *string*</span></span>  
<span data-ttu-id="9e026-295">**Domyślna**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="9e026-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="9e026-296">**Ustawić za pomocą**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="9e026-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="9e026-297">**Zmienna środowiskowa**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="9e026-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="9e026-298">Zestaw o nazwie (`string`) lub typu (`TStartup`) można odwoływać się.</span><span class="sxs-lookup"><span data-stu-id="9e026-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="9e026-299">Jeśli wiele `UseStartup` metody są nazywane, pierwszeństwo ma ostatnią.</span><span class="sxs-lookup"><span data-stu-id="9e026-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="9e026-302">Główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="9e026-302">Web Root</span></span>

<span data-ttu-id="9e026-303">Ustawia ścieżkę względną do statycznego zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="9e026-304">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="9e026-304">**Key**: webroot</span></span>  
<span data-ttu-id="9e026-305">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="9e026-305">**Type**: *string*</span></span>  
<span data-ttu-id="9e026-306">**Domyślne**: Jeśli nie jest określony, wartością domyślną jest "(Content Root)/wwwroot", jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="9e026-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="9e026-307">Jeśli ścieżka nie istnieje, jest używany dostawca pliku zerowa.</span><span class="sxs-lookup"><span data-stu-id="9e026-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="9e026-308">**Ustawić za pomocą**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="9e026-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="9e026-309">**Zmienna środowiskowa**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="9e026-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="9e026-312">Zastąp konfigurację</span><span class="sxs-lookup"><span data-stu-id="9e026-312">Override configuration</span></span>

<span data-ttu-id="9e026-313">Użyj [konfiguracji](xref:fundamentals/configuration/index) Aby skonfigurować hosta.</span><span class="sxs-lookup"><span data-stu-id="9e026-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="9e026-314">W poniższym przykładzie konfiguracja hosta jest opcjonalnie określić w *hosting.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="9e026-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="9e026-315">Wszelkie konfiguracja została załadowana z *hosting.json* plik może być zastąpiona przez argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="9e026-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="9e026-316">Zbudowany konfiguracji (w `config`) służy do konfigurowania hosta z `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="9e026-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-317">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9e026-318">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="9e026-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="9e026-319">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="9e026-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e026-321">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="9e026-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="9e026-322">Zastępowanie konfiguracji dostarczonych przez `UseUrls` z *hosting.json* config argument pierwszą, wiersza polecenia config drugi:</span><span class="sxs-lookup"><span data-stu-id="9e026-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="9e026-323">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez `GetSection` (na przykład `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="9e026-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="9e026-324">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="9e026-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="9e026-325">`UseConfiguration` Metoda oczekuje klucze do dopasowania `WebHostBuilder` kluczy (na przykład `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="9e026-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="9e026-326">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="9e026-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="9e026-327">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="9e026-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="9e026-328">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="9e026-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="9e026-329">Aby określić hosta, uruchom na określony adres URL, żądaną wartość mogą zostać przekazane w wierszu polecenia z podczas wykonywania [dotnet Uruchom](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="9e026-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="9e026-330">Argument wiersza polecenia zastępuje `urls` wartość z *hosting.json* pliku, a serwer nasłuchuje na porcie 8080:</span><span class="sxs-lookup"><span data-stu-id="9e026-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="9e026-331">Zarządzanie hosta</span><span class="sxs-lookup"><span data-stu-id="9e026-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e026-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e026-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9e026-333">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="9e026-333">**Run**</span></span>

<span data-ttu-id="9e026-334">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący, dopóki nie zostanie zamknięta hosta:</span><span class="sxs-lookup"><span data-stu-id="9e026-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9e026-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="9e026-335">**Start**</span></span>

<span data-ttu-id="9e026-336">Uruchom hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="9e026-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9e026-337">Jeśli listę adresów URL są przekazywane do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="9e026-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="9e026-338">Inicjowanie i uruchomić nowy host, przy użyciu wstępnie skonfigurowane ustawienia domyślne aplikacji `CreateDefaultBuilder` przy użyciu metody statycznej wygody.</span><span class="sxs-lookup"><span data-stu-id="9e026-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="9e026-339">Te metody uruchomienia serwera bez dane wyjściowe konsoli i z [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) poczekaj, aż podział (Ctrl-C/sigint — lub sigterm —):</span><span class="sxs-lookup"><span data-stu-id="9e026-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="9e026-340">**Start (RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="9e026-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="9e026-341">Rozpoczynać `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="9e026-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="9e026-342">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="9e026-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="9e026-343">`WaitForShutdown` bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="9e026-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9e026-344">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="9e026-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9e026-345">**Start (ciąg adresu url, RequestDelegate aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="9e026-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="9e026-346">Uruchom przy użyciu adresu URL i `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="9e026-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="9e026-347">Tworzy wynik, w postaci **Start (aplikacja RequestDelegate)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9e026-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="9e026-348">**Start (akcji&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="9e026-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="9e026-349">Użyj wystąpienia `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) do używania oprogramowania pośredniczącego routingu:</span><span class="sxs-lookup"><span data-stu-id="9e026-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="9e026-350">Użyj następujących żądań przeglądarki z przykładem:</span><span class="sxs-lookup"><span data-stu-id="9e026-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="9e026-351">Żądanie</span><span class="sxs-lookup"><span data-stu-id="9e026-351">Request</span></span>                                    | <span data-ttu-id="9e026-352">Odpowiedź</span><span class="sxs-lookup"><span data-stu-id="9e026-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="9e026-353">Witaj, pole!</span><span class="sxs-lookup"><span data-stu-id="9e026-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="9e026-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="9e026-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="9e026-355">Zgłasza wyjątek z ciągiem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="9e026-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="9e026-356">Zgłasza wyjątek z ciągiem "Uh Niestety!"</span><span class="sxs-lookup"><span data-stu-id="9e026-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="9e026-357">Sante, Jan!</span><span class="sxs-lookup"><span data-stu-id="9e026-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="9e026-358">Cześć ludzie!</span><span class="sxs-lookup"><span data-stu-id="9e026-358">Hello World!</span></span>                             |

<span data-ttu-id="9e026-359">`WaitForShutdown` bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="9e026-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9e026-360">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="9e026-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9e026-361">**Start (ciągu adresu url, Akcja&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="9e026-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="9e026-362">Użyj adresu URL i wystąpienie `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9e026-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="9e026-363">Tworzy wynik, w postaci **Start (akcji&lt;IRouteBuilder&gt; routeBuilder)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9e026-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="9e026-364">**StartWith (akcji&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="9e026-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="9e026-365">Podaj delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9e026-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="9e026-366">Tworzenie żądania w przeglądarce, aby `http://localhost:5000` odpowiedź "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="9e026-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="9e026-367">`WaitForShutdown` bloki aż do wystawienia podział (Ctrl-C/sigint — lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="9e026-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="9e026-368">Aplikacja jest wyświetlana `Console.WriteLine` komunikat i czeka na keypress zakończyć.</span><span class="sxs-lookup"><span data-stu-id="9e026-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="9e026-369">**StartWith (ciągu adresu url, Akcja&lt;IApplicationBuilder&gt; aplikacji)**</span><span class="sxs-lookup"><span data-stu-id="9e026-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="9e026-370">Podaj adres URL i delegat konfigurujący `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9e026-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="9e026-371">Tworzy wynik, w postaci **StartWith (akcji&lt;IApplicationBuilder&gt; aplikacji)**, z wyjątkiem aplikacji odpowiada na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="9e026-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e026-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e026-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e026-373">**Uruchom**</span><span class="sxs-lookup"><span data-stu-id="9e026-373">**Run**</span></span>

<span data-ttu-id="9e026-374">`Run` Metoda uruchamiania aplikacji sieci web i blokuje wątek wywołujący, dopóki nie zostanie zamknięta hosta:</span><span class="sxs-lookup"><span data-stu-id="9e026-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="9e026-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="9e026-375">**Start**</span></span>

<span data-ttu-id="9e026-376">Uruchom hosta w sposób nieblokujące przez wywołanie jego `Start` metody:</span><span class="sxs-lookup"><span data-stu-id="9e026-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="9e026-377">Jeśli listę adresów URL są przekazywane do `Start` metody go nasłuchuje na określone adresy URL:</span><span class="sxs-lookup"><span data-stu-id="9e026-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="9e026-378">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="9e026-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="9e026-379">[Interfejsu IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) zawiera informacje o aplikacji sieci web środowiska macierzystego.</span><span class="sxs-lookup"><span data-stu-id="9e026-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="9e026-380">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="9e026-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="9e026-381">A [podejście oparte na Konwencji](xref:fundamentals/environments#startup-conventions) może służyć do konfigurowania aplikacji podczas uruchamiania, na podstawie środowiska.</span><span class="sxs-lookup"><span data-stu-id="9e026-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="9e026-382">Możesz też wprowadzić `IHostingEnvironment` do `Startup` konstruktora do użycia w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9e026-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="9e026-383">Oprócz `IsDevelopment` — metoda rozszerzenia, `IHostingEnvironment` oferuje `IsStaging`, `IsProduction`, i `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="9e026-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="9e026-384">Zobacz [używać wiele środowisk](xref:fundamentals/environments) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="9e026-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="9e026-385">`IHostingEnvironment` Usługi także mogą zostać dodane bezpośrednio do `Configure` metody do konfigurowania potoku przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="9e026-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="9e026-386">`IHostingEnvironment` mogą zostać dodane do `Invoke` metody podczas tworzenia niestandardowego [oprogramowanie pośredniczące](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="9e026-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="9e026-387">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="9e026-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="9e026-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umożliwia działań po uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="9e026-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="9e026-389">Anulowanie tokenów używany do rejestrowania są trzy właściwości w interfejsie `Action` metod, które definiują zdarzenia uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="9e026-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="9e026-390">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="9e026-390">Cancellation Token</span></span>    | <span data-ttu-id="9e026-391">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="9e026-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="9e026-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="9e026-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="9e026-393">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="9e026-393">The host has fully started.</span></span> |
| [<span data-ttu-id="9e026-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="9e026-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="9e026-395">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="9e026-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="9e026-396">Wszystkie żądania powinna zostać przetworzona.</span><span class="sxs-lookup"><span data-stu-id="9e026-396">All requests should be processed.</span></span> <span data-ttu-id="9e026-397">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="9e026-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="9e026-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="9e026-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="9e026-399">Host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="9e026-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="9e026-400">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="9e026-400">Requests may still be processing.</span></span> <span data-ttu-id="9e026-401">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="9e026-401">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="9e026-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) żąda zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="9e026-403">Następujące klasy używa `StopApplication` można bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="9e026-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="9e026-404">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="9e026-404">Scope validation</span></span>

<span data-ttu-id="9e026-405">W programie ASP.NET Core 2.0 lub nowszej [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ustawia [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) do `true` Jeśli programowanie jest środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="9e026-406">Gdy `ValidateScopes` ma ustawioną wartość `true`, domyślny dostawca usług sprawdza upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="9e026-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="9e026-407">Usługi w zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług głównego.</span><span class="sxs-lookup"><span data-stu-id="9e026-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="9e026-408">Usługi w zakresie nie są bezpośrednio lub pośrednio wprowadzić do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9e026-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="9e026-409">Dostawcy usług głównego jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="9e026-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="9e026-410">Okres istnienia dostawcy usług głównego odpowiada istnienia aplikacji/serwer. Jeśli dostawca rozpoczyna się od aplikacji i został usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e026-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="9e026-411">Usługi w zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="9e026-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="9e026-412">Zakresami usługi jest tworzony w kontenerze katalogu głównego, okres istnienia usługi jest skutecznie podwyższany do pojedynczego wystąpienia ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwera aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="9e026-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="9e026-413">Walidacja zakresów usługi przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="9e026-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="9e026-414">Sprawdzanie poprawności zakresy, w tym w środowisku produkcyjnym, należy skonfigurować [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) z [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) w Konstruktorze hosta:</span><span class="sxs-lookup"><span data-stu-id="9e026-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="9e026-415">Rozwiązywanie problemów z System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="9e026-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="9e026-416">**Dotyczy tylko platformy ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="9e026-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="9e026-417">Host może zostać utworzony przez wstrzykiwanie `IStartup` bezpośrednio do kontenera iniekcji zależności zamiast wywoływania `UseStartup` lub `Configure`:</span><span class="sxs-lookup"><span data-stu-id="9e026-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="9e026-418">Jeśli host jest wbudowana w ten sposób, może wystąpić następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="9e026-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="9e026-419">Dzieje się tak dlatego [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (bieżącego zestawu) jest wymagane do skanowania pod kątem `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="9e026-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="9e026-420">Jeśli aplikacja ręcznie injects `IStartup` do kontenera iniekcji zależności, Dodaj następujące wywołanie do `WebHostBuilder` o podanej nazwie zestawu:</span><span class="sxs-lookup"><span data-stu-id="9e026-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="9e026-421">Możesz też dodać manekina `Configure` do `WebHostBuilder`, który określa `applicationName`(`ApplicationKey`) automatycznie:</span><span class="sxs-lookup"><span data-stu-id="9e026-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="9e026-422">**Uwaga**: jest to tylko wymagane wraz z wydaniem programu ASP.NET 2.0 rdzeni i tylko wtedy gdy aplikacja nie wywołać `UseStartup` lub `Configure`.</span><span class="sxs-lookup"><span data-stu-id="9e026-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="9e026-423">Aby uzyskać więcej informacji, zobacz [anonsów: Microsoft.Extensions.PlatformAbstractions została usunięta (komentarz)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) i [próbki StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="9e026-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e026-424">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9e026-424">Additional resources</span></span>

* [<span data-ttu-id="9e026-425">Hosting w systemie Windows z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="9e026-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="9e026-426">Hosting w systemie Linux z Nginx</span><span class="sxs-lookup"><span data-stu-id="9e026-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="9e026-427">Hosting w systemie Linux z Apache</span><span class="sxs-lookup"><span data-stu-id="9e026-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="9e026-428">Host usługi systemu Windows</span><span class="sxs-lookup"><span data-stu-id="9e026-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
