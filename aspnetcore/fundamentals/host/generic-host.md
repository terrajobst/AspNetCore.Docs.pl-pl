---
title: Host ogólny .NET
author: rick-anderson
description: Dowiedz się więcej o hoście ogólnym programu .NET Core, który jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: f14917ad924e2c762a14c2cb5f51391d4be06e7b
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378753"
---
# <a name="net-generic-host"></a><span data-ttu-id="68f23-103">Host ogólny .NET</span><span class="sxs-lookup"><span data-stu-id="68f23-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68f23-104">W tym artykule przedstawiono hosta ogólnego platformy .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) i przedstawiono wskazówki dotyczące korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="68f23-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="68f23-105">Co to jest host?</span><span class="sxs-lookup"><span data-stu-id="68f23-105">What's a host?</span></span>

<span data-ttu-id="68f23-106">*Host* to obiekt, który hermetyzuje zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="68f23-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="68f23-107">Iniekcja zależności (DI)</span><span class="sxs-lookup"><span data-stu-id="68f23-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="68f23-108">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="68f23-108">Logging</span></span>
* <span data-ttu-id="68f23-109">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="68f23-109">Configuration</span></span>
* <span data-ttu-id="68f23-110">implementacje `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="68f23-110">`IHostedService` implementations</span></span>

<span data-ttu-id="68f23-111">Po uruchomieniu hosta wywołuje `IHostedService.StartAsync` dla każdej implementacji <xref:Microsoft.Extensions.Hosting.IHostedService>, która znajduje się w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="68f23-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="68f23-112">W aplikacji sieci Web jedna z implementacji `IHostedService` to usługa sieci Web, która uruchamia [implementację serwera http](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="68f23-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="68f23-113">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="68f23-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="68f23-114">W wersjach ASP.NET Core wcześniejszych niż 3,0 [host sieci Web](xref:fundamentals/host/web-host) jest używany do obciążeń http.</span><span class="sxs-lookup"><span data-stu-id="68f23-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="68f23-115">Host sieci Web nie jest już zalecany dla aplikacji sieci Web i pozostaje dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="68f23-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="68f23-116">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="68f23-116">Set up a host</span></span>

<span data-ttu-id="68f23-117">Host jest zazwyczaj konfigurowany, zbudowany i uruchamiany przez kod w klasie `Program`.</span><span class="sxs-lookup"><span data-stu-id="68f23-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="68f23-118">Metoda `Main`:</span><span class="sxs-lookup"><span data-stu-id="68f23-118">The `Main` method:</span></span>

* <span data-ttu-id="68f23-119">Wywołuje metodę `CreateHostBuilder` w celu utworzenia i skonfigurowania obiektu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="68f23-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="68f23-120">Wywołuje metody `Build` i `Run` w obiekcie konstruktora.</span><span class="sxs-lookup"><span data-stu-id="68f23-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="68f23-121">Oto kod *program.cs* dla obciążeń innych niż HTTP z jedną implementacją `IHostedService`, która została dodana do kontenera di.</span><span class="sxs-lookup"><span data-stu-id="68f23-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="68f23-122">W przypadku obciążeń HTTP Metoda `Main` jest taka sama, ale wywołania `CreateHostBuilder` `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="68f23-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="68f23-123">Jeśli aplikacja używa Entity Framework Core, nie zmieniaj nazwy ani podpisu metody `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68f23-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="68f23-124">[Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) oczekują na znalezienie metody `CreateHostBuilder`, która konfiguruje hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="68f23-125">Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="68f23-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="68f23-126">Ustawienia domyślnego konstruktora</span><span class="sxs-lookup"><span data-stu-id="68f23-126">Default builder settings</span></span>

<span data-ttu-id="68f23-127">Metoda <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="68f23-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="68f23-128">Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na ścieżkę zwracaną przez <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="68f23-129">Ładuje konfigurację hosta z:</span><span class="sxs-lookup"><span data-stu-id="68f23-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="68f23-130">Zmienne środowiskowe poprzedzone prefiksem "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="68f23-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="68f23-131">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="68f23-131">Command-line arguments.</span></span>
* <span data-ttu-id="68f23-132">Ładuje konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="68f23-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="68f23-133">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="68f23-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="68f23-134">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="68f23-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="68f23-135">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development`.</span><span class="sxs-lookup"><span data-stu-id="68f23-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="68f23-136">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="68f23-136">Environment variables.</span></span>
  * <span data-ttu-id="68f23-137">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="68f23-137">Command-line arguments.</span></span>
