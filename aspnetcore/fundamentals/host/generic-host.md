---
title: Ogólny hosta platformy .NET
author: tdykstra
description: Dowiedz się więcej o hosta rodzajowego Core .NET który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/01/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: d787559eaecd6d4d6cfe01e37baf28774a90c5c3
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724428"
---
# <a name="net-generic-host"></a><span data-ttu-id="746e8-103">Ogólny hosta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="746e8-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="746e8-104">W tym artykule przedstawiono hosta rodzajowego Core .NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>) oraz wskazówki dotyczące sposobu używania go.</span><span class="sxs-lookup"><span data-stu-id="746e8-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="746e8-105">Co to jest hostem?</span><span class="sxs-lookup"><span data-stu-id="746e8-105">What's a host?</span></span>

<span data-ttu-id="746e8-106">A *hosta* jest obiektem, który hermetyzuje zasobów aplikacji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="746e8-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="746e8-107">Wstrzykiwanie zależności (DI)</span><span class="sxs-lookup"><span data-stu-id="746e8-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="746e8-108">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="746e8-108">Logging</span></span>
* <span data-ttu-id="746e8-109">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="746e8-109">Configuration</span></span>
* <span data-ttu-id="746e8-110">`IHostedService` Implementacje</span><span class="sxs-lookup"><span data-stu-id="746e8-110">`IHostedService` implementations</span></span>

<span data-ttu-id="746e8-111">Po uruchomieniu hosta wywoływanych przez nią `IHostedService.StartAsync` każdego wykonania <xref:Microsoft.Extensions.Hosting.IHostedService> znalezione w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="746e8-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="746e8-112">W aplikacji sieci web, jeden z `IHostedService` implementacje jest usługą sieci web, który rozpoczyna się [implementację serwera HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="746e8-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="746e8-113">Głównym powodem wraz ze wszystkimi zasobami współzależne aplikacji w jeden obiekt jest zarządzanie okresem istnienia: kontrolę nad uruchamianiem aplikacji i łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="746e8-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="746e8-114">W wersjach starszych niż 3.0, platformy ASP.NET Core [hosta sieci Web](xref:fundamentals/host/web-host) jest używany w przypadku obciążeń HTTP.</span><span class="sxs-lookup"><span data-stu-id="746e8-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="746e8-115">Host sieci Web nie jest już zalecany dla aplikacji sieci web i pozostają dostępne tylko dla zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="746e8-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="746e8-116">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="746e8-116">Set up a host</span></span>

<span data-ttu-id="746e8-117">Host jest skonfigurowany zwykle, skompilowane i wykonywania przez kod w `Program` klasy.</span><span class="sxs-lookup"><span data-stu-id="746e8-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="746e8-118">`Main` Metody:</span><span class="sxs-lookup"><span data-stu-id="746e8-118">The `Main` method:</span></span>

* <span data-ttu-id="746e8-119">Wywołania `CreateHostBuilder` metodę, aby utworzyć i skonfigurować obiekt konstruktora.</span><span class="sxs-lookup"><span data-stu-id="746e8-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="746e8-120">Wywołania `Build` i `Run` metody do konstruktora obiektu.</span><span class="sxs-lookup"><span data-stu-id="746e8-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="746e8-121">Oto *Program.cs* kodu w przypadku obciążeń innych niż HTTP, za pomocą jednego `IHostedService` dodany do kontenera DI implementacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="746e8-122">W przypadku obciążeń HTTP `Main` metody jest tym samym ale `CreateHostBuilder` wywołania `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="746e8-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="746e8-123">Jeśli aplikacja korzysta z platformy Entity Framework Core, nie należy zmieniać nazwy lub podpis `CreateHostBuilder` metody.</span><span class="sxs-lookup"><span data-stu-id="746e8-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="746e8-124">[Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) poszukiwać `CreateHostBuilder` metodę, która umożliwia skonfigurowanie hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="746e8-125">Aby uzyskać więcej informacji, zobacz [Tworzenie typu DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="746e8-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="746e8-126">Domyślne ustawienia konstruktora</span><span class="sxs-lookup"><span data-stu-id="746e8-126">Default builder settings</span></span> 

<span data-ttu-id="746e8-127"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> Metody:</span><span class="sxs-lookup"><span data-stu-id="746e8-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="746e8-128">Ustawia zawartość katalogu głównego ścieżka zwrócona przez element <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-128">Sets the content root to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="746e8-129">Konfiguracja hosta obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="746e8-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="746e8-130">Zmienne środowiskowe z prefiksem "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="746e8-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="746e8-131">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="746e8-131">Command-line arguments.</span></span>
* <span data-ttu-id="746e8-132">Konfiguracja aplikacji obciążeń z:</span><span class="sxs-lookup"><span data-stu-id="746e8-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="746e8-133">*appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="746e8-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="746e8-134">*appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="746e8-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="746e8-135">[Klucz tajny Menedżera](xref:security/app-secrets) uruchamiania aplikacji `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="746e8-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="746e8-136">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="746e8-136">Environment variables.</span></span>
  * <span data-ttu-id="746e8-137">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="746e8-137">Command-line arguments.</span></span>
