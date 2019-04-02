---
title: Ogólny hosta platformy .NET
author: guardrex
description: Dowiedz się więcej o ogólnych Host platformy ASP.NET Core, który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: bb6afe59fcad685d18cdc9c8d90cfcc7b3a6541d
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809198"
---
# <a name="net-generic-host"></a><span data-ttu-id="ef53a-103">Ogólny hosta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="ef53a-103">.NET Generic Host</span></span>

<span data-ttu-id="ef53a-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ef53a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ef53a-105">Aplikacje platformy ASP.NET Core, konfigurowanie i uruchamiania hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-105">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="ef53a-106">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-106">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="ef53a-107">W tym artykule opisano hosta rodzajowego Core .NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span><span class="sxs-lookup"><span data-stu-id="ef53a-107">This article covers the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>).</span></span>

<span data-ttu-id="ef53a-108">Ogólny hosta różni się od hosta sieci Web, w tym, że są oddzieleni potoku HTTP z hosta internetowego interfejsu API, umożliwiające szersze gamę scenariuszy hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-108">Generic Host differs from Web Host in that it decouples the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="ef53a-109">Wiadomości, zadania w tle i innych obciążeń innych niż HTTP można używać ogólnych hosta i korzystać z możliwości przekrojowe, takie jak konfiguracja, wstrzykiwanie zależności (DI) i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="ef53a-109">Messaging, background tasks, and other non-HTTP workloads can use Generic Host and benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="ef53a-110">Począwszy od programu ASP.NET Core 3.0 ogólnego hostów jest zalecane protokołów HTTP, jak i obciążeń innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef53a-110">Starting in ASP.NET Core 3.0, Generic Host is recommended for both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="ef53a-111">Implementację serwera HTTP, jeśli uwzględniony, działa jako implementację <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-111">An HTTP server implementation, if included, runs as an implementation of <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="ef53a-112"><xref:Microsoft.Extensions.Hosting.IHostedService> to interfejs, który może służyć także do innych obciążeń.</span><span class="sxs-lookup"><span data-stu-id="ef53a-112"><xref:Microsoft.Extensions.Hosting.IHostedService> is an interface that can be used for other workloads as well.</span></span>

<span data-ttu-id="ef53a-113">Host sieci Web nie jest już zalecany dla aplikacji sieci web, ale pozostaje dostępna, zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="ef53a-113">Web Host is no longer recommended for web apps but remains available for backward compatibility.</span></span>

> [!NOTE]
> <span data-ttu-id="ef53a-114">Ta dalszej części tego artykułu zostało zaktualizowane dla 3.0.</span><span class="sxs-lookup"><span data-stu-id="ef53a-114">This remainder of this article hasn't been updated for 3.0.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ef53a-115">Aplikacje platformy ASP.NET Core, konfigurowanie i uruchamiania hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-115">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="ef53a-116">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-116">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="ef53a-117">W tym artykule opisano Host rodzajowego Core ASP.NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef53a-117">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="ef53a-118">Ogólny hosta ma na celu rozdzielenie potoku HTTP z hosta internetowego interfejsu API, umożliwiające szersze gamę scenariuszy hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-118">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="ef53a-119">Komunikaty, zadania w tle i innych obciążeń innych niż HTTP oparte na ogólnych hosta korzyści z możliwości przekrojowe, takie jak konfiguracja, wstrzykiwanie zależności (DI) i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="ef53a-119">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="ef53a-120">Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiednie w scenariuszach hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="ef53a-120">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="ef53a-121">W przypadku scenariuszy hostingu w sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="ef53a-121">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="ef53a-122">Ogólny Host będzie Zastąp hosta sieci Web w przyszłym wydaniu i pełnić rolę hosta podstawowego interfejsu API zarówno w przypadku protokołu HTTP, jak i scenariuszy innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef53a-122">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

::: moniker-end