* <span data-ttu-id="68f23-138">Dodaje następujących dostawców [rejestrowania](xref:fundamentals/logging/index) :</span><span class="sxs-lookup"><span data-stu-id="68f23-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="68f23-139">Konsola</span><span class="sxs-lookup"><span data-stu-id="68f23-139">Console</span></span>
  * <span data-ttu-id="68f23-140">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="68f23-140">Debug</span></span>
  * <span data-ttu-id="68f23-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="68f23-141">EventSource</span></span>
  * <span data-ttu-id="68f23-142">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="68f23-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="68f23-143">Umożliwia [weryfikację zakresu](xref:fundamentals/dependency-injection#scope-validation) i [Sprawdzanie poprawności zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) , gdy środowisko jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="68f23-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="68f23-144">Metoda `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="68f23-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="68f23-145">Ładuje konfigurację hosta ze zmiennych środowiskowych poprzedzonych prefiksem "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="68f23-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="68f23-146">Ustawia serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web i konfiguruje go przy użyciu dostawców konfiguracji hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="68f23-147">Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="68f23-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="68f23-148">Dodaje [oprogramowanie pośredniczące do filtrowania hosta](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="68f23-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="68f23-149">Dodaje [przekazane nagłówki pośredniczące](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) , jeśli ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.</span><span class="sxs-lookup"><span data-stu-id="68f23-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="68f23-150">Włącza integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="68f23-150">Enables IIS integration.</span></span> <span data-ttu-id="68f23-151">Aby poznać domyślne opcje usług IIS, zobacz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="68f23-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="68f23-152">[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [Ustawienia dla usługi Web Apps](#settings-for-web-apps) w dalszej części tego artykułu pokazują, jak zastąpić ustawienia domyślnego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="68f23-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="68f23-153">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="68f23-153">Framework-provided services</span></span>

<span data-ttu-id="68f23-154">Usługi zarejestrowane automatycznie obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="68f23-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="68f23-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="68f23-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="68f23-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="68f23-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="68f23-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="68f23-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="68f23-158">Aby uzyskać więcej informacji na temat usług udostępnianych przez platformę, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="68f23-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="68f23-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="68f23-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="68f23-160">Wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) do dowolnej klasy w celu obsługi zadań po uruchomieniu i bezpiecznego zamykania.</span><span class="sxs-lookup"><span data-stu-id="68f23-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="68f23-161">Trzy właściwości interfejsu to tokeny anulowania używane do rejestrowania metod obsługi zdarzeń uruchamiania i zatrzymywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="68f23-162">Interfejs zawiera również metodę `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="68f23-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="68f23-163">Poniższy przykład to implementacja `IHostedService`, która rejestruje zdarzenia `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="68f23-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="68f23-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="68f23-164">IHostLifetime</span></span>

<span data-ttu-id="68f23-165">Implementacja <xref:Microsoft.Extensions.Hosting.IHostLifetime> kontroluje moment uruchomienia hosta i jego zatrzymania.</span><span class="sxs-lookup"><span data-stu-id="68f23-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="68f23-166">Używana jest Ostatnia zarejestrowana implementacja.</span><span class="sxs-lookup"><span data-stu-id="68f23-166">The last implementation registered is used.</span></span>

<span data-ttu-id="68f23-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` to domyślna implementacja `IHostLifetime`.</span><span class="sxs-lookup"><span data-stu-id="68f23-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="68f23-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="68f23-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="68f23-169">nasłuchuje kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, aby rozpocząć proces zamykania.</span><span class="sxs-lookup"><span data-stu-id="68f23-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="68f23-170">Odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="68f23-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="68f23-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="68f23-171">IHostEnvironment</span></span>

<span data-ttu-id="68f23-172">Wsuń usługę <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do klasy, aby uzyskać informacje o następujących elementach:</span><span class="sxs-lookup"><span data-stu-id="68f23-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="68f23-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="68f23-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="68f23-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="68f23-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="68f23-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="68f23-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="68f23-176">Aplikacje sieci Web implementują interfejs `IWebHostEnvironment`, który dziedziczy `IHostEnvironment` i dodaje:</span><span class="sxs-lookup"><span data-stu-id="68f23-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="68f23-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="68f23-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="68f23-178">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="68f23-178">Host configuration</span></span>

<span data-ttu-id="68f23-179">Konfiguracja hosta jest używana we właściwościach implementacji <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="68f23-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="68f23-180">Konfiguracja hosta jest dostępna z [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="68f23-181">Po `ConfigureAppConfiguration` `HostBuilderContext.Configuration` jest zastępowana konfiguracją aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="68f23-182">Aby dodać konfigurację hosta, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68f23-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="68f23-183">`ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="68f23-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="68f23-184">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="68f23-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="68f23-185">Dostawca zmiennych środowiskowych z prefiksami `DOTNET_` i argumenty wiersza polecenia są uwzględniane przez CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="68f23-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="68f23-186">W przypadku usługi Web Apps zostanie dodany dostawca zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="68f23-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="68f23-187">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="68f23-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="68f23-188">Na przykład wartość zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` zmieni się na wartość konfiguracji hosta dla klucza `environment`.</span><span class="sxs-lookup"><span data-stu-id="68f23-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="68f23-189">Poniższy przykład umożliwia utworzenie konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="68f23-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="68f23-190">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="68f23-190">App configuration</span></span>

<span data-ttu-id="68f23-191">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="68f23-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="68f23-192">`ConfigureAppConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="68f23-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="68f23-193">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="68f23-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="68f23-194">Konfiguracja utworzona przez `ConfigureAppConfiguration` jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i jako usługa z di.</span><span class="sxs-lookup"><span data-stu-id="68f23-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="68f23-195">Konfiguracja hosta jest również dodawana do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="68f23-196">Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="68f23-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="68f23-197">Ustawienia dla wszystkich typów aplikacji</span><span class="sxs-lookup"><span data-stu-id="68f23-197">Settings for all app types</span></span>

<span data-ttu-id="68f23-198">Ta sekcja zawiera listę ustawień hosta, które dotyczą zarówno obciążeń HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="68f23-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="68f23-199">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="68f23-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="68f23-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="68f23-200">ApplicationName</span></span>

