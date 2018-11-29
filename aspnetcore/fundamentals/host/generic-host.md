---
title: Ogólny hosta platformy .NET
author: guardrex
description: Więcej informacji na temat ogólnych hosta na platformie .NET, który jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 4d435984d8169b558ab026ef8541c90f7a2a96b9
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618158"
---
# <a name="net-generic-host"></a><span data-ttu-id="71d72-103">Ogólny hosta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="71d72-103">.NET Generic Host</span></span>

<span data-ttu-id="71d72-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="71d72-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="71d72-105">Aplikacje platformy .NET core, konfigurowanie i uruchamianie *hosta*.</span><span class="sxs-lookup"><span data-stu-id="71d72-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="71d72-106">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71d72-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="71d72-107">W tym temacie omówiono Host rodzajowego Core ASP.NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>), co jest przydatne do hostowania aplikacji, które nie przetwarzają żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="71d72-107">This topic covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="71d72-108">Pokrycia hosta sieci Web (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), zobacz <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="71d72-108">For coverage of the Web Host (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="71d72-109">Celem ogólnego hosta jest rozdzielenie potoku HTTP z hosta internetowego interfejsu API, umożliwiające szersze gamę scenariuszy hosta.</span><span class="sxs-lookup"><span data-stu-id="71d72-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="71d72-110">Komunikaty, zadania w tle i innych obciążeń innych niż HTTP oparte na korzyść ogólnego hosta z przekrojowe możliwości, takich jak konfiguracja, wstrzykiwanie zależności (DI) i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="71d72-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="71d72-111">Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiednie w scenariuszach hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="71d72-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="71d72-112">W przypadku scenariuszy hostingu w sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="71d72-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="71d72-113">Ogólny Host jest w fazie projektowania w celu zastąpienia hosta sieci Web w przyszłym wydaniu i pełnić rolę hosta podstawowego interfejsu API zarówno w przypadku protokołu HTTP, jak i scenariuszy innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="71d72-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="71d72-114">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71d72-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="71d72-115">Podczas uruchamiania przykładowej aplikacji [programu Visual Studio Code](https://code.visualstudio.com/), użyj *zewnętrznych lub w zintegrowanym terminalu*.</span><span class="sxs-lookup"><span data-stu-id="71d72-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="71d72-116">Nie, uruchom przykład w `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="71d72-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="71d72-117">Aby ustawić konsoli w programie Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="71d72-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="71d72-118">Otwórz *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="71d72-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="71d72-119">W **.NET Core uruchamianie (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu.</span><span class="sxs-lookup"><span data-stu-id="71d72-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="71d72-120">Ustaw wartość na wartość `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="71d72-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="71d72-121">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="71d72-121">Introduction</span></span>

<span data-ttu-id="71d72-122">Biblioteka ogólnego hosta jest dostępna w <xref:Microsoft.Extensions.Hosting> przestrzeni nazw i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="71d72-122">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="71d72-123">`Microsoft.Extensions.Hosting` Pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="71d72-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="71d72-124"><xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="71d72-124"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="71d72-125">Każdy `IHostedService` implementacja jest wykonywany w kolejności od [usługi rejestracji w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="71d72-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="71d72-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana w każdej `IHostedService` podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w kolejności odwrotnej rejestracji, gdy kończy pracę bez problemu zmieniała hosta.</span><span class="sxs-lookup"><span data-stu-id="71d72-126"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each `IHostedService` when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="71d72-127">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="71d72-127">Set up a host</span></span>

<span data-ttu-id="71d72-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> jest to główny składnik, używanego przez aplikacje i biblioteki do zainicjowania, tworzeniu i uruchamianiu hosta:</span><span class="sxs-lookup"><span data-stu-id="71d72-128"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="71d72-129">Opcje</span><span class="sxs-lookup"><span data-stu-id="71d72-129">Options</span></span>

<span data-ttu-id="71d72-130"><xref:Microsoft.Extensions.Hosting.HostOptions> Skonfiguruj opcje <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="71d72-130"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="71d72-131">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="71d72-131">Shutdown timeout</span></span>

<span data-ttu-id="71d72-132"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="71d72-132"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="71d72-133">Wartość domyślna to 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="71d72-133">The default value is five seconds.</span></span>

<span data-ttu-id="71d72-134">Następująca Konfiguracja opcji w `Program.Main` zwiększa domyślna pięć drugi zamykania wartość limitu czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="71d72-134">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="71d72-135">Usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="71d72-135">Default services</span></span>

<span data-ttu-id="71d72-136">Następujące usługi są rejestrowane podczas inicjowania hosta:</span><span class="sxs-lookup"><span data-stu-id="71d72-136">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="71d72-137">[Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="71d72-137">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="71d72-138">[Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="71d72-138">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="71d72-139"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="71d72-139"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="71d72-140"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="71d72-140"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="71d72-141">[Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="71d72-141">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="71d72-142">[Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="71d72-142">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="71d72-143">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="71d72-143">Host configuration</span></span>

<span data-ttu-id="71d72-144">Konfiguracja hosta jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="71d72-144">Host configuration is created by:</span></span>

* <span data-ttu-id="71d72-145">Wywoływanie metody rozszerzenia na <xref:Microsoft.Extensions.Hosting.IHostBuilder> można ustawić [zawartości głównego](#content-root) i [środowiska](#environment).</span><span class="sxs-lookup"><span data-stu-id="71d72-145">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="71d72-146">Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="71d72-146">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="71d72-147">Metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="71d72-147">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="71d72-148">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="71d72-148">Application key (name)</span></span>

<span data-ttu-id="71d72-149">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) właściwość ma wartość z konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="71d72-149">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="71d72-150">Aby jawnie ustawić wartość, użyj [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="71d72-150">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="71d72-151">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="71d72-151">**Key**: applicationName</span></span>  
<span data-ttu-id="71d72-152">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="71d72-152">**Type**: *string*</span></span>  
<span data-ttu-id="71d72-153">**Domyślne**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71d72-153">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="71d72-154">**Można ustawić przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="71d72-154">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="71d72-155">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="71d72-155">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="71d72-156">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="71d72-156">Content root</span></span>

<span data-ttu-id="71d72-157">To ustawienie określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="71d72-157">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="71d72-158">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="71d72-158">**Key**: contentRoot</span></span>  
<span data-ttu-id="71d72-159">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="71d72-159">**Type**: *string*</span></span>  
<span data-ttu-id="71d72-160">**Domyślne**: wartość domyślna to folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71d72-160">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="71d72-161">**Można ustawić przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="71d72-161">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="71d72-162">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="71d72-162">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="71d72-163">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="71d72-163">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="71d72-164">Środowisko</span><span class="sxs-lookup"><span data-stu-id="71d72-164">Environment</span></span>

<span data-ttu-id="71d72-165">Ustawia aplikacji [środowiska](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="71d72-165">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="71d72-166">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="71d72-166">**Key**: environment</span></span>  
<span data-ttu-id="71d72-167">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="71d72-167">**Type**: *string*</span></span>  
<span data-ttu-id="71d72-168">**Domyślne**: produkcji</span><span class="sxs-lookup"><span data-stu-id="71d72-168">**Default**: Production</span></span>  
<span data-ttu-id="71d72-169">**Można ustawić przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="71d72-169">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="71d72-170">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="71d72-170">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="71d72-171">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="71d72-171">The environment can be set to any value.</span></span> <span data-ttu-id="71d72-172">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="71d72-172">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="71d72-173">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="71d72-173">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="71d72-174">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="71d72-174">ConfigureHostConfiguration</span></span>

<span data-ttu-id="71d72-175">`ConfigureHostConfiguration` używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta.</span><span class="sxs-lookup"><span data-stu-id="71d72-175">`ConfigureHostConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="71d72-176">Konfiguracja hosta jest wykorzystywany do inicjacji <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71d72-176">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="71d72-177">`ConfigureHostConfiguration` można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="71d72-177">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="71d72-178">Host używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="71d72-178">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="71d72-179">Konfiguracja hosta są automatycznie przekazywane do konfiguracji aplikacji ([ConfigureAppConfiguration](#configureappconfiguration) i pozostałej części aplikacji).</span><span class="sxs-lookup"><span data-stu-id="71d72-179">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="71d72-180">Brak dostawców są domyślnie dołączone.</span><span class="sxs-lookup"><span data-stu-id="71d72-180">No providers are included by default.</span></span> <span data-ttu-id="71d72-181">Należy jawnie określić niezależnie od dostawcy konfiguracji aplikacji wymaga w `ConfigureHostConfiguration`, w tym:</span><span class="sxs-lookup"><span data-stu-id="71d72-181">You must explicitly specify whatever configuration providers the app requires in `ConfigureHostConfiguration`, including:</span></span>

* <span data-ttu-id="71d72-182">Plik konfiguracji (na przykład z *hostsettings.json* pliku).</span><span class="sxs-lookup"><span data-stu-id="71d72-182">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="71d72-183">Zmienne konfiguracji środowiska.</span><span class="sxs-lookup"><span data-stu-id="71d72-183">Environment variable configuration.</span></span>
* <span data-ttu-id="71d72-184">Konfiguracja argumentu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="71d72-184">Command-line argument configuration.</span></span>
* <span data-ttu-id="71d72-185">Innych dostawców wymaganej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="71d72-185">Any other required configuration providers.</span></span>

<span data-ttu-id="71d72-186">Plik konfiguracji hosta jest włączona, określając ścieżki podstawowej aplikacji za pomocą `SetBasePath` następuje wywołanie jednej z [pliku dostawcy konfiguracji](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="71d72-186">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="71d72-187">Przykładowa aplikacja używa pliku JSON, *hostsettings.json*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> korzystanie z ustawień konfiguracji hosta tego pliku.</span><span class="sxs-lookup"><span data-stu-id="71d72-187">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="71d72-188">Aby dodać [zmiennych konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w Konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="71d72-188">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="71d72-189">`AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="71d72-189">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="71d72-190">Przykładowa aplikacja korzysta z prefiksem `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="71d72-190">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="71d72-191">Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane.</span><span class="sxs-lookup"><span data-stu-id="71d72-191">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="71d72-192">Po skonfigurowaniu hostów przykładową aplikację, wartość zmiennej środowiskowej, aby uzyskać `PREFIX_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.</span><span class="sxs-lookup"><span data-stu-id="71d72-192">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="71d72-193">Podczas programowania, korzystając z [programu Visual Studio](https://www.visualstudio.com/) lub uruchamianie aplikacji za pomocą `dotnet run`, zmienne środowiskowe, może być ustawiona w *Properties/launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="71d72-193">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="71d72-194">W [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *.vscode/launch.json* pliku podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="71d72-194">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="71d72-195">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="71d72-195">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="71d72-196">[Wiersza polecenia konfiguracji](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawany przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="71d72-196">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="71d72-197">Konfiguracja wiersza polecenia zostanie dodany ostatnio pozwalającą na argumenty wiersza polecenia, aby zastąpić konfiguracji udostępnianych przez starszych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="71d72-197">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="71d72-198">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="71d72-198">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="71d72-199">Dodatkowa konfiguracja może być wyposażone w [applicationName](#application-key-name) i [contentRoot](#content-root) kluczy.</span><span class="sxs-lookup"><span data-stu-id="71d72-199">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="71d72-200">Przykład `HostBuilder` konfiguracji przy użyciu `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="71d72-200">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="71d72-201">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="71d72-201">ConfigureAppConfiguration</span></span>

<span data-ttu-id="71d72-202">Konfiguracja aplikacji jest tworzony przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> na <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementacji.</span><span class="sxs-lookup"><span data-stu-id="71d72-202">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="71d72-203">`ConfigureAppConfiguration` używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71d72-203">`ConfigureAppConfiguration` uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="71d72-204">`ConfigureAppConfiguration` można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="71d72-204">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="71d72-205">Aplikacja używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="71d72-205">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="71d72-206">Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="71d72-206">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="71d72-207">Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta, dostarczone przez [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="71d72-207">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="71d72-208">Przykład korzystającą configuration `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="71d72-208">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="71d72-209">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="71d72-209">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="71d72-210">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="71d72-210">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="71d72-211">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="71d72-211">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="71d72-212">Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="71d72-212">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="71d72-213">Przykładowa aplikacja przenosi jego pliki ustawień aplikacji w formacie JSON i *hostsettings.json* następującym `<Content>` elementu:</span><span class="sxs-lookup"><span data-stu-id="71d72-213">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="71d72-214">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="71d72-214">ConfigureServices</span></span>

<span data-ttu-id="71d72-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usług do aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="71d72-215"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="71d72-216">`ConfigureServices` można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="71d72-216">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="71d72-217">Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="71d72-217">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="71d72-218">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="71d72-218">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="71d72-219">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` metodę rozszerzenia, aby dodać usługę do zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu `TimedHostedService`, do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="71d72-219">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="71d72-220">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="71d72-220">ConfigureLogging</span></span>

<span data-ttu-id="71d72-221"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podane <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="71d72-221"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="71d72-222">`ConfigureLogging` może zostać wywołana wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="71d72-222">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="71d72-223">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="71d72-223">UseConsoleLifetime</span></span>

<span data-ttu-id="71d72-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje `Ctrl+C`SIGTERM lub wywołań i /SIGINT <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> można uruchomić procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="71d72-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for `Ctrl+C`/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="71d72-225">`UseConsoleLifetime` Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="71d72-225">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="71d72-226"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> wstępnie jest zarejestrowany jako domyślna implementacja okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="71d72-226"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="71d72-227">Ostatniego okresu istnienia zarejestrowany jest używany.</span><span class="sxs-lookup"><span data-stu-id="71d72-227">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="71d72-228">Konfiguracja kontenera</span><span class="sxs-lookup"><span data-stu-id="71d72-228">Container configuration</span></span>

<span data-ttu-id="71d72-229">Aby zapewnić obsługę podłączania innych kontenerów, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span><span class="sxs-lookup"><span data-stu-id="71d72-229">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>.</span></span> <span data-ttu-id="71d72-230">Zapewnianie fabrykę nie jest częścią DI rejestracja kontenera jest jednak wewnętrzne hosta używany do tworzenia konkretnych kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="71d72-230">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="71d72-231">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę użyty do utworzenia dostawcy usługi app service.</span><span class="sxs-lookup"><span data-stu-id="71d72-231">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="71d72-232">Konfiguracja kontenera niestandardowego jest zarządzana przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> metody.</span><span class="sxs-lookup"><span data-stu-id="71d72-232">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="71d72-233">`ConfigureContainer` udostępnia silnie typizowane środowisko na potrzeby konfigurowania kontenera na podstawie odpowiedniego hosta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="71d72-233">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="71d72-234">`ConfigureContainer` można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="71d72-234">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="71d72-235">Tworzenie kontenera usług dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="71d72-235">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="71d72-236">Podaj fabryki kontenera usług:</span><span class="sxs-lookup"><span data-stu-id="71d72-236">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="71d72-237">Używaj fabryki i konfigurowanie kontenera niestandardowego usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="71d72-237">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="71d72-238">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="71d72-238">Extensibility</span></span>

<span data-ttu-id="71d72-239">Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="71d72-239">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="71d72-240">W poniższym przykładzie pokazano, jak metoda rozszerzenia rozszerza `IHostBuilder` implementację [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przykład przedstawiona w <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="71d72-240">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="71d72-241">Aplikacja ustanawia `UseHostedService` metodę rozszerzenia, aby zarejestrować usługi hostowanej przekazanej `T`:</span><span class="sxs-lookup"><span data-stu-id="71d72-241">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="71d72-242">Zarządzać hosta</span><span class="sxs-lookup"><span data-stu-id="71d72-242">Manage the host</span></span>

<span data-ttu-id="71d72-243"><xref:Microsoft.Extensions.Hosting.IHost> Implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie `IHostedService` implementacji, które są zarejestrowane w usłudze service container.</span><span class="sxs-lookup"><span data-stu-id="71d72-243">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="71d72-244">Uruchom</span><span class="sxs-lookup"><span data-stu-id="71d72-244">Run</span></span>

<span data-ttu-id="71d72-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uruchamia aplikację i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="71d72-245"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="71d72-246">RunAsync</span><span class="sxs-lookup"><span data-stu-id="71d72-246">RunAsync</span></span>

<span data-ttu-id="71d72-247"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uruchamia aplikację i zwraca `Task` który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="71d72-247"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="71d72-248">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="71d72-248">RunConsoleAsync</span></span>

<span data-ttu-id="71d72-249"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Umożliwia obsługę konsoli, tworzy i uruchamia hosta i czeka, aż `Ctrl+C`/SIGINT lub SIGTERM, aby zamknąć.</span><span class="sxs-lookup"><span data-stu-id="71d72-249"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="71d72-250">Początkowy i StopAsync</span><span class="sxs-lookup"><span data-stu-id="71d72-250">Start and StopAsync</span></span>

<span data-ttu-id="71d72-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="71d72-251"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="71d72-252"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="71d72-252"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="71d72-253">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="71d72-253">StartAsync and StopAsync</span></span>

<span data-ttu-id="71d72-254"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> Uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="71d72-254"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="71d72-255"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> Zatrzymuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="71d72-255"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="71d72-256">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="71d72-256">WaitForShutdown</span></span>

<span data-ttu-id="71d72-257"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalany za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (nasłuchuje `Ctrl+C`/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="71d72-257"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="71d72-258">`WaitForShutdown` wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="71d72-258">`WaitForShutdown` calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="71d72-259">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="71d72-259">WaitForShutdownAsync</span></span>

<span data-ttu-id="71d72-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> Zwraca `Task` który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="71d72-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a `Task` that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="71d72-261">Zewnętrznej kontroli</span><span class="sxs-lookup"><span data-stu-id="71d72-261">External control</span></span>

<span data-ttu-id="71d72-262">Zewnętrznej kontroli hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="71d72-262">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="71d72-263"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> nosi nazwę na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który powoduje oczekiwanie, dopóki nie zostanie ukończone przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="71d72-263"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="71d72-264">Może to służyć do opóźnienie uruchamiania, dopóki nie zasygnalizowane przez zdarzenie zewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="71d72-264">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="71d72-265">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="71d72-265">IHostingEnvironment interface</span></span>

<span data-ttu-id="71d72-266"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o aplikacji w środowisku hostingu.</span><span class="sxs-lookup"><span data-stu-id="71d72-266"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="71d72-267">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać `IHostingEnvironment` aby można było używać jej właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="71d72-267">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="71d72-268">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="71d72-268">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="71d72-269">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="71d72-269">IApplicationLifetime interface</span></span>

<span data-ttu-id="71d72-270"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> Umożliwia działań po uruchamiania i zamykania, w tym łagodne zamykanie żądania.</span><span class="sxs-lookup"><span data-stu-id="71d72-270"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="71d72-271">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="71d72-271">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="71d72-272">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="71d72-272">Cancellation Token</span></span> | <span data-ttu-id="71d72-273">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="71d72-273">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="71d72-274">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="71d72-274">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="71d72-275">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="71d72-275">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="71d72-276">Wszystkie żądania powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="71d72-276">All requests should be processed.</span></span> <span data-ttu-id="71d72-277">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="71d72-277">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="71d72-278">Wykonuje łagodne zamykanie hosta.</span><span class="sxs-lookup"><span data-stu-id="71d72-278">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="71d72-279">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="71d72-279">Requests may still be processing.</span></span> <span data-ttu-id="71d72-280">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="71d72-280">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="71d72-281">Wstrzykiwanie Konstruktor `IApplicationLifetime` usługi do każdej klasy.</span><span class="sxs-lookup"><span data-stu-id="71d72-281">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="71d72-282">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( `IHostedService` implementacji) do rejestrowania zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="71d72-282">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="71d72-283">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="71d72-283">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="71d72-284"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71d72-284"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="71d72-285">Następujące klasy używa `StopApplication` bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="71d72-285">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="71d72-286">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="71d72-286">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="71d72-287">Hosting repozytorium przykładów w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="71d72-287">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