<span data-ttu-id="ef53a-123">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef53a-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ef53a-124">Podczas uruchamiania przykładowej aplikacji [programu Visual Studio Code](https://code.visualstudio.com/), użyj *zewnętrznych lub w zintegrowanym terminalu*.</span><span class="sxs-lookup"><span data-stu-id="ef53a-124">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="ef53a-125">Nie, uruchom przykład w `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="ef53a-125">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="ef53a-126">Aby ustawić konsoli w programie Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="ef53a-126">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="ef53a-127">Otwórz *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-127">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="ef53a-128">W **.NET Core uruchamianie (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-128">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="ef53a-129">Ustaw wartość na wartość `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="ef53a-129">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="ef53a-130">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="ef53a-130">Introduction</span></span>

<span data-ttu-id="ef53a-131">Biblioteka ogólnego hosta jest dostępna w <xref:Microsoft.Extensions.Hosting> przestrzeni nazw i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-131">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="ef53a-132">`Microsoft.Extensions.Hosting` Pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="ef53a-132">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="ef53a-133"><xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-133"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="ef53a-134">Każdy <xref:Microsoft.Extensions.Hosting.IHostedService> implementacja jest wykonywany w kolejności od [usługi rejestracji w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="ef53a-134">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="ef53a-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana w każdej <xref:Microsoft.Extensions.Hosting.IHostedService> podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w kolejności odwrotnej rejestracji, gdy kończy pracę bez problemu zmieniała hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-135"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="ef53a-136">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="ef53a-136">Set up a host</span></span>

<span data-ttu-id="ef53a-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> jest to główny składnik, używanego przez aplikacje i biblioteki do zainicjowania, tworzeniu i uruchamianiu hosta:</span><span class="sxs-lookup"><span data-stu-id="ef53a-137"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="ef53a-138">Opcje</span><span class="sxs-lookup"><span data-stu-id="ef53a-138">Options</span></span>

<span data-ttu-id="ef53a-139"><xref:Microsoft.Extensions.Hosting.HostOptions> Skonfiguruj opcje <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-139"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="ef53a-140">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="ef53a-140">Shutdown timeout</span></span>

<span data-ttu-id="ef53a-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="ef53a-142">Wartość domyślna to 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="ef53a-142">The default value is five seconds.</span></span>

<span data-ttu-id="ef53a-143">Następująca Konfiguracja opcji w `Program.Main` zwiększa domyślna pięć drugi zamykania wartość limitu czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="ef53a-143">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="ef53a-144">Usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="ef53a-144">Default services</span></span>

<span data-ttu-id="ef53a-145">Następujące usługi są rejestrowane podczas inicjowania hosta:</span><span class="sxs-lookup"><span data-stu-id="ef53a-145">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="ef53a-146">[Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="ef53a-146">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="ef53a-147">[Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="ef53a-147">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="ef53a-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="ef53a-148"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="ef53a-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="ef53a-149"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="ef53a-150">[Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="ef53a-150">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="ef53a-151">[Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="ef53a-151">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="ef53a-152">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="ef53a-152">Host configuration</span></span>

<span data-ttu-id="ef53a-153">Konfiguracja hosta jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="ef53a-153">Host configuration is created by:</span></span>

* <span data-ttu-id="ef53a-154">Wywoływanie metody rozszerzenia na <xref:Microsoft.Extensions.Hosting.IHostBuilder> można ustawić [zawartości głównego](#content-root) i [środowiska](#environment).</span><span class="sxs-lookup"><span data-stu-id="ef53a-154">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="ef53a-155">Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-155">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="ef53a-156">Metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="ef53a-156">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="ef53a-157">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="ef53a-157">Application key (name)</span></span>

<span data-ttu-id="ef53a-158">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) właściwość ma wartość z konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-158">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="ef53a-159">Aby jawnie ustawić wartość, użyj [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="ef53a-159">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="ef53a-160">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="ef53a-160">**Key**: applicationName</span></span>  
<span data-ttu-id="ef53a-161">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="ef53a-161">**Type**: *string*</span></span>  
<span data-ttu-id="ef53a-162">**Domyślne**: Nazwa zestawu zawierającego wpis aplikacji punktu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-162">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="ef53a-163">**Można ustawić przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="ef53a-163">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="ef53a-164">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="ef53a-164">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="ef53a-165">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="ef53a-165">Content root</span></span>

<span data-ttu-id="ef53a-166">To ustawienie określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="ef53a-166">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="ef53a-167">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="ef53a-167">**Key**: contentRoot</span></span>  
<span data-ttu-id="ef53a-168">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="ef53a-168">**Type**: *string*</span></span>  
<span data-ttu-id="ef53a-169">**Domyślne**: Wartość domyślna to folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-169">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="ef53a-170">**Można ustawić przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="ef53a-170">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="ef53a-171">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="ef53a-171">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="ef53a-172">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="ef53a-172">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="ef53a-173">Środowisko</span><span class="sxs-lookup"><span data-stu-id="ef53a-173">Environment</span></span>

<span data-ttu-id="ef53a-174">Ustawia aplikacji [środowiska](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="ef53a-174">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="ef53a-175">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="ef53a-175">**Key**: environment</span></span>  
<span data-ttu-id="ef53a-176">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="ef53a-176">**Type**: *string*</span></span>  
<span data-ttu-id="ef53a-177">**Domyślne**: Produkcji</span><span class="sxs-lookup"><span data-stu-id="ef53a-177">**Default**: Production</span></span>  
<span data-ttu-id="ef53a-178">**Można ustawić przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="ef53a-178">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="ef53a-179">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="ef53a-179">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="ef53a-180">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="ef53a-180">The environment can be set to any value.</span></span> <span data-ttu-id="ef53a-181">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="ef53a-181">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="ef53a-182">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="ef53a-182">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="ef53a-183">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="ef53a-183">ConfigureHostConfiguration</span></span>

<span data-ttu-id="ef53a-184"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-184"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="ef53a-185">Konfiguracja hosta jest wykorzystywany do inicjacji <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-185">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="ef53a-186"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-186"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="ef53a-187">Host używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="ef53a-187">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="ef53a-188">Konfiguracja hosta są automatycznie przekazywane do konfiguracji aplikacji ([ConfigureAppConfiguration](#configureappconfiguration) i pozostałej części aplikacji).</span><span class="sxs-lookup"><span data-stu-id="ef53a-188">Host configuration automatically flows to app configuration ([ConfigureAppConfiguration](#configureappconfiguration) and the rest of the app).</span></span>

<span data-ttu-id="ef53a-189">Brak dostawców są domyślnie dołączone.</span><span class="sxs-lookup"><span data-stu-id="ef53a-189">No providers are included by default.</span></span> <span data-ttu-id="ef53a-190">Należy jawnie określić niezależnie od dostawcy konfiguracji aplikacji wymaga w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, w tym:</span><span class="sxs-lookup"><span data-stu-id="ef53a-190">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="ef53a-191">Plik konfiguracji (na przykład z *hostsettings.json* pliku).</span><span class="sxs-lookup"><span data-stu-id="ef53a-191">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="ef53a-192">Zmienne konfiguracji środowiska.</span><span class="sxs-lookup"><span data-stu-id="ef53a-192">Environment variable configuration.</span></span>
* <span data-ttu-id="ef53a-193">Konfiguracja argumentu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="ef53a-193">Command-line argument configuration.</span></span>
* <span data-ttu-id="ef53a-194">Innych dostawców wymaganej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-194">Any other required configuration providers.</span></span>

<span data-ttu-id="ef53a-195">Plik konfiguracji hosta jest włączona, określając ścieżki podstawowej aplikacji za pomocą `SetBasePath` następuje wywołanie jednej z [pliku dostawcy konfiguracji](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ef53a-195">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="ef53a-196">Przykładowa aplikacja używa pliku JSON, *hostsettings.json*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> korzystanie z ustawień konfiguracji hosta tego pliku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-196">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="ef53a-197">Aby dodać [zmiennych konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w Konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-197">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="ef53a-198">`AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ef53a-198">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="ef53a-199">Przykładowa aplikacja korzysta z prefiksem `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="ef53a-199">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="ef53a-200">Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane.</span><span class="sxs-lookup"><span data-stu-id="ef53a-200">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="ef53a-201">Po skonfigurowaniu hostów przykładową aplikację, wartość zmiennej środowiskowej, aby uzyskać `PREFIX_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.</span><span class="sxs-lookup"><span data-stu-id="ef53a-201">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="ef53a-202">Podczas programowania, korzystając z [programu Visual Studio](https://www.visualstudio.com/) lub uruchamianie aplikacji za pomocą `dotnet run`, zmienne środowiskowe, może być ustawiona w *Properties/launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-202">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="ef53a-203">W [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *.vscode/launch.json* pliku podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="ef53a-203">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="ef53a-204">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-204">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="ef53a-205">[Wiersza polecenia konfiguracji](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawany przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-205">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="ef53a-206">Konfiguracja wiersza polecenia zostanie dodany ostatnio pozwalającą na argumenty wiersza polecenia, aby zastąpić konfiguracji udostępnianych przez starszych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-206">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="ef53a-207">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ef53a-207">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="ef53a-208">Dodatkowa konfiguracja może być wyposażone w [applicationName](#application-key-name) i [contentRoot](#content-root) kluczy.</span><span class="sxs-lookup"><span data-stu-id="ef53a-208">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="ef53a-209">Przykład `HostBuilder` konfiguracji przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="ef53a-209">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="ef53a-210">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="ef53a-210">ConfigureAppConfiguration</span></span>

<span data-ttu-id="ef53a-211">Konfiguracja aplikacji jest tworzony przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> na <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementacji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-211">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="ef53a-212"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-212"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="ef53a-213"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-213"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="ef53a-214">Aplikacja używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="ef53a-214">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="ef53a-215">Konfiguracji utworzonej przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> znajduje się w temacie [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-215">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="ef53a-216">Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta, dostarczone przez [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="ef53a-216">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="ef53a-217">Przykład korzystającą configuration <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="ef53a-217">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="ef53a-218">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ef53a-218">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="ef53a-219">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="ef53a-219">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="ef53a-220">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="ef53a-220">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="ef53a-221">Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-221">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="ef53a-222">Przykładowa aplikacja przenosi jego pliki ustawień aplikacji w formacie JSON i *hostsettings.json* następującym `<Content>` elementu:</span><span class="sxs-lookup"><span data-stu-id="ef53a-222">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="ef53a-223">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="ef53a-223">ConfigureServices</span></span>

<span data-ttu-id="ef53a-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usług do aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="ef53a-224"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="ef53a-225"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-225"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="ef53a-226">Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-226">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="ef53a-227">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-227">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="ef53a-228">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` metodę rozszerzenia, aby dodać usługę do zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu `TimedHostedService`, do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ef53a-228">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="ef53a-229">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="ef53a-229">ConfigureLogging</span></span>

<span data-ttu-id="ef53a-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podane <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-230"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="ef53a-231"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> może zostać wywołana wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-231"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="ef53a-232">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="ef53a-232">UseConsoleLifetime</span></span>

<span data-ttu-id="ef53a-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje sygnał SIGTERM lub wywołań i Ctrl + C/SIGINT <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> można uruchomić procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="ef53a-233"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="ef53a-234"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="ef53a-234"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="ef53a-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> wstępnie jest zarejestrowany jako domyślna implementacja okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="ef53a-235"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="ef53a-236">Ostatniego okresu istnienia zarejestrowany jest używany.</span><span class="sxs-lookup"><span data-stu-id="ef53a-236">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="ef53a-237">Konfiguracja kontenera</span><span class="sxs-lookup"><span data-stu-id="ef53a-237">Container configuration</span></span>

<span data-ttu-id="ef53a-238">Aby zapewnić obsługę podłączania innych kontenerów, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-238">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="ef53a-239">Zapewnianie fabrykę nie jest częścią DI rejestracja kontenera jest jednak wewnętrzne hosta używany do tworzenia konkretnych kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="ef53a-239">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="ef53a-240">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę użyty do utworzenia dostawcy usługi app service.</span><span class="sxs-lookup"><span data-stu-id="ef53a-240">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="ef53a-241">Konfiguracja kontenera niestandardowego jest zarządzana przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> metody.</span><span class="sxs-lookup"><span data-stu-id="ef53a-241">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="ef53a-242"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> udostępnia silnie typizowane środowisko na potrzeby konfigurowania kontenera na podstawie odpowiedniego hosta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef53a-242"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="ef53a-243"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="ef53a-243"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="ef53a-244">Tworzenie kontenera usług dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ef53a-244">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="ef53a-245">Podaj fabryki kontenera usług:</span><span class="sxs-lookup"><span data-stu-id="ef53a-245">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="ef53a-246">Używaj fabryki i konfigurowanie kontenera niestandardowego usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ef53a-246">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="ef53a-247">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="ef53a-247">Extensibility</span></span>

<span data-ttu-id="ef53a-248">Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-248">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="ef53a-249">W poniższym przykładzie pokazano, jak metoda rozszerzenia rozszerza <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementację [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przykład przedstawiona w <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-249">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="ef53a-250">Aplikacja ustanawia `UseHostedService` metodę rozszerzenia, aby zarejestrować usługi hostowanej przekazanej `T`:</span><span class="sxs-lookup"><span data-stu-id="ef53a-250">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="ef53a-251">Zarządzać hosta</span><span class="sxs-lookup"><span data-stu-id="ef53a-251">Manage the host</span></span>

<span data-ttu-id="ef53a-252"><xref:Microsoft.Extensions.Hosting.IHost> Implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji, które są zarejestrowane w usłudze service container.</span><span class="sxs-lookup"><span data-stu-id="ef53a-252">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="ef53a-253">Uruchom</span><span class="sxs-lookup"><span data-stu-id="ef53a-253">Run</span></span>

<span data-ttu-id="ef53a-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uruchamia aplikację i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="ef53a-254"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="ef53a-255">RunAsync</span><span class="sxs-lookup"><span data-stu-id="ef53a-255">RunAsync</span></span>

<span data-ttu-id="ef53a-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="ef53a-256"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="ef53a-257">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="ef53a-257">RunConsoleAsync</span></span>

<span data-ttu-id="ef53a-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Włączenie obsługi konsoli, kompilacje uruchamia hosta i czeka na klawisze Ctrl + C/SIGINT lub SIGTERM zamknąć.</span><span class="sxs-lookup"><span data-stu-id="ef53a-258"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="ef53a-259">Początkowy i StopAsync</span><span class="sxs-lookup"><span data-stu-id="ef53a-259">Start and StopAsync</span></span>

<span data-ttu-id="ef53a-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="ef53a-260"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="ef53a-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-261"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="ef53a-262">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="ef53a-262">StartAsync and StopAsync</span></span>

<span data-ttu-id="ef53a-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> Uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="ef53a-263"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="ef53a-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> Zatrzymuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="ef53a-264"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="ef53a-265">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="ef53a-265">WaitForShutdown</span></span>

<span data-ttu-id="ef53a-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalany za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (nasłuchuje klawisze Ctrl + C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="ef53a-266"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="ef53a-267"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-267"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="ef53a-268">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="ef53a-268">WaitForShutdownAsync</span></span>

<span data-ttu-id="ef53a-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> Zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-269"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="ef53a-270">Zewnętrznej kontroli</span><span class="sxs-lookup"><span data-stu-id="ef53a-270">External control</span></span>

<span data-ttu-id="ef53a-271">Zewnętrznej kontroli hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="ef53a-271">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="ef53a-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> nosi nazwę na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który powoduje oczekiwanie, dopóki nie zostanie ukończone przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="ef53a-272"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="ef53a-273">Może to służyć do opóźnienie uruchamiania, dopóki nie zasygnalizowane przez zdarzenie zewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="ef53a-273">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="ef53a-274">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="ef53a-274">IHostingEnvironment interface</span></span>

<span data-ttu-id="ef53a-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o aplikacji w środowisku hostingu.</span><span class="sxs-lookup"><span data-stu-id="ef53a-275"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="ef53a-276">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> aby można było używać jej właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="ef53a-276">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="ef53a-277">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="ef53a-277">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="ef53a-278">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="ef53a-278">IApplicationLifetime interface</span></span>

<span data-ttu-id="ef53a-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> Umożliwia działań po uruchamiania i zamykania, w tym łagodne zamykanie żądania.</span><span class="sxs-lookup"><span data-stu-id="ef53a-279"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="ef53a-280">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania <xref:System.Action> metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="ef53a-280">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="ef53a-281">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="ef53a-281">Cancellation Token</span></span> | <span data-ttu-id="ef53a-282">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="ef53a-282">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="ef53a-283">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="ef53a-283">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="ef53a-284">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="ef53a-284">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="ef53a-285">Wszystkie żądania powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="ef53a-285">All requests should be processed.</span></span> <span data-ttu-id="ef53a-286">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="ef53a-286">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="ef53a-287">Wykonuje łagodne zamykanie hosta.</span><span class="sxs-lookup"><span data-stu-id="ef53a-287">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="ef53a-288">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="ef53a-288">Requests may still be processing.</span></span> <span data-ttu-id="ef53a-289">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="ef53a-289">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="ef53a-290">Wstrzykiwanie Konstruktor <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> usługi do każdej klasy.</span><span class="sxs-lookup"><span data-stu-id="ef53a-290">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="ef53a-291">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji) do rejestrowania zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="ef53a-291">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="ef53a-292">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="ef53a-292">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="ef53a-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef53a-293"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="ef53a-294">Następujące klasy używa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="ef53a-294">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ef53a-295">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ef53a-295">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
