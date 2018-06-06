---
title: .NET rodzajowego hosta
author: guardrex
description: Więcej informacji na temat ogólnych hosta w .NET, który jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: a851f2faf13792b2c232c124371d07710ae1fce3
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734474"
---
# <a name="net-generic-host"></a><span data-ttu-id="7a81c-103">.NET rodzajowego hosta</span><span class="sxs-lookup"><span data-stu-id="7a81c-103">.NET Generic Host</span></span>

<span data-ttu-id="7a81c-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7a81c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7a81c-105">Aplikacje .NET skonfigurować i uruchomić *hosta*.</span><span class="sxs-lookup"><span data-stu-id="7a81c-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="7a81c-106">Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="7a81c-107">W tym temacie omówiono Host ogólnego ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), co jest przydatne do hostowania aplikacji, które nie przetwarzają żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a81c-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="7a81c-108">Pokrycia hosta sieci Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="7a81c-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="7a81c-109">Celem rodzajowego hosta jest oddzielana potoku HTTP z interfejsu API hosta sieci Web, aby włączyć szerszego scenariuszy hosta.</span><span class="sxs-lookup"><span data-stu-id="7a81c-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="7a81c-110">Do obsługi komunikatów, zadania w tle i innych obciążeń innych niż HTTP na podstawie ogólnego hosta korzyści z kompleksowymi możliwości, takie jak konfiguracja, iniekcji zależności (Podpisane) i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="7a81c-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="7a81c-111">Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiedni dla scenariuszy hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="7a81c-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="7a81c-112">W scenariuszach hostingu sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="7a81c-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="7a81c-113">Ogólny hosta jest w fazie projektowania hosta sieci Web w przyszłych wydaniach i działa jako podstawowy hosta interfejsu API w protokołów HTTP i scenariusze innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a81c-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="7a81c-114">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7a81c-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7a81c-115">Podczas uruchamiania przykładową aplikację [Visual Studio Code](https://code.visualstudio.com/), użyj *terminal zewnętrznego lub zintegrowane*.</span><span class="sxs-lookup"><span data-stu-id="7a81c-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="7a81c-116">Nie uruchamiaj próbki `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="7a81c-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="7a81c-117">Aby ustawić konsoli w programie Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="7a81c-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="7a81c-118">Otwórz *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7a81c-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="7a81c-119">W **.NET Core uruchom (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu.</span><span class="sxs-lookup"><span data-stu-id="7a81c-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="7a81c-120">Ustaw wartość na jedną `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="7a81c-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="7a81c-121">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="7a81c-121">Introduction</span></span>

