---
title: Host ogólny .NET
author: rick-anderson
description: Dowiedz się więcej o hoście ogólnym programu .NET Core, który jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0f8f03dabf65f2cbfe4c41d36b02a25d7902cefb
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219223"
---
# <a name="net-generic-host"></a><span data-ttu-id="e2092-103">Host ogólny .NET</span><span class="sxs-lookup"><span data-stu-id="e2092-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="e2092-104">W tym artykule przedstawiono hosta ogólnego platformy .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) i przedstawiono wskazówki dotyczące korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="e2092-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="e2092-105">Co to jest host?</span><span class="sxs-lookup"><span data-stu-id="e2092-105">What's a host?</span></span>

<span data-ttu-id="e2092-106">*Host* to obiekt, który hermetyzuje zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="e2092-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="e2092-107">Iniekcja zależności (DI)</span><span class="sxs-lookup"><span data-stu-id="e2092-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="e2092-108">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="e2092-108">Logging</span></span>
* <span data-ttu-id="e2092-109">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="e2092-109">Configuration</span></span>
* <span data-ttu-id="e2092-110">implementacje `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="e2092-110">`IHostedService` implementations</span></span>

<span data-ttu-id="e2092-111">Po uruchomieniu hosta wywołuje `IHostedService.StartAsync` przy każdej implementacji <xref:Microsoft.Extensions.Hosting.IHostedService>, która znajduje się w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="e2092-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="e2092-112">W aplikacji sieci Web jedna z implementacji `IHostedService` jest usługą sieci Web, która uruchamia [implementację serwera http](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="e2092-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="e2092-113">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="e2092-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="e2092-114">W wersjach ASP.NET Core wcześniejszych niż 3,0 [host sieci Web](xref:fundamentals/host/web-host) jest używany do obciążeń http.</span><span class="sxs-lookup"><span data-stu-id="e2092-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="e2092-115">Host sieci Web nie jest już zalecany dla aplikacji sieci Web i pozostaje dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="e2092-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="e2092-116">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-116">Set up a host</span></span>

<span data-ttu-id="e2092-117">Host jest zazwyczaj konfigurowany, zbudowany i uruchamiany przez kod w klasie `Program`.</span><span class="sxs-lookup"><span data-stu-id="e2092-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="e2092-118">Metoda `Main`:</span><span class="sxs-lookup"><span data-stu-id="e2092-118">The `Main` method:</span></span>

* <span data-ttu-id="e2092-119">Wywołuje metodę `CreateHostBuilder`, aby utworzyć i skonfigurować obiekt konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e2092-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="e2092-120">Wywołuje metody `Build` i `Run` w obiekcie konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e2092-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="e2092-121">Oto kod *program.cs* dla obciążenia innego niż HTTP z jedną implementacją `IHostedService` dodaną do kontenera di.</span><span class="sxs-lookup"><span data-stu-id="e2092-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="e2092-122">W przypadku obciążeń HTTP Metoda `Main` jest taka sama, ale `CreateHostBuilder` wywołuje `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="e2092-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="e2092-123">Jeśli aplikacja używa Entity Framework Core, nie zmieniaj nazwy ani podpisu metody `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="e2092-124">[Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) oczekują na znalezienie `CreateHostBuilder`j metody, która konfiguruje hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="e2092-125">Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="e2092-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="e2092-126">Ustawienia domyślnego konstruktora</span><span class="sxs-lookup"><span data-stu-id="e2092-126">Default builder settings</span></span>

<span data-ttu-id="e2092-127">Metoda <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="e2092-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="e2092-128">Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na ścieżkę zwracaną przez <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="e2092-129">Ładuje konfigurację hosta z:</span><span class="sxs-lookup"><span data-stu-id="e2092-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="e2092-130">Zmienne środowiskowe poprzedzone prefiksem `DOTNET_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-130">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="e2092-131">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-131">Command-line arguments.</span></span>
* <span data-ttu-id="e2092-132">Ładuje konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="e2092-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="e2092-133">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e2092-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="e2092-134">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e2092-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="e2092-135">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development`owym.</span><span class="sxs-lookup"><span data-stu-id="e2092-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="e2092-136">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e2092-136">Environment variables.</span></span>
  * <span data-ttu-id="e2092-137">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-137">Command-line arguments.</span></span>
* <span data-ttu-id="e2092-138">Dodaje następujących dostawców [rejestrowania](xref:fundamentals/logging/index) :</span><span class="sxs-lookup"><span data-stu-id="e2092-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="e2092-139">Konsola</span><span class="sxs-lookup"><span data-stu-id="e2092-139">Console</span></span>
  * <span data-ttu-id="e2092-140">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="e2092-140">Debug</span></span>
  * <span data-ttu-id="e2092-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="e2092-141">EventSource</span></span>
  * <span data-ttu-id="e2092-142">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="e2092-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="e2092-143">Umożliwia [weryfikację zakresu](xref:fundamentals/dependency-injection#scope-validation) i [Sprawdzanie poprawności zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) , gdy środowisko jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="e2092-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="e2092-144">Metoda `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="e2092-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="e2092-145">Ładuje konfigurację hosta ze zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-145">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="e2092-146">Ustawia serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web i konfiguruje go przy użyciu dostawców konfiguracji hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="e2092-147">Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="e2092-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="e2092-148">Dodaje [oprogramowanie pośredniczące do filtrowania hosta](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="e2092-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="e2092-149">Dodaje [przekazane nagłówki pośredniczące](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) , jeśli `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest równe `true`.</span><span class="sxs-lookup"><span data-stu-id="e2092-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="e2092-150">Włącza integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e2092-150">Enables IIS integration.</span></span> <span data-ttu-id="e2092-151">Aby poznać domyślne opcje usług IIS, zobacz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="e2092-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="e2092-152">[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [Ustawienia dla usługi Web Apps](#settings-for-web-apps) w dalszej części tego artykułu pokazują, jak zastąpić ustawienia domyślnego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e2092-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="e2092-153">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="e2092-153">Framework-provided services</span></span>

<span data-ttu-id="e2092-154">Następujące usługi są rejestrowane automatycznie:</span><span class="sxs-lookup"><span data-stu-id="e2092-154">The following services are registered automatically:</span></span>

* [<span data-ttu-id="e2092-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="e2092-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="e2092-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="e2092-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="e2092-158">Aby uzyskać więcej informacji na temat usług udostępnianych przez platformę, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="e2092-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="e2092-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="e2092-160">Wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) do dowolnej klasy w celu obsługi zadań po uruchomieniu i bezpiecznego zamykania.</span><span class="sxs-lookup"><span data-stu-id="e2092-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="e2092-161">Trzy właściwości interfejsu to tokeny anulowania używane do rejestrowania metod obsługi zdarzeń uruchamiania i zatrzymywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="e2092-162">Interfejs zawiera również metodę `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="e2092-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="e2092-163">Poniższy przykład jest implementacją `IHostedService`, która rejestruje zdarzenia `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="e2092-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="e2092-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-164">IHostLifetime</span></span>

<span data-ttu-id="e2092-165">Implementacja <xref:Microsoft.Extensions.Hosting.IHostLifetime> kontroluje, gdy host zostanie uruchomiony i gdy zostanie zatrzymana.</span><span class="sxs-lookup"><span data-stu-id="e2092-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="e2092-166">Używana jest Ostatnia zarejestrowana implementacja.</span><span class="sxs-lookup"><span data-stu-id="e2092-166">The last implementation registered is used.</span></span>

<span data-ttu-id="e2092-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest domyślną implementacją `IHostLifetime`.</span><span class="sxs-lookup"><span data-stu-id="e2092-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="e2092-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="e2092-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="e2092-169">Nasłuchuje <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*>, aby rozpocząć proces zamykania.</span><span class="sxs-lookup"><span data-stu-id="e2092-169">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="e2092-170">Odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="e2092-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="e2092-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="e2092-171">IHostEnvironment</span></span>

<span data-ttu-id="e2092-172">Wsuń usługę <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do klasy, aby uzyskać informacje o następujących ustawieniach:</span><span class="sxs-lookup"><span data-stu-id="e2092-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="e2092-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="e2092-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="e2092-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="e2092-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="e2092-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="e2092-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="e2092-176">Aplikacje sieci Web implementują interfejs `IWebHostEnvironment`, który dziedziczy `IHostEnvironment` i dodaje [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="e2092-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="e2092-177">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-177">Host configuration</span></span>

<span data-ttu-id="e2092-178">Konfiguracja hosta jest używana we właściwościach implementacji <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="e2092-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="e2092-179">Konfiguracja hosta jest dostępna z [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="e2092-180">Po `ConfigureAppConfiguration``HostBuilderContext.Configuration` jest zastępowana konfiguracją aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="e2092-181">Aby dodać konfigurację hosta, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="e2092-182">`ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="e2092-183">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="e2092-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="e2092-184">Dostawca zmiennych środowiskowych z prefiksami `DOTNET_` i argumenty wiersza polecenia są uwzględniane przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-184">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e2092-185">W przypadku aplikacji sieci Web zostanie dodany dostawca zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="e2092-186">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e2092-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="e2092-187">Na przykład wartość zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` ma wartość Konfiguracja hosta dla klucza `environment`.</span><span class="sxs-lookup"><span data-stu-id="e2092-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="e2092-188">Poniższy przykład umożliwia utworzenie konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="e2092-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="e2092-189">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="e2092-189">App configuration</span></span>

