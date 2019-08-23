---
title: Host ogólny .NET
author: tdykstra
description: Dowiedz się więcej o hoście ogólnym programu .NET Core, który jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 9f5ecc7840fc7ffd9432a3bb67d0418efb7e8fd6
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975616"
---
# <a name="net-generic-host"></a><span data-ttu-id="7a51f-103">Host ogólny .NET</span><span class="sxs-lookup"><span data-stu-id="7a51f-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7a51f-104">W tym artykule przedstawiono hosta ogólnego platformy .NET Core<xref:Microsoft.Extensions.Hosting.HostBuilder>() i przedstawiono wskazówki dotyczące korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="7a51f-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="7a51f-105">Co to jest host?</span><span class="sxs-lookup"><span data-stu-id="7a51f-105">What's a host?</span></span>

<span data-ttu-id="7a51f-106">*Host* to obiekt, który hermetyzuje zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="7a51f-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="7a51f-107">Iniekcja zależności (DI)</span><span class="sxs-lookup"><span data-stu-id="7a51f-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="7a51f-108">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="7a51f-108">Logging</span></span>
* <span data-ttu-id="7a51f-109">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="7a51f-109">Configuration</span></span>
* <span data-ttu-id="7a51f-110">`IHostedService`metod</span><span class="sxs-lookup"><span data-stu-id="7a51f-110">`IHostedService` implementations</span></span>

<span data-ttu-id="7a51f-111">Po uruchomieniu hosta wywołuje `IHostedService.StartAsync` on każdą <xref:Microsoft.Extensions.Hosting.IHostedService> implementację, która znajduje się w kontenerze di.</span><span class="sxs-lookup"><span data-stu-id="7a51f-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="7a51f-112">W aplikacji sieci Web jedną z `IHostedService` implementacji jest usługa sieci Web, która uruchamia implementację [serwera http](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="7a51f-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="7a51f-113">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7a51f-114">W wersjach ASP.NET Core wcześniejszych niż 3,0 [host sieci Web](xref:fundamentals/host/web-host) jest używany do obciążeń http.</span><span class="sxs-lookup"><span data-stu-id="7a51f-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="7a51f-115">Host sieci Web nie jest już zalecany dla aplikacji sieci Web i pozostaje dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="7a51f-116">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="7a51f-116">Set up a host</span></span>

<span data-ttu-id="7a51f-117">Host jest zazwyczaj konfigurowany, zbudowany i uruchamiany przez kod w `Program` klasie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="7a51f-118">`Main` Metody:</span><span class="sxs-lookup"><span data-stu-id="7a51f-118">The `Main` method:</span></span>

* <span data-ttu-id="7a51f-119">`CreateHostBuilder` Wywołuje metodę, aby utworzyć i skonfigurować obiekt konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7a51f-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="7a51f-120">Wywołania `Build` i`Run` metody w obiekcie konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7a51f-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="7a51f-121">Oto kod *program.cs* dla obciążenia innego niż HTTP z jedną `IHostedService` implementacją dodaną do kontenera di.</span><span class="sxs-lookup"><span data-stu-id="7a51f-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="7a51f-122">W przypadku obciążeń `Main` http Metoda jest taka sama, ale `CreateHostBuilder` wywołuje `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="7a51f-123">Jeśli aplikacja używa Entity Framework Core, nie zmieniaj nazwy ani podpisu `CreateHostBuilder` metody.</span><span class="sxs-lookup"><span data-stu-id="7a51f-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="7a51f-124">[Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) oczekują na znalezienie `CreateHostBuilder` metody, która konfiguruje hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="7a51f-125">Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="7a51f-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="7a51f-126">Ustawienia domyślnego konstruktora</span><span class="sxs-lookup"><span data-stu-id="7a51f-126">Default builder settings</span></span> 

<span data-ttu-id="7a51f-127"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> Metody:</span><span class="sxs-lookup"><span data-stu-id="7a51f-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="7a51f-128">Ustawia katalog główny zawartości na ścieżkę zwracaną przez <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-128">Sets the content root to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="7a51f-129">Ładuje konfigurację hosta z:</span><span class="sxs-lookup"><span data-stu-id="7a51f-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="7a51f-130">Zmienne środowiskowe poprzedzone prefiksem "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="7a51f-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="7a51f-131">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-131">Command-line arguments.</span></span>
* <span data-ttu-id="7a51f-132">Ładuje konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="7a51f-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="7a51f-133">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7a51f-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="7a51f-134">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="7a51f-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="7a51f-135">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana `Development` w środowisku.</span><span class="sxs-lookup"><span data-stu-id="7a51f-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="7a51f-136">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="7a51f-136">Environment variables.</span></span>
  * <span data-ttu-id="7a51f-137">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-137">Command-line arguments.</span></span>
