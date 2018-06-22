---
title: .NET rodzajowego hosta
author: guardrex
description: Więcej informacji na temat ogólnych hosta w .NET, który jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 33e5829ce4a09e132743b4174a588cf232a44775
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276266"
---
# <a name="net-generic-host"></a><span data-ttu-id="057b2-103">.NET rodzajowego hosta</span><span class="sxs-lookup"><span data-stu-id="057b2-103">.NET Generic Host</span></span>

<span data-ttu-id="057b2-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="057b2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="057b2-105">Aplikacje .NET skonfigurować i uruchomić *hosta*.</span><span class="sxs-lookup"><span data-stu-id="057b2-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="057b2-106">Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="057b2-107">W tym temacie omówiono Host ogólnego ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), co jest przydatne do hostowania aplikacji, które nie przetwarzają żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="057b2-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="057b2-108">Pokrycia hosta sieci Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="057b2-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="057b2-109">Celem rodzajowego hosta jest oddzielana potoku HTTP z interfejsu API hosta sieci Web, aby włączyć szerszego scenariuszy hosta.</span><span class="sxs-lookup"><span data-stu-id="057b2-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="057b2-110">Do obsługi komunikatów, zadania w tle i innych obciążeń innych niż HTTP na podstawie ogólnego hosta korzyści z kompleksowymi możliwości, takie jak konfiguracja, iniekcji zależności (Podpisane) i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="057b2-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="057b2-111">Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiedni dla scenariuszy hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="057b2-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="057b2-112">W scenariuszach hostingu sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="057b2-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="057b2-113">Ogólny hosta jest w fazie projektowania hosta sieci Web w przyszłych wydaniach i działa jako podstawowy hosta interfejsu API w protokołów HTTP i scenariusze innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="057b2-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="057b2-114">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="057b2-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="057b2-115">Podczas uruchamiania przykładową aplikację [Visual Studio Code](https://code.visualstudio.com/), użyj *terminal zewnętrznego lub zintegrowane*.</span><span class="sxs-lookup"><span data-stu-id="057b2-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="057b2-116">Nie uruchamiaj próbki `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="057b2-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="057b2-117">Aby ustawić konsoli w programie Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="057b2-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="057b2-118">Otwórz *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="057b2-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="057b2-119">W **.NET Core uruchom (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu.</span><span class="sxs-lookup"><span data-stu-id="057b2-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="057b2-120">Ustaw wartość na jedną `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="057b2-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="057b2-121">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="057b2-121">Introduction</span></span>