<span data-ttu-id="e2092-190">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="e2092-191">`ConfigureAppConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="e2092-192">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="e2092-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="e2092-193">Konfiguracja utworzona przez `ConfigureAppConfiguration` jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i jako usługa z elementu di.</span><span class="sxs-lookup"><span data-stu-id="e2092-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="e2092-194">Konfiguracja hosta jest również dodawana do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="e2092-195">Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="e2092-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="e2092-196">Ustawienia dla wszystkich typów aplikacji</span><span class="sxs-lookup"><span data-stu-id="e2092-196">Settings for all app types</span></span>

<span data-ttu-id="e2092-197">Ta sekcja zawiera listę ustawień hosta, które dotyczą zarówno obciążeń HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2092-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="e2092-198">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="e2092-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="e2092-199">ApplicationName</span></span>

<span data-ttu-id="e2092-200">Właściwość [IHostEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="e2092-201">**Klucz**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="e2092-201">**Key**: `applicationName`</span></span>  
<span data-ttu-id="e2092-202">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-202">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-203">**Wartość domyślna**: Nazwa zestawu, który zawiera punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="e2092-204">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="e2092-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="e2092-205">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="e2092-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="e2092-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="e2092-206">ContentRootPath</span></span>

<span data-ttu-id="e2092-207">Właściwość [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="e2092-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="e2092-208">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="e2092-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="e2092-209">**Klucz**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="e2092-209">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="e2092-210">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-210">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-211">**Domyślnie**: folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="e2092-212">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="e2092-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="e2092-213">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseContentRoot` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e2092-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="e2092-214">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="e2092-214">For more information, see:</span></span>