* <span data-ttu-id="7a51f-138">Dodaje następujących dostawców [rejestrowania](xref:fundamentals/logging/index) :</span><span class="sxs-lookup"><span data-stu-id="7a51f-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="7a51f-139">Konsola</span><span class="sxs-lookup"><span data-stu-id="7a51f-139">Console</span></span>
  * <span data-ttu-id="7a51f-140">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="7a51f-140">Debug</span></span>
  * <span data-ttu-id="7a51f-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="7a51f-141">EventSource</span></span>
  * <span data-ttu-id="7a51f-142">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="7a51f-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="7a51f-143">Umożliwia [weryfikację zakresu](xref:fundamentals/dependency-injection#scope-validation) i [Sprawdzanie poprawności zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) , gdy środowisko jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="7a51f-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="7a51f-144">`ConfigureWebHostDefaults` Metody:</span><span class="sxs-lookup"><span data-stu-id="7a51f-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="7a51f-145">Ładuje konfigurację hosta ze zmiennych środowiskowych poprzedzonych prefiksem "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="7a51f-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="7a51f-146">Ustawia serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web i konfiguruje go przy użyciu dostawców konfiguracji hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="7a51f-147">Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="7a51f-148">Dodaje [oprogramowanie pośredniczące do filtrowania hosta](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="7a51f-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="7a51f-149">Dodaje [przekazane nagłówki pośredniczące](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) , jeśli ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.</span><span class="sxs-lookup"><span data-stu-id="7a51f-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="7a51f-150">Włącza integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="7a51f-150">Enables IIS integration.</span></span> <span data-ttu-id="7a51f-151">Aby poznać domyślne opcje usług IIS, <xref:host-and-deploy/iis/index#iis-options>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="7a51f-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="7a51f-152">[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [Ustawienia dla usługi Web Apps](#settings-for-web-apps) w dalszej części tego artykułu pokazują, jak zastąpić ustawienia domyślnego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7a51f-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="7a51f-153">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="7a51f-153">Framework-provided services</span></span>

<span data-ttu-id="7a51f-154">Usługi zarejestrowane automatycznie obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="7a51f-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="7a51f-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="7a51f-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="7a51f-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="7a51f-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="7a51f-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="7a51f-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="7a51f-158">Aby zapoznać się z listą wszystkich usług udostępnianych przez <xref:fundamentals/dependency-injection#framework-provided-services>platformę, zobacz.</span><span class="sxs-lookup"><span data-stu-id="7a51f-158">For a list of all framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="7a51f-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="7a51f-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="7a51f-160">`IApplicationLifetime`Wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej) do dowolnej klasy w celu obsługi zadań po uruchomieniu i bezpiecznego zamykania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="7a51f-161">Trzy właściwości interfejsu to tokeny anulowania używane do rejestrowania metod obsługi zdarzeń uruchamiania i zatrzymywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="7a51f-162">Interfejs zawiera `StopApplication` również metodę.</span><span class="sxs-lookup"><span data-stu-id="7a51f-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="7a51f-163">Poniższy przykład to `IHostedService` implementacja, która `IApplicationLifetime` rejestruje zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="7a51f-163">The following example is an `IHostedService` implementation that registers the `IApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="7a51f-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="7a51f-164">IHostLifetime</span></span>

<span data-ttu-id="7a51f-165"><xref:Microsoft.Extensions.Hosting.IHostLifetime> Implementacja kontroluje moment uruchomienia hosta i jego zatrzymania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="7a51f-166">Używana jest Ostatnia zarejestrowana implementacja.</span><span class="sxs-lookup"><span data-stu-id="7a51f-166">The last implementation registered is used.</span></span>

<span data-ttu-id="7a51f-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>jest implementacją `IHostLifetime` domyślną.</span><span class="sxs-lookup"><span data-stu-id="7a51f-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="7a51f-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="7a51f-169">nasłuchuje kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> i wywołań, aby rozpocząć proces zamykania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="7a51f-170">Odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="7a51f-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="7a51f-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="7a51f-171">IHostEnvironment</span></span>

<span data-ttu-id="7a51f-172"><xref:Microsoft.Extensions.Hosting.IHostEnvironment> Wsuń usługę do klasy, aby uzyskać informacje o następujących elementach:</span><span class="sxs-lookup"><span data-stu-id="7a51f-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="7a51f-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="7a51f-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="7a51f-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="7a51f-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="7a51f-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="7a51f-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="7a51f-176">Aplikacje sieci Web implementują `IWebHostEnvironment` interfejs, który dziedziczy `IHostEnvironment` i dodaje:</span><span class="sxs-lookup"><span data-stu-id="7a51f-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="7a51f-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="7a51f-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="7a51f-178">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="7a51f-178">Host configuration</span></span>

<span data-ttu-id="7a51f-179">Konfiguracja hosta jest używana we właściwościach <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="7a51f-180">Konfiguracja hosta jest dostępna z [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) wewnątrz <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="7a51f-181">Po `ConfigureAppConfiguration` program`HostBuilderContext.Configuration` zostanie zastąpiony konfiguracją aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="7a51f-182">Aby dodać konfigurację hosta, <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> Wywołaj `IHostBuilder`polecenie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="7a51f-183">`ConfigureHostConfiguration`może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7a51f-184">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="7a51f-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="7a51f-185">Dostawca zmiennych środowiskowych z prefiksem `DOTNET_` i argumentami wiersza polecenia są dołączone przez CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="7a51f-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="7a51f-186">W przypadku aplikacji sieci Web jest dodawany dostawca zmiennych środowiskowych z prefiksem `ASPNETCORE_` .</span><span class="sxs-lookup"><span data-stu-id="7a51f-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="7a51f-187">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="7a51f-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="7a51f-188">Na przykład wartość `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej jest wartością konfiguracji hosta `environment` dla klucza.</span><span class="sxs-lookup"><span data-stu-id="7a51f-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="7a51f-189">Poniższy przykład umożliwia utworzenie konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="7a51f-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="7a51f-190">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="7a51f-190">App configuration</span></span>

<span data-ttu-id="7a51f-191">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> metody `IHostBuilder`on.</span><span class="sxs-lookup"><span data-stu-id="7a51f-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="7a51f-192">`ConfigureAppConfiguration`może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7a51f-193">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="7a51f-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="7a51f-194">Konfiguracja utworzona przez `ConfigureAppConfiguration` program jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i jako usługa z programu di.</span><span class="sxs-lookup"><span data-stu-id="7a51f-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="7a51f-195">Konfiguracja hosta jest również dodawana do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="7a51f-196">Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="7a51f-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="7a51f-197">Ustawienia dla wszystkich typów aplikacji</span><span class="sxs-lookup"><span data-stu-id="7a51f-197">Settings for all app types</span></span>

<span data-ttu-id="7a51f-198">Ta sekcja zawiera listę ustawień hosta, które dotyczą zarówno obciążeń HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a51f-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="7a51f-199">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć `DOTNET_` prefiks lub. `ASPNETCORE_`</span><span class="sxs-lookup"><span data-stu-id="7a51f-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

### <a name="applicationname"></a><span data-ttu-id="7a51f-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="7a51f-200">ApplicationName</span></span>

<span data-ttu-id="7a51f-201">Właściwość [IHostEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="7a51f-202">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="7a51f-202">**Key**: applicationName</span></span>  
<span data-ttu-id="7a51f-203">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-203">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-204">**Wartość domyślna**: Nazwa zestawu, który zawiera punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="7a51f-205">**Zmienna środowiskowa**:`<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="7a51f-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="7a51f-206">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="7a51f-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="7a51f-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="7a51f-207">ContentRootPath</span></span>

<span data-ttu-id="7a51f-208">Właściwość [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="7a51f-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="7a51f-209">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="7a51f-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="7a51f-210">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="7a51f-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="7a51f-211">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-211">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-212">**Wartość domyślna**: Folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="7a51f-213">**Zmienna środowiskowa**:`<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="7a51f-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="7a51f-214">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseContentRoot` Wywołaj `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

### <a name="environmentname"></a><span data-ttu-id="7a51f-215">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="7a51f-215">EnvironmentName</span></span>

<span data-ttu-id="7a51f-216">Dla właściwości [IHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="7a51f-216">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="7a51f-217">Wartości zdefiniowane przez platformę `Development`obejmują `Staging`,, `Production`i.</span><span class="sxs-lookup"><span data-stu-id="7a51f-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="7a51f-218">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="7a51f-218">Values aren't case sensitive.</span></span>

<span data-ttu-id="7a51f-219">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="7a51f-219">**Key**: environment</span></span>  
<span data-ttu-id="7a51f-220">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-220">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-221">**Wartość domyślna**: Narzędzi</span><span class="sxs-lookup"><span data-stu-id="7a51f-221">**Default**: Production</span></span>  
<span data-ttu-id="7a51f-222">**Zmienna środowiskowa**:`<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="7a51f-222">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="7a51f-223">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseEnvironment` Wywołaj `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-223">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="7a51f-224">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="7a51f-224">ShutdownTimeout</span></span>

<span data-ttu-id="7a51f-225">[HostOptions. shutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="7a51f-226">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="7a51f-226">The default value is five seconds.</span></span>  <span data-ttu-id="7a51f-227">Podczas okresu przekroczenia limitu czasu Host:</span><span class="sxs-lookup"><span data-stu-id="7a51f-227">During the timeout period, the host:</span></span>

* <span data-ttu-id="7a51f-228">Wyzwala [IHostApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="7a51f-228">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="7a51f-229">Próbuje zatrzymać usługi hostowane, rejestrowanie błędów dla usług, których zatrzymanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="7a51f-229">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="7a51f-230">Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-230">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="7a51f-231">Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-231">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="7a51f-232">Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="7a51f-232">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="7a51f-233">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="7a51f-233">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="7a51f-234">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="7a51f-234">**Type**: *int*</span></span>  
<span data-ttu-id="7a51f-235">**Wartość domyślna**: **zmienna środowiskowa**5 sekund:`<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="7a51f-235">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="7a51f-236">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub skonfiguruj `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="7a51f-236">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="7a51f-237">Poniższy przykład ustawia limit czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="7a51f-237">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="7a51f-238">Ustawienia usługi Web Apps</span><span class="sxs-lookup"><span data-stu-id="7a51f-238">Settings for web apps</span></span>

<span data-ttu-id="7a51f-239">Niektóre ustawienia hosta mają zastosowanie tylko do obciążeń protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a51f-239">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="7a51f-240">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć `DOTNET_` prefiks lub. `ASPNETCORE_`</span><span class="sxs-lookup"><span data-stu-id="7a51f-240">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="7a51f-241">Metody rozszerzenia dla `IWebHostBuilder` programu są dostępne dla tych ustawień.</span><span class="sxs-lookup"><span data-stu-id="7a51f-241">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="7a51f-242">Przykłady kodu, które pokazują, jak wywołać metody `webBuilder` rozszerzane jest `IWebHostBuilder`wystąpieniem, tak jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="7a51f-242">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="7a51f-243">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="7a51f-243">CaptureStartupErrors</span></span>

<span data-ttu-id="7a51f-244">Gdy `false`, błędy podczas uruchamiania, kończy się hostem.</span><span class="sxs-lookup"><span data-stu-id="7a51f-244">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="7a51f-245">Gdy `true`Host przechwytuje wyjątki podczas uruchamiania, a następnie próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="7a51f-245">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="7a51f-246">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="7a51f-246">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="7a51f-247">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="7a51f-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7a51f-248">**Wartość domyślna**: Wartość domyślna to, `true` chybażeaplikacjabędziedziałaćzKestrelzausługamiIIS,gdziedomyślniejestto.`false`</span><span class="sxs-lookup"><span data-stu-id="7a51f-248">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="7a51f-249">**Zmienna środowiskowa**:`<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="7a51f-249">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="7a51f-250">Aby ustawić tę wartość, użyj konfiguracji lub wywołania `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-250">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="7a51f-251">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="7a51f-251">DetailedErrors</span></span>

<span data-ttu-id="7a51f-252">Po włączeniu lub gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe błędy.</span><span class="sxs-lookup"><span data-stu-id="7a51f-252">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="7a51f-253">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="7a51f-253">**Key**: detailedErrors</span></span>  
<span data-ttu-id="7a51f-254">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="7a51f-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7a51f-255">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="7a51f-255">**Default**: false</span></span>  
<span data-ttu-id="7a51f-256">**Zmienna środowiskowa**:`<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="7a51f-256">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="7a51f-257">Aby ustawić tę wartość, użyj konfiguracji lub wywołania `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-257">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="7a51f-258">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="7a51f-258">HostingStartupAssemblies</span></span>

<span data-ttu-id="7a51f-259">Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-259">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="7a51f-260">Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-260">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="7a51f-261">W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-261">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="7a51f-262">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="7a51f-262">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="7a51f-263">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-263">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-264">**Wartość domyślna**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="7a51f-264">**Default**: Empty string</span></span>  
<span data-ttu-id="7a51f-265">**Zmienna środowiskowa**:`<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="7a51f-265">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="7a51f-266">Aby ustawić tę wartość, użyj konfiguracji lub wywołania `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-266">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="7a51f-267">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="7a51f-267">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="7a51f-268">Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-268">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="7a51f-269">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="7a51f-269">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="7a51f-270">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-270">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-271">**Wartość domyślna**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="7a51f-271">**Default**: Empty string</span></span>  
<span data-ttu-id="7a51f-272">**Zmienna środowiskowa**:`<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="7a51f-272">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="7a51f-273">Aby ustawić tę wartość, użyj konfiguracji lub wywołania `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-273">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="7a51f-274">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="7a51f-274">HTTPS_Port</span></span>

<span data-ttu-id="7a51f-275">Port przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7a51f-275">The HTTPS redirect port.</span></span> <span data-ttu-id="7a51f-276">Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="7a51f-276">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="7a51f-277">**Key**: https_port **Type**:
**wartość domyślna**: Nie ustawiono wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="7a51f-277">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="7a51f-278">**Zmienna środowiskowa**:`<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="7a51f-278">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="7a51f-279">Aby ustawić tę wartość, użyj konfiguracji lub wywołania `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="7a51f-280">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="7a51f-280">PreferHostingUrls</span></span>

<span data-ttu-id="7a51f-281">Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy `IWebHostBuilder` użyciu zamiast tych skonfigurowanych `IServer` przy użyciu implementacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-281">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="7a51f-282">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="7a51f-282">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="7a51f-283">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="7a51f-283">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7a51f-284">Wartość **Domyślna**: prawda</span><span class="sxs-lookup"><span data-stu-id="7a51f-284">**Default**: true</span></span>  
<span data-ttu-id="7a51f-285">**Zmienna środowiskowa**:`<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="7a51f-285">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="7a51f-286">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-286">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="7a51f-287">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7a51f-287">PreventHostingStartup</span></span>

<span data-ttu-id="7a51f-288">Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-288">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="7a51f-289">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-289">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="7a51f-290">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7a51f-290">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="7a51f-291">**Typ**: *bool* (`true` or `1`)</span><span class="sxs-lookup"><span data-stu-id="7a51f-291">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7a51f-292">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="7a51f-292">**Default**: false</span></span>  
<span data-ttu-id="7a51f-293">**Zmienna środowiskowa**:`<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="7a51f-293">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="7a51f-294">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="7a51f-294">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="7a51f-295">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="7a51f-295">StartupAssembly</span></span>

<span data-ttu-id="7a51f-296">Zestaw do wyszukiwania `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="7a51f-296">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="7a51f-297">**Klucz**: startupAssembly **Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-297">**Key**: startupAssembly **Type**: *string*</span></span>  
<span data-ttu-id="7a51f-298">**Wartość domyślna**: Zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="7a51f-298">**Default**: The app's assembly</span></span>  
<span data-ttu-id="7a51f-299">**Zmienna środowiskowa**:`<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="7a51f-299">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="7a51f-300">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołania `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="7a51f-300">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="7a51f-301">`UseStartup`może przyjmować nazwę zestawu (`string`) lub typ (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="7a51f-301">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="7a51f-302">Jeśli wywoływana `UseStartup` jest wiele metod, pierwszeństwo ma Ostatnia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-302">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="7a51f-303">adresy URL</span><span class="sxs-lookup"><span data-stu-id="7a51f-303">URLs</span></span>

<span data-ttu-id="7a51f-304">Rozdzielana średnikami lista adresów IP lub adresów hostów z portami i protokołami, na których serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="7a51f-304">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="7a51f-305">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="7a51f-305">For example, `http://localhost:123`.</span></span> <span data-ttu-id="7a51f-306">Użyj "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu ( `http://*:5000`na przykład).</span><span class="sxs-lookup"><span data-stu-id="7a51f-306">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="7a51f-307">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7a51f-307">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="7a51f-308">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-308">Supported formats vary among servers.</span></span>

<span data-ttu-id="7a51f-309">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="7a51f-309">**Key**: urls</span></span>  
<span data-ttu-id="7a51f-310">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-310">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-311">**Wartość domyślna** `http://localhost:5000` : `https://localhost:5001`i zmienna
 **środowiskowa**:`<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="7a51f-311">**Default**: `http://localhost:5000` and `https://localhost:5001`
**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="7a51f-312">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-312">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="7a51f-313">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="7a51f-313">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="7a51f-314">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-314">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="7a51f-315">WebRoot</span><span class="sxs-lookup"><span data-stu-id="7a51f-315">WebRoot</span></span>

<span data-ttu-id="7a51f-316">Ścieżka względna do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-316">The relative path to the app's static assets.</span></span>

<span data-ttu-id="7a51f-317">**Klucz**: Webroot</span><span class="sxs-lookup"><span data-stu-id="7a51f-317">**Key**: webroot</span></span>  
<span data-ttu-id="7a51f-318">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-318">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-319">**Wartość domyślna**: *(Katalog zawartości)/wwwroot*, jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="7a51f-319">**Default**: *(Content Root)/wwwroot*, if the path exists.</span></span> <span data-ttu-id="7a51f-320">Jeśli ścieżka nie istnieje, jest używany dostawca plików No-op.</span><span class="sxs-lookup"><span data-stu-id="7a51f-320">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="7a51f-321">**Zmienna środowiskowa**:`<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="7a51f-321">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="7a51f-322">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="7a51f-322">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="7a51f-323">Zarządzanie okresem istnienia hosta</span><span class="sxs-lookup"><span data-stu-id="7a51f-323">Manage the host lifetime</span></span>

<span data-ttu-id="7a51f-324">Wywołaj metody z skompilowanej <xref:Microsoft.Extensions.Hosting.IHost> implementacji, aby uruchomić i zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="7a51f-324">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="7a51f-325">Te metody mają wpływ <xref:Microsoft.Extensions.Hosting.IHostedService> na wszystkie implementacje zarejestrowane w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="7a51f-325">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="7a51f-326">Uruchom</span><span class="sxs-lookup"><span data-stu-id="7a51f-326">Run</span></span>

<span data-ttu-id="7a51f-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*>uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="7a51f-328">RunAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-328">RunAsync</span></span>

<span data-ttu-id="7a51f-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*>uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task> , która kończy się po wyzwoleniu tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="7a51f-330">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-330">RunConsoleAsync</span></span>

<span data-ttu-id="7a51f-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>włącza obsługę konsoli, kompiluje i uruchamia hosta i czeka na zamknięcie klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="7a51f-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="7a51f-332">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="7a51f-332">Start</span></span>

<span data-ttu-id="7a51f-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*>uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="7a51f-334">StartAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-334">StartAsync</span></span>

<span data-ttu-id="7a51f-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task> , który kończy się po wyzwoleniu tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="7a51f-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>jest wywoływana na początku `StartAsync`, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="7a51f-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="7a51f-337">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="7a51f-337">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="7a51f-338">StopAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-338">StopAsync</span></span>

<span data-ttu-id="7a51f-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="7a51f-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="7a51f-340">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="7a51f-340">WaitForShutdown</span></span>

<span data-ttu-id="7a51f-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>blokuje wątek wywołujący do momentu wyzwolenia przez IHostLifetime, na przykład za pośrednictwem kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="7a51f-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="7a51f-342">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-342">WaitForShutdownAsync</span></span>

<span data-ttu-id="7a51f-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>Zwraca wartość <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>, która kończy się, gdy zamknięcie jest wyzwalane za pośrednictwem danego tokenu i wywołań. <xref:System.Threading.Tasks.Task></span><span class="sxs-lookup"><span data-stu-id="7a51f-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="7a51f-344">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="7a51f-344">External control</span></span>

<span data-ttu-id="7a51f-345">Bezpośrednią kontrolę nad okresem istnienia hosta można uzyskać przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="7a51f-345">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7a51f-346">ASP.NET Core aplikacje konfigurują i uruchamiają hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-346">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="7a51f-347">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-347">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="7a51f-348">W tym artykule opisano ASP.NET Core hosta ogólnego (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a51f-348">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="7a51f-349">Przeznaczeniem hosta ogólnego jest oddzielenie potoku HTTP od interfejsu API hosta sieci Web w celu umożliwienia szerszej tablicy scenariuszy hostów.</span><span class="sxs-lookup"><span data-stu-id="7a51f-349">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="7a51f-350">Obsługa komunikatów, zadań w tle i innych obciążeń innych niż HTTP na podstawie ogólnej korzyści z hosta, takich jak konfiguracja, iniekcja zależności (DI) i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-350">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="7a51f-351">Host ogólny jest nowy w ASP.NET Core 2,1 i nie jest odpowiedni dla scenariuszy hostingu w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7a51f-351">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="7a51f-352">W przypadku scenariuszy hostingu w sieci Web należy użyć [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="7a51f-352">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="7a51f-353">Host ogólny zastąpi hosta sieci Web w przyszłej wersji i będzie pełnić rolę podstawowego interfejsu API hosta zarówno w scenariuszach HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="7a51f-353">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="7a51f-354">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7a51f-354">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7a51f-355">Podczas uruchamiania przykładowej aplikacji w [Visual Studio Code](https://code.visualstudio.com/)należy użyć *zewnętrznego lub zintegrowanego terminalu*.</span><span class="sxs-lookup"><span data-stu-id="7a51f-355">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="7a51f-356">Nie uruchamiaj próbki w `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="7a51f-356">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="7a51f-357">Aby ustawić konsolę w Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="7a51f-357">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="7a51f-358">Otwórz plik *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7a51f-358">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="7a51f-359">W konfiguracji **uruchamiania programu .NET Core (konsoli)** zlokalizuj wpis **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="7a51f-359">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="7a51f-360">Ustaw wartość na `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="7a51f-360">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="7a51f-361">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="7a51f-361">Introduction</span></span>

<span data-ttu-id="7a51f-362">Ogólna Biblioteka hostów jest dostępna w <xref:Microsoft.Extensions.Hosting> przestrzeni nazw i udostępniana przez pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) .</span><span class="sxs-lookup"><span data-stu-id="7a51f-362">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="7a51f-363">Pakiet jest zawarty w [pakiecie Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszym). `Microsoft.Extensions.Hosting`</span><span class="sxs-lookup"><span data-stu-id="7a51f-363">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="7a51f-364"><xref:Microsoft.Extensions.Hosting.IHostedService>jest punktem wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="7a51f-364"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="7a51f-365">Każda <xref:Microsoft.Extensions.Hosting.IHostedService> implementacja jest wykonywana w kolejności [rejestracji usługi w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="7a51f-365">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="7a51f-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>jest wywoływana przy każdym <xref:Microsoft.Extensions.Hosting.IHostedService> uruchomieniu hosta i <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w odwrotnej kolejności rejestracji, gdy host jest zamykany prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="7a51f-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="7a51f-367">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="7a51f-367">Set up a host</span></span>

<span data-ttu-id="7a51f-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder>jest głównym składnikiem używanym przez biblioteki i aplikacje do inicjowania, kompilowania i uruchamiania hosta:</span><span class="sxs-lookup"><span data-stu-id="7a51f-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="7a51f-369">Opcje</span><span class="sxs-lookup"><span data-stu-id="7a51f-369">Options</span></span>

<span data-ttu-id="7a51f-370"><xref:Microsoft.Extensions.Hosting.HostOptions>Skonfiguruj opcje <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-370"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="7a51f-371">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="7a51f-371">Shutdown timeout</span></span>

<span data-ttu-id="7a51f-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="7a51f-373">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="7a51f-373">The default value is five seconds.</span></span>

<span data-ttu-id="7a51f-374">Następująca konfiguracja opcji w programie `Program.Main` zwiększa domyślny pięć sekund limitu czasu zamykania na 20 s:</span><span class="sxs-lookup"><span data-stu-id="7a51f-374">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="7a51f-375">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="7a51f-375">Default services</span></span>

<span data-ttu-id="7a51f-376">Podczas inicjowania hosta zarejestrowano następujące usługi:</span><span class="sxs-lookup"><span data-stu-id="7a51f-376">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="7a51f-377">[Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="7a51f-377">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="7a51f-378">[Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="7a51f-378">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="7a51f-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="7a51f-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="7a51f-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="7a51f-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="7a51f-381">[Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="7a51f-381">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="7a51f-382">[Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="7a51f-382">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="7a51f-383">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="7a51f-383">Host configuration</span></span>

<span data-ttu-id="7a51f-384">Konfiguracja hosta jest tworzona przez:</span><span class="sxs-lookup"><span data-stu-id="7a51f-384">Host configuration is created by:</span></span>

* <span data-ttu-id="7a51f-385">Wywoływanie metod rozszerzenia <xref:Microsoft.Extensions.Hosting.IHostBuilder> w celu ustawienia [katalogu głównego zawartości](#content-root) i [środowiska](#environment).</span><span class="sxs-lookup"><span data-stu-id="7a51f-385">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="7a51f-386">Odczytywanie konfiguracji od dostawców konfiguracji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>w programie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-386">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="7a51f-387">Metody rozszerzające</span><span class="sxs-lookup"><span data-stu-id="7a51f-387">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="7a51f-388">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="7a51f-388">Application key (name)</span></span>

<span data-ttu-id="7a51f-389">Właściwość [IHostingEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-389">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="7a51f-390">Aby jawnie ustawić wartość, użyj [HostDefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="7a51f-390">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="7a51f-391">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="7a51f-391">**Key**: applicationName</span></span>  
<span data-ttu-id="7a51f-392">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-392">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-393">**Wartość domyślna**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-393">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="7a51f-394">**Ustaw przy użyciu**:`HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="7a51f-394">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="7a51f-395">**Zmienna**środowiskowa `<PREFIX_>APPLICATIONNAME` :`<PREFIX_>` (jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="7a51f-395">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="7a51f-396">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="7a51f-396">Content root</span></span>

<span data-ttu-id="7a51f-397">To ustawienie określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="7a51f-397">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="7a51f-398">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="7a51f-398">**Key**: contentRoot</span></span>  
<span data-ttu-id="7a51f-399">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-399">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-400">**Wartość domyślna**: Domyślnie znajduje się w folderze, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-400">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="7a51f-401">**Ustaw przy użyciu**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="7a51f-401">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="7a51f-402">**Zmienna**środowiskowa `<PREFIX_>CONTENTROOT` :`<PREFIX_>` (jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="7a51f-402">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="7a51f-403">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="7a51f-403">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="7a51f-404">Środowisko</span><span class="sxs-lookup"><span data-stu-id="7a51f-404">Environment</span></span>

<span data-ttu-id="7a51f-405">Ustawia [środowisko](xref:fundamentals/environments)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-405">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="7a51f-406">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="7a51f-406">**Key**: environment</span></span>  
<span data-ttu-id="7a51f-407">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="7a51f-407">**Type**: *string*</span></span>  
<span data-ttu-id="7a51f-408">**Wartość domyślna**: Narzędzi</span><span class="sxs-lookup"><span data-stu-id="7a51f-408">**Default**: Production</span></span>  
<span data-ttu-id="7a51f-409">**Ustaw przy użyciu**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="7a51f-409">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="7a51f-410">**Zmienna**środowiskowa `<PREFIX_>ENVIRONMENT` :`<PREFIX_>` (jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="7a51f-410">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="7a51f-411">Dla środowiska można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="7a51f-411">The environment can be set to any value.</span></span> <span data-ttu-id="7a51f-412">Wartości zdefiniowane przez platformę `Development`obejmują `Staging`,, `Production`i.</span><span class="sxs-lookup"><span data-stu-id="7a51f-412">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="7a51f-413">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="7a51f-413">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="7a51f-414">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="7a51f-414">ConfigureHostConfiguration</span></span>

<span data-ttu-id="7a51f-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*><xref:Microsoft.Extensions.Configuration.IConfiguration> <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> za pomocą programu można utworzyć dla hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="7a51f-416">Konfiguracja hosta służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-416">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="7a51f-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="7a51f-418">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="7a51f-418">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="7a51f-419">Żaden dostawca nie jest domyślnie uwzględniany.</span><span class="sxs-lookup"><span data-stu-id="7a51f-419">No providers are included by default.</span></span> <span data-ttu-id="7a51f-420">Należy jawnie określić dostawców konfiguracji, których wymaga aplikacja, w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>tym:</span><span class="sxs-lookup"><span data-stu-id="7a51f-420">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="7a51f-421">Konfiguracja pliku (na przykład z pliku *HostSettings. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="7a51f-421">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="7a51f-422">Konfiguracja zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="7a51f-422">Environment variable configuration.</span></span>
* <span data-ttu-id="7a51f-423">Konfiguracja argumentu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-423">Command-line argument configuration.</span></span>
* <span data-ttu-id="7a51f-424">Każdy inny wymagany dostawca konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-424">Any other required configuration providers.</span></span>

<span data-ttu-id="7a51f-425">Konfiguracja pliku hosta jest włączana przez określenie ścieżki `SetBasePath` podstawowej aplikacji, po której następuje wywołanie jednego z [dostawców konfiguracji plików](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="7a51f-425">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="7a51f-426">Przykładowa aplikacja używa pliku JSON, *HostSettings. JSON*oraz wywołań <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> , aby użyć ustawień konfiguracji hosta pliku.</span><span class="sxs-lookup"><span data-stu-id="7a51f-426">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="7a51f-427">Aby dodać [konfigurację zmiennej środowiskowej](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, wywołaj <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> polecenie na konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-427">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="7a51f-428">`AddEnvironmentVariables`akceptuje opcjonalny prefiks zdefiniowany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7a51f-428">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="7a51f-429">Przykładowa aplikacja używa prefiksu `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="7a51f-429">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="7a51f-430">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="7a51f-430">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="7a51f-431">Po skonfigurowaniu hosta przykładowej aplikacji wartość zmiennej środowiskowej dla `PREFIX_ENVIRONMENT` `environment` klucza będzie wartością konfiguracji hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-431">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="7a51f-432">Podczas programowania podczas korzystania z [programu Visual Studio](https://visualstudio.microsoft.com) lub uruchamiania aplikacji `dotnet run`w programie zmienne środowiskowe mogą być ustawiane w pliku *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7a51f-432">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="7a51f-433">W [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe mogą być ustawiane w pliku *. programu vscode/Launch. JSON* podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-433">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="7a51f-434">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-434">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="7a51f-435">[Konfiguracja wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawana przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-435">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="7a51f-436">Konfiguracja wiersza polecenia jest dodawana jako Ostatnia, aby zezwolić na argumenty wiersza polecenia w celu przesłonięcia konfiguracji udostępnionej przez wcześniejszych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-436">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="7a51f-437">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7a51f-437">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="7a51f-438">Dodatkową konfigurację można uzyskać za pomocą programu [ApplicationName](#application-key-name) i kluczy [contentRoot](#content-root) .</span><span class="sxs-lookup"><span data-stu-id="7a51f-438">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="7a51f-439">Przykładowa `HostBuilder` konfiguracja <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="7a51f-439">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="7a51f-440">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="7a51f-440">ConfigureAppConfiguration</span></span>

<span data-ttu-id="7a51f-441">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-441">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="7a51f-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*><xref:Microsoft.Extensions.Configuration.IConfiguration> <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> za pomocą programu można utworzyć dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="7a51f-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="7a51f-444">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="7a51f-444">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="7a51f-445">Konfiguracja utworzona przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> program jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i <xref:Microsoft.Extensions.Hosting.IHost.Services*>w.</span><span class="sxs-lookup"><span data-stu-id="7a51f-445">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="7a51f-446">Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta dostarczoną przez [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="7a51f-446">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="7a51f-447">Przykładowa konfiguracja aplikacji <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="7a51f-447">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="7a51f-448">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7a51f-448">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="7a51f-449">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="7a51f-449">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="7a51f-450">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="7a51f-450">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="7a51f-451">Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="7a51f-451">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="7a51f-452">Aplikacja Przykładowa przenosi pliki ustawień aplikacji JSON i *HostSettings. JSON* z następującym `<Content>` elementem:</span><span class="sxs-lookup"><span data-stu-id="7a51f-452">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="7a51f-453">Metody rozszerzenia konfiguracji, takie jak <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> i <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> wymagające dodatkowych pakietów NuGet, takie jak [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) i [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="7a51f-453">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="7a51f-454">Jeśli aplikacja nie używa [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), należy dodać te pakiety do projektu oprócz podstawowego pakietu [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) .</span><span class="sxs-lookup"><span data-stu-id="7a51f-454">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="7a51f-455">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-455">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="7a51f-456">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="7a51f-456">ConfigureServices</span></span>

<span data-ttu-id="7a51f-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>dodaje usługi do kontenera iniekcji [zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="7a51f-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="7a51f-459">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="7a51f-459">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="7a51f-460">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-460">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="7a51f-461">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) `AddHostedService` używa metody rozszerzającej, aby dodać usługę dla zdarzeń okresu istnienia, `LifetimeEventsHostedService` `TimedHostedService`oraz zadanie w tle czasu, do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7a51f-461">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="7a51f-462">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="7a51f-462">ConfigureLogging</span></span>

<span data-ttu-id="7a51f-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>dodaje delegata do konfigurowania podanego <xref:Microsoft.Extensions.Logging.ILoggingBuilder>elementu.</span><span class="sxs-lookup"><span data-stu-id="7a51f-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="7a51f-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="7a51f-465">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="7a51f-465">UseConsoleLifetime</span></span>

<span data-ttu-id="7a51f-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>nasłuchuje kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> i wywołań, aby rozpocząć proces zamykania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="7a51f-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="7a51f-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="7a51f-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>jest wstępnie zarejestrowany jako domyślna implementacja okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="7a51f-469">Używany jest ostatni zarejestrowany okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-469">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="7a51f-470">Konfiguracja kontenera</span><span class="sxs-lookup"><span data-stu-id="7a51f-470">Container configuration</span></span>

<span data-ttu-id="7a51f-471">Aby zapewnić obsługę podłączania w innych kontenerach, host może <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>akceptować.</span><span class="sxs-lookup"><span data-stu-id="7a51f-471">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="7a51f-472">Dostarczenie fabryki nie jest częścią rejestracji typu DI Container, ale zamiast tego jest wewnętrznym hostem używanym do tworzenia konkretnych kontenerów DI.</span><span class="sxs-lookup"><span data-stu-id="7a51f-472">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="7a51f-473">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę używaną do utworzenia dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-473">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="7a51f-474">Niestandardowa konfiguracja kontenera jest zarządzana przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> metodę.</span><span class="sxs-lookup"><span data-stu-id="7a51f-474">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="7a51f-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>Program zapewnia silnie wpisaną funkcję konfigurowania kontenera na podstawie podstawowego interfejsu API hosta.</span><span class="sxs-lookup"><span data-stu-id="7a51f-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="7a51f-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="7a51f-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="7a51f-477">Utwórz kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7a51f-477">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="7a51f-478">Podaj fabrykę kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="7a51f-478">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="7a51f-479">Użyj fabryki i skonfiguruj niestandardowy kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7a51f-479">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="7a51f-480">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="7a51f-480">Extensibility</span></span>

<span data-ttu-id="7a51f-481">Rozszerzalność hosta jest wykonywana z metodami <xref:Microsoft.Extensions.Hosting.IHostBuilder>rozszerzenia w systemie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-481">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="7a51f-482">Poniższy przykład pokazuje, jak Metoda rozszerzania rozszerza <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementację z przykładem [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przedstawionym w <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-482">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="7a51f-483">Aplikacja ustanowi `UseHostedService` metodę rozszerzenia w celu zarejestrowania `T`przesyłanej usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="7a51f-483">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="7a51f-484">Zarządzanie hostem</span><span class="sxs-lookup"><span data-stu-id="7a51f-484">Manage the host</span></span>

<span data-ttu-id="7a51f-485">Implementacja jest odpowiedzialna za uruchamianie i <xref:Microsoft.Extensions.Hosting.IHostedService> zatrzymywanie implementacji, które są zarejestrowane w kontenerze usługi. <xref:Microsoft.Extensions.Hosting.IHost></span><span class="sxs-lookup"><span data-stu-id="7a51f-485">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="7a51f-486">Uruchom</span><span class="sxs-lookup"><span data-stu-id="7a51f-486">Run</span></span>

<span data-ttu-id="7a51f-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*>uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta:</span><span class="sxs-lookup"><span data-stu-id="7a51f-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="7a51f-488">RunAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-488">RunAsync</span></span>

<span data-ttu-id="7a51f-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*>uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task> , która kończy się po wyzwoleniu tokenu anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="7a51f-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="7a51f-490">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-490">RunConsoleAsync</span></span>

<span data-ttu-id="7a51f-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>włącza obsługę konsoli, kompiluje i uruchamia hosta i czeka na zamknięcie klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="7a51f-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="7a51f-492">Uruchom i StopAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-492">Start and StopAsync</span></span>

<span data-ttu-id="7a51f-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*>uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="7a51f-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="7a51f-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="7a51f-495">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-495">StartAsync and StopAsync</span></span>

<span data-ttu-id="7a51f-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="7a51f-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="7a51f-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>kończy działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="7a51f-498">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="7a51f-498">WaitForShutdown</span></span>

<span data-ttu-id="7a51f-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>jest wyzwalany za <xref:Microsoft.Extensions.Hosting.IHostLifetime>pośrednictwem, <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> na przykład (nasłuchuje w przypadku kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="7a51f-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="7a51f-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="7a51f-501">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="7a51f-501">WaitForShutdownAsync</span></span>

<span data-ttu-id="7a51f-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>Zwraca wartość <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>, która kończy się, gdy zamknięcie jest wyzwalane za pośrednictwem danego tokenu i wywołań. <xref:System.Threading.Tasks.Task></span><span class="sxs-lookup"><span data-stu-id="7a51f-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="7a51f-503">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="7a51f-503">External control</span></span>

<span data-ttu-id="7a51f-504">Kontrolę zewnętrzną hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="7a51f-504">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="7a51f-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*>jest wywoływana na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="7a51f-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="7a51f-506">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="7a51f-506">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="7a51f-507">IHostingEnvironment, interfejs</span><span class="sxs-lookup"><span data-stu-id="7a51f-507">IHostingEnvironment interface</span></span>

<span data-ttu-id="7a51f-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>zawiera informacje o środowisku hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="7a51f-509">Użyj [iniekcji konstruktorów](xref:fundamentals/dependency-injection) , <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> Aby uzyskać w celu użycia jej właściwości i metod rozszerzających:</span><span class="sxs-lookup"><span data-stu-id="7a51f-509">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="7a51f-510">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7a51f-510">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="7a51f-511">IApplicationLifetime, interfejs</span><span class="sxs-lookup"><span data-stu-id="7a51f-511">IApplicationLifetime interface</span></span>

<span data-ttu-id="7a51f-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime>zezwala na działania po uruchomieniu i zamknięcia, w tym bezpieczne żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="7a51f-513">Trzy właściwości interfejsu są tokenami anulowania używanymi do rejestrowania <xref:System.Action> metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="7a51f-513">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="7a51f-514">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="7a51f-514">Cancellation Token</span></span> | <span data-ttu-id="7a51f-515">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="7a51f-515">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="7a51f-516">Host został w pełni uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="7a51f-516">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="7a51f-517">Host kończy bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-517">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="7a51f-518">Wszystkie żądania powinny być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="7a51f-518">All requests should be processed.</span></span> <span data-ttu-id="7a51f-519">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-519">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="7a51f-520">Host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="7a51f-520">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="7a51f-521">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="7a51f-521">Requests may still be processing.</span></span> <span data-ttu-id="7a51f-522">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-522">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="7a51f-523">Konstruktor — wstrzyknięcie <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> usługi do dowolnej klasy.</span><span class="sxs-lookup"><span data-stu-id="7a51f-523">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="7a51f-524">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji), aby zarejestrować zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="7a51f-524">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="7a51f-525">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="7a51f-525">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="7a51f-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>żąda zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7a51f-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="7a51f-527">Następująca Klasa służy <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> do bezpiecznego zamykania aplikacji w przypadku wywołania `Shutdown` metody klasy:</span><span class="sxs-lookup"><span data-stu-id="7a51f-527">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7a51f-528">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7a51f-528">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