<span data-ttu-id="057b2-122">Biblioteka rodzajowego hosta jest dostępna w [przestrzeni nazw Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="057b2-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="057b2-123">`Microsoft.Extensions.Hosting` Pakietu znajduje się w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="057b2-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="057b2-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) jest punkt wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="057b2-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="057b2-125">Każdy `IHostedService` implementacji jest wykonywany w celu z [usługi rejestracji w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="057b2-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="057b2-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) jest wywoływana w każdym `IHostedService` po uruchomieniu hosta i [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) jest wywoływane w kolejności odwrotnej rejestracji, gdy host zamyka bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="057b2-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="057b2-127">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="057b2-127">Set up a host</span></span>

<span data-ttu-id="057b2-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) jest głównym składnikiem korzystające z biblioteki i aplikacji do zainicjowania, tworzenie i uruchamianie hosta:</span><span class="sxs-lookup"><span data-stu-id="057b2-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="057b2-129">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="057b2-129">Host configuration</span></span>

<span data-ttu-id="057b2-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) zależy od następujących metod można ustawić wartości konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="057b2-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="057b2-131">Konstruktor konfiguracji</span><span class="sxs-lookup"><span data-stu-id="057b2-131">Configuration builder</span></span>
* <span data-ttu-id="057b2-132">Konfiguracja — metoda rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="057b2-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="057b2-133">Konstruktor konfiguracji</span><span class="sxs-lookup"><span data-stu-id="057b2-133">Configuration builder</span></span>

<span data-ttu-id="057b2-134">Konfiguracja Konstruktora hosta jest tworzony przez wywołanie metody [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="057b2-135">`ConfigureHostConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla hosta.</span><span class="sxs-lookup"><span data-stu-id="057b2-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="057b2-136">Inicjuje konstruktora konfiguracji [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) do użycia w procesie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="057b2-137">Domyślnie nie jest dodawany konfiguracji zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="057b2-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="057b2-138">Wywołanie [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) na Konstruktor hosta, aby skonfigurować hosta z zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="057b2-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="057b2-139">`AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="057b2-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="057b2-140">Przykładowa aplikacja korzysta z prefiksem `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="057b2-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="057b2-141">Prefiks jest usuwany przeczytaniu zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="057b2-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="057b2-142">Jeśli została skonfigurowana Przykładowa aplikacja hosta, wartość zmiennej środowiskowej dla `PREFIX_ENVIRONMENT` staje się wartością konfiguracji hosta dla `environment` klucza.</span><span class="sxs-lookup"><span data-stu-id="057b2-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="057b2-143">Podczas tworzenia przy użyciu [programu Visual Studio](https://www.visualstudio.com/) lub uruchamiania aplikacji z `dotnet run`, zmienne środowiskowe może być ustawiona w *Properties/launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="057b2-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="057b2-144">W [Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe może być ustawiona w *.vscode/launch.json* plik w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="057b2-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="057b2-145">Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="057b2-145">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="057b2-146">`ConfigureHostConfiguration` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="057b2-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="057b2-147">Host używa jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="057b2-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="057b2-148">*hostsettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="057b2-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="057b2-149">Przykład `HostBuilder` konfiguracji przy użyciu `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="057b2-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="057b2-150">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="057b2-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="057b2-151">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="057b2-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="057b2-152">`AddConfiguration` Metoda oczekuje klucze do dopasowania `HostBuilder` kluczy (na przykład `environment`).</span><span class="sxs-lookup"><span data-stu-id="057b2-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="057b2-153">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta.</span><span class="sxs-lookup"><span data-stu-id="057b2-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="057b2-154">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="057b2-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="057b2-155">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="057b2-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="057b2-156">Konfiguracja — metoda rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="057b2-156">Extension method configuration</span></span>

<span data-ttu-id="057b2-157">Metody rozszerzenia są wywoływane na `IHostBuilder` implementacji, aby skonfigurować główny zawartości i środowiska.</span><span class="sxs-lookup"><span data-stu-id="057b2-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="057b2-158">Główny zawartości</span><span class="sxs-lookup"><span data-stu-id="057b2-158">Content Root</span></span>

<span data-ttu-id="057b2-159">To ustawienie określa, gdzie hosta rozpocznie się wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="057b2-159">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="057b2-160">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="057b2-160">**Key**: contentRoot</span></span>  
<span data-ttu-id="057b2-161">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="057b2-161">**Type**: *string*</span></span>  
<span data-ttu-id="057b2-162">**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-162">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="057b2-163">**Ustawić za pomocą**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="057b2-163">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="057b2-164">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowanych przez użytkownika](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="057b2-164">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="057b2-165">Jeśli ścieżka nie istnieje, host nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="057b2-165">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="057b2-166">Środowisko</span><span class="sxs-lookup"><span data-stu-id="057b2-166">Environment</span></span>

<span data-ttu-id="057b2-167">Ustawia aplikacji [środowiska](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="057b2-167">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="057b2-168">**Klucz**: środowiska</span><span class="sxs-lookup"><span data-stu-id="057b2-168">**Key**: environment</span></span>  
<span data-ttu-id="057b2-169">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="057b2-169">**Type**: *string*</span></span>  
<span data-ttu-id="057b2-170">**Domyślna**: produkcji</span><span class="sxs-lookup"><span data-stu-id="057b2-170">**Default**: Production</span></span>  
<span data-ttu-id="057b2-171">**Ustawić za pomocą**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="057b2-171">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="057b2-172">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowanych przez użytkownika](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="057b2-172">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="057b2-173">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="057b2-173">The environment can be set to any value.</span></span> <span data-ttu-id="057b2-174">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="057b2-174">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="057b2-175">Wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="057b2-175">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="057b2-176">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="057b2-176">ConfigureAppConfiguration</span></span>

<span data-ttu-id="057b2-177">Konfiguracja Konstruktora aplikacji jest tworzony przez wywołanie metody [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-177">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="057b2-178">`ConfigureAppConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-178">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="057b2-179">`ConfigureAppConfiguration` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="057b2-179">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="057b2-180">Aplikacja korzysta z jednego z tych opcji ustawia wartość ostatnio.</span><span class="sxs-lookup"><span data-stu-id="057b2-180">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="057b2-181">Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) dla kolejnych operacji i w [usług](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="057b2-181">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="057b2-182">Przykład aplikacji konfiguracji przy użyciu `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="057b2-182">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="057b2-183">*appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="057b2-183">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="057b2-184">*appSettings. Development.JSON*:</span><span class="sxs-lookup"><span data-stu-id="057b2-184">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="057b2-185">*appSettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="057b2-185">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="057b2-186">[AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="057b2-186">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="057b2-187">`GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="057b2-187">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="057b2-188">`AddConfiguration` Metoda oczekuje dokładnego dopasowania do konfiguracji kluczy (na przykład `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="057b2-188">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="057b2-189">Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-189">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="057b2-190">Ten problem zostanie rozwiązany w kolejnej wersji.</span><span class="sxs-lookup"><span data-stu-id="057b2-190">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="057b2-191">Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="057b2-191">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="057b2-192">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="057b2-192">ConfigureServices</span></span>

<span data-ttu-id="057b2-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) dodaje usług do aplikacji [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="057b2-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="057b2-194">`ConfigureServices` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="057b2-194">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="057b2-195">Hostowana usługa jest klasa z logiką zadania tła, który implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="057b2-195">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="057b2-196">Aby uzyskać więcej informacji, zobacz [zadania związane z usług hostowanych w tle](xref:fundamentals/host/hosted-services) tematu.</span><span class="sxs-lookup"><span data-stu-id="057b2-196">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="057b2-197">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` — metoda rozszerzenia, aby dodać usługę dla zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu, `TimedHostedService`, do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="057b2-197">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="057b2-198">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="057b2-198">ConfigureLogging</span></span>

<span data-ttu-id="057b2-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) dodaje delegata do konfigurowania udostępnionych [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="057b2-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="057b2-200">`ConfigureLogging` może zostać wywołana wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="057b2-200">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="057b2-201">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="057b2-201">UseConsoleLifetime</span></span>

<span data-ttu-id="057b2-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) nasłuchuje `Ctrl+C`/SIGINT lub sigterm — i wywołania [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) można uruchomić procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="057b2-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="057b2-203">`UseConsoleLifetime` Odblokowuje rozszerzenia takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="057b2-203">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="057b2-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) wstępnie jest zarejestrowany jako domyślna implementacja okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="057b2-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="057b2-205">Okres istnienia ostatniego zarejestrowany jest używany.</span><span class="sxs-lookup"><span data-stu-id="057b2-205">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="057b2-206">Kontenera konfiguracji</span><span class="sxs-lookup"><span data-stu-id="057b2-206">Container configuration</span></span>

<span data-ttu-id="057b2-207">Aby zapewnić obsługę podłączania innych kontenerów, host może zaakceptować [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="057b2-207">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="057b2-208">Udostępnia fabrykę nie jest częścią rejestracji kontenera Podpisane, ale zamiast tego jest używany do tworzenia konkretnych kontenera Podpisane hosta — wewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="057b2-208">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="057b2-209">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) zastępuje domyślną fabrykę używany do tworzenia aplikacji usługodawcy.</span><span class="sxs-lookup"><span data-stu-id="057b2-209">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="057b2-210">Niestandardowe kontenera konfiguracji jest zarządzana przez [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metody.</span><span class="sxs-lookup"><span data-stu-id="057b2-210">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="057b2-211">`ConfigureContainer` zapewnia obsługę jednoznacznie konfigurowania kontenera na górze odpowiedniego hosta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="057b2-211">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="057b2-212">`ConfigureContainer` może być wywołana wiele razy z dodatku wyników.</span><span class="sxs-lookup"><span data-stu-id="057b2-212">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="057b2-213">Utwórz kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="057b2-213">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="057b2-214">Podaj fabryki kontenera usług:</span><span class="sxs-lookup"><span data-stu-id="057b2-214">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="057b2-215">Użyj fabryka i skonfiguruj kontener usług niestandardowych dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="057b2-215">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="057b2-216">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="057b2-216">Extensibility</span></span>

<span data-ttu-id="057b2-217">Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="057b2-217">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="057b2-218">W poniższym przykładzie pokazano, jak rozszerza metodę rozszerzenia `IHostBuilder` implementację [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="057b2-218">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="057b2-219">Metoda rozszerzenia (w aplikacji) rejestruje RabbitMQ `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="057b2-219">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="057b2-220">Zarządzanie hosta</span><span class="sxs-lookup"><span data-stu-id="057b2-220">Manage the host</span></span>

<span data-ttu-id="057b2-221">[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie `IHostedService` implementacji, które są zarejestrowane w kontenerze usług.</span><span class="sxs-lookup"><span data-stu-id="057b2-221">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="057b2-222">Uruchom</span><span class="sxs-lookup"><span data-stu-id="057b2-222">Run</span></span>

<span data-ttu-id="057b2-223">[Uruchom](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uruchamia aplikację i blokuje wątek wywołujący, dopóki nie zostanie zamknięta hosta:</span><span class="sxs-lookup"><span data-stu-id="057b2-223">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="057b2-224">RunAsync</span><span class="sxs-lookup"><span data-stu-id="057b2-224">RunAsync</span></span>

<span data-ttu-id="057b2-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uruchamia aplikację i zwraca `Task` który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="057b2-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="057b2-226">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="057b2-226">RunConsoleAsync</span></span>

<span data-ttu-id="057b2-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) włącza obsługę konsoli, kompiluje i uruchamia hosta i czeka na `Ctrl+C`/SIGINT lub sigterm — w celu zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="057b2-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="057b2-228">Start i StopAsync</span><span class="sxs-lookup"><span data-stu-id="057b2-228">Start and StopAsync</span></span>

<span data-ttu-id="057b2-229">[Uruchom](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) synchronicznie uruchamia hosta.</span><span class="sxs-lookup"><span data-stu-id="057b2-229">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="057b2-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="057b2-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="057b2-231">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="057b2-231">StartAsync and StopAsync</span></span>

<span data-ttu-id="057b2-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="057b2-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) zatrzymuje aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="057b2-234">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="057b2-234">WaitForShutdown</span></span>

<span data-ttu-id="057b2-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) zostanie wywołany za pomocą [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), takich jak [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (nasłuchuje `Ctrl+C`/SIGINT lub sigterm —).</span><span class="sxs-lookup"><span data-stu-id="057b2-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="057b2-236">`WaitForShutdown` wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="057b2-236">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="057b2-237">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="057b2-237">WaitForShutdownAsync</span></span>

<span data-ttu-id="057b2-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) zwraca `Task` który zostaje ukończony po wyzwoleniu zamknięcia za pomocą danego tokenu i wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="057b2-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="057b2-239">Zewnętrzny</span><span class="sxs-lookup"><span data-stu-id="057b2-239">External control</span></span>

<span data-ttu-id="057b2-240">Formant zewnętrznego hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="057b2-240">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="057b2-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) nazywa się na początku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), który oczekuje jego zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="057b2-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="057b2-242">Może to służyć do opóźnienia uruchomienia aż zgłoszony przez zdarzenie zewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="057b2-242">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="057b2-243">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="057b2-243">IHostingEnvironment interface</span></span>

<span data-ttu-id="057b2-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) zapewnia środowiska macierzystego informacji o aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="057b2-245">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="057b2-245">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="057b2-246">Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="057b2-246">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="057b2-247">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="057b2-247">IApplicationLifetime interface</span></span>

<span data-ttu-id="057b2-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umożliwia po uruchamiania i wyłączania działań, włącznie z żądaniami łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="057b2-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="057b2-249">Anulowanie tokenów używany do rejestrowania są trzy właściwości w interfejsie `Action` metod, które definiują zdarzenia uruchamiania i wyłączania.</span><span class="sxs-lookup"><span data-stu-id="057b2-249">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="057b2-250">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="057b2-250">Cancellation Token</span></span> | <span data-ttu-id="057b2-251">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="057b2-251">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="057b2-252">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="057b2-252">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="057b2-253">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="057b2-253">The host has fully started.</span></span> |
| [<span data-ttu-id="057b2-254">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="057b2-254">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="057b2-255">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="057b2-255">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="057b2-256">Wszystkie żądania powinna zostać przetworzona.</span><span class="sxs-lookup"><span data-stu-id="057b2-256">All requests should be processed.</span></span> <span data-ttu-id="057b2-257">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="057b2-257">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="057b2-258">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="057b2-258">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="057b2-259">Host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="057b2-259">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="057b2-260">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="057b2-260">Requests may still be processing.</span></span> <span data-ttu-id="057b2-261">Bloki zamknięcia aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="057b2-261">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="057b2-262">Wstaw konstruktora `IApplicationLifetime` usługi do żadnej klasy.</span><span class="sxs-lookup"><span data-stu-id="057b2-262">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="057b2-263">[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji contructor do `LifetimeEventsHostedService` klasy ( `IHostedService` implementacji) można zarejestrować zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="057b2-263">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="057b2-264">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="057b2-264">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="057b2-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) żąda zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="057b2-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="057b2-266">Następujące klasy używa `StopApplication` można bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="057b2-266">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="057b2-267">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="057b2-267">Additional resources</span></span>

* [<span data-ttu-id="057b2-268">Zadania w tle z usługami hostowanymi</span><span class="sxs-lookup"><span data-stu-id="057b2-268">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="057b2-269">Hosting przykłady repozytorium w witrynie GitHub</span><span class="sxs-lookup"><span data-stu-id="057b2-269">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