* [<span data-ttu-id="e2092-215">Podstawy: zawartość główna</span><span class="sxs-lookup"><span data-stu-id="e2092-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="e2092-216">WebRoot</span><span class="sxs-lookup"><span data-stu-id="e2092-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="e2092-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="e2092-217">EnvironmentName</span></span>

<span data-ttu-id="e2092-218">Dla właściwości [IHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="e2092-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="e2092-219">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`.</span><span class="sxs-lookup"><span data-stu-id="e2092-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="e2092-220">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="e2092-220">Values aren't case-sensitive.</span></span>

<span data-ttu-id="e2092-221">**Klucz**: `environment`</span><span class="sxs-lookup"><span data-stu-id="e2092-221">**Key**: `environment`</span></span>  
<span data-ttu-id="e2092-222">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-222">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-223">**Wartość domyślna**: `Production`</span><span class="sxs-lookup"><span data-stu-id="e2092-223">**Default**: `Production`</span></span>  
<span data-ttu-id="e2092-224">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="e2092-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="e2092-225">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseEnvironment` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e2092-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="e2092-226">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="e2092-226">ShutdownTimeout</span></span>

<span data-ttu-id="e2092-227">[HostOptions. shutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="e2092-228">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="e2092-228">The default value is five seconds.</span></span>  <span data-ttu-id="e2092-229">Podczas okresu przekroczenia limitu czasu Host:</span><span class="sxs-lookup"><span data-stu-id="e2092-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="e2092-230">Wyzwala [IHostApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="e2092-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="e2092-231">Próbuje zatrzymać usługi hostowane, rejestrowanie błędów dla usług, których zatrzymanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="e2092-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="e2092-232">Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="e2092-233">Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="e2092-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="e2092-234">Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="e2092-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="e2092-235">**Klucz**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="e2092-235">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="e2092-236">**Typ**: `int`</span><span class="sxs-lookup"><span data-stu-id="e2092-236">**Type**: `int`</span></span>  
<span data-ttu-id="e2092-237">**Wartość domyślna**: 5 sekund</span><span class="sxs-lookup"><span data-stu-id="e2092-237">**Default**: 5 seconds</span></span>  
<span data-ttu-id="e2092-238">**Zmienna środowiskowa**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="e2092-238">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="e2092-239">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub skonfiguruj `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="e2092-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="e2092-240">Poniższy przykład ustawia limit czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="e2092-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

### <a name="disable-app-configuration-reload-on-change"></a><span data-ttu-id="e2092-241">Wyłącz ponowne ładowanie konfiguracji aplikacji przy zmianie</span><span class="sxs-lookup"><span data-stu-id="e2092-241">Disable app configuration reload on change</span></span>

<span data-ttu-id="e2092-242">[Domyślnie](xref:fundamentals/configuration/index#default)plik *appSettings. JSON* i *appSettings. { Środowisko}. kod JSON* jest ponownie ładowany, gdy plik ulegnie zmianie.</span><span class="sxs-lookup"><span data-stu-id="e2092-242">By [default](xref:fundamentals/configuration/index#default), *appsettings.json* and *appsettings.{Environment}.json* are reloaded when the file changes.</span></span> <span data-ttu-id="e2092-243">Aby wyłączyć to zachowanie ponownego ładowania w ASP.NET Core 5,0 w wersji zapoznawczej 3 lub nowszej, Ustaw klucz `hostBuilder:reloadConfigOnChange` na `false`.</span><span class="sxs-lookup"><span data-stu-id="e2092-243">To disable this reload behavior in ASP.NET Core 5.0 Preview 3 or later, set the `hostBuilder:reloadConfigOnChange` key to `false`.</span></span>

<span data-ttu-id="e2092-244">**Klucz**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="e2092-244">**Key**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="e2092-245">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-245">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-246">**Wartość domyślna**: `true`</span><span class="sxs-lookup"><span data-stu-id="e2092-246">**Default**: `true`</span></span>  
<span data-ttu-id="e2092-247">**Argument wiersza polecenia**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="e2092-247">**Command-line argument**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="e2092-248">**Zmienna środowiskowa**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="e2092-248">**Environment variable**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span></span>

> [!WARNING]
> <span data-ttu-id="e2092-249">Separator dwukropek (`:`) nie działa ze zmiennymi kluczy hierarchicznych na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="e2092-249">The colon (`:`) separator doesn't work with environment variable hierarchical keys on all platforms.</span></span> <span data-ttu-id="e2092-250">Aby uzyskać więcej informacji, zobacz [zmienne środowiskowe](xref:fundamentals/configuration/index#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="e2092-250">For more information, see [Environment variables](xref:fundamentals/configuration/index#environment-variables).</span></span>

## <a name="settings-for-web-apps"></a><span data-ttu-id="e2092-251">Ustawienia usługi Web Apps</span><span class="sxs-lookup"><span data-stu-id="e2092-251">Settings for web apps</span></span>

<span data-ttu-id="e2092-252">Niektóre ustawienia hosta mają zastosowanie tylko do obciążeń protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2092-252">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="e2092-253">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-253">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="e2092-254">Metody rozszerzające w `IWebHostBuilder` są dostępne dla tych ustawień.</span><span class="sxs-lookup"><span data-stu-id="e2092-254">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="e2092-255">Przykłady kodu, które pokazują, jak wywoływać metody rozszerzenia Załóżmy, że `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="e2092-255">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="e2092-256">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="e2092-256">CaptureStartupErrors</span></span>

<span data-ttu-id="e2092-257">Gdy `false`, błędy podczas uruchamiania, kończenie działania hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-257">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="e2092-258">Gdy `true`, Host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="e2092-258">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="e2092-259">**Klucz**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="e2092-259">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="e2092-260">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-260">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-261">**Domyślnie**: wartość domyślna to `false`, chyba że aplikacja zostanie uruchomiona z Kestrel za pomocą usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="e2092-261">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="e2092-262">**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="e2092-262">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="e2092-263">Aby ustawić tę wartość, użyj konfiguracji lub `CaptureStartupErrors`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-263">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="e2092-264">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="e2092-264">DetailedErrors</span></span>

<span data-ttu-id="e2092-265">Po włączeniu lub gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe błędy.</span><span class="sxs-lookup"><span data-stu-id="e2092-265">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="e2092-266">**Klucz**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="e2092-266">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="e2092-267">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-267">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-268">**Wartość domyślna**: `false`</span><span class="sxs-lookup"><span data-stu-id="e2092-268">**Default**: `false`</span></span>  
<span data-ttu-id="e2092-269">**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="e2092-269">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="e2092-270">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-270">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="e2092-271">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="e2092-271">HostingStartupAssemblies</span></span>

<span data-ttu-id="e2092-272">Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e2092-272">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="e2092-273">Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-273">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="e2092-274">W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e2092-274">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="e2092-275">**Klucz**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="e2092-275">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="e2092-276">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-276">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-277">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="e2092-277">**Default**: Empty string</span></span>  
<span data-ttu-id="e2092-278">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e2092-278">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="e2092-279">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="e2092-280">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="e2092-280">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="e2092-281">Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e2092-281">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="e2092-282">**Klucz**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="e2092-282">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="e2092-283">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-283">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-284">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="e2092-284">**Default**: Empty string</span></span>  
<span data-ttu-id="e2092-285">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e2092-285">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="e2092-286">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-286">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="e2092-287">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="e2092-287">HTTPS_Port</span></span>

<span data-ttu-id="e2092-288">Port przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2092-288">The HTTPS redirect port.</span></span> <span data-ttu-id="e2092-289">Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="e2092-289">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="e2092-290">**Klucz**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="e2092-290">**Key**: `https_port`</span></span>  
<span data-ttu-id="e2092-291">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-291">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-292">Wartość **Domyślna**: nie ustawiono wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="e2092-292">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="e2092-293">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="e2092-293">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="e2092-294">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-294">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="e2092-295">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="e2092-295">PreferHostingUrls</span></span>

<span data-ttu-id="e2092-296">Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy użyciu `IWebHostBuilder` zamiast adresów URL skonfigurowanych przy użyciu implementacji `IServer`.</span><span class="sxs-lookup"><span data-stu-id="e2092-296">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="e2092-297">**Klucz**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="e2092-297">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="e2092-298">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-298">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-299">**Wartość domyślna**: `true`</span><span class="sxs-lookup"><span data-stu-id="e2092-299">**Default**: `true`</span></span>  
<span data-ttu-id="e2092-300">**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="e2092-300">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="e2092-301">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `PreferHostingUrls`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-301">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="e2092-302">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="e2092-302">PreventHostingStartup</span></span>

<span data-ttu-id="e2092-303">Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-303">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="e2092-304">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e2092-304">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="e2092-305">**Klucz**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="e2092-305">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="e2092-306">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-306">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-307">**Wartość domyślna**: `false`</span><span class="sxs-lookup"><span data-stu-id="e2092-307">**Default**: `false`</span></span>  
<span data-ttu-id="e2092-308">**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="e2092-308">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="e2092-309">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseSetting` wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-309">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="e2092-310">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="e2092-310">StartupAssembly</span></span>

<span data-ttu-id="e2092-311">Zestaw do wyszukiwania klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e2092-311">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="e2092-312">**Klucz**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="e2092-312">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="e2092-313">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-313">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-314">**Domyślnie**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="e2092-314">**Default**: The app's assembly</span></span>  
<span data-ttu-id="e2092-315">**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="e2092-315">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="e2092-316">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseStartup`wywołań.</span><span class="sxs-lookup"><span data-stu-id="e2092-316">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="e2092-317">`UseStartup` może przyjmować nazwę zestawu (`string`) lub typ (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="e2092-317">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="e2092-318">Jeśli wywoływana jest wiele metod `UseStartup`, pierwszeństwo ma Ostatnia.</span><span class="sxs-lookup"><span data-stu-id="e2092-318">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="e2092-319">Adresy URL</span><span class="sxs-lookup"><span data-stu-id="e2092-319">URLs</span></span>

<span data-ttu-id="e2092-320">Rozdzielana średnikami lista adresów IP lub adresów hostów z portami i protokołami, na których serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="e2092-320">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="e2092-321">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="e2092-321">For example, `http://localhost:123`.</span></span> <span data-ttu-id="e2092-322">Użyj "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="e2092-322">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="e2092-323">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e2092-323">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="e2092-324">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="e2092-324">Supported formats vary among servers.</span></span>

<span data-ttu-id="e2092-325">**Klucz**: `urls`</span><span class="sxs-lookup"><span data-stu-id="e2092-325">**Key**: `urls`</span></span>  
<span data-ttu-id="e2092-326">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-326">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-327">**Wartość domyślna**: `http://localhost:5000` i `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="e2092-327">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="e2092-328">**Zmienna środowiskowa**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="e2092-328">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="e2092-329">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseUrls`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-329">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="e2092-330">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="e2092-330">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="e2092-331">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e2092-331">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="e2092-332">WebRoot</span><span class="sxs-lookup"><span data-stu-id="e2092-332">WebRoot</span></span>

<span data-ttu-id="e2092-333">Ścieżka względna do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-333">The relative path to the app's static assets.</span></span>

<span data-ttu-id="e2092-334">**Klucz**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="e2092-334">**Key**: `webroot`</span></span>  
<span data-ttu-id="e2092-335">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-335">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-336">**Wartość domyślna**: wartość domyślna to `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="e2092-336">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="e2092-337">Ścieżka do *elementu {content root}/wwwroot* musi istnieć.</span><span class="sxs-lookup"><span data-stu-id="e2092-337">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="e2092-338">Jeśli ścieżka nie istnieje, jest używany dostawca plików No-op.</span><span class="sxs-lookup"><span data-stu-id="e2092-338">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="e2092-339">**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="e2092-339">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="e2092-340">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseWebRoot`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-340">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="e2092-341">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="e2092-341">For more information, see:</span></span>

* [<span data-ttu-id="e2092-342">Podstawy: katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="e2092-342">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="e2092-343">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="e2092-343">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="e2092-344">Zarządzanie okresem istnienia hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-344">Manage the host lifetime</span></span>

<span data-ttu-id="e2092-345">Wywołaj metody na skompilowanej implementacji <xref:Microsoft.Extensions.Hosting.IHost>, aby uruchomić i zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="e2092-345">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="e2092-346">Te metody wpływają na wszystkie implementacje <xref:Microsoft.Extensions.Hosting.IHostedService>, które są zarejestrowane w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="e2092-346">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="e2092-347">Run</span><span class="sxs-lookup"><span data-stu-id="e2092-347">Run</span></span>

<span data-ttu-id="e2092-348"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-348"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="e2092-349">RunAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-349">RunAsync</span></span>

<span data-ttu-id="e2092-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="e2092-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="e2092-351">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-351">RunConsoleAsync</span></span>

<span data-ttu-id="e2092-352"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie programu <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="e2092-352"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="e2092-353">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="e2092-353">Start</span></span>

<span data-ttu-id="e2092-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="e2092-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="e2092-355">StartAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-355">StartAsync</span></span>

<span data-ttu-id="e2092-356"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task>, który kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="e2092-356"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="e2092-357"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku `StartAsync`, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="e2092-357"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="e2092-358">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="e2092-358">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="e2092-359">StopAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-359">StopAsync</span></span>

<span data-ttu-id="e2092-360"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="e2092-360"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="e2092-361">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="e2092-361">WaitForShutdown</span></span>

<span data-ttu-id="e2092-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący do momentu wyzwolenia przez IHostLifetime, na przykład za pośrednictwem <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="e2092-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="e2092-363">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-363">WaitForShutdownAsync</span></span>

<span data-ttu-id="e2092-364"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-364"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="e2092-365">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="e2092-365">External control</span></span>

<span data-ttu-id="e2092-366">Bezpośrednią kontrolę nad okresem istnienia hosta można uzyskać przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="e2092-366">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

::: moniker range=">= aspnetcore-3.0 <= aspnetcore-3.1"

<span data-ttu-id="e2092-367">W tym artykule przedstawiono hosta ogólnego platformy .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) i przedstawiono wskazówki dotyczące korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="e2092-367">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="e2092-368">Co to jest host?</span><span class="sxs-lookup"><span data-stu-id="e2092-368">What's a host?</span></span>

<span data-ttu-id="e2092-369">*Host* to obiekt, który hermetyzuje zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="e2092-369">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="e2092-370">Iniekcja zależności (DI)</span><span class="sxs-lookup"><span data-stu-id="e2092-370">Dependency injection (DI)</span></span>
* <span data-ttu-id="e2092-371">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="e2092-371">Logging</span></span>
* <span data-ttu-id="e2092-372">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="e2092-372">Configuration</span></span>
* <span data-ttu-id="e2092-373">implementacje `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="e2092-373">`IHostedService` implementations</span></span>

<span data-ttu-id="e2092-374">Po uruchomieniu hosta wywołuje `IHostedService.StartAsync` przy każdej implementacji <xref:Microsoft.Extensions.Hosting.IHostedService>, która znajduje się w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="e2092-374">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="e2092-375">W aplikacji sieci Web jedna z implementacji `IHostedService` jest usługą sieci Web, która uruchamia [implementację serwera http](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="e2092-375">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="e2092-376">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="e2092-376">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="e2092-377">W wersjach ASP.NET Core wcześniejszych niż 3,0 [host sieci Web](xref:fundamentals/host/web-host) jest używany do obciążeń http.</span><span class="sxs-lookup"><span data-stu-id="e2092-377">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="e2092-378">Host sieci Web nie jest już zalecany dla aplikacji sieci Web i pozostaje dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="e2092-378">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="e2092-379">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-379">Set up a host</span></span>

<span data-ttu-id="e2092-380">Host jest zazwyczaj konfigurowany, zbudowany i uruchamiany przez kod w klasie `Program`.</span><span class="sxs-lookup"><span data-stu-id="e2092-380">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="e2092-381">Metoda `Main`:</span><span class="sxs-lookup"><span data-stu-id="e2092-381">The `Main` method:</span></span>

* <span data-ttu-id="e2092-382">Wywołuje metodę `CreateHostBuilder`, aby utworzyć i skonfigurować obiekt konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e2092-382">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="e2092-383">Wywołuje metody `Build` i `Run` w obiekcie konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e2092-383">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="e2092-384">Oto kod *program.cs* dla obciążenia innego niż HTTP z jedną implementacją `IHostedService` dodaną do kontenera di.</span><span class="sxs-lookup"><span data-stu-id="e2092-384">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="e2092-385">W przypadku obciążeń HTTP Metoda `Main` jest taka sama, ale `CreateHostBuilder` wywołuje `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="e2092-385">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="e2092-386">Jeśli aplikacja używa Entity Framework Core, nie zmieniaj nazwy ani podpisu metody `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-386">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="e2092-387">[Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) oczekują na znalezienie `CreateHostBuilder`j metody, która konfiguruje hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-387">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="e2092-388">Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="e2092-388">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="e2092-389">Ustawienia domyślnego konstruktora</span><span class="sxs-lookup"><span data-stu-id="e2092-389">Default builder settings</span></span>

<span data-ttu-id="e2092-390">Metoda <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="e2092-390">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="e2092-391">Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na ścieżkę zwracaną przez <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-391">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="e2092-392">Ładuje konfigurację hosta z:</span><span class="sxs-lookup"><span data-stu-id="e2092-392">Loads host configuration from:</span></span>
  * <span data-ttu-id="e2092-393">Zmienne środowiskowe poprzedzone prefiksem `DOTNET_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-393">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="e2092-394">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-394">Command-line arguments.</span></span>
* <span data-ttu-id="e2092-395">Ładuje konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="e2092-395">Loads app configuration from:</span></span>
  * <span data-ttu-id="e2092-396">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e2092-396">*appsettings.json*.</span></span>
  * <span data-ttu-id="e2092-397">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e2092-397">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="e2092-398">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development`owym.</span><span class="sxs-lookup"><span data-stu-id="e2092-398">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="e2092-399">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e2092-399">Environment variables.</span></span>
  * <span data-ttu-id="e2092-400">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-400">Command-line arguments.</span></span>
* <span data-ttu-id="e2092-401">Dodaje następujących dostawców [rejestrowania](xref:fundamentals/logging/index) :</span><span class="sxs-lookup"><span data-stu-id="e2092-401">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="e2092-402">Konsola</span><span class="sxs-lookup"><span data-stu-id="e2092-402">Console</span></span>
  * <span data-ttu-id="e2092-403">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="e2092-403">Debug</span></span>
  * <span data-ttu-id="e2092-404">EventSource</span><span class="sxs-lookup"><span data-stu-id="e2092-404">EventSource</span></span>
  * <span data-ttu-id="e2092-405">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="e2092-405">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="e2092-406">Umożliwia [weryfikację zakresu](xref:fundamentals/dependency-injection#scope-validation) i [Sprawdzanie poprawności zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) , gdy środowisko jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="e2092-406">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="e2092-407">Metoda `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="e2092-407">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="e2092-408">Ładuje konfigurację hosta ze zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-408">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="e2092-409">Ustawia serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web i konfiguruje go przy użyciu dostawców konfiguracji hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-409">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="e2092-410">Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="e2092-410">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="e2092-411">Dodaje [oprogramowanie pośredniczące do filtrowania hosta](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="e2092-411">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="e2092-412">Dodaje [przekazane nagłówki pośredniczące](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) , jeśli `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest równe `true`.</span><span class="sxs-lookup"><span data-stu-id="e2092-412">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="e2092-413">Włącza integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e2092-413">Enables IIS integration.</span></span> <span data-ttu-id="e2092-414">Aby poznać domyślne opcje usług IIS, zobacz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="e2092-414">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="e2092-415">[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [Ustawienia dla usługi Web Apps](#settings-for-web-apps) w dalszej części tego artykułu pokazują, jak zastąpić ustawienia domyślnego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e2092-415">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="e2092-416">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="e2092-416">Framework-provided services</span></span>

<span data-ttu-id="e2092-417">Następujące usługi są rejestrowane automatycznie:</span><span class="sxs-lookup"><span data-stu-id="e2092-417">The following services are registered automatically:</span></span>

* [<span data-ttu-id="e2092-418">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-418">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="e2092-419">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-419">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="e2092-420">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="e2092-420">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="e2092-421">Aby uzyskać więcej informacji na temat usług udostępnianych przez platformę, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="e2092-421">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="e2092-422">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-422">IHostApplicationLifetime</span></span>

<span data-ttu-id="e2092-423">Wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) do dowolnej klasy w celu obsługi zadań po uruchomieniu i bezpiecznego zamykania.</span><span class="sxs-lookup"><span data-stu-id="e2092-423">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="e2092-424">Trzy właściwości interfejsu to tokeny anulowania używane do rejestrowania metod obsługi zdarzeń uruchamiania i zatrzymywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-424">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="e2092-425">Interfejs zawiera również metodę `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="e2092-425">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="e2092-426">Poniższy przykład jest implementacją `IHostedService`, która rejestruje zdarzenia `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="e2092-426">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="e2092-427">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-427">IHostLifetime</span></span>

<span data-ttu-id="e2092-428">Implementacja <xref:Microsoft.Extensions.Hosting.IHostLifetime> kontroluje, gdy host zostanie uruchomiony i gdy zostanie zatrzymana.</span><span class="sxs-lookup"><span data-stu-id="e2092-428">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="e2092-429">Używana jest Ostatnia zarejestrowana implementacja.</span><span class="sxs-lookup"><span data-stu-id="e2092-429">The last implementation registered is used.</span></span>

<span data-ttu-id="e2092-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest domyślną implementacją `IHostLifetime`.</span><span class="sxs-lookup"><span data-stu-id="e2092-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="e2092-431">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="e2092-431">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="e2092-432">Nasłuchuje <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*>, aby rozpocząć proces zamykania.</span><span class="sxs-lookup"><span data-stu-id="e2092-432">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="e2092-433">Odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="e2092-433">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="e2092-434">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="e2092-434">IHostEnvironment</span></span>

<span data-ttu-id="e2092-435">Wsuń usługę <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do klasy, aby uzyskać informacje o następujących ustawieniach:</span><span class="sxs-lookup"><span data-stu-id="e2092-435">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="e2092-436">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="e2092-436">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="e2092-437">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="e2092-437">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="e2092-438">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="e2092-438">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="e2092-439">Aplikacje sieci Web implementują interfejs `IWebHostEnvironment`, który dziedziczy `IHostEnvironment` i dodaje [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="e2092-439">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="e2092-440">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-440">Host configuration</span></span>

<span data-ttu-id="e2092-441">Konfiguracja hosta jest używana we właściwościach implementacji <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="e2092-441">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="e2092-442">Konfiguracja hosta jest dostępna z [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-442">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="e2092-443">Po `ConfigureAppConfiguration``HostBuilderContext.Configuration` jest zastępowana konfiguracją aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-443">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="e2092-444">Aby dodać konfigurację hosta, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-444">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="e2092-445">`ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-445">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="e2092-446">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="e2092-446">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="e2092-447">Dostawca zmiennych środowiskowych z prefiksami `DOTNET_` i argumenty wiersza polecenia są uwzględniane przez `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-447">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="e2092-448">W przypadku aplikacji sieci Web zostanie dodany dostawca zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-448">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="e2092-449">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e2092-449">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="e2092-450">Na przykład wartość zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` ma wartość Konfiguracja hosta dla klucza `environment`.</span><span class="sxs-lookup"><span data-stu-id="e2092-450">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="e2092-451">Poniższy przykład umożliwia utworzenie konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="e2092-451">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="e2092-452">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="e2092-452">App configuration</span></span>

<span data-ttu-id="e2092-453">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e2092-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="e2092-454">`ConfigureAppConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-454">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="e2092-455">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="e2092-455">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="e2092-456">Konfiguracja utworzona przez `ConfigureAppConfiguration` jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i jako usługa z elementu di.</span><span class="sxs-lookup"><span data-stu-id="e2092-456">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="e2092-457">Konfiguracja hosta jest również dodawana do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-457">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="e2092-458">Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="e2092-458">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="e2092-459">Ustawienia dla wszystkich typów aplikacji</span><span class="sxs-lookup"><span data-stu-id="e2092-459">Settings for all app types</span></span>

<span data-ttu-id="e2092-460">Ta sekcja zawiera listę ustawień hosta, które dotyczą zarówno obciążeń HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2092-460">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="e2092-461">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-461">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="e2092-462">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="e2092-462">ApplicationName</span></span>

<span data-ttu-id="e2092-463">Właściwość [IHostEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-463">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="e2092-464">**Klucz**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="e2092-464">**Key**: `applicationName`</span></span>  
<span data-ttu-id="e2092-465">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-465">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-466">**Wartość domyślna**: Nazwa zestawu, który zawiera punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-466">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="e2092-467">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="e2092-467">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="e2092-468">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="e2092-468">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="e2092-469">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="e2092-469">ContentRootPath</span></span>

<span data-ttu-id="e2092-470">Właściwość [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="e2092-470">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="e2092-471">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="e2092-471">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="e2092-472">**Klucz**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="e2092-472">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="e2092-473">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-473">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-474">**Domyślnie**: folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-474">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="e2092-475">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="e2092-475">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="e2092-476">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseContentRoot` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e2092-476">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="e2092-477">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="e2092-477">For more information, see:</span></span>

* [<span data-ttu-id="e2092-478">Podstawy: zawartość główna</span><span class="sxs-lookup"><span data-stu-id="e2092-478">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="e2092-479">WebRoot</span><span class="sxs-lookup"><span data-stu-id="e2092-479">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="e2092-480">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="e2092-480">EnvironmentName</span></span>

<span data-ttu-id="e2092-481">Dla właściwości [IHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="e2092-481">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="e2092-482">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`.</span><span class="sxs-lookup"><span data-stu-id="e2092-482">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="e2092-483">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="e2092-483">Values aren't case-sensitive.</span></span>

<span data-ttu-id="e2092-484">**Klucz**: `environment`</span><span class="sxs-lookup"><span data-stu-id="e2092-484">**Key**: `environment`</span></span>  
<span data-ttu-id="e2092-485">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-485">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-486">**Wartość domyślna**: `Production`</span><span class="sxs-lookup"><span data-stu-id="e2092-486">**Default**: `Production`</span></span>  
<span data-ttu-id="e2092-487">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="e2092-487">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="e2092-488">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseEnvironment` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e2092-488">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="e2092-489">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="e2092-489">ShutdownTimeout</span></span>

<span data-ttu-id="e2092-490">[HostOptions. shutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-490">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="e2092-491">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="e2092-491">The default value is five seconds.</span></span>  <span data-ttu-id="e2092-492">Podczas okresu przekroczenia limitu czasu Host:</span><span class="sxs-lookup"><span data-stu-id="e2092-492">During the timeout period, the host:</span></span>

* <span data-ttu-id="e2092-493">Wyzwala [IHostApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="e2092-493">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="e2092-494">Próbuje zatrzymać usługi hostowane, rejestrowanie błędów dla usług, których zatrzymanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="e2092-494">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="e2092-495">Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-495">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="e2092-496">Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="e2092-496">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="e2092-497">Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="e2092-497">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="e2092-498">**Klucz**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="e2092-498">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="e2092-499">**Typ**: `int`</span><span class="sxs-lookup"><span data-stu-id="e2092-499">**Type**: `int`</span></span>  
<span data-ttu-id="e2092-500">**Wartość domyślna**: 5 sekund</span><span class="sxs-lookup"><span data-stu-id="e2092-500">**Default**: 5 seconds</span></span>  
<span data-ttu-id="e2092-501">**Zmienna środowiskowa**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="e2092-501">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="e2092-502">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub skonfiguruj `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="e2092-502">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="e2092-503">Poniższy przykład ustawia limit czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="e2092-503">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="e2092-504">Ustawienia usługi Web Apps</span><span class="sxs-lookup"><span data-stu-id="e2092-504">Settings for web apps</span></span>

<span data-ttu-id="e2092-505">Niektóre ustawienia hosta mają zastosowanie tylko do obciążeń protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2092-505">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="e2092-506">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-506">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="e2092-507">Metody rozszerzające w `IWebHostBuilder` są dostępne dla tych ustawień.</span><span class="sxs-lookup"><span data-stu-id="e2092-507">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="e2092-508">Przykłady kodu, które pokazują, jak wywoływać metody rozszerzenia Załóżmy, że `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="e2092-508">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="e2092-509">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="e2092-509">CaptureStartupErrors</span></span>

<span data-ttu-id="e2092-510">Gdy `false`, błędy podczas uruchamiania, kończenie działania hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-510">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="e2092-511">Gdy `true`, Host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="e2092-511">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="e2092-512">**Klucz**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="e2092-512">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="e2092-513">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-513">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-514">**Domyślnie**: wartość domyślna to `false`, chyba że aplikacja zostanie uruchomiona z Kestrel za pomocą usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="e2092-514">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="e2092-515">**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="e2092-515">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="e2092-516">Aby ustawić tę wartość, użyj konfiguracji lub `CaptureStartupErrors`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-516">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="e2092-517">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="e2092-517">DetailedErrors</span></span>

<span data-ttu-id="e2092-518">Po włączeniu lub gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe błędy.</span><span class="sxs-lookup"><span data-stu-id="e2092-518">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="e2092-519">**Klucz**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="e2092-519">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="e2092-520">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-520">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-521">**Wartość domyślna**: `false`</span><span class="sxs-lookup"><span data-stu-id="e2092-521">**Default**: `false`</span></span>  
<span data-ttu-id="e2092-522">**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="e2092-522">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="e2092-523">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-523">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="e2092-524">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="e2092-524">HostingStartupAssemblies</span></span>

<span data-ttu-id="e2092-525">Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e2092-525">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="e2092-526">Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-526">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="e2092-527">W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e2092-527">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="e2092-528">**Klucz**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="e2092-528">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="e2092-529">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-529">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-530">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="e2092-530">**Default**: Empty string</span></span>  
<span data-ttu-id="e2092-531">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e2092-531">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="e2092-532">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-532">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="e2092-533">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="e2092-533">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="e2092-534">Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e2092-534">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="e2092-535">**Klucz**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="e2092-535">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="e2092-536">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-536">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-537">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="e2092-537">**Default**: Empty string</span></span>  
<span data-ttu-id="e2092-538">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="e2092-538">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="e2092-539">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-539">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="e2092-540">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="e2092-540">HTTPS_Port</span></span>

<span data-ttu-id="e2092-541">Port przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2092-541">The HTTPS redirect port.</span></span> <span data-ttu-id="e2092-542">Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="e2092-542">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="e2092-543">**Klucz**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="e2092-543">**Key**: `https_port`</span></span>  
<span data-ttu-id="e2092-544">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-544">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-545">Wartość **Domyślna**: nie ustawiono wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="e2092-545">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="e2092-546">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="e2092-546">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="e2092-547">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-547">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="e2092-548">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="e2092-548">PreferHostingUrls</span></span>

<span data-ttu-id="e2092-549">Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy użyciu `IWebHostBuilder` zamiast adresów URL skonfigurowanych przy użyciu implementacji `IServer`.</span><span class="sxs-lookup"><span data-stu-id="e2092-549">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="e2092-550">**Klucz**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="e2092-550">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="e2092-551">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-551">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-552">**Wartość domyślna**: `true`</span><span class="sxs-lookup"><span data-stu-id="e2092-552">**Default**: `true`</span></span>  
<span data-ttu-id="e2092-553">**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="e2092-553">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="e2092-554">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `PreferHostingUrls`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-554">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="e2092-555">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="e2092-555">PreventHostingStartup</span></span>

<span data-ttu-id="e2092-556">Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-556">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="e2092-557">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e2092-557">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="e2092-558">**Klucz**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="e2092-558">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="e2092-559">**Typ**: `bool` (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="e2092-559">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="e2092-560">**Wartość domyślna**: `false`</span><span class="sxs-lookup"><span data-stu-id="e2092-560">**Default**: `false`</span></span>  
<span data-ttu-id="e2092-561">**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="e2092-561">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="e2092-562">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseSetting` wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-562">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="e2092-563">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="e2092-563">StartupAssembly</span></span>

<span data-ttu-id="e2092-564">Zestaw do wyszukiwania klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e2092-564">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="e2092-565">**Klucz**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="e2092-565">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="e2092-566">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-566">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-567">**Domyślnie**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="e2092-567">**Default**: The app's assembly</span></span>  
<span data-ttu-id="e2092-568">**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="e2092-568">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="e2092-569">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseStartup`wywołań.</span><span class="sxs-lookup"><span data-stu-id="e2092-569">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="e2092-570">`UseStartup` może przyjmować nazwę zestawu (`string`) lub typ (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="e2092-570">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="e2092-571">Jeśli wywoływana jest wiele metod `UseStartup`, pierwszeństwo ma Ostatnia.</span><span class="sxs-lookup"><span data-stu-id="e2092-571">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="e2092-572">Adresy URL</span><span class="sxs-lookup"><span data-stu-id="e2092-572">URLs</span></span>

<span data-ttu-id="e2092-573">Rozdzielana średnikami lista adresów IP lub adresów hostów z portami i protokołami, na których serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="e2092-573">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="e2092-574">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="e2092-574">For example, `http://localhost:123`.</span></span> <span data-ttu-id="e2092-575">Użyj "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="e2092-575">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="e2092-576">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e2092-576">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="e2092-577">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="e2092-577">Supported formats vary among servers.</span></span>

<span data-ttu-id="e2092-578">**Klucz**: `urls`</span><span class="sxs-lookup"><span data-stu-id="e2092-578">**Key**: `urls`</span></span>  
<span data-ttu-id="e2092-579">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-579">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-580">**Wartość domyślna**: `http://localhost:5000` i `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="e2092-580">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="e2092-581">**Zmienna środowiskowa**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="e2092-581">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="e2092-582">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseUrls`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-582">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="e2092-583">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="e2092-583">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="e2092-584">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="e2092-584">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="e2092-585">WebRoot</span><span class="sxs-lookup"><span data-stu-id="e2092-585">WebRoot</span></span>

<span data-ttu-id="e2092-586">Ścieżka względna do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-586">The relative path to the app's static assets.</span></span>

<span data-ttu-id="e2092-587">**Klucz**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="e2092-587">**Key**: `webroot`</span></span>  
<span data-ttu-id="e2092-588">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-588">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-589">**Wartość domyślna**: wartość domyślna to `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="e2092-589">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="e2092-590">Ścieżka do *elementu {content root}/wwwroot* musi istnieć.</span><span class="sxs-lookup"><span data-stu-id="e2092-590">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="e2092-591">Jeśli ścieżka nie istnieje, jest używany dostawca plików No-op.</span><span class="sxs-lookup"><span data-stu-id="e2092-591">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="e2092-592">**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="e2092-592">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="e2092-593">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseWebRoot`wywołań:</span><span class="sxs-lookup"><span data-stu-id="e2092-593">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="e2092-594">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="e2092-594">For more information, see:</span></span>

* [<span data-ttu-id="e2092-595">Podstawy: katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="e2092-595">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="e2092-596">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="e2092-596">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="e2092-597">Zarządzanie okresem istnienia hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-597">Manage the host lifetime</span></span>

<span data-ttu-id="e2092-598">Wywołaj metody na skompilowanej implementacji <xref:Microsoft.Extensions.Hosting.IHost>, aby uruchomić i zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="e2092-598">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="e2092-599">Te metody wpływają na wszystkie implementacje <xref:Microsoft.Extensions.Hosting.IHostedService>, które są zarejestrowane w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="e2092-599">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="e2092-600">Run</span><span class="sxs-lookup"><span data-stu-id="e2092-600">Run</span></span>

<span data-ttu-id="e2092-601"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-601"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="e2092-602">RunAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-602">RunAsync</span></span>

<span data-ttu-id="e2092-603"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="e2092-603"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="e2092-604">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-604">RunConsoleAsync</span></span>

<span data-ttu-id="e2092-605"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie programu <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="e2092-605"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="e2092-606">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="e2092-606">Start</span></span>

<span data-ttu-id="e2092-607"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="e2092-607"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="e2092-608">StartAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-608">StartAsync</span></span>

<span data-ttu-id="e2092-609"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task>, który kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="e2092-609"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="e2092-610"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku `StartAsync`, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="e2092-610"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="e2092-611">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="e2092-611">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="e2092-612">StopAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-612">StopAsync</span></span>

<span data-ttu-id="e2092-613"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="e2092-613"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="e2092-614">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="e2092-614">WaitForShutdown</span></span>

<span data-ttu-id="e2092-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący do momentu wyzwolenia przez IHostLifetime, na przykład za pośrednictwem <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="e2092-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="e2092-616">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-616">WaitForShutdownAsync</span></span>

<span data-ttu-id="e2092-617"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-617"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="e2092-618">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="e2092-618">External control</span></span>

<span data-ttu-id="e2092-619">Bezpośrednią kontrolę nad okresem istnienia hosta można uzyskać przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="e2092-619">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="e2092-620">ASP.NET Core aplikacje konfigurują i uruchamiają hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-620">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="e2092-621">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-621">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="e2092-622">W tym artykule opisano ASP.NET Core hosta ogólnego (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2092-622">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="e2092-623">Przeznaczeniem hosta ogólnego jest oddzielenie potoku HTTP od interfejsu API hosta sieci Web w celu umożliwienia szerszej tablicy scenariuszy hostów.</span><span class="sxs-lookup"><span data-stu-id="e2092-623">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="e2092-624">Obsługa komunikatów, zadań w tle i innych obciążeń innych niż HTTP na podstawie ogólnej korzyści z hosta, takich jak konfiguracja, iniekcja zależności (DI) i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="e2092-624">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="e2092-625">Host ogólny jest nowy w ASP.NET Core 2,1 i nie jest odpowiedni dla scenariuszy hostingu w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e2092-625">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="e2092-626">W przypadku scenariuszy hostingu w sieci Web należy użyć [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="e2092-626">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="e2092-627">Host ogólny zastąpi hosta sieci Web w przyszłej wersji i będzie pełnić rolę podstawowego interfejsu API hosta zarówno w scenariuszach HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2092-627">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="e2092-628">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e2092-628">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e2092-629">Podczas uruchamiania przykładowej aplikacji w [Visual Studio Code](https://code.visualstudio.com/)należy użyć *zewnętrznego lub zintegrowanego terminalu*.</span><span class="sxs-lookup"><span data-stu-id="e2092-629">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="e2092-630">Nie uruchamiaj przykładu w `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="e2092-630">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="e2092-631">Aby ustawić konsolę w Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="e2092-631">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="e2092-632">Otwórz plik *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e2092-632">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="e2092-633">W konfiguracji **uruchamiania programu .NET Core (konsoli)** zlokalizuj wpis **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="e2092-633">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="e2092-634">Ustaw wartość na `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="e2092-634">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="e2092-635">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="e2092-635">Introduction</span></span>

<span data-ttu-id="e2092-636">Ogólna Biblioteka hostów jest dostępna w przestrzeni nazw <xref:Microsoft.Extensions.Hosting> i udostępniana przez pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) .</span><span class="sxs-lookup"><span data-stu-id="e2092-636">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="e2092-637">Pakiet `Microsoft.Extensions.Hosting` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszym).</span><span class="sxs-lookup"><span data-stu-id="e2092-637">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="e2092-638"><xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="e2092-638"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="e2092-639">Każda implementacja <xref:Microsoft.Extensions.Hosting.IHostedService> jest wykonywana w kolejności [rejestracji usługi w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="e2092-639">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="e2092-640"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana dla każdego <xref:Microsoft.Extensions.Hosting.IHostedService> podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w odwrotnej kolejności rejestracji, gdy host zostanie bezpiecznie zamknięty.</span><span class="sxs-lookup"><span data-stu-id="e2092-640"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="e2092-641">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-641">Set up a host</span></span>

<span data-ttu-id="e2092-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder> jest głównym składnikiem używanym przez biblioteki i aplikacje do inicjowania, kompilowania i uruchamiania hosta:</span><span class="sxs-lookup"><span data-stu-id="e2092-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="e2092-643">Opcje</span><span class="sxs-lookup"><span data-stu-id="e2092-643">Options</span></span>

<span data-ttu-id="e2092-644"><xref:Microsoft.Extensions.Hosting.HostOptions> skonfigurować opcje <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="e2092-644"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="e2092-645">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="e2092-645">Shutdown timeout</span></span>

<span data-ttu-id="e2092-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="e2092-647">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="e2092-647">The default value is five seconds.</span></span>

<span data-ttu-id="e2092-648">Następująca konfiguracja opcji w `Program.Main` powoduje zwiększenie domyślnego limitu czasu zamykania pięciu sekund:</span><span class="sxs-lookup"><span data-stu-id="e2092-648">The following option configuration in `Program.Main` increases the default five-second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="e2092-649">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="e2092-649">Default services</span></span>

<span data-ttu-id="e2092-650">Podczas inicjowania hosta zarejestrowano następujące usługi:</span><span class="sxs-lookup"><span data-stu-id="e2092-650">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="e2092-651">[Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="e2092-651">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="e2092-652">[Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="e2092-652">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="e2092-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="e2092-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="e2092-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="e2092-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="e2092-655">[Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="e2092-655">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="e2092-656">[Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="e2092-656">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="e2092-657">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="e2092-657">Host configuration</span></span>

<span data-ttu-id="e2092-658">Konfiguracja hosta jest tworzona przez:</span><span class="sxs-lookup"><span data-stu-id="e2092-658">Host configuration is created by:</span></span>

* <span data-ttu-id="e2092-659">Wywoływanie metod rozszerzających <xref:Microsoft.Extensions.Hosting.IHostBuilder> w celu ustawienia [katalogu głównego zawartości](#content-root) i [środowiska](#environment).</span><span class="sxs-lookup"><span data-stu-id="e2092-659">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="e2092-660">Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-660">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="e2092-661">Metody rozszerzające</span><span class="sxs-lookup"><span data-stu-id="e2092-661">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="e2092-662">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="e2092-662">Application key (name)</span></span>

<span data-ttu-id="e2092-663">Właściwość [IHostingEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-663">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="e2092-664">Aby jawnie ustawić wartość, użyj [HostDefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="e2092-664">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="e2092-665">**Klucz**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="e2092-665">**Key**: `applicationName`</span></span>  
<span data-ttu-id="e2092-666">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-666">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-667">**Wartość domyślna**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-667">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="e2092-668">**Ustaw przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="e2092-668">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="e2092-669">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="e2092-669">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="e2092-670">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="e2092-670">Content root</span></span>

<span data-ttu-id="e2092-671">To ustawienie określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="e2092-671">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="e2092-672">**Klucz**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="e2092-672">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="e2092-673">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-673">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-674">**Domyślnie**: Domyślnie folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-674">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="e2092-675">**Ustaw przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="e2092-675">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="e2092-676">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="e2092-676">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="e2092-677">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="e2092-677">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="e2092-678">Aby uzyskać więcej informacji, zobacz temat [podstawy: zawartość główna](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="e2092-678">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="e2092-679">Środowisko</span><span class="sxs-lookup"><span data-stu-id="e2092-679">Environment</span></span>

<span data-ttu-id="e2092-680">Ustawia [środowisko](xref:fundamentals/environments)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-680">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="e2092-681">**Klucz**: `environment`</span><span class="sxs-lookup"><span data-stu-id="e2092-681">**Key**: `environment`</span></span>  
<span data-ttu-id="e2092-682">**Typ**: `string`</span><span class="sxs-lookup"><span data-stu-id="e2092-682">**Type**: `string`</span></span>  
<span data-ttu-id="e2092-683">**Wartość domyślna**: `Production`</span><span class="sxs-lookup"><span data-stu-id="e2092-683">**Default**: `Production`</span></span>  
<span data-ttu-id="e2092-684">**Ustaw przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="e2092-684">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="e2092-685">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="e2092-685">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="e2092-686">Dla środowiska można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="e2092-686">The environment can be set to any value.</span></span> <span data-ttu-id="e2092-687">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`.</span><span class="sxs-lookup"><span data-stu-id="e2092-687">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="e2092-688">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="e2092-688">Values aren't case-sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="e2092-689">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="e2092-689">ConfigureHostConfiguration</span></span>

<span data-ttu-id="e2092-690"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do tworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-690"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="e2092-691">Konfiguracja hosta służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-691">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="e2092-692"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-692"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="e2092-693">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="e2092-693">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="e2092-694">Żaden dostawca nie jest domyślnie uwzględniany.</span><span class="sxs-lookup"><span data-stu-id="e2092-694">No providers are included by default.</span></span> <span data-ttu-id="e2092-695">Należy jawnie określić dostawców konfiguracji, których wymaga aplikacja w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, w tym:</span><span class="sxs-lookup"><span data-stu-id="e2092-695">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="e2092-696">Konfiguracja pliku (na przykład z pliku *HostSettings. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="e2092-696">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="e2092-697">Konfiguracja zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="e2092-697">Environment variable configuration.</span></span>
* <span data-ttu-id="e2092-698">Konfiguracja argumentu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-698">Command-line argument configuration.</span></span>
* <span data-ttu-id="e2092-699">Każdy inny wymagany dostawca konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e2092-699">Any other required configuration providers.</span></span>

<span data-ttu-id="e2092-700">Konfiguracja pliku hosta jest włączana przez określenie ścieżki podstawowej aplikacji z `SetBasePath`, a następnie wywołanie jednego z [dostawców konfiguracji plików](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="e2092-700">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="e2092-701">Przykładowa aplikacja używa pliku JSON, *HostSettings. JSON*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*>, aby użyć ustawień konfiguracji hosta pliku.</span><span class="sxs-lookup"><span data-stu-id="e2092-701">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="e2092-702">Aby dodać [konfigurację zmiennej środowiskowej](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, wywołaj <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> na konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-702">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="e2092-703">`AddEnvironmentVariables` akceptuje opcjonalnego prefiksu zdefiniowanego przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e2092-703">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="e2092-704">Przykładowa aplikacja używa prefiksu `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="e2092-704">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="e2092-705">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="e2092-705">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="e2092-706">Po skonfigurowaniu hosta przykładowej aplikacji wartość zmiennej środowiskowej dla `PREFIX_ENVIRONMENT` będzie wartością konfiguracji hosta dla klucza `environment`.</span><span class="sxs-lookup"><span data-stu-id="e2092-706">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="e2092-707">Podczas programowania podczas korzystania z [programu Visual Studio](https://visualstudio.microsoft.com) lub uruchamiania aplikacji z `dotnet run`, zmienne środowiskowe można ustawić w pliku *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e2092-707">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="e2092-708">W [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe mogą być ustawiane w pliku *. programu vscode/Launch. JSON* podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-708">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="e2092-709">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e2092-709">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="e2092-710">[Konfiguracja wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawana przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-710">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="e2092-711">Konfiguracja wiersza polecenia jest dodawana jako Ostatnia, aby zezwolić na argumenty wiersza polecenia w celu przesłonięcia konfiguracji udostępnionej przez wcześniejszych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e2092-711">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="e2092-712">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e2092-712">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="e2092-713">Dodatkową konfigurację można uzyskać za pomocą programu [ApplicationName](#application-key-name) i kluczy [contentRoot](#content-root) .</span><span class="sxs-lookup"><span data-stu-id="e2092-713">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="e2092-714">Przykład konfiguracji `HostBuilder` przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="e2092-714">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="e2092-715">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="e2092-715">ConfigureAppConfiguration</span></span>

<span data-ttu-id="e2092-716">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w implementacji <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2092-716">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="e2092-717"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do tworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-717"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="e2092-718"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-718"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="e2092-719">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="e2092-719">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="e2092-720">Konfiguracja utworzona przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-720">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="e2092-721">Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta dostarczoną przez [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="e2092-721">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="e2092-722">Przykładowa konfiguracja aplikacji przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="e2092-722">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="e2092-723">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e2092-723">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="e2092-724">*appSettings. Plik Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e2092-724">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="e2092-725">*appSettings. Production. JSON*:</span><span class="sxs-lookup"><span data-stu-id="e2092-725">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="e2092-726">Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="e2092-726">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="e2092-727">Aplikacja Przykładowa przenosi pliki ustawień aplikacji JSON i *HostSettings. JSON* z następującym `<Content>` elementem:</span><span class="sxs-lookup"><span data-stu-id="e2092-727">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="e2092-728">Metody rozszerzenia konfiguracji, takie jak <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> i <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> wymagają dodatkowych pakietów NuGet, takich jak [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) i [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="e2092-728">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="e2092-729">Jeśli aplikacja nie używa [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), należy dodać te pakiety do projektu oprócz podstawowego pakietu [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) .</span><span class="sxs-lookup"><span data-stu-id="e2092-729">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="e2092-730">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e2092-730">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="e2092-731">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="e2092-731">ConfigureServices</span></span>

<span data-ttu-id="e2092-732"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usługi do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-732"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e2092-733"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-733"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="e2092-734">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="e2092-734">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="e2092-735">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="e2092-735">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="e2092-736">[Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa metody rozszerzenia `AddHostedService`, aby dodać usługę dla zdarzeń okresu istnienia, `LifetimeEventsHostedService`i zadania w tle z przekroczeniem czasu, `TimedHostedService`do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e2092-736">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="e2092-737">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="e2092-737">ConfigureLogging</span></span>

<span data-ttu-id="e2092-738"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podanej <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2092-738"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="e2092-739"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> mogą być wywoływane wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-739"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="e2092-740">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="e2092-740">UseConsoleLifetime</span></span>

<span data-ttu-id="e2092-741"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> w celu uruchomienia procesu zamykania.</span><span class="sxs-lookup"><span data-stu-id="e2092-741"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="e2092-742"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> odblokowywanie rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="e2092-742"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="e2092-743">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest wstępnie zarejestrowany jako domyślna implementacja okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="e2092-743">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="e2092-744">Używany jest ostatni zarejestrowany okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="e2092-744">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="e2092-745">Konfiguracja kontenera</span><span class="sxs-lookup"><span data-stu-id="e2092-745">Container configuration</span></span>

<span data-ttu-id="e2092-746">Aby zapewnić obsługę podłączania w innych kontenerach, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="e2092-746">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="e2092-747">Dostarczenie fabryki nie jest częścią rejestracji typu DI Container, ale zamiast tego jest wewnętrznym hostem używanym do tworzenia konkretnych kontenerów DI.</span><span class="sxs-lookup"><span data-stu-id="e2092-747">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="e2092-748">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę używaną do tworzenia dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-748">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="e2092-749">Niestandardowa konfiguracja kontenera jest zarządzana przez metodę <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-749">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="e2092-750"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> zapewnia silnie określone środowisko do konfigurowania kontenera na podstawie podstawowego interfejsu API hosta.</span><span class="sxs-lookup"><span data-stu-id="e2092-750"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="e2092-751"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="e2092-751"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="e2092-752">Utwórz kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e2092-752">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="e2092-753">Podaj fabrykę kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="e2092-753">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="e2092-754">Użyj fabryki i skonfiguruj niestandardowy kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e2092-754">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="e2092-755">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="e2092-755">Extensibility</span></span>

<span data-ttu-id="e2092-756">Rozszerzalność hosta jest wykonywana przy użyciu metod rozszerzających na <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="e2092-756">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="e2092-757">Poniższy przykład pokazuje, jak Metoda rozszerzania rozszerza implementację <xref:Microsoft.Extensions.Hosting.IHostBuilder> z przykładem [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przedstawionym w <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="e2092-757">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="e2092-758">Aplikacja ustanawia `UseHostedService` metodę rozszerzenia w celu zarejestrowania usługi hostowanej przekazaną w `T`:</span><span class="sxs-lookup"><span data-stu-id="e2092-758">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="e2092-759">Zarządzanie hostem</span><span class="sxs-lookup"><span data-stu-id="e2092-759">Manage the host</span></span>

<span data-ttu-id="e2092-760">Implementacja <xref:Microsoft.Extensions.Hosting.IHost> jest odpowiedzialna za uruchamianie i zatrzymywanie implementacji <xref:Microsoft.Extensions.Hosting.IHostedService> zarejestrowanych w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="e2092-760">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="e2092-761">Run</span><span class="sxs-lookup"><span data-stu-id="e2092-761">Run</span></span>

<span data-ttu-id="e2092-762"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta:</span><span class="sxs-lookup"><span data-stu-id="e2092-762"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="e2092-763">RunAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-763">RunAsync</span></span>

<span data-ttu-id="e2092-764"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="e2092-764"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="e2092-765">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-765">RunConsoleAsync</span></span>

<span data-ttu-id="e2092-766"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie programu <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="e2092-766"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="e2092-767">Uruchom i StopAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-767">Start and StopAsync</span></span>

<span data-ttu-id="e2092-768"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="e2092-768"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="e2092-769"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="e2092-769"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="e2092-770">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-770">StartAsync and StopAsync</span></span>

<span data-ttu-id="e2092-771"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="e2092-771"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="e2092-772"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="e2092-772"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="e2092-773">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="e2092-773">WaitForShutdown</span></span>

<span data-ttu-id="e2092-774"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalane za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (nasłuchuje na <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="e2092-774"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM).</span></span> <span data-ttu-id="e2092-775"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> wywołań.</span><span class="sxs-lookup"><span data-stu-id="e2092-775"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="e2092-776">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="e2092-776">WaitForShutdownAsync</span></span>

<span data-ttu-id="e2092-777"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="e2092-777"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="e2092-778">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="e2092-778">External control</span></span>

<span data-ttu-id="e2092-779">Kontrolę zewnętrzną hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="e2092-779">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="e2092-780"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="e2092-780"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="e2092-781">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="e2092-781">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="e2092-782">IHostingEnvironment, interfejs</span><span class="sxs-lookup"><span data-stu-id="e2092-782">IHostingEnvironment interface</span></span>

<span data-ttu-id="e2092-783"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o środowisku hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-783"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="e2092-784">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) , aby uzyskać <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> w celu użycia jej właściwości i metod rozszerzania:</span><span class="sxs-lookup"><span data-stu-id="e2092-784">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="e2092-785">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e2092-785">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="e2092-786">IApplicationLifetime, interfejs</span><span class="sxs-lookup"><span data-stu-id="e2092-786">IApplicationLifetime interface</span></span>

<span data-ttu-id="e2092-787"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> pozwala na działania po uruchomieniu i zamknięcia, w tym bezpieczne żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="e2092-787"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="e2092-788">Trzy właściwości interfejsu są tokenami anulowania używanymi do rejestrowania metod <xref:System.Action>, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="e2092-788">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="e2092-789">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="e2092-789">Cancellation Token</span></span> | <span data-ttu-id="e2092-790">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="e2092-790">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="e2092-791">Host został w pełni uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="e2092-791">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="e2092-792">Host kończy bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="e2092-792">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="e2092-793">Wszystkie żądania powinny być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="e2092-793">All requests should be processed.</span></span> <span data-ttu-id="e2092-794">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-794">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="e2092-795">Host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="e2092-795">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="e2092-796">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="e2092-796">Requests may still be processing.</span></span> <span data-ttu-id="e2092-797">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-797">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="e2092-798">Konstruktor — wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> do dowolnej klasy.</span><span class="sxs-lookup"><span data-stu-id="e2092-798">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="e2092-799">[Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do klasy `LifetimeEventsHostedService` (implementacja <xref:Microsoft.Extensions.Hosting.IHostedService>), aby zarejestrować zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="e2092-799">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="e2092-800">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="e2092-800">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="e2092-801"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e2092-801"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="e2092-802">Następująca Klasa używa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, aby bezpiecznie zamknąć aplikację w przypadku wywołania metody `Shutdown` klasy:</span><span class="sxs-lookup"><span data-stu-id="e2092-802">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="e2092-803">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e2092-803">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