<span data-ttu-id="68f23-201">Właściwość [IHostEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="68f23-202">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="68f23-202">**Key**: applicationName</span></span>  
<span data-ttu-id="68f23-203">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-203">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-204">**Wartość domyślna**: Nazwa zestawu, który zawiera punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="68f23-205">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="68f23-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="68f23-206">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="68f23-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="68f23-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="68f23-207">ContentRootPath</span></span>

<span data-ttu-id="68f23-208">Właściwość [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="68f23-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="68f23-209">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="68f23-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="68f23-210">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="68f23-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="68f23-211">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-211">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-212">**Domyślnie**: folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="68f23-213">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="68f23-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="68f23-214">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseContentRoot` w `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="68f23-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="68f23-215">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="68f23-215">For more information, see:</span></span>

* [<span data-ttu-id="68f23-216">Podstawy: zawartość główna</span><span class="sxs-lookup"><span data-stu-id="68f23-216">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="68f23-217">WebRoot</span><span class="sxs-lookup"><span data-stu-id="68f23-217">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="68f23-218">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="68f23-218">EnvironmentName</span></span>

<span data-ttu-id="68f23-219">Dla właściwości [IHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="68f23-219">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="68f23-220">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging` i `Production`.</span><span class="sxs-lookup"><span data-stu-id="68f23-220">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="68f23-221">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="68f23-221">Values aren't case sensitive.</span></span>

<span data-ttu-id="68f23-222">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="68f23-222">**Key**: environment</span></span>  
<span data-ttu-id="68f23-223">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-223">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-224">**Wartość domyślna**: produkcja</span><span class="sxs-lookup"><span data-stu-id="68f23-224">**Default**: Production</span></span>  
<span data-ttu-id="68f23-225">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="68f23-225">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="68f23-226">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseEnvironment` w `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="68f23-226">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="68f23-227">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="68f23-227">ShutdownTimeout</span></span>

<span data-ttu-id="68f23-228">[HostOptions. shutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="68f23-229">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="68f23-229">The default value is five seconds.</span></span>  <span data-ttu-id="68f23-230">Podczas okresu przekroczenia limitu czasu Host:</span><span class="sxs-lookup"><span data-stu-id="68f23-230">During the timeout period, the host:</span></span>

* <span data-ttu-id="68f23-231">Wyzwala [IHostApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="68f23-231">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="68f23-232">Próbuje zatrzymać usługi hostowane, rejestrowanie błędów dla usług, których zatrzymanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="68f23-232">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="68f23-233">Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-233">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="68f23-234">Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="68f23-234">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="68f23-235">Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="68f23-235">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="68f23-236">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="68f23-236">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="68f23-237">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="68f23-237">**Type**: *int*</span></span>  
<span data-ttu-id="68f23-238">**Domyślnie**: 5 sekund **zmienna środowiskowa**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="68f23-238">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="68f23-239">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub skonfiguruj `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="68f23-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="68f23-240">Poniższy przykład ustawia limit czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="68f23-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="68f23-241">Ustawienia usługi Web Apps</span><span class="sxs-lookup"><span data-stu-id="68f23-241">Settings for web apps</span></span>

<span data-ttu-id="68f23-242">Niektóre ustawienia hosta mają zastosowanie tylko do obciążeń protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="68f23-242">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="68f23-243">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="68f23-243">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="68f23-244">Metody rozszerzające w `IWebHostBuilder` są dostępne dla tych ustawień.</span><span class="sxs-lookup"><span data-stu-id="68f23-244">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="68f23-245">Przykłady kodu, które pokazują, jak wywołać metody rozszerzenia przyjmuje się, że `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="68f23-245">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="68f23-246">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="68f23-246">CaptureStartupErrors</span></span>

<span data-ttu-id="68f23-247">Gdy `false`, błędy podczas uruchamiania, kończenie działania hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-247">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="68f23-248">Gdy `true`, Host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="68f23-248">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="68f23-249">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="68f23-249">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="68f23-250">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="68f23-250">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="68f23-251">**Domyślnie**: wartość domyślna to `false`, chyba że aplikacja zostanie uruchomiona z Kestrel za pomocą usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="68f23-251">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="68f23-252">**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="68f23-252">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="68f23-253">Aby ustawić tę wartość, użyj opcji Configuration lub Call `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="68f23-253">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="68f23-254">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="68f23-254">DetailedErrors</span></span>

<span data-ttu-id="68f23-255">Po włączeniu lub gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe błędy.</span><span class="sxs-lookup"><span data-stu-id="68f23-255">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="68f23-256">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="68f23-256">**Key**: detailedErrors</span></span>  
<span data-ttu-id="68f23-257">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="68f23-257">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="68f23-258">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="68f23-258">**Default**: false</span></span>  
<span data-ttu-id="68f23-259">**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="68f23-259">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="68f23-260">Aby ustawić tę wartość, użyj opcji Configuration lub Call `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="68f23-260">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="68f23-261">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="68f23-261">HostingStartupAssemblies</span></span>

<span data-ttu-id="68f23-262">Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="68f23-262">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="68f23-263">Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-263">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="68f23-264">W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="68f23-264">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="68f23-265">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="68f23-265">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="68f23-266">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-266">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-267">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="68f23-267">**Default**: Empty string</span></span>  
<span data-ttu-id="68f23-268">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="68f23-268">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="68f23-269">Aby ustawić tę wartość, użyj opcji Configuration lub Call `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="68f23-269">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="68f23-270">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="68f23-270">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="68f23-271">Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="68f23-271">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="68f23-272">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="68f23-272">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="68f23-273">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-273">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-274">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="68f23-274">**Default**: Empty string</span></span>  
<span data-ttu-id="68f23-275">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="68f23-275">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="68f23-276">Aby ustawić tę wartość, użyj opcji Configuration lub Call `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="68f23-276">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="68f23-277">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="68f23-277">HTTPS_Port</span></span>

<span data-ttu-id="68f23-278">Port przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="68f23-278">The HTTPS redirect port.</span></span> <span data-ttu-id="68f23-279">Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="68f23-279">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="68f23-280">**Klucz**: https_port</span><span class="sxs-lookup"><span data-stu-id="68f23-280">**Key**: https_port</span></span>  
<span data-ttu-id="68f23-281">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-281">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-282">Wartość **Domyślna**: nie ustawiono wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="68f23-282">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="68f23-283">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="68f23-283">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="68f23-284">Aby ustawić tę wartość, użyj opcji Configuration lub Call `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="68f23-284">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="68f23-285">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="68f23-285">PreferHostingUrls</span></span>

<span data-ttu-id="68f23-286">Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy użyciu `IWebHostBuilder` zamiast dla skonfigurowanych przy użyciu implementacji `IServer`.</span><span class="sxs-lookup"><span data-stu-id="68f23-286">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="68f23-287">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="68f23-287">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="68f23-288">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="68f23-288">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="68f23-289">Wartość **Domyślna**: prawda</span><span class="sxs-lookup"><span data-stu-id="68f23-289">**Default**: true</span></span>  
<span data-ttu-id="68f23-290">**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="68f23-290">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="68f23-291">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="68f23-291">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="68f23-292">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="68f23-292">PreventHostingStartup</span></span>

<span data-ttu-id="68f23-293">Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-293">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="68f23-294">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="68f23-294">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="68f23-295">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="68f23-295">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="68f23-296">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="68f23-296">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="68f23-297">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="68f23-297">**Default**: false</span></span>  
<span data-ttu-id="68f23-298">**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="68f23-298">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="68f23-299">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="68f23-299">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="68f23-300">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="68f23-300">StartupAssembly</span></span>

<span data-ttu-id="68f23-301">Zestaw do wyszukiwania klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="68f23-301">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="68f23-302">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="68f23-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="68f23-303">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-303">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-304">**Domyślnie**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="68f23-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="68f23-305">**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="68f23-305">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="68f23-306">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="68f23-306">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="68f23-307">`UseStartup` może przyjmować nazwę zestawu (`string`) lub typ (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="68f23-307">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="68f23-308">W przypadku wywołania wielu metod `UseStartup` ostatni ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="68f23-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="68f23-309">adresy URL</span><span class="sxs-lookup"><span data-stu-id="68f23-309">URLs</span></span>

<span data-ttu-id="68f23-310">Rozdzielana średnikami lista adresów IP lub adresów hostów z portami i protokołami, na których serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="68f23-310">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="68f23-311">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="68f23-311">For example, `http://localhost:123`.</span></span> <span data-ttu-id="68f23-312">Użyj opcji "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="68f23-312">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="68f23-313">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="68f23-313">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="68f23-314">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="68f23-314">Supported formats vary among servers.</span></span>

<span data-ttu-id="68f23-315">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="68f23-315">**Key**: urls</span></span>  
<span data-ttu-id="68f23-316">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-316">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-317">**Wartość domyślna**: `http://localhost:5000` i `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="68f23-317">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="68f23-318">**Zmienna środowiskowa**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="68f23-318">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="68f23-319">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="68f23-319">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="68f23-320">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="68f23-320">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="68f23-321">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="68f23-321">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="68f23-322">WebRoot</span><span class="sxs-lookup"><span data-stu-id="68f23-322">WebRoot</span></span>

<span data-ttu-id="68f23-323">Ścieżka względna do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-323">The relative path to the app's static assets.</span></span>

<span data-ttu-id="68f23-324">**Klucz**: Webroot</span><span class="sxs-lookup"><span data-stu-id="68f23-324">**Key**: webroot</span></span>  
<span data-ttu-id="68f23-325">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-325">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-326">**Wartość domyślna**: wartość domyślna to `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="68f23-326">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="68f23-327">Ścieżka do *elementu {content root}/wwwroot* musi istnieć.</span><span class="sxs-lookup"><span data-stu-id="68f23-327">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="68f23-328">Jeśli ścieżka nie istnieje, jest używany dostawca plików No-op.</span><span class="sxs-lookup"><span data-stu-id="68f23-328">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="68f23-329">**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="68f23-329">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="68f23-330">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="68f23-330">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="68f23-331">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="68f23-331">For more information, see:</span></span>

* [<span data-ttu-id="68f23-332">Podstawy: katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="68f23-332">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="68f23-333">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="68f23-333">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="68f23-334">Zarządzanie okresem istnienia hosta</span><span class="sxs-lookup"><span data-stu-id="68f23-334">Manage the host lifetime</span></span>

<span data-ttu-id="68f23-335">Wywołaj metody na skompilowanej implementacji <xref:Microsoft.Extensions.Hosting.IHost>, aby uruchomić i zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="68f23-335">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="68f23-336">Te metody mają wpływ na wszystkie implementacje <xref:Microsoft.Extensions.Hosting.IHostedService>, które są zarejestrowane w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="68f23-336">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="68f23-337">Uruchom</span><span class="sxs-lookup"><span data-stu-id="68f23-337">Run</span></span>

<span data-ttu-id="68f23-338"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-338"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="68f23-339">RunAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-339">RunAsync</span></span>

<span data-ttu-id="68f23-340"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="68f23-340"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="68f23-341">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-341">RunConsoleAsync</span></span>

<span data-ttu-id="68f23-342"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> włącza obsługę konsoli, kompiluje i uruchamia hosta i czeka na zamknięcie klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="68f23-342"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="68f23-343">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="68f23-343">Start</span></span>

<span data-ttu-id="68f23-344"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia Host synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="68f23-344"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="68f23-345">StartAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-345">StartAsync</span></span>

<span data-ttu-id="68f23-346"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task>, który kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="68f23-346"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="68f23-347"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku `StartAsync`, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="68f23-347"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="68f23-348">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="68f23-348">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="68f23-349">StopAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-349">StopAsync</span></span>

<span data-ttu-id="68f23-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="68f23-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="68f23-351">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="68f23-351">WaitForShutdown</span></span>

<span data-ttu-id="68f23-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący do momentu wyzwolenia przez IHostLifetime, na przykład za pośrednictwem kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="68f23-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="68f23-353">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-353">WaitForShutdownAsync</span></span>

<span data-ttu-id="68f23-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, które kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="68f23-355">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="68f23-355">External control</span></span>

<span data-ttu-id="68f23-356">Bezpośrednią kontrolę nad okresem istnienia hosta można uzyskać przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="68f23-356">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="68f23-357">ASP.NET Core aplikacje konfigurują i uruchamiają hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-357">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="68f23-358">Host jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.</span><span class="sxs-lookup"><span data-stu-id="68f23-358">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="68f23-359">W tym artykule opisano ASP.NET Core hosta ogólnego (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="68f23-359">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="68f23-360">Przeznaczeniem hosta ogólnego jest oddzielenie potoku HTTP od interfejsu API hosta sieci Web w celu umożliwienia szerszej tablicy scenariuszy hostów.</span><span class="sxs-lookup"><span data-stu-id="68f23-360">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="68f23-361">Obsługa komunikatów, zadań w tle i innych obciążeń innych niż HTTP na podstawie ogólnej korzyści z hosta, takich jak konfiguracja, iniekcja zależności (DI) i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="68f23-361">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="68f23-362">Host ogólny jest nowy w ASP.NET Core 2,1 i nie jest odpowiedni dla scenariuszy hostingu w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="68f23-362">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="68f23-363">W przypadku scenariuszy hostingu w sieci Web należy użyć [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="68f23-363">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="68f23-364">Host ogólny zastąpi hosta sieci Web w przyszłej wersji i będzie pełnić rolę podstawowego interfejsu API hosta zarówno w scenariuszach HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="68f23-364">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="68f23-365">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="68f23-365">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="68f23-366">Podczas uruchamiania przykładowej aplikacji w [Visual Studio Code](https://code.visualstudio.com/)należy użyć *zewnętrznego lub zintegrowanego terminalu*.</span><span class="sxs-lookup"><span data-stu-id="68f23-366">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="68f23-367">Nie uruchamiaj próbki w `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="68f23-367">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="68f23-368">Aby ustawić konsolę w Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="68f23-368">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="68f23-369">Otwórz plik *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="68f23-369">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="68f23-370">W konfiguracji **uruchamiania programu .NET Core (konsoli)** zlokalizuj wpis **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="68f23-370">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="68f23-371">Ustaw wartość na `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="68f23-371">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="68f23-372">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="68f23-372">Introduction</span></span>

<span data-ttu-id="68f23-373">Ogólna Biblioteka hostów jest dostępna w przestrzeni nazw <xref:Microsoft.Extensions.Hosting> i udostępniana przez pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) .</span><span class="sxs-lookup"><span data-stu-id="68f23-373">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="68f23-374">Pakiet `Microsoft.Extensions.Hosting` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszym).</span><span class="sxs-lookup"><span data-stu-id="68f23-374">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="68f23-375"><xref:Microsoft.Extensions.Hosting.IHostedService> to punkt wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="68f23-375"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="68f23-376">Każda implementacja <xref:Microsoft.Extensions.Hosting.IHostedService> jest wykonywana w kolejności [rejestracji usługi w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="68f23-376">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="68f23-377"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana dla każdej <xref:Microsoft.Extensions.Hosting.IHostedService> podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w odwrotnej kolejności rejestracji, gdy host zostanie bezpiecznie zamknięty.</span><span class="sxs-lookup"><span data-stu-id="68f23-377"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="68f23-378">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="68f23-378">Set up a host</span></span>

<span data-ttu-id="68f23-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> to główny składnik używany przez biblioteki i aplikacje do inicjowania, kompilowania i uruchamiania hosta:</span><span class="sxs-lookup"><span data-stu-id="68f23-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="68f23-380">Opcje</span><span class="sxs-lookup"><span data-stu-id="68f23-380">Options</span></span>

<span data-ttu-id="68f23-381"><xref:Microsoft.Extensions.Hosting.HostOptions> Skonfiguruj opcje dla <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="68f23-381"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="68f23-382">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="68f23-382">Shutdown timeout</span></span>

<span data-ttu-id="68f23-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="68f23-384">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="68f23-384">The default value is five seconds.</span></span>

<span data-ttu-id="68f23-385">Następująca konfiguracja opcji w `Program.Main` powoduje zwiększenie domyślnego 5-sekundowego limitu czasu zamykania na 20 s:</span><span class="sxs-lookup"><span data-stu-id="68f23-385">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="68f23-386">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="68f23-386">Default services</span></span>

<span data-ttu-id="68f23-387">Podczas inicjowania hosta zarejestrowano następujące usługi:</span><span class="sxs-lookup"><span data-stu-id="68f23-387">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="68f23-388">[Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="68f23-388">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="68f23-389">[Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="68f23-389">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="68f23-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="68f23-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="68f23-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="68f23-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="68f23-392">[Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="68f23-392">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="68f23-393">[Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="68f23-393">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="68f23-394">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="68f23-394">Host configuration</span></span>

<span data-ttu-id="68f23-395">Konfiguracja hosta jest tworzona przez:</span><span class="sxs-lookup"><span data-stu-id="68f23-395">Host configuration is created by:</span></span>

* <span data-ttu-id="68f23-396">Wywoływanie metod rozszerzających <xref:Microsoft.Extensions.Hosting.IHostBuilder> w celu ustawienia [katalogu głównego zawartości](#content-root) i [środowiska](#environment).</span><span class="sxs-lookup"><span data-stu-id="68f23-396">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="68f23-397">Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-397">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="68f23-398">Metody rozszerzające</span><span class="sxs-lookup"><span data-stu-id="68f23-398">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="68f23-399">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="68f23-399">Application key (name)</span></span>

<span data-ttu-id="68f23-400">Właściwość [IHostingEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-400">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="68f23-401">Aby jawnie ustawić wartość, użyj [HostDefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="68f23-401">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="68f23-402">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="68f23-402">**Key**: applicationName</span></span>  
<span data-ttu-id="68f23-403">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-403">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-404">**Wartość domyślna**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-404">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="68f23-405">**Ustaw przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="68f23-405">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="68f23-406">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="68f23-406">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="68f23-407">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="68f23-407">Content root</span></span>

<span data-ttu-id="68f23-408">To ustawienie określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="68f23-408">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="68f23-409">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="68f23-409">**Key**: contentRoot</span></span>  
<span data-ttu-id="68f23-410">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-410">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-411">**Domyślnie**: Domyślnie folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-411">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="68f23-412">**Ustaw przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="68f23-412">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="68f23-413">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="68f23-413">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="68f23-414">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="68f23-414">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="68f23-415">Aby uzyskać więcej informacji, zobacz temat [podstawy: zawartość główna](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="68f23-415">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="68f23-416">Środowisko</span><span class="sxs-lookup"><span data-stu-id="68f23-416">Environment</span></span>

<span data-ttu-id="68f23-417">Ustawia [środowisko](xref:fundamentals/environments)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-417">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="68f23-418">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="68f23-418">**Key**: environment</span></span>  
<span data-ttu-id="68f23-419">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="68f23-419">**Type**: *string*</span></span>  
<span data-ttu-id="68f23-420">**Wartość domyślna**: produkcja</span><span class="sxs-lookup"><span data-stu-id="68f23-420">**Default**: Production</span></span>  
<span data-ttu-id="68f23-421">**Ustaw przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="68f23-421">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="68f23-422">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="68f23-422">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="68f23-423">Dla środowiska można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="68f23-423">The environment can be set to any value.</span></span> <span data-ttu-id="68f23-424">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging` i `Production`.</span><span class="sxs-lookup"><span data-stu-id="68f23-424">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="68f23-425">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="68f23-425">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="68f23-426">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="68f23-426">ConfigureHostConfiguration</span></span>

<span data-ttu-id="68f23-427"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do utworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-427"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="68f23-428">Konfiguracja hosta służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-428">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="68f23-429"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="68f23-429"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="68f23-430">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="68f23-430">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="68f23-431">Żaden dostawca nie jest domyślnie uwzględniany.</span><span class="sxs-lookup"><span data-stu-id="68f23-431">No providers are included by default.</span></span> <span data-ttu-id="68f23-432">Należy jawnie określić wszystkich dostawców konfiguracji wymaganych przez aplikację w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, w tym:</span><span class="sxs-lookup"><span data-stu-id="68f23-432">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="68f23-433">Konfiguracja pliku (na przykład z pliku *HostSettings. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="68f23-433">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="68f23-434">Konfiguracja zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="68f23-434">Environment variable configuration.</span></span>
* <span data-ttu-id="68f23-435">Konfiguracja argumentu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="68f23-435">Command-line argument configuration.</span></span>
* <span data-ttu-id="68f23-436">Każdy inny wymagany dostawca konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="68f23-436">Any other required configuration providers.</span></span>

<span data-ttu-id="68f23-437">Konfiguracja pliku hosta jest włączana przez określenie ścieżki podstawowej aplikacji z `SetBasePath`, a następnie wywołaniem jednego z [dostawców konfiguracji plików](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="68f23-437">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="68f23-438">Przykładowa aplikacja używa pliku JSON, *HostSettings. JSON*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*>, aby użyć ustawień konfiguracji hosta pliku.</span><span class="sxs-lookup"><span data-stu-id="68f23-438">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="68f23-439">Aby dodać [konfigurację zmiennej środowiskowej](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, wywołaj <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-439">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="68f23-440">`AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="68f23-440">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="68f23-441">Przykładowa aplikacja używa prefiksu `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="68f23-441">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="68f23-442">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="68f23-442">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="68f23-443">Po skonfigurowaniu hosta przykładowej aplikacji wartość zmiennej środowiskowej dla `PREFIX_ENVIRONMENT` będzie wartością konfiguracji hosta dla klucza `environment`.</span><span class="sxs-lookup"><span data-stu-id="68f23-443">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="68f23-444">Podczas tworzenia w przypadku korzystania z [programu Visual Studio](https://visualstudio.microsoft.com) lub uruchamiania aplikacji z `dotnet run` zmienne środowiskowe można ustawić w pliku *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="68f23-444">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="68f23-445">W [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe mogą być ustawiane w pliku *. programu vscode/Launch. JSON* podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="68f23-445">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="68f23-446">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="68f23-446">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="68f23-447">[Konfiguracja wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawana przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-447">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="68f23-448">Konfiguracja wiersza polecenia jest dodawana jako Ostatnia, aby zezwolić na argumenty wiersza polecenia w celu przesłonięcia konfiguracji udostępnionej przez wcześniejszych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="68f23-448">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="68f23-449">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="68f23-449">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="68f23-450">Dodatkową konfigurację można uzyskać za pomocą programu [ApplicationName](#application-key-name) i kluczy [contentRoot](#content-root) .</span><span class="sxs-lookup"><span data-stu-id="68f23-450">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="68f23-451">Przykład `HostBuilder` konfiguracja przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="68f23-451">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="68f23-452">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="68f23-452">ConfigureAppConfiguration</span></span>

<span data-ttu-id="68f23-453">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w implementacji <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68f23-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="68f23-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do utworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="68f23-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="68f23-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="68f23-456">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="68f23-456">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="68f23-457">Konfiguracja utworzona przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-457">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="68f23-458">Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta dostarczoną przez [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="68f23-458">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="68f23-459">Przykładowa konfiguracja aplikacji przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="68f23-459">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="68f23-460">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="68f23-460">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="68f23-461">*appSettings. Plik Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="68f23-461">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="68f23-462">*appSettings. Production. JSON*:</span><span class="sxs-lookup"><span data-stu-id="68f23-462">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="68f23-463">Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="68f23-463">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="68f23-464">Aplikacja Przykładowa przenosi pliki ustawień aplikacji JSON i *HostSettings. JSON* o następujący `<Content>` element:</span><span class="sxs-lookup"><span data-stu-id="68f23-464">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="68f23-465">Metody rozszerzenia konfiguracji, takie jak <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> i <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> wymagają dodatkowych pakietów NuGet, takich jak [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) i [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="68f23-465">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="68f23-466">Jeśli aplikacja nie używa [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), należy dodać te pakiety do projektu oprócz podstawowego pakietu [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) .</span><span class="sxs-lookup"><span data-stu-id="68f23-466">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="68f23-467">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="68f23-467">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="68f23-468">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="68f23-468">ConfigureServices</span></span>

<span data-ttu-id="68f23-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usługi do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="68f23-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="68f23-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="68f23-471">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="68f23-471">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="68f23-472">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="68f23-472">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="68f23-473">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa metody rozszerzenia `AddHostedService`, aby dodać usługę dla zdarzeń okresu istnienia, `LifetimeEventsHostedService` i zadania w tle z przekroczeniem czasu, `TimedHostedService` do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="68f23-473">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="68f23-474">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="68f23-474">ConfigureLogging</span></span>

<span data-ttu-id="68f23-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podanej <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68f23-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="68f23-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="68f23-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="68f23-477">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="68f23-477">UseConsoleLifetime</span></span>

<span data-ttu-id="68f23-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje w przypadku kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM i wywołań <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> w celu uruchomienia procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="68f23-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="68f23-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="68f23-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="68f23-480">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest wstępnie zarejestrowany jako domyślna implementacja okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="68f23-480">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="68f23-481">Używany jest ostatni zarejestrowany okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="68f23-481">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="68f23-482">Konfiguracja kontenera</span><span class="sxs-lookup"><span data-stu-id="68f23-482">Container configuration</span></span>

<span data-ttu-id="68f23-483">Aby zapewnić obsługę podłączania w innych kontenerach, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="68f23-483">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="68f23-484">Dostarczenie fabryki nie jest częścią rejestracji typu DI Container, ale zamiast tego jest wewnętrznym hostem używanym do tworzenia konkretnych kontenerów DI.</span><span class="sxs-lookup"><span data-stu-id="68f23-484">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="68f23-485">[UseServiceProviderFactory (IServiceProviderFactory @ no__t-1TContainerBuilder @ no__t-2)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę używaną do utworzenia dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-485">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="68f23-486">Niestandardowa konfiguracja kontenera jest zarządzana przez metodę <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-486">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="68f23-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> zapewnia silnie określone środowisko do konfigurowania kontenera na podstawie podstawowego interfejsu API hosta.</span><span class="sxs-lookup"><span data-stu-id="68f23-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="68f23-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="68f23-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="68f23-489">Utwórz kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="68f23-489">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="68f23-490">Podaj fabrykę kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="68f23-490">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="68f23-491">Użyj fabryki i skonfiguruj niestandardowy kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="68f23-491">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="68f23-492">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="68f23-492">Extensibility</span></span>

<span data-ttu-id="68f23-493">Rozszerzalność hosta jest wykonywana przy użyciu metod rozszerzających <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="68f23-493">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="68f23-494">Poniższy przykład pokazuje, jak Metoda rozszerzania rozszerza implementację <xref:Microsoft.Extensions.Hosting.IHostBuilder> z przykładem [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przedstawionym w <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="68f23-494">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="68f23-495">Aplikacja ustanawia metodę rozszerzenia `UseHostedService` w celu zarejestrowania usługi hostowanej przekazaną w `T`:</span><span class="sxs-lookup"><span data-stu-id="68f23-495">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="68f23-496">Zarządzanie hostem</span><span class="sxs-lookup"><span data-stu-id="68f23-496">Manage the host</span></span>

<span data-ttu-id="68f23-497">Implementacja <xref:Microsoft.Extensions.Hosting.IHost> odpowiada za uruchamianie i zatrzymywanie implementacji <xref:Microsoft.Extensions.Hosting.IHostedService>, które są zarejestrowane w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="68f23-497">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="68f23-498">Uruchom</span><span class="sxs-lookup"><span data-stu-id="68f23-498">Run</span></span>

<span data-ttu-id="68f23-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta:</span><span class="sxs-lookup"><span data-stu-id="68f23-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="68f23-500">RunAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-500">RunAsync</span></span>

<span data-ttu-id="68f23-501"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="68f23-501"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="68f23-502">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-502">RunConsoleAsync</span></span>

<span data-ttu-id="68f23-503"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> włącza obsługę konsoli, kompiluje i uruchamia hosta i czeka na zamknięcie klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="68f23-503"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="68f23-504">Uruchom i StopAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-504">Start and StopAsync</span></span>

<span data-ttu-id="68f23-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia Host synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="68f23-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="68f23-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="68f23-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="68f23-507">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-507">StartAsync and StopAsync</span></span>

<span data-ttu-id="68f23-508"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="68f23-508"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="68f23-509"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> zatrzyma aplikację.</span><span class="sxs-lookup"><span data-stu-id="68f23-509"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="68f23-510">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="68f23-510">WaitForShutdown</span></span>

<span data-ttu-id="68f23-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalane za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takiego jak `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (nasłuchuje w przypadku kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="68f23-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="68f23-512">wywołania <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="68f23-513">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="68f23-513">WaitForShutdownAsync</span></span>

<span data-ttu-id="68f23-514"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, które kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="68f23-514"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="68f23-515">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="68f23-515">External control</span></span>

<span data-ttu-id="68f23-516">Kontrolę zewnętrzną hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="68f23-516">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="68f23-517"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="68f23-517"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="68f23-518">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="68f23-518">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="68f23-519">IHostingEnvironment, interfejs</span><span class="sxs-lookup"><span data-stu-id="68f23-519">IHostingEnvironment interface</span></span>

<span data-ttu-id="68f23-520"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o środowisku hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-520"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="68f23-521">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) , aby uzyskać <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, aby użyć jego właściwości i metod rozszerzających:</span><span class="sxs-lookup"><span data-stu-id="68f23-521">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="68f23-522">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="68f23-522">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="68f23-523">IApplicationLifetime, interfejs</span><span class="sxs-lookup"><span data-stu-id="68f23-523">IApplicationLifetime interface</span></span>

<span data-ttu-id="68f23-524"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> umożliwia działanie po uruchomieniu i zamknięciu, w tym bezpieczne żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="68f23-524"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="68f23-525">Trzy właściwości interfejsu są tokenami anulowania używanymi do rejestrowania metod <xref:System.Action>, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="68f23-525">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="68f23-526">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="68f23-526">Cancellation Token</span></span> | <span data-ttu-id="68f23-527">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="68f23-527">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="68f23-528">Host został w pełni uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="68f23-528">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="68f23-529">Host kończy bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="68f23-529">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="68f23-530">Wszystkie żądania powinny być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="68f23-530">All requests should be processed.</span></span> <span data-ttu-id="68f23-531">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="68f23-531">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="68f23-532">Host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="68f23-532">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="68f23-533">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="68f23-533">Requests may still be processing.</span></span> <span data-ttu-id="68f23-534">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="68f23-534">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="68f23-535">Konstruktor — wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> do dowolnej klasy.</span><span class="sxs-lookup"><span data-stu-id="68f23-535">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="68f23-536">[Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do klasy `LifetimeEventsHostedService` (implementacja <xref:Microsoft.Extensions.Hosting.IHostedService>), aby zarejestrować zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="68f23-536">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="68f23-537">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="68f23-537">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="68f23-538"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="68f23-538"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="68f23-539">Następująca Klasa używa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, aby bezpiecznie zamknąć aplikację w przypadku wywołania metody `Shutdown` klasy:</span><span class="sxs-lookup"><span data-stu-id="68f23-539">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="68f23-540">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="68f23-540">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