<span data-ttu-id="7a81c-122">Biblioteka rodzajowego hosta jest dostępna w [przestrzeni nazw Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="7a81c-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="7a81c-123">`Microsoft.Extensions.Hosting` Pakietu znajduje się w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="7a81c-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="7a81c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) jest punkt wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="7a81c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="7a81c-125">Każdy `IHostedService` implementacji jest wykonywany w celu z [usługi rejestracji w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="7a81c-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="7a81c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) jest wywoływana w każdym `IHostedService` po uruchomieniu hosta i [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) jest wywoływane w kolejności odwrotnej rejestracji, gdy host zamyka bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="7a81c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="7a81c-127">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="7a81c-127">Set up a host</span></span>

<span data-ttu-id="7a81c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) jest głównym składnikiem korzystające z biblioteki i aplikacji do zainicjowania, tworzenie i uruchamianie hosta:</span><span class="sxs-lookup"><span data-stu-id="7a81c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="7a81c-129">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="7a81c-129">Host configuration</span></span>

<span data-ttu-id="7a81c-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) zależy od następujących metod można ustawić wartości konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="7a81c-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="7a81c-131">Konstruktor konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7a81c-131">Configuration builder</span></span>
* <span data-ttu-id="7a81c-132">Konfiguracja — metoda rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="7a81c-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="7a81c-133">Konstruktor konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7a81c-133">Configuration builder</span></span>

<span data-ttu-id="7a81c-134">Konfiguracja Konstruktora hosta jest tworzony przez wywołanie metody [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="7a81c-135">`ConfigureHostConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla hosta.</span><span class="sxs-lookup"><span data-stu-id="7a81c-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="7a81c-136">Inicjuje konstruktora konfiguracji [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) do użycia w procesie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span> <span data-ttu-id="7a81c-137">`ConfigureHostConfiguration` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="7a81c-137">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7a81c-138">Host używa jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="7a81c-138">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="7a81c-139">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="7a81c-139">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="7a81c-140">Przykład `HostBuilder` konfiguracji przy użyciu `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="7a81c-140">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="7a81c-141">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="7a81c-141">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="7a81c-142">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="7a81c-142">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="7a81c-143">`AddConfiguration` Metoda oczekuje klucze do dopasowania `HostBuilder` kluczy (na przykład `environment`).</span><span class="sxs-lookup"><span data-stu-id="7a81c-143">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="7a81c-144">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="7a81c-144">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="7a81c-145">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-145">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="7a81c-146">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="7a81c-146">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="7a81c-147">Konfiguracja — metoda rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="7a81c-147">Extension method configuration</span></span>

<span data-ttu-id="7a81c-148">Metody rozszerzenia są wywoływane na `IHostBuilder` implementacji, aby skonfigurować główny zawartości i środowiska.</span><span class="sxs-lookup"><span data-stu-id="7a81c-148">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="7a81c-149">Główny zawartości</span><span class="sxs-lookup"><span data-stu-id="7a81c-149">Content Root</span></span>

<span data-ttu-id="7a81c-150">To ustawienie określa, gdzie hosta rozpocznie się wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="7a81c-150">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="7a81c-151">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="7a81c-151">**Key**: contentRoot</span></span>  
<span data-ttu-id="7a81c-152">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="7a81c-152">**Type**: *string*</span></span>  
<span data-ttu-id="7a81c-153">**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-153">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="7a81c-154">**Ustawić za pomocą**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="7a81c-154">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="7a81c-155">**Zmienna środowiskowa**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="7a81c-155">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="7a81c-156">Jeśli ścieżka nie istnieje, host nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="7a81c-156">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="7a81c-157">Środowisko</span><span class="sxs-lookup"><span data-stu-id="7a81c-157">Environment</span></span>

<span data-ttu-id="7a81c-158">Ustawia aplikacji [środowiska](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7a81c-158">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="7a81c-159">**Klucz**: środowiska</span><span class="sxs-lookup"><span data-stu-id="7a81c-159">**Key**: environment</span></span>  
<span data-ttu-id="7a81c-160">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="7a81c-160">**Type**: *string*</span></span>  
<span data-ttu-id="7a81c-161">**Domyślna**: produkcji</span><span class="sxs-lookup"><span data-stu-id="7a81c-161">**Default**: Production</span></span>  
<span data-ttu-id="7a81c-162">**Ustawić za pomocą**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="7a81c-162">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="7a81c-163">**Zmienna środowiskowa**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="7a81c-163">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="7a81c-164">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="7a81c-164">The environment can be set to any value.</span></span> <span data-ttu-id="7a81c-165">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="7a81c-165">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="7a81c-166">Wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="7a81c-166">Values aren't case sensitive.</span></span> <span data-ttu-id="7a81c-167">Domyślnie *środowiska* są odczytywane z `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="7a81c-167">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="7a81c-168">Korzystając z [programu Visual Studio](https://www.visualstudio.com/), zmienne środowiskowe może być ustawiona w *launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7a81c-168">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="7a81c-169">Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7a81c-169">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="7a81c-170">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="7a81c-170">ConfigureAppConfiguration</span></span>

<span data-ttu-id="7a81c-171">Konfiguracja Konstruktora aplikacji jest tworzony przez wywołanie metody [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-171">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="7a81c-172">`ConfigureAppConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-172">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="7a81c-173">`ConfigureAppConfiguration` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="7a81c-173">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7a81c-174">Aplikacja korzysta z jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="7a81c-174">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="7a81c-175">Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) dla kolejnych operacji i w [usług](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="7a81c-175">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="7a81c-176">Przykład aplikacji konfiguracji przy użyciu `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="7a81c-176">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="7a81c-177">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="7a81c-177">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="7a81c-178">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="7a81c-178">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="7a81c-179">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="7a81c-179">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="7a81c-180">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="7a81c-180">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="7a81c-181">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="7a81c-181">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="7a81c-182">`AddConfiguration` Metoda oczekuje dokładnego dopasowania do konfiguracji kluczy (na przykład `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="7a81c-182">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="7a81c-183">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-183">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="7a81c-184">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-184">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="7a81c-185">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="7a81c-185">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="7a81c-186">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="7a81c-186">ConfigureServices</span></span>

<span data-ttu-id="7a81c-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) dodaje usług do aplikacji [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="7a81c-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="7a81c-188">`ConfigureServices` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="7a81c-188">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="7a81c-189">Hostowana usługa jest klasa z logiką zadania tła, który implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="7a81c-189">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="7a81c-190">Aby uzyskać więcej informacji, zobacz [zadania związane z usług hostowanych w tle](xref:fundamentals/host/hosted-services) tematu.</span><span class="sxs-lookup"><span data-stu-id="7a81c-190">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="7a81c-191">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` — metoda rozszerzenia, aby dodać usługę dla zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu, `TimedHostedService`, do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7a81c-191">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="7a81c-192">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="7a81c-192">ConfigureLogging</span></span>

<span data-ttu-id="7a81c-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) dodaje delegata do konfigurowania udostępnionych [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="7a81c-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="7a81c-194">`ConfigureLogging` może zostać wywołana wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="7a81c-194">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="7a81c-195">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="7a81c-195">UseConsoleLifetime</span></span>

<span data-ttu-id="7a81c-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) nasłuchuje `Ctrl+C`/SIGINT lub sigterm — i wywołania [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) można uruchomić procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="7a81c-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="7a81c-197">`UseConsoleLifetime` Odblokowuje rozszerzenia takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="7a81c-197">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="7a81c-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) wstępnie jest zarejestrowany jako domyślna implementacja okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="7a81c-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="7a81c-199">Okres istnienia ostatniego zarejestrowany jest używany.</span><span class="sxs-lookup"><span data-stu-id="7a81c-199">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="7a81c-200">Kontenera konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7a81c-200">Container configuration</span></span>

<span data-ttu-id="7a81c-201">Aby zapewnić obsługę podłączania innych kontenerów, host może zaakceptować [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="7a81c-201">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="7a81c-202">Udostępnia fabrykę nie jest częścią rejestracji kontenera Podpisane, ale zamiast tego jest używany do tworzenia konkretnych kontenera Podpisane hosta — wewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="7a81c-202">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="7a81c-203">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) zastępuje domyślną fabrykę używany do tworzenia aplikacji usługodawcy.</span><span class="sxs-lookup"><span data-stu-id="7a81c-203">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="7a81c-204">Niestandardowe kontenera konfiguracji jest zarządzana przez [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metody.</span><span class="sxs-lookup"><span data-stu-id="7a81c-204">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="7a81c-205">`ConfigureContainer` zapewnia obsługę jednoznacznie konfigurowania kontenera na górze odpowiedniego hosta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7a81c-205">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="7a81c-206">`ConfigureContainer` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="7a81c-206">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="7a81c-207">Utwórz kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7a81c-207">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="7a81c-208">Podaj fabryki kontenera usług:</span><span class="sxs-lookup"><span data-stu-id="7a81c-208">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="7a81c-209">Użyj fabryka i skonfiguruj kontener usług niestandardowych dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7a81c-209">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="7a81c-210">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="7a81c-210">Extensibility</span></span>

<span data-ttu-id="7a81c-211">Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7a81c-211">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="7a81c-212">W poniższym przykładzie pokazano, jak rozszerza metodę rozszerzenia `IHostBuilder` implementację [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="7a81c-212">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="7a81c-213">Metoda rozszerzenia (w aplikacji) rejestruje RabbitMQ `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="7a81c-213">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="7a81c-214">Zarządzanie hosta</span><span class="sxs-lookup"><span data-stu-id="7a81c-214">Manage the host</span></span>

<span data-ttu-id="7a81c-215">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie `IHostedService` implementacji, które są zarejestrowane w kontenerze usług.</span><span class="sxs-lookup"><span data-stu-id="7a81c-215">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="7a81c-216">Uruchom</span><span class="sxs-lookup"><span data-stu-id="7a81c-216">Run</span></span>

<span data-ttu-id="7a81c-217">[Uruchom](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uruchamia aplikację i blokuje wątek wywołujący, dopóki nie zostanie zamknięta hosta:</span><span class="sxs-lookup"><span data-stu-id="7a81c-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="7a81c-218">RunAsync</span><span class="sxs-lookup"><span data-stu-id="7a81c-218">RunAsync</span></span>

<span data-ttu-id="7a81c-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uruchamia aplikację i zwraca `Task` który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="7a81c-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="7a81c-220">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="7a81c-220">RunConsoleAsync</span></span>

<span data-ttu-id="7a81c-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) włącza obsługę konsoli, kompiluje i uruchamia hosta i czeka na `Ctrl+C`/SIGINT lub sigterm — w celu zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="7a81c-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="7a81c-222">Start i StopAsync</span><span class="sxs-lookup"><span data-stu-id="7a81c-222">Start and StopAsync</span></span>

<span data-ttu-id="7a81c-223">[Uruchom](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) synchronicznie uruchamia hosta.</span><span class="sxs-lookup"><span data-stu-id="7a81c-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="7a81c-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="7a81c-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="7a81c-225">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="7a81c-225">StartAsync and StopAsync</span></span>

<span data-ttu-id="7a81c-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="7a81c-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) zatrzymuje aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="7a81c-228">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="7a81c-228">WaitForShutdown</span></span>

<span data-ttu-id="7a81c-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) zostanie wywołany za pomocą [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), takich jak [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (nasłuchuje `Ctrl+C`/SIGINT lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="7a81c-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="7a81c-230">`WaitForShutdown` wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="7a81c-230">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="7a81c-231">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="7a81c-231">WaitForShutdownAsync</span></span>

<span data-ttu-id="7a81c-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) zwraca `Task` który zostaje ukończony po wyzwoleniu zamknięcia za pomocą danego tokenu i wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="7a81c-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="7a81c-233">Zewnętrzny</span><span class="sxs-lookup"><span data-stu-id="7a81c-233">External control</span></span>

<span data-ttu-id="7a81c-234">Formant zewnętrznego hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="7a81c-234">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="7a81c-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) nazywa się na początku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), który oczekuje jego zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="7a81c-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="7a81c-236">Może to służyć do opóźnienia uruchomienia aż zgłoszony przez zdarzenie zewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="7a81c-236">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="7a81c-237">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="7a81c-237">IHostingEnvironment interface</span></span>

<span data-ttu-id="7a81c-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) zapewnia środowiska macierzystego informacji o aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="7a81c-239">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="7a81c-239">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="7a81c-240">Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7a81c-240">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="7a81c-241">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="7a81c-241">IApplicationLifetime interface</span></span>

<span data-ttu-id="7a81c-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umożliwia po uruchamiania i wyłączania działań, włącznie z żądaniami łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="7a81c-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="7a81c-243">Anulowanie tokenów używany do rejestrowania są trzy właściwości w interfejsie `Action` metod, które definiują zdarzenia uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="7a81c-243">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="7a81c-244">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="7a81c-244">Cancellation Token</span></span> | <span data-ttu-id="7a81c-245">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="7a81c-245">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="7a81c-246">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="7a81c-246">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="7a81c-247">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="7a81c-247">The host has fully started.</span></span> |
| [<span data-ttu-id="7a81c-248">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="7a81c-248">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="7a81c-249">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="7a81c-249">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="7a81c-250">Wszystkie żądania powinna zostać przetworzona.</span><span class="sxs-lookup"><span data-stu-id="7a81c-250">All requests should be processed.</span></span> <span data-ttu-id="7a81c-251">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="7a81c-251">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="7a81c-252">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="7a81c-252">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="7a81c-253">Host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="7a81c-253">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="7a81c-254">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="7a81c-254">Requests may still be processing.</span></span> <span data-ttu-id="7a81c-255">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="7a81c-255">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="7a81c-256">Wstaw konstruktora `IApplicationLifetime` usługi do żadnej klasy.</span><span class="sxs-lookup"><span data-stu-id="7a81c-256">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="7a81c-257">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji contructor do `LifetimeEventsHostedService` klasy ( `IHostedService` implementacji) można zarejestrować zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="7a81c-257">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="7a81c-258">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="7a81c-258">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="7a81c-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) żąda zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a81c-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="7a81c-260">Następujące klasy używa `StopApplication` można bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="7a81c-260">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="7a81c-261">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7a81c-261">Additional resources</span></span>

* [<span data-ttu-id="7a81c-262">Zadania w tle z usługami hostowanymi</span><span class="sxs-lookup"><span data-stu-id="7a81c-262">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="7a81c-263">Hosting przykłady repozytorium w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="7a81c-263">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