* <span data-ttu-id="746e8-138">Dodaje następujące [rejestrowania](xref:fundamentals/logging/index) dostawców:</span><span class="sxs-lookup"><span data-stu-id="746e8-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="746e8-139">Konsola</span><span class="sxs-lookup"><span data-stu-id="746e8-139">Console</span></span>
  * <span data-ttu-id="746e8-140">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="746e8-140">Debug</span></span>
  * <span data-ttu-id="746e8-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="746e8-141">EventSource</span></span>
  * <span data-ttu-id="746e8-142">Dziennik zdarzeń (tylko wtedy, gdy systemem Windows)</span><span class="sxs-lookup"><span data-stu-id="746e8-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="746e8-143">Włącza [zakresu walidacji](xref:fundamentals/dependency-injection#scope-validation) i [weryfikacji zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) po środowisko programowania.</span><span class="sxs-lookup"><span data-stu-id="746e8-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="746e8-144">`ConfigureWebHostDefaults` Metody:</span><span class="sxs-lookup"><span data-stu-id="746e8-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="746e8-145">Obciążenia hostów konfiguracji ze zmiennych środowiskowych z prefiksem "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="746e8-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="746e8-146">Zestawy [Kestrel](xref:fundamentals/servers/kestrel) serwera jako serwera sieci web i konfiguruje go za pomocą dostawcy usług konfiguracji hosta aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="746e8-147">Opcjach domyślny serwer Kestrel, zobacz temat <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="746e8-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="746e8-148">Dodaje [filtrowanie hosta oprogramowania pośredniczącego](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="746e8-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="746e8-149">Dodaje [oprogramowanie pośredniczące przekazane nagłówki](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) Jeśli ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.</span><span class="sxs-lookup"><span data-stu-id="746e8-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="746e8-150">Umożliwia integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="746e8-150">Enables IIS integration.</span></span> <span data-ttu-id="746e8-151">Opcjach domyślny usług IIS, zobacz temat <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="746e8-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="746e8-152">[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [ustawień aplikacji sieci web](#settings-for-web-apps) sekcje w dalszej części tego artykułu opisano sposób zastępują ustawienia konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="746e8-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="746e8-153">Dostarczone do struktury usługi</span><span class="sxs-lookup"><span data-stu-id="746e8-153">Framework-provided services</span></span>

<span data-ttu-id="746e8-154">Usługi, które są automatycznie rejestrowane są następujące:</span><span class="sxs-lookup"><span data-stu-id="746e8-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="746e8-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="746e8-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="746e8-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="746e8-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="746e8-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="746e8-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="746e8-158">Aby uzyskać listę wszystkich usług dostarczone przez framework, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="746e8-158">For a list of all framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="746e8-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="746e8-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="746e8-160">Wstrzykiwanie <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) usługi do każdej klasy do obsługi zadań uruchamiania po i łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="746e8-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="746e8-161">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania początkowego aplikacji i metody obsługi zdarzeń zatrzymania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="746e8-162">Interfejs zawiera również `StopApplication` metody.</span><span class="sxs-lookup"><span data-stu-id="746e8-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="746e8-163">Poniższy przykład jest `IHostedService` implementację, która rejestruje `IApplicationLifetime` zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="746e8-163">The following example is an `IHostedService` implementation that registers the `IApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="746e8-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="746e8-164">IHostLifetime</span></span>

<span data-ttu-id="746e8-165"><xref:Microsoft.Extensions.Hosting.IHostLifetime> Implementacji kontroluje podczas uruchamiania hosta i po zatrzymaniu.</span><span class="sxs-lookup"><span data-stu-id="746e8-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="746e8-166">Ostatniego wykonania zarejestrowany jest używany.</span><span class="sxs-lookup"><span data-stu-id="746e8-166">The last implementation registered is used.</span></span>

<span data-ttu-id="746e8-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Wartość domyślna to `IHostLifetime` implementacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="746e8-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="746e8-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="746e8-169">nasłuchuje sygnał SIGTERM lub wywołań i Ctrl + C/SIGINT <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> można uruchomić procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="746e8-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="746e8-170">Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="746e8-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="746e8-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="746e8-171">IHostEnvironment</span></span>

<span data-ttu-id="746e8-172">Wstrzykiwanie <xref:Microsoft.Extensions.Hosting.IHostEnvironment> usługi do klasy, aby uzyskać informacje o następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="746e8-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="746e8-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="746e8-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="746e8-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="746e8-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="746e8-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="746e8-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="746e8-176">Wdrożenie aplikacji sieci Web `IWebHostEnvironment` interfejs, który dziedziczy `IHostEnvironment` i dodaje:</span><span class="sxs-lookup"><span data-stu-id="746e8-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="746e8-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="746e8-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="746e8-178">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="746e8-178">Host configuration</span></span>

<span data-ttu-id="746e8-179">Konfiguracja hosta jest używana do właściwości <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="746e8-180">Konfiguracja hosta jest dostępny z [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) wewnątrz <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="746e8-181">Po `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` zostaje zastąpiona opcją konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="746e8-182">Aby dodać konfigurację hosta, należy wywołać <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="746e8-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="746e8-183">`ConfigureHostConfiguration` można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="746e8-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="746e8-184">Host używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="746e8-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="746e8-185">Dostawca zmiennej środowiska z prefiksem `DOTNET_` i argumenty wiersza polecenia są uwzględniane przy CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="746e8-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="746e8-186">W przypadku aplikacji sieci web dostawcy zmiennej środowiska z prefiksem `ASPNETCORE_` zostanie dodany.</span><span class="sxs-lookup"><span data-stu-id="746e8-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="746e8-187">Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane.</span><span class="sxs-lookup"><span data-stu-id="746e8-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="746e8-188">Na przykład tej wartości zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.</span><span class="sxs-lookup"><span data-stu-id="746e8-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="746e8-189">Poniższy przykład umożliwia utworzenie konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="746e8-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="746e8-190">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="746e8-190">App configuration</span></span>

<span data-ttu-id="746e8-191">Konfiguracja aplikacji jest tworzony przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> na `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="746e8-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="746e8-192">`ConfigureAppConfiguration` można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="746e8-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="746e8-193">Aplikacja używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="746e8-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="746e8-194">Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i as a service firmy DI.</span><span class="sxs-lookup"><span data-stu-id="746e8-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="746e8-195">Konfiguracja hosta jest także dodawane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="746e8-196">Aby uzyskać więcej informacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="746e8-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="746e8-197">Ustawienia dla wszystkich typów aplikacji</span><span class="sxs-lookup"><span data-stu-id="746e8-197">Settings for all app types</span></span>

<span data-ttu-id="746e8-198">W tej sekcji przedstawiono ustawienia hosta, które są stosowane do protokołów HTTP, jak i obciążeń innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="746e8-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="746e8-199">Domyślnie zmienne środowiskowe używane, aby skonfigurować te ustawienia mogą mieć `DOTNET_` lub `ASPNETCORE_` prefiks.</span><span class="sxs-lookup"><span data-stu-id="746e8-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

### <a name="applicationname"></a><span data-ttu-id="746e8-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="746e8-200">ApplicationName</span></span>

<span data-ttu-id="746e8-201">[IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) właściwość ma wartość z konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="746e8-202">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="746e8-202">**Key**: applicationName</span></span>  
<span data-ttu-id="746e8-203">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-203">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-204">**Domyślne**: Nazwa zestawu, który zawiera wpis aplikacji punktu.</span><span class="sxs-lookup"><span data-stu-id="746e8-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="746e8-205">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="746e8-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="746e8-206">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="746e8-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="746e8-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="746e8-207">ContentRootPath</span></span>

<span data-ttu-id="746e8-208">[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) właściwość określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="746e8-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="746e8-209">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="746e8-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="746e8-210">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="746e8-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="746e8-211">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-211">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-212">**Domyślne**: Folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="746e8-213">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="746e8-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="746e8-214">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseContentRoot` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="746e8-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

### <a name="environmentname"></a><span data-ttu-id="746e8-215">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="746e8-215">EnvironmentName</span></span>

<span data-ttu-id="746e8-216">[IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) właściwość można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="746e8-216">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="746e8-217">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="746e8-217">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="746e8-218">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="746e8-218">Values aren't case sensitive.</span></span>

<span data-ttu-id="746e8-219">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="746e8-219">**Key**: environment</span></span>  
<span data-ttu-id="746e8-220">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-220">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-221">**Domyślne**: Produkcji</span><span class="sxs-lookup"><span data-stu-id="746e8-221">**Default**: Production</span></span>  
<span data-ttu-id="746e8-222">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="746e8-222">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="746e8-223">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseEnvironment` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="746e8-223">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="746e8-224">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="746e8-224">ShutdownTimeout</span></span>

<span data-ttu-id="746e8-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawiono limit czasu <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-225">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="746e8-226">Wartość domyślna to 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="746e8-226">The default value is five seconds.</span></span>  <span data-ttu-id="746e8-227">W trakcie okresu czasu hosta:</span><span class="sxs-lookup"><span data-stu-id="746e8-227">During the timeout period, the host:</span></span>

* <span data-ttu-id="746e8-228">Wyzwalacze [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="746e8-228">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="746e8-229">Podejmuje próby zatrzymania usług hostowanych, rejestrowanie błędów, których nie można zatrzymać usługi.</span><span class="sxs-lookup"><span data-stu-id="746e8-229">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="746e8-230">Jeśli upłynie limit czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostaną zatrzymane podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-230">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="746e8-231">Usługi zatrzymania nawet wtedy, gdy jeszcze nie zakończyło się przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="746e8-231">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="746e8-232">Jeśli usługi wymagają dodatkowego czasu, aby zatrzymać, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="746e8-232">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="746e8-233">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="746e8-233">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="746e8-234">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="746e8-234">**Type**: *int*</span></span>  
<span data-ttu-id="746e8-235">**Domyślne**: 5 sekund **zmiennej środowiskowej**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="746e8-235">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="746e8-236">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub skonfigurować `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="746e8-236">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="746e8-237">W poniższym przykładzie ustawiono limit czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="746e8-237">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="746e8-238">Ustawienia dla aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="746e8-238">Settings for web apps</span></span>

<span data-ttu-id="746e8-239">Niektóre ustawienia hosta dotyczą tylko obciążenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="746e8-239">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="746e8-240">Domyślnie zmienne środowiskowe używane, aby skonfigurować te ustawienia mogą mieć `DOTNET_` lub `ASPNETCORE_` prefiks.</span><span class="sxs-lookup"><span data-stu-id="746e8-240">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="746e8-241">Metody rozszerzenia na `IWebHostBuilder` są dostępne dla tych ustawień.</span><span class="sxs-lookup"><span data-stu-id="746e8-241">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="746e8-242">Przyjęto założenie, przykłady kodu, które pokazują sposób wywołania metody rozszerzenia `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="746e8-242">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="746e8-243">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="746e8-243">CaptureStartupErrors</span></span>

<span data-ttu-id="746e8-244">Gdy `false`, błędy podczas uruchamiania wynik na hoście, kończenie.</span><span class="sxs-lookup"><span data-stu-id="746e8-244">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="746e8-245">Gdy `true`, host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="746e8-245">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="746e8-246">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="746e8-246">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="746e8-247">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="746e8-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="746e8-248">**Domyślne**: Wartość domyślna to `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, w którym domyślnie są `true`.</span><span class="sxs-lookup"><span data-stu-id="746e8-248">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="746e8-249">**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="746e8-249">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="746e8-250">Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="746e8-250">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="746e8-251">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="746e8-251">DetailedErrors</span></span>

<span data-ttu-id="746e8-252">Po włączeniu lub w przypadku, gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe informacje o błędach.</span><span class="sxs-lookup"><span data-stu-id="746e8-252">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="746e8-253">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="746e8-253">**Key**: detailedErrors</span></span>  
<span data-ttu-id="746e8-254">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="746e8-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="746e8-255">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="746e8-255">**Default**: false</span></span>  
<span data-ttu-id="746e8-256">**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="746e8-256">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="746e8-257">Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="746e8-257">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="746e8-258">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="746e8-258">HostingStartupAssemblies</span></span>

<span data-ttu-id="746e8-259">Rozdzielana średnikami ciąg hostingu zestawy startowe załadować podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="746e8-259">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="746e8-260">Mimo że konfiguracja ma domyślnie wartość pustego ciągu, hostingu zestawy startowe zawsze zawierać zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-260">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="746e8-261">Hostingu zestawy startowe są udostępniane, są dodawane do zestawu aplikacji dotyczące ładowania, gdy aplikacja tworzy swoich usług wspólne podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="746e8-261">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="746e8-262">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="746e8-262">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="746e8-263">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-263">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-264">**Domyślne**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="746e8-264">**Default**: Empty string</span></span>  
<span data-ttu-id="746e8-265">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="746e8-265">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="746e8-266">Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="746e8-266">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="746e8-267">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="746e8-267">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="746e8-268">Rozdzielana średnikami ciąg hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="746e8-268">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="746e8-269">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="746e8-269">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="746e8-270">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-270">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-271">**Domyślne**: Pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="746e8-271">**Default**: Empty string</span></span>  
<span data-ttu-id="746e8-272">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="746e8-272">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="746e8-273">Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="746e8-273">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="httpsport"></a><span data-ttu-id="746e8-274">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="746e8-274">HTTPS_Port</span></span>

<span data-ttu-id="746e8-275">HTTPS przekierowania portu.</span><span class="sxs-lookup"><span data-stu-id="746e8-275">The HTTPS redirect port.</span></span> <span data-ttu-id="746e8-276">Używane w [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="746e8-276">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="746e8-277">**Klucz**: https_port **typu**: *ciąg*
**domyślne**: Nie ustawiono wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="746e8-277">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="746e8-278">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="746e8-278">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="746e8-279">Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="746e8-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="746e8-280">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="746e8-280">PreferHostingUrls</span></span>

<span data-ttu-id="746e8-281">Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `IWebHostBuilder` zamiast konfigurowane przy użyciu `IServer` implementacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-281">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="746e8-282">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="746e8-282">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="746e8-283">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="746e8-283">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="746e8-284">**Domyślne**: true</span><span class="sxs-lookup"><span data-stu-id="746e8-284">**Default**: true</span></span>  
<span data-ttu-id="746e8-285">**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="746e8-285">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="746e8-286">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="746e8-286">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="746e8-287">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="746e8-287">PreventHostingStartup</span></span>

<span data-ttu-id="746e8-288">Uniemożliwia automatyczne ładowanie obsługi zestawów uruchamiania, w tym hosting zestawy startowe skonfigurowany przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-288">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="746e8-289">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="746e8-289">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="746e8-290">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="746e8-290">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="746e8-291">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="746e8-291">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="746e8-292">**Domyślne**: false</span><span class="sxs-lookup"><span data-stu-id="746e8-292">**Default**: false</span></span>  
<span data-ttu-id="746e8-293">**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="746e8-293">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="746e8-294">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseSetting` :</span><span class="sxs-lookup"><span data-stu-id="746e8-294">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="746e8-295">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="746e8-295">StartupAssembly</span></span>

<span data-ttu-id="746e8-296">Zestaw, aby wyszukać `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="746e8-296">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="746e8-297">**Klucz**: startupAssembly **typu**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-297">**Key**: startupAssembly **Type**: *string*</span></span>  
<span data-ttu-id="746e8-298">**Domyślne**: Zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="746e8-298">**Default**: The app's assembly</span></span>  
<span data-ttu-id="746e8-299">**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="746e8-299">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="746e8-300">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="746e8-300">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="746e8-301">`UseStartup` może to nazwa zestawu (`string`) lub typu (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="746e8-301">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="746e8-302">Jeśli wiele `UseStartup` metody są wywoływane, ostatni z nich ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="746e8-302">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="746e8-303">adresy URL</span><span class="sxs-lookup"><span data-stu-id="746e8-303">URLs</span></span>

<span data-ttu-id="746e8-304">Rozdzielana średnikami lista adresów IP lub adresów hosta, portów i protokołów, które serwer powinien nasłuchiwać żądań protokołu.</span><span class="sxs-lookup"><span data-stu-id="746e8-304">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="746e8-305">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="746e8-305">For example, `http://localhost:123`.</span></span> <span data-ttu-id="746e8-306">Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="746e8-306">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="746e8-307">Protokół (`http://` lub `https://`) muszą być dołączone do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="746e8-307">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="746e8-308">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="746e8-308">Supported formats vary among servers.</span></span>

<span data-ttu-id="746e8-309">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="746e8-309">**Key**: urls</span></span>  
<span data-ttu-id="746e8-310">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-310">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-311">**Domyślne**: `http://localhost:5000` i `https://localhost:5001` 
 **zmiennej środowiskowej**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="746e8-311">**Default**: `http://localhost:5000` and `https://localhost:5001`
**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="746e8-312">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="746e8-312">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="746e8-313">Kestrel ma swój własny konfiguracji punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="746e8-313">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="746e8-314">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="746e8-314">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="746e8-315">WebRoot</span><span class="sxs-lookup"><span data-stu-id="746e8-315">WebRoot</span></span>

<span data-ttu-id="746e8-316">Ścieżka względna do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-316">The relative path to the app's static assets.</span></span>

<span data-ttu-id="746e8-317">**Klucz**: webroot</span><span class="sxs-lookup"><span data-stu-id="746e8-317">**Key**: webroot</span></span>  
<span data-ttu-id="746e8-318">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-318">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-319">**Domyślne**: *(Zawartość katalogu głównego) / wwwroot*, jeśli ścieżka istnieje.</span><span class="sxs-lookup"><span data-stu-id="746e8-319">**Default**: *(Content Root)/wwwroot*, if the path exists.</span></span> <span data-ttu-id="746e8-320">Jeśli ścieżka nie istnieje, jest używany dostawca pliku pusta.</span><span class="sxs-lookup"><span data-stu-id="746e8-320">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="746e8-321">**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="746e8-321">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="746e8-322">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="746e8-322">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="746e8-323">Zarządzanie okresem istnienia hosta</span><span class="sxs-lookup"><span data-stu-id="746e8-323">Manage the host lifetime</span></span>

<span data-ttu-id="746e8-324">Wywoływanie metod na wbudowanych <xref:Microsoft.Extensions.Hosting.IHost> implementacji, uruchamianie i zatrzymywanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-324">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="746e8-325">Te metody mają wpływ na wszystkie <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji, które są zarejestrowane w usłudze service container.</span><span class="sxs-lookup"><span data-stu-id="746e8-325">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="746e8-326">Uruchom</span><span class="sxs-lookup"><span data-stu-id="746e8-326">Run</span></span>

<span data-ttu-id="746e8-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uruchamia aplikację i blokuje wątek wywołujący, aż host zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="746e8-327"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="746e8-328">RunAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-328">RunAsync</span></span>

<span data-ttu-id="746e8-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia systemu.</span><span class="sxs-lookup"><span data-stu-id="746e8-329"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="746e8-330">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-330">RunConsoleAsync</span></span>

<span data-ttu-id="746e8-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Włączenie obsługi konsoli, kompilacje uruchamia hosta i czeka na klawisze Ctrl + C/SIGINT lub SIGTERM zamknąć.</span><span class="sxs-lookup"><span data-stu-id="746e8-331"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="746e8-332">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="746e8-332">Start</span></span>

<span data-ttu-id="746e8-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="746e8-333"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="746e8-334">StartAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-334">StartAsync</span></span>

<span data-ttu-id="746e8-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> Uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia systemu.</span><span class="sxs-lookup"><span data-stu-id="746e8-335"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="746e8-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> nosi nazwę na początku `StartAsync`, który powoduje oczekiwanie, dopóki nie zostanie ukończone przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="746e8-336"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="746e8-337">Może to służyć do opóźnienie uruchamiania, dopóki nie zasygnalizowane przez zdarzenie zewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="746e8-337">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="746e8-338">StopAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-338">StopAsync</span></span>

<span data-ttu-id="746e8-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="746e8-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="746e8-340">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="746e8-340">WaitForShutdown</span></span>

<span data-ttu-id="746e8-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący, aż zamknięcie jest wyzwalany przez IHostLifetime, takie jak za pomocą klawiszy Ctrl + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="746e8-341"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="746e8-342">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-342">WaitForShutdownAsync</span></span>

<span data-ttu-id="746e8-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> Zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="746e8-344">Zewnętrznej kontroli</span><span class="sxs-lookup"><span data-stu-id="746e8-344">External control</span></span>

<span data-ttu-id="746e8-345">Bezpośrednia kontrola nad okresem istnienia hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="746e8-345">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="746e8-346">Aplikacje platformy ASP.NET Core, konfigurowanie i uruchamiania hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-346">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="746e8-347">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-347">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="746e8-348">W tym artykule opisano Host rodzajowego Core ASP.NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="746e8-348">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="746e8-349">Ogólny hosta ma na celu rozdzielenie potoku HTTP z hosta internetowego interfejsu API, umożliwiające szersze gamę scenariuszy hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-349">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="746e8-350">Komunikaty, zadania w tle i innych obciążeń innych niż HTTP oparte na ogólnych hosta korzyści z możliwości przekrojowe, takie jak konfiguracja, wstrzykiwanie zależności (DI) i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="746e8-350">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="746e8-351">Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiednie w scenariuszach hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="746e8-351">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="746e8-352">W przypadku scenariuszy hostingu w sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="746e8-352">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="746e8-353">Ogólny Host będzie Zastąp hosta sieci Web w przyszłym wydaniu i pełnić rolę hosta podstawowego interfejsu API zarówno w przypadku protokołu HTTP, jak i scenariuszy innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="746e8-353">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="746e8-354">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="746e8-354">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="746e8-355">Podczas uruchamiania przykładowej aplikacji [programu Visual Studio Code](https://code.visualstudio.com/), użyj *zewnętrznych lub w zintegrowanym terminalu*.</span><span class="sxs-lookup"><span data-stu-id="746e8-355">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="746e8-356">Nie, uruchom przykład w `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="746e8-356">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="746e8-357">Aby ustawić konsoli w programie Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="746e8-357">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="746e8-358">Otwórz *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="746e8-358">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="746e8-359">W **.NET Core uruchamianie (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu.</span><span class="sxs-lookup"><span data-stu-id="746e8-359">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="746e8-360">Ustaw wartość na wartość `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="746e8-360">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="746e8-361">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="746e8-361">Introduction</span></span>

<span data-ttu-id="746e8-362">Biblioteka ogólnego hosta jest dostępna w <xref:Microsoft.Extensions.Hosting> przestrzeni nazw i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="746e8-362">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="746e8-363">`Microsoft.Extensions.Hosting` Pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="746e8-363">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="746e8-364"><xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="746e8-364"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="746e8-365">Każdy <xref:Microsoft.Extensions.Hosting.IHostedService> implementacja jest wykonywany w kolejności od [usługi rejestracji w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="746e8-365">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="746e8-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana w każdej <xref:Microsoft.Extensions.Hosting.IHostedService> podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w kolejności odwrotnej rejestracji, gdy kończy pracę bez problemu zmieniała hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-366"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="746e8-367">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="746e8-367">Set up a host</span></span>

<span data-ttu-id="746e8-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder> jest to główny składnik, używanego przez aplikacje i biblioteki do zainicjowania, tworzeniu i uruchamianiu hosta:</span><span class="sxs-lookup"><span data-stu-id="746e8-368"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="746e8-369">Opcje</span><span class="sxs-lookup"><span data-stu-id="746e8-369">Options</span></span>

<span data-ttu-id="746e8-370"><xref:Microsoft.Extensions.Hosting.HostOptions> Skonfiguruj opcje <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="746e8-370"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="746e8-371">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="746e8-371">Shutdown timeout</span></span>

<span data-ttu-id="746e8-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-372"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="746e8-373">Wartość domyślna to 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="746e8-373">The default value is five seconds.</span></span>

<span data-ttu-id="746e8-374">Następująca Konfiguracja opcji w `Program.Main` zwiększa domyślna pięć drugi zamykania wartość limitu czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="746e8-374">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="746e8-375">Usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="746e8-375">Default services</span></span>

<span data-ttu-id="746e8-376">Następujące usługi są rejestrowane podczas inicjowania hosta:</span><span class="sxs-lookup"><span data-stu-id="746e8-376">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="746e8-377">[Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="746e8-377">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="746e8-378">[Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="746e8-378">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="746e8-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="746e8-379"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="746e8-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="746e8-380"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="746e8-381">[Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="746e8-381">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="746e8-382">[Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="746e8-382">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="746e8-383">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="746e8-383">Host configuration</span></span>

<span data-ttu-id="746e8-384">Konfiguracja hosta jest tworzony przez:</span><span class="sxs-lookup"><span data-stu-id="746e8-384">Host configuration is created by:</span></span>

* <span data-ttu-id="746e8-385">Wywoływanie metody rozszerzenia na <xref:Microsoft.Extensions.Hosting.IHostBuilder> można ustawić [zawartości głównego](#content-root) i [środowiska](#environment).</span><span class="sxs-lookup"><span data-stu-id="746e8-385">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="746e8-386">Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-386">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="746e8-387">Metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="746e8-387">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="746e8-388">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="746e8-388">Application key (name)</span></span>

<span data-ttu-id="746e8-389">[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) właściwość ma wartość z konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-389">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="746e8-390">Aby jawnie ustawić wartość, użyj [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="746e8-390">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="746e8-391">**Klucz**: applicationName</span><span class="sxs-lookup"><span data-stu-id="746e8-391">**Key**: applicationName</span></span>  
<span data-ttu-id="746e8-392">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-392">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-393">**Domyślne**: Nazwa zestawu zawierającego wpis aplikacji punktu.</span><span class="sxs-lookup"><span data-stu-id="746e8-393">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="746e8-394">**Można ustawić przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="746e8-394">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="746e8-395">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="746e8-395">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="746e8-396">Zawartość katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="746e8-396">Content root</span></span>

<span data-ttu-id="746e8-397">To ustawienie określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="746e8-397">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="746e8-398">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="746e8-398">**Key**: contentRoot</span></span>  
<span data-ttu-id="746e8-399">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-399">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-400">**Domyślne**: Wartość domyślna to folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-400">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="746e8-401">**Można ustawić przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="746e8-401">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="746e8-402">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="746e8-402">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="746e8-403">Jeśli ścieżka nie istnieje, host nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="746e8-403">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a><span data-ttu-id="746e8-404">Środowisko</span><span class="sxs-lookup"><span data-stu-id="746e8-404">Environment</span></span>

<span data-ttu-id="746e8-405">Ustawia aplikacji [środowiska](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="746e8-405">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="746e8-406">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="746e8-406">**Key**: environment</span></span>  
<span data-ttu-id="746e8-407">**Typ**: *ciągu*</span><span class="sxs-lookup"><span data-stu-id="746e8-407">**Type**: *string*</span></span>  
<span data-ttu-id="746e8-408">**Domyślne**: Produkcji</span><span class="sxs-lookup"><span data-stu-id="746e8-408">**Default**: Production</span></span>  
<span data-ttu-id="746e8-409">**Można ustawić przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="746e8-409">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="746e8-410">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="746e8-410">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="746e8-411">Środowisko można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="746e8-411">The environment can be set to any value.</span></span> <span data-ttu-id="746e8-412">Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`.</span><span class="sxs-lookup"><span data-stu-id="746e8-412">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="746e8-413">Wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="746e8-413">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="746e8-414">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="746e8-414">ConfigureHostConfiguration</span></span>

<span data-ttu-id="746e8-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-415"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="746e8-416">Konfiguracja hosta jest wykorzystywany do inicjacji <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-416">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="746e8-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="746e8-417"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="746e8-418">Host używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="746e8-418">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="746e8-419">Brak dostawców są domyślnie dołączone.</span><span class="sxs-lookup"><span data-stu-id="746e8-419">No providers are included by default.</span></span> <span data-ttu-id="746e8-420">Należy jawnie określić niezależnie od dostawcy konfiguracji aplikacji wymaga w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, w tym:</span><span class="sxs-lookup"><span data-stu-id="746e8-420">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="746e8-421">Plik konfiguracji (na przykład z *hostsettings.json* pliku).</span><span class="sxs-lookup"><span data-stu-id="746e8-421">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="746e8-422">Zmienne konfiguracji środowiska.</span><span class="sxs-lookup"><span data-stu-id="746e8-422">Environment variable configuration.</span></span>
* <span data-ttu-id="746e8-423">Konfiguracja argumentu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="746e8-423">Command-line argument configuration.</span></span>
* <span data-ttu-id="746e8-424">Innych dostawców wymaganej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="746e8-424">Any other required configuration providers.</span></span>

<span data-ttu-id="746e8-425">Plik konfiguracji hosta jest włączona, określając ścieżki podstawowej aplikacji za pomocą `SetBasePath` następuje wywołanie jednej z [pliku dostawcy konfiguracji](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="746e8-425">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="746e8-426">Przykładowa aplikacja używa pliku JSON, *hostsettings.json*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> korzystanie z ustawień konfiguracji hosta tego pliku.</span><span class="sxs-lookup"><span data-stu-id="746e8-426">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="746e8-427">Aby dodać [zmiennych konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w Konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-427">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="746e8-428">`AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="746e8-428">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="746e8-429">Przykładowa aplikacja korzysta z prefiksem `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="746e8-429">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="746e8-430">Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane.</span><span class="sxs-lookup"><span data-stu-id="746e8-430">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="746e8-431">Po skonfigurowaniu hostów przykładową aplikację, wartość zmiennej środowiskowej, aby uzyskać `PREFIX_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.</span><span class="sxs-lookup"><span data-stu-id="746e8-431">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="746e8-432">Podczas programowania, korzystając z [programu Visual Studio](https://visualstudio.microsoft.com) lub uruchamianie aplikacji za pomocą `dotnet run`, zmienne środowiskowe, może być ustawiona w *Properties/launchSettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="746e8-432">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="746e8-433">W [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *.vscode/launch.json* pliku podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="746e8-433">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="746e8-434">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="746e8-434">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="746e8-435">[Wiersza polecenia konfiguracji](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawany przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-435">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="746e8-436">Konfiguracja wiersza polecenia zostanie dodany ostatnio pozwalającą na argumenty wiersza polecenia, aby zastąpić konfiguracji udostępnianych przez starszych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="746e8-436">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="746e8-437">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="746e8-437">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="746e8-438">Dodatkowa konfiguracja może być wyposażone w [applicationName](#application-key-name) i [contentRoot](#content-root) kluczy.</span><span class="sxs-lookup"><span data-stu-id="746e8-438">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="746e8-439">Przykład `HostBuilder` konfiguracji przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="746e8-439">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="746e8-440">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="746e8-440">ConfigureAppConfiguration</span></span>

<span data-ttu-id="746e8-441">Konfiguracja aplikacji jest tworzony przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> na <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-441">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="746e8-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-442"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="746e8-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="746e8-443"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="746e8-444">Aplikacja używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="746e8-444">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="746e8-445">Konfiguracji utworzonej przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> znajduje się w temacie [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-445">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="746e8-446">Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta, dostarczone przez [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="746e8-446">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="746e8-447">Przykład korzystającą configuration <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="746e8-447">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="746e8-448">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="746e8-448">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="746e8-449">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="746e8-449">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="746e8-450">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="746e8-450">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="746e8-451">Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="746e8-451">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="746e8-452">Przykładowa aplikacja przenosi jego pliki ustawień aplikacji w formacie JSON i *hostsettings.json* następującym `<Content>` elementu:</span><span class="sxs-lookup"><span data-stu-id="746e8-452">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="746e8-453">Metody rozszerzenia konfiguracji, takich jak <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> i <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> wymagają dodatkowych pakietów NuGet, takich jak [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) i [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="746e8-453">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="746e8-454">Chyba że aplikacja korzysta z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), te pakiety muszą zostać dodane do projektu, oprócz podstawowe [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) pakietu.</span><span class="sxs-lookup"><span data-stu-id="746e8-454">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="746e8-455">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="746e8-455">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="746e8-456">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="746e8-456">ConfigureServices</span></span>

<span data-ttu-id="746e8-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usług do aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="746e8-457"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="746e8-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="746e8-458"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="746e8-459">Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="746e8-459">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="746e8-460">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="746e8-460">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="746e8-461">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` metodę rozszerzenia, aby dodać usługę do zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu `TimedHostedService`, do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="746e8-461">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="746e8-462">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="746e8-462">ConfigureLogging</span></span>

<span data-ttu-id="746e8-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podane <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="746e8-463"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="746e8-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> może zostać wywołana wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="746e8-464"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="746e8-465">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="746e8-465">UseConsoleLifetime</span></span>

<span data-ttu-id="746e8-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje sygnał SIGTERM lub wywołań i Ctrl + C/SIGINT <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> można uruchomić procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="746e8-466"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="746e8-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="746e8-467"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="746e8-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> wstępnie jest zarejestrowany jako domyślna implementacja okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="746e8-468"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="746e8-469">Ostatniego okresu istnienia zarejestrowany jest używany.</span><span class="sxs-lookup"><span data-stu-id="746e8-469">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="746e8-470">Konfiguracja kontenera</span><span class="sxs-lookup"><span data-stu-id="746e8-470">Container configuration</span></span>

<span data-ttu-id="746e8-471">Aby zapewnić obsługę podłączania innych kontenerów, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="746e8-471">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="746e8-472">Zapewnianie fabrykę nie jest częścią DI rejestracja kontenera jest jednak wewnętrzne hosta używany do tworzenia konkretnych kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="746e8-472">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="746e8-473">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę użyty do utworzenia dostawcy usługi app service.</span><span class="sxs-lookup"><span data-stu-id="746e8-473">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="746e8-474">Konfiguracja kontenera niestandardowego jest zarządzana przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> metody.</span><span class="sxs-lookup"><span data-stu-id="746e8-474">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="746e8-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> udostępnia silnie typizowane środowisko na potrzeby konfigurowania kontenera na podstawie odpowiedniego hosta interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="746e8-475"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="746e8-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> można wywołać wiele razy z wynikami dodatku.</span><span class="sxs-lookup"><span data-stu-id="746e8-476"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="746e8-477">Tworzenie kontenera usług dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="746e8-477">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="746e8-478">Podaj fabryki kontenera usług:</span><span class="sxs-lookup"><span data-stu-id="746e8-478">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="746e8-479">Używaj fabryki i konfigurowanie kontenera niestandardowego usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="746e8-479">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="746e8-480">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="746e8-480">Extensibility</span></span>

<span data-ttu-id="746e8-481">Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="746e8-481">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="746e8-482">W poniższym przykładzie pokazano, jak metoda rozszerzenia rozszerza <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementację [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przykład przedstawiona w <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="746e8-482">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="746e8-483">Aplikacja ustanawia `UseHostedService` metodę rozszerzenia, aby zarejestrować usługi hostowanej przekazanej `T`:</span><span class="sxs-lookup"><span data-stu-id="746e8-483">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="746e8-484">Zarządzać hosta</span><span class="sxs-lookup"><span data-stu-id="746e8-484">Manage the host</span></span>

<span data-ttu-id="746e8-485"><xref:Microsoft.Extensions.Hosting.IHost> Implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji, które są zarejestrowane w usłudze service container.</span><span class="sxs-lookup"><span data-stu-id="746e8-485">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="746e8-486">Uruchom</span><span class="sxs-lookup"><span data-stu-id="746e8-486">Run</span></span>

<span data-ttu-id="746e8-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uruchamia aplikację i blokuje wątek wywołujący, aż zamknie hosta:</span><span class="sxs-lookup"><span data-stu-id="746e8-487"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="746e8-488">RunAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-488">RunAsync</span></span>

<span data-ttu-id="746e8-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="746e8-489"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="746e8-490">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-490">RunConsoleAsync</span></span>

<span data-ttu-id="746e8-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Włączenie obsługi konsoli, kompilacje uruchamia hosta i czeka na klawisze Ctrl + C/SIGINT lub SIGTERM zamknąć.</span><span class="sxs-lookup"><span data-stu-id="746e8-491"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="746e8-492">Początkowy i StopAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-492">Start and StopAsync</span></span>

<span data-ttu-id="746e8-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="746e8-493"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="746e8-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="746e8-494"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="746e8-495">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-495">StartAsync and StopAsync</span></span>

<span data-ttu-id="746e8-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> Uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="746e8-496"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="746e8-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> Zatrzymuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="746e8-497"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="746e8-498">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="746e8-498">WaitForShutdown</span></span>

<span data-ttu-id="746e8-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalany za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (nasłuchuje klawisze Ctrl + C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="746e8-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="746e8-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="746e8-501">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="746e8-501">WaitForShutdownAsync</span></span>

<span data-ttu-id="746e8-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> Zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="746e8-502"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="746e8-503">Zewnętrznej kontroli</span><span class="sxs-lookup"><span data-stu-id="746e8-503">External control</span></span>

<span data-ttu-id="746e8-504">Zewnętrznej kontroli hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="746e8-504">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="746e8-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> nosi nazwę na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który powoduje oczekiwanie, dopóki nie zostanie ukończone przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="746e8-505"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="746e8-506">Może to służyć do opóźnienie uruchamiania, dopóki nie zasygnalizowane przez zdarzenie zewnętrzne.</span><span class="sxs-lookup"><span data-stu-id="746e8-506">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="746e8-507">Interfejs IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="746e8-507">IHostingEnvironment interface</span></span>

<span data-ttu-id="746e8-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o aplikacji w środowisku hostingu.</span><span class="sxs-lookup"><span data-stu-id="746e8-508"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="746e8-509">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> aby można było używać jej właściwości i metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="746e8-509">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="746e8-510">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="746e8-510">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="746e8-511">Interfejs IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="746e8-511">IApplicationLifetime interface</span></span>

<span data-ttu-id="746e8-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> Umożliwia działań po uruchamiania i zamykania, w tym łagodne zamykanie żądania.</span><span class="sxs-lookup"><span data-stu-id="746e8-512"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="746e8-513">Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania <xref:System.Action> metod, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="746e8-513">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="746e8-514">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="746e8-514">Cancellation Token</span></span> | <span data-ttu-id="746e8-515">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="746e8-515">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="746e8-516">Host pełni została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="746e8-516">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="746e8-517">Host jest kończonych łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="746e8-517">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="746e8-518">Wszystkie żądania powinny zostać przetworzone.</span><span class="sxs-lookup"><span data-stu-id="746e8-518">All requests should be processed.</span></span> <span data-ttu-id="746e8-519">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="746e8-519">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="746e8-520">Wykonuje łagodne zamykanie hosta.</span><span class="sxs-lookup"><span data-stu-id="746e8-520">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="746e8-521">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="746e8-521">Requests may still be processing.</span></span> <span data-ttu-id="746e8-522">Blokuje zamknięcia, aż do zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="746e8-522">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="746e8-523">Wstrzykiwanie Konstruktor <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> usługi do każdej klasy.</span><span class="sxs-lookup"><span data-stu-id="746e8-523">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="746e8-524">[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji) do rejestrowania zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="746e8-524">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="746e8-525">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="746e8-525">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="746e8-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="746e8-526"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="746e8-527">Następujące klasy używa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="746e8-527">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="746e8-528">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="746e8-528">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
