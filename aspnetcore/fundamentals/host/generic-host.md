---
title: Host ogólny .NET
author: rick-anderson
description: Dowiedz się więcej o hoście ogólnym programu .NET Core, który jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/02/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 6a0ef02db883db3bc91722786cd042ccec092735
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659923"
---
# <a name="net-generic-host"></a><span data-ttu-id="56955-103">Host ogólny .NET</span><span class="sxs-lookup"><span data-stu-id="56955-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="56955-104">W tym artykule przedstawiono hosta ogólnego platformy .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) i przedstawiono wskazówki dotyczące korzystania z niego.</span><span class="sxs-lookup"><span data-stu-id="56955-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="56955-105">Co to jest host?</span><span class="sxs-lookup"><span data-stu-id="56955-105">What's a host?</span></span>

<span data-ttu-id="56955-106">*Host* to obiekt, który hermetyzuje zasoby aplikacji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="56955-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="56955-107">Iniekcja zależności (DI)</span><span class="sxs-lookup"><span data-stu-id="56955-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="56955-108">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="56955-108">Logging</span></span>
* <span data-ttu-id="56955-109">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="56955-109">Configuration</span></span>
* <span data-ttu-id="56955-110">implementacje `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="56955-110">`IHostedService` implementations</span></span>

<span data-ttu-id="56955-111">Po uruchomieniu hosta wywołuje `IHostedService.StartAsync` przy każdej implementacji <xref:Microsoft.Extensions.Hosting.IHostedService>, która znajduje się w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="56955-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="56955-112">W aplikacji sieci Web jedna z implementacji `IHostedService` jest usługą sieci Web, która uruchamia [implementację serwera http](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="56955-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="56955-113">Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="56955-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="56955-114">W wersjach ASP.NET Core wcześniejszych niż 3,0 [host sieci Web](xref:fundamentals/host/web-host) jest używany do obciążeń http.</span><span class="sxs-lookup"><span data-stu-id="56955-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="56955-115">Host sieci Web nie jest już zalecany dla aplikacji sieci Web i pozostaje dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="56955-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="56955-116">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="56955-116">Set up a host</span></span>

<span data-ttu-id="56955-117">Host jest zazwyczaj konfigurowany, zbudowany i uruchamiany przez kod w klasie `Program`.</span><span class="sxs-lookup"><span data-stu-id="56955-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="56955-118">Metoda `Main`:</span><span class="sxs-lookup"><span data-stu-id="56955-118">The `Main` method:</span></span>

* <span data-ttu-id="56955-119">Wywołuje metodę `CreateHostBuilder`, aby utworzyć i skonfigurować obiekt konstruktora.</span><span class="sxs-lookup"><span data-stu-id="56955-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="56955-120">Wywołuje metody `Build` i `Run` w obiekcie konstruktora.</span><span class="sxs-lookup"><span data-stu-id="56955-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="56955-121">Oto kod *program.cs* dla obciążenia innego niż HTTP z jedną implementacją `IHostedService` dodaną do kontenera di.</span><span class="sxs-lookup"><span data-stu-id="56955-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="56955-122">W przypadku obciążeń HTTP Metoda `Main` jest taka sama, ale `CreateHostBuilder` wywołuje `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="56955-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="56955-123">Jeśli aplikacja używa Entity Framework Core, nie zmieniaj nazwy ani podpisu metody `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="56955-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="56955-124">[Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) oczekują na znalezienie `CreateHostBuilder`j metody, która konfiguruje hosta bez uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="56955-125">Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="56955-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="56955-126">Ustawienia domyślnego konstruktora</span><span class="sxs-lookup"><span data-stu-id="56955-126">Default builder settings</span></span>

<span data-ttu-id="56955-127">Metoda <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="56955-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="56955-128">Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na ścieżkę zwracaną przez <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="56955-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="56955-129">Ładuje konfigurację hosta z:</span><span class="sxs-lookup"><span data-stu-id="56955-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="56955-130">Zmienne środowiskowe poprzedzone prefiksem "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="56955-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="56955-131">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="56955-131">Command-line arguments.</span></span>
* <span data-ttu-id="56955-132">Ładuje konfigurację aplikacji z:</span><span class="sxs-lookup"><span data-stu-id="56955-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="56955-133">*appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="56955-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="56955-134">*appSettings. {Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="56955-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="56955-135">[Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development`owym.</span><span class="sxs-lookup"><span data-stu-id="56955-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="56955-136">Zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="56955-136">Environment variables.</span></span>
  * <span data-ttu-id="56955-137">Argumenty wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="56955-137">Command-line arguments.</span></span>
* <span data-ttu-id="56955-138">Dodaje następujących dostawców [rejestrowania](xref:fundamentals/logging/index) :</span><span class="sxs-lookup"><span data-stu-id="56955-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="56955-139">Konsola</span><span class="sxs-lookup"><span data-stu-id="56955-139">Console</span></span>
  * <span data-ttu-id="56955-140">Debugowanie</span><span class="sxs-lookup"><span data-stu-id="56955-140">Debug</span></span>
  * <span data-ttu-id="56955-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="56955-141">EventSource</span></span>
  * <span data-ttu-id="56955-142">EventLog (tylko w przypadku uruchamiania w systemie Windows)</span><span class="sxs-lookup"><span data-stu-id="56955-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="56955-143">Umożliwia [weryfikację zakresu](xref:fundamentals/dependency-injection#scope-validation) i [Sprawdzanie poprawności zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) , gdy środowisko jest opracowywane.</span><span class="sxs-lookup"><span data-stu-id="56955-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="56955-144">Metoda `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="56955-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="56955-145">Ładuje konfigurację hosta ze zmiennych środowiskowych poprzedzonych prefiksem "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="56955-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="56955-146">Ustawia serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web i konfiguruje go przy użyciu dostawców konfiguracji hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="56955-147">Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="56955-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="56955-148">Dodaje [oprogramowanie pośredniczące do filtrowania hosta](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="56955-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="56955-149">Dodaje [przekazane nagłówki pośredniczące](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) , jeśli ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.</span><span class="sxs-lookup"><span data-stu-id="56955-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="56955-150">Włącza integrację usług IIS.</span><span class="sxs-lookup"><span data-stu-id="56955-150">Enables IIS integration.</span></span> <span data-ttu-id="56955-151">Aby poznać domyślne opcje usług IIS, zobacz <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="56955-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="56955-152">[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [Ustawienia dla usługi Web Apps](#settings-for-web-apps) w dalszej części tego artykułu pokazują, jak zastąpić ustawienia domyślnego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="56955-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="56955-153">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="56955-153">Framework-provided services</span></span>

<span data-ttu-id="56955-154">Usługi zarejestrowane automatycznie obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="56955-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="56955-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="56955-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="56955-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="56955-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="56955-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="56955-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="56955-158">Aby uzyskać więcej informacji na temat usług udostępnianych przez platformę, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="56955-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="56955-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="56955-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="56955-160">Wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) do dowolnej klasy w celu obsługi zadań po uruchomieniu i bezpiecznego zamykania.</span><span class="sxs-lookup"><span data-stu-id="56955-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="56955-161">Trzy właściwości interfejsu to tokeny anulowania używane do rejestrowania metod obsługi zdarzeń uruchamiania i zatrzymywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="56955-162">Interfejs zawiera również metodę `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="56955-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="56955-163">Poniższy przykład jest implementacją `IHostedService`, która rejestruje zdarzenia `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="56955-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="56955-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="56955-164">IHostLifetime</span></span>

<span data-ttu-id="56955-165">Implementacja <xref:Microsoft.Extensions.Hosting.IHostLifetime> kontroluje, gdy host zostanie uruchomiony i gdy zostanie zatrzymana.</span><span class="sxs-lookup"><span data-stu-id="56955-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="56955-166">Używana jest Ostatnia zarejestrowana implementacja.</span><span class="sxs-lookup"><span data-stu-id="56955-166">The last implementation registered is used.</span></span>

<span data-ttu-id="56955-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest domyślną implementacją `IHostLifetime`.</span><span class="sxs-lookup"><span data-stu-id="56955-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="56955-168">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="56955-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="56955-169">Nasłuchuje kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*>, aby rozpocząć proces zamykania.</span><span class="sxs-lookup"><span data-stu-id="56955-169">Listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="56955-170">Odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="56955-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="56955-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="56955-171">IHostEnvironment</span></span>

<span data-ttu-id="56955-172">Wsuń usługę <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do klasy, aby uzyskać informacje o następujących kwestiach:</span><span class="sxs-lookup"><span data-stu-id="56955-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="56955-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="56955-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="56955-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="56955-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="56955-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="56955-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="56955-176">Aplikacje sieci Web implementują interfejs `IWebHostEnvironment`, który dziedziczy `IHostEnvironment` i dodaje [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="56955-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="56955-177">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="56955-177">Host configuration</span></span>

<span data-ttu-id="56955-178">Konfiguracja hosta jest używana we właściwościach implementacji <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="56955-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="56955-179">Konfiguracja hosta jest dostępna z [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="56955-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="56955-180">Po `ConfigureAppConfiguration``HostBuilderContext.Configuration` jest zastępowana konfiguracją aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="56955-181">Aby dodać konfigurację hosta, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="56955-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="56955-182">`ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="56955-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="56955-183">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="56955-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="56955-184">Dostawca zmiennych środowiskowych z prefiksami `DOTNET_` i argumenty wiersza polecenia są uwzględniane przez CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="56955-184">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="56955-185">W przypadku aplikacji sieci Web zostanie dodany dostawca zmiennych środowiskowych z prefiksem `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="56955-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="56955-186">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="56955-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="56955-187">Na przykład wartość zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` ma wartość Konfiguracja hosta dla klucza `environment`.</span><span class="sxs-lookup"><span data-stu-id="56955-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="56955-188">Poniższy przykład umożliwia utworzenie konfiguracji hosta:</span><span class="sxs-lookup"><span data-stu-id="56955-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="56955-189">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="56955-189">App configuration</span></span>

<span data-ttu-id="56955-190">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="56955-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="56955-191">`ConfigureAppConfiguration` może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="56955-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="56955-192">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="56955-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="56955-193">Konfiguracja utworzona przez `ConfigureAppConfiguration` jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i jako usługa z elementu di.</span><span class="sxs-lookup"><span data-stu-id="56955-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="56955-194">Konfiguracja hosta jest również dodawana do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="56955-195">Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="56955-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="56955-196">Ustawienia dla wszystkich typów aplikacji</span><span class="sxs-lookup"><span data-stu-id="56955-196">Settings for all app types</span></span>

<span data-ttu-id="56955-197">Ta sekcja zawiera listę ustawień hosta, które dotyczą zarówno obciążeń HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="56955-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="56955-198">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="56955-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="56955-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="56955-199">ApplicationName</span></span>

<span data-ttu-id="56955-200">Właściwość [IHostEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="56955-201">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="56955-201">**Key**: applicationName</span></span>  
<span data-ttu-id="56955-202">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-202">**Type**: *string*</span></span>  
<span data-ttu-id="56955-203">**Wartość domyślna**: Nazwa zestawu, który zawiera punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="56955-204">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="56955-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="56955-205">Aby ustawić tę wartość, należy użyć zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="56955-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="56955-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="56955-206">ContentRootPath</span></span>

<span data-ttu-id="56955-207">Właściwość [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="56955-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="56955-208">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="56955-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="56955-209">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="56955-209">**Key**: contentRoot</span></span>  
<span data-ttu-id="56955-210">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-210">**Type**: *string*</span></span>  
<span data-ttu-id="56955-211">**Domyślnie**: folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="56955-212">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="56955-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="56955-213">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseContentRoot` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="56955-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="56955-214">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="56955-214">For more information, see:</span></span>

* [<span data-ttu-id="56955-215">Podstawy: zawartość główna</span><span class="sxs-lookup"><span data-stu-id="56955-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="56955-216">WebRoot</span><span class="sxs-lookup"><span data-stu-id="56955-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="56955-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="56955-217">EnvironmentName</span></span>

<span data-ttu-id="56955-218">Dla właściwości [IHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="56955-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="56955-219">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`.</span><span class="sxs-lookup"><span data-stu-id="56955-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="56955-220">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="56955-220">Values aren't case sensitive.</span></span>

<span data-ttu-id="56955-221">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="56955-221">**Key**: environment</span></span>  
<span data-ttu-id="56955-222">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-222">**Type**: *string*</span></span>  
<span data-ttu-id="56955-223">**Wartość domyślna**: produkcja</span><span class="sxs-lookup"><span data-stu-id="56955-223">**Default**: Production</span></span>  
<span data-ttu-id="56955-224">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="56955-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="56955-225">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseEnvironment` na `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="56955-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="56955-226">shutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="56955-226">ShutdownTimeout</span></span>

<span data-ttu-id="56955-227">[HostOptions. shutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="56955-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="56955-228">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="56955-228">The default value is five seconds.</span></span>  <span data-ttu-id="56955-229">Podczas okresu przekroczenia limitu czasu Host:</span><span class="sxs-lookup"><span data-stu-id="56955-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="56955-230">Wyzwala [IHostApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="56955-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="56955-231">Próbuje zatrzymać usługi hostowane, rejestrowanie błędów dla usług, których zatrzymanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="56955-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="56955-232">Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="56955-233">Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="56955-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="56955-234">Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.</span><span class="sxs-lookup"><span data-stu-id="56955-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="56955-235">**Klucz**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="56955-235">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="56955-236">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="56955-236">**Type**: *int*</span></span>  
<span data-ttu-id="56955-237">**Domyślnie**: 5 sekund **zmienna środowiskowa**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="56955-237">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="56955-238">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub skonfiguruj `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="56955-238">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="56955-239">Poniższy przykład ustawia limit czasu na 20 sekund:</span><span class="sxs-lookup"><span data-stu-id="56955-239">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="56955-240">Ustawienia usługi Web Apps</span><span class="sxs-lookup"><span data-stu-id="56955-240">Settings for web apps</span></span>

<span data-ttu-id="56955-241">Niektóre ustawienia hosta mają zastosowanie tylko do obciążeń protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="56955-241">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="56955-242">Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="56955-242">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="56955-243">Metody rozszerzające w `IWebHostBuilder` są dostępne dla tych ustawień.</span><span class="sxs-lookup"><span data-stu-id="56955-243">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="56955-244">Przykłady kodu, które pokazują, jak wywoływać metody rozszerzenia Załóżmy, że `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="56955-244">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="56955-245">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="56955-245">CaptureStartupErrors</span></span>

<span data-ttu-id="56955-246">Gdy `false`, błędy podczas uruchamiania, kończenie działania hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-246">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="56955-247">Gdy `true`, Host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.</span><span class="sxs-lookup"><span data-stu-id="56955-247">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="56955-248">**Klucz**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="56955-248">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="56955-249">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="56955-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="56955-250">**Domyślnie**: wartość domyślna to `false`, chyba że aplikacja zostanie uruchomiona z Kestrel za pomocą usług IIS, gdzie wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="56955-250">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="56955-251">**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="56955-251">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="56955-252">Aby ustawić tę wartość, użyj konfiguracji lub `CaptureStartupErrors`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-252">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="56955-253">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="56955-253">DetailedErrors</span></span>

<span data-ttu-id="56955-254">Po włączeniu lub gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe błędy.</span><span class="sxs-lookup"><span data-stu-id="56955-254">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="56955-255">**Klucz**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="56955-255">**Key**: detailedErrors</span></span>  
<span data-ttu-id="56955-256">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="56955-256">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="56955-257">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="56955-257">**Default**: false</span></span>  
<span data-ttu-id="56955-258">**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="56955-258">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="56955-259">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-259">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="56955-260">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="56955-260">HostingStartupAssemblies</span></span>

<span data-ttu-id="56955-261">Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="56955-261">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="56955-262">Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-262">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="56955-263">W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="56955-263">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="56955-264">**Klucz**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="56955-264">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="56955-265">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-265">**Type**: *string*</span></span>  
<span data-ttu-id="56955-266">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="56955-266">**Default**: Empty string</span></span>  
<span data-ttu-id="56955-267">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="56955-267">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="56955-268">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-268">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="56955-269">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="56955-269">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="56955-270">Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="56955-270">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="56955-271">**Klucz**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="56955-271">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="56955-272">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-272">**Type**: *string*</span></span>  
<span data-ttu-id="56955-273">**Wartość domyślna**: pusty ciąg</span><span class="sxs-lookup"><span data-stu-id="56955-273">**Default**: Empty string</span></span>  
<span data-ttu-id="56955-274">**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="56955-274">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="56955-275">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-275">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="56955-276">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="56955-276">HTTPS_Port</span></span>

<span data-ttu-id="56955-277">Port przekierowania protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56955-277">The HTTPS redirect port.</span></span> <span data-ttu-id="56955-278">Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="56955-278">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="56955-279">**Klucz**: https_port</span><span class="sxs-lookup"><span data-stu-id="56955-279">**Key**: https_port</span></span>  
<span data-ttu-id="56955-280">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-280">**Type**: *string*</span></span>  
<span data-ttu-id="56955-281">Wartość **Domyślna**: nie ustawiono wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="56955-281">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="56955-282">**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="56955-282">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="56955-283">Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-283">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="56955-284">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="56955-284">PreferHostingUrls</span></span>

<span data-ttu-id="56955-285">Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy użyciu `IWebHostBuilder`, a nie dla implementacji `IServer`.</span><span class="sxs-lookup"><span data-stu-id="56955-285">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="56955-286">**Klucz**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="56955-286">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="56955-287">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="56955-287">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="56955-288">Wartość **Domyślna**: prawda</span><span class="sxs-lookup"><span data-stu-id="56955-288">**Default**: true</span></span>  
<span data-ttu-id="56955-289">**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="56955-289">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="56955-290">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `PreferHostingUrls`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-290">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="56955-291">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="56955-291">PreventHostingStartup</span></span>

<span data-ttu-id="56955-292">Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-292">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="56955-293">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="56955-293">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="56955-294">**Klucz**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="56955-294">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="56955-295">**Typ**: *bool* (`true` lub `1`)</span><span class="sxs-lookup"><span data-stu-id="56955-295">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="56955-296">**Wartość domyślna**: FAŁSZ</span><span class="sxs-lookup"><span data-stu-id="56955-296">**Default**: false</span></span>  
<span data-ttu-id="56955-297">**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="56955-297">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="56955-298">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseSetting` wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-298">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="56955-299">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="56955-299">StartupAssembly</span></span>

<span data-ttu-id="56955-300">Zestaw do wyszukiwania klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="56955-300">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="56955-301">**Klucz**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="56955-301">**Key**: startupAssembly</span></span>  
<span data-ttu-id="56955-302">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-302">**Type**: *string*</span></span>  
<span data-ttu-id="56955-303">**Domyślnie**: zestaw aplikacji</span><span class="sxs-lookup"><span data-stu-id="56955-303">**Default**: The app's assembly</span></span>  
<span data-ttu-id="56955-304">**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="56955-304">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="56955-305">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseStartup`wywołań.</span><span class="sxs-lookup"><span data-stu-id="56955-305">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="56955-306">`UseStartup` może przyjmować nazwę zestawu (`string`) lub typ (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="56955-306">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="56955-307">Jeśli wywoływana jest wiele metod `UseStartup`, pierwszeństwo ma Ostatnia.</span><span class="sxs-lookup"><span data-stu-id="56955-307">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="56955-308">Adresy URL</span><span class="sxs-lookup"><span data-stu-id="56955-308">URLs</span></span>

<span data-ttu-id="56955-309">Rozdzielana średnikami lista adresów IP lub adresów hostów z portami i protokołami, na których serwer powinien nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="56955-309">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="56955-310">Na przykład `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="56955-310">For example, `http://localhost:123`.</span></span> <span data-ttu-id="56955-311">Użyj "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="56955-311">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="56955-312">Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="56955-312">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="56955-313">Obsługiwane formaty różnią się między serwerami.</span><span class="sxs-lookup"><span data-stu-id="56955-313">Supported formats vary among servers.</span></span>

<span data-ttu-id="56955-314">**Klucz**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="56955-314">**Key**: urls</span></span>  
<span data-ttu-id="56955-315">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-315">**Type**: *string*</span></span>  
<span data-ttu-id="56955-316">**Wartość domyślna**: `http://localhost:5000` i `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="56955-316">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="56955-317">**Zmienna środowiskowa**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="56955-317">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="56955-318">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseUrls`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-318">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="56955-319">Kestrel ma własny interfejs API konfiguracji punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="56955-319">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="56955-320">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="56955-320">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="56955-321">WebRoot</span><span class="sxs-lookup"><span data-stu-id="56955-321">WebRoot</span></span>

<span data-ttu-id="56955-322">Ścieżka względna do statycznych zasobów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-322">The relative path to the app's static assets.</span></span>

<span data-ttu-id="56955-323">**Klucz**: Webroot</span><span class="sxs-lookup"><span data-stu-id="56955-323">**Key**: webroot</span></span>  
<span data-ttu-id="56955-324">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-324">**Type**: *string*</span></span>  
<span data-ttu-id="56955-325">**Wartość domyślna**: wartość domyślna to `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="56955-325">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="56955-326">Ścieżka do *elementu {content root}/wwwroot* musi istnieć.</span><span class="sxs-lookup"><span data-stu-id="56955-326">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="56955-327">Jeśli ścieżka nie istnieje, jest używany dostawca plików No-op.</span><span class="sxs-lookup"><span data-stu-id="56955-327">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="56955-328">**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="56955-328">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="56955-329">Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseWebRoot`wywołań:</span><span class="sxs-lookup"><span data-stu-id="56955-329">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="56955-330">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="56955-330">For more information, see:</span></span>

* [<span data-ttu-id="56955-331">Podstawy: katalog główny sieci Web</span><span class="sxs-lookup"><span data-stu-id="56955-331">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="56955-332">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="56955-332">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="56955-333">Zarządzanie okresem istnienia hosta</span><span class="sxs-lookup"><span data-stu-id="56955-333">Manage the host lifetime</span></span>

<span data-ttu-id="56955-334">Wywołaj metody na skompilowanej implementacji <xref:Microsoft.Extensions.Hosting.IHost>, aby uruchomić i zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="56955-334">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="56955-335">Te metody wpływają na wszystkie implementacje <xref:Microsoft.Extensions.Hosting.IHostedService>, które są zarejestrowane w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="56955-335">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="56955-336">Uruchom polecenie</span><span class="sxs-lookup"><span data-stu-id="56955-336">Run</span></span>

<span data-ttu-id="56955-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="56955-338">RunAsync</span><span class="sxs-lookup"><span data-stu-id="56955-338">RunAsync</span></span>

<span data-ttu-id="56955-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="56955-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="56955-340">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="56955-340">RunConsoleAsync</span></span>

<span data-ttu-id="56955-341"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="56955-341"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="56955-342">Uruchamianie</span><span class="sxs-lookup"><span data-stu-id="56955-342">Start</span></span>

<span data-ttu-id="56955-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="56955-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="56955-344">StartAsync</span><span class="sxs-lookup"><span data-stu-id="56955-344">StartAsync</span></span>

<span data-ttu-id="56955-345"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task>, który kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="56955-345"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="56955-346"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku `StartAsync`, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="56955-346"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="56955-347">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="56955-347">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="56955-348">StopAsync</span><span class="sxs-lookup"><span data-stu-id="56955-348">StopAsync</span></span>

<span data-ttu-id="56955-349"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="56955-349"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="56955-350">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="56955-350">WaitForShutdown</span></span>

<span data-ttu-id="56955-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący do momentu wyzwolenia przez IHostLifetime, na przykład za pośrednictwem kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="56955-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="56955-352">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="56955-352">WaitForShutdownAsync</span></span>

<span data-ttu-id="56955-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="56955-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="56955-354">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="56955-354">External control</span></span>

<span data-ttu-id="56955-355">Bezpośrednią kontrolę nad okresem istnienia hosta można uzyskać przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="56955-355">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="56955-356">ASP.NET Core aplikacje konfigurują i uruchamiają hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-356">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="56955-357">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-357">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="56955-358">W tym artykule opisano ASP.NET Core hosta ogólnego (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="56955-358">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="56955-359">Przeznaczeniem hosta ogólnego jest oddzielenie potoku HTTP od interfejsu API hosta sieci Web w celu umożliwienia szerszej tablicy scenariuszy hostów.</span><span class="sxs-lookup"><span data-stu-id="56955-359">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="56955-360">Obsługa komunikatów, zadań w tle i innych obciążeń innych niż HTTP na podstawie ogólnej korzyści z hosta, takich jak konfiguracja, iniekcja zależności (DI) i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="56955-360">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="56955-361">Host ogólny jest nowy w ASP.NET Core 2,1 i nie jest odpowiedni dla scenariuszy hostingu w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="56955-361">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="56955-362">W przypadku scenariuszy hostingu w sieci Web należy użyć [hosta sieci Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="56955-362">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="56955-363">Host ogólny zastąpi hosta sieci Web w przyszłej wersji i będzie pełnić rolę podstawowego interfejsu API hosta zarówno w scenariuszach HTTP, jak i innych niż HTTP.</span><span class="sxs-lookup"><span data-stu-id="56955-363">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="56955-364">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56955-364">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="56955-365">Podczas uruchamiania przykładowej aplikacji w [Visual Studio Code](https://code.visualstudio.com/)należy użyć *zewnętrznego lub zintegrowanego terminalu*.</span><span class="sxs-lookup"><span data-stu-id="56955-365">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="56955-366">Nie uruchamiaj przykładu w `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="56955-366">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="56955-367">Aby ustawić konsolę w Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="56955-367">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="56955-368">Otwórz plik *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="56955-368">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="56955-369">W konfiguracji **uruchamiania programu .NET Core (konsoli)** zlokalizuj wpis **konsoli** .</span><span class="sxs-lookup"><span data-stu-id="56955-369">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="56955-370">Ustaw wartość na `externalTerminal` lub `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="56955-370">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="56955-371">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="56955-371">Introduction</span></span>

<span data-ttu-id="56955-372">Ogólna Biblioteka hostów jest dostępna w przestrzeni nazw <xref:Microsoft.Extensions.Hosting> i udostępniana przez pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) .</span><span class="sxs-lookup"><span data-stu-id="56955-372">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="56955-373">Pakiet `Microsoft.Extensions.Hosting` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszym).</span><span class="sxs-lookup"><span data-stu-id="56955-373">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="56955-374"><xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu.</span><span class="sxs-lookup"><span data-stu-id="56955-374"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="56955-375">Każda implementacja <xref:Microsoft.Extensions.Hosting.IHostedService> jest wykonywana w kolejności [rejestracji usługi w ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="56955-375">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="56955-376"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana dla każdego <xref:Microsoft.Extensions.Hosting.IHostedService> podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w odwrotnej kolejności rejestracji, gdy host zostanie bezpiecznie zamknięty.</span><span class="sxs-lookup"><span data-stu-id="56955-376"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="56955-377">Konfigurowanie hosta</span><span class="sxs-lookup"><span data-stu-id="56955-377">Set up a host</span></span>

<span data-ttu-id="56955-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder> jest głównym składnikiem używanym przez biblioteki i aplikacje do inicjowania, kompilowania i uruchamiania hosta:</span><span class="sxs-lookup"><span data-stu-id="56955-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="56955-379">Opcje</span><span class="sxs-lookup"><span data-stu-id="56955-379">Options</span></span>

<span data-ttu-id="56955-380"><xref:Microsoft.Extensions.Hosting.HostOptions> skonfigurować opcje <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="56955-380"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="56955-381">Limit czasu zamykania</span><span class="sxs-lookup"><span data-stu-id="56955-381">Shutdown timeout</span></span>

<span data-ttu-id="56955-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="56955-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="56955-383">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="56955-383">The default value is five seconds.</span></span>

<span data-ttu-id="56955-384">Następująca konfiguracja opcji w `Program.Main` zwiększa domyślny pięciu sekund limitu czasu zamykania na 20 s:</span><span class="sxs-lookup"><span data-stu-id="56955-384">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="56955-385">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="56955-385">Default services</span></span>

<span data-ttu-id="56955-386">Podczas inicjowania hosta zarejestrowano następujące usługi:</span><span class="sxs-lookup"><span data-stu-id="56955-386">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="56955-387">[Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="56955-387">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="56955-388">[Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="56955-388">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="56955-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="56955-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="56955-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="56955-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="56955-391">[Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="56955-391">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="56955-392">[Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="56955-392">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="56955-393">Konfiguracja hosta</span><span class="sxs-lookup"><span data-stu-id="56955-393">Host configuration</span></span>

<span data-ttu-id="56955-394">Konfiguracja hosta jest tworzona przez:</span><span class="sxs-lookup"><span data-stu-id="56955-394">Host configuration is created by:</span></span>

* <span data-ttu-id="56955-395">Wywoływanie metod rozszerzających <xref:Microsoft.Extensions.Hosting.IHostBuilder> w celu ustawienia [katalogu głównego zawartości](#content-root) i [środowiska](#environment).</span><span class="sxs-lookup"><span data-stu-id="56955-395">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="56955-396">Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="56955-396">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="56955-397">Metody rozszerzające</span><span class="sxs-lookup"><span data-stu-id="56955-397">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="56955-398">Klucz aplikacji (nazwa)</span><span class="sxs-lookup"><span data-stu-id="56955-398">Application key (name)</span></span>

<span data-ttu-id="56955-399">Właściwość [IHostingEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-399">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="56955-400">Aby jawnie ustawić wartość, użyj [HostDefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span><span class="sxs-lookup"><span data-stu-id="56955-400">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="56955-401">**Klucz**: ApplicationName</span><span class="sxs-lookup"><span data-stu-id="56955-401">**Key**: applicationName</span></span>  
<span data-ttu-id="56955-402">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-402">**Type**: *string*</span></span>  
<span data-ttu-id="56955-403">**Wartość domyślna**: Nazwa zestawu zawierającego punkt wejścia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-403">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="56955-404">**Ustaw przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="56955-404">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="56955-405">**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="56955-405">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="56955-406">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="56955-406">Content root</span></span>

<span data-ttu-id="56955-407">To ustawienie określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.</span><span class="sxs-lookup"><span data-stu-id="56955-407">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="56955-408">**Klucz**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="56955-408">**Key**: contentRoot</span></span>  
<span data-ttu-id="56955-409">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-409">**Type**: *string*</span></span>  
<span data-ttu-id="56955-410">**Domyślnie**: Domyślnie folder, w którym znajduje się zestaw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-410">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="56955-411">**Ustaw przy użyciu**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="56955-411">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="56955-412">**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="56955-412">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="56955-413">Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="56955-413">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="56955-414">Aby uzyskać więcej informacji, zobacz temat [podstawy: zawartość główna](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="56955-414">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="56955-415">Środowisko</span><span class="sxs-lookup"><span data-stu-id="56955-415">Environment</span></span>

<span data-ttu-id="56955-416">Ustawia [środowisko](xref:fundamentals/environments)aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-416">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="56955-417">**Klucz**: środowisko</span><span class="sxs-lookup"><span data-stu-id="56955-417">**Key**: environment</span></span>  
<span data-ttu-id="56955-418">**Typ**: *ciąg*</span><span class="sxs-lookup"><span data-stu-id="56955-418">**Type**: *string*</span></span>  
<span data-ttu-id="56955-419">**Wartość domyślna**: produkcja</span><span class="sxs-lookup"><span data-stu-id="56955-419">**Default**: Production</span></span>  
<span data-ttu-id="56955-420">**Ustaw przy użyciu**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="56955-420">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="56955-421">**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="56955-421">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="56955-422">Dla środowiska można ustawić dowolną wartość.</span><span class="sxs-lookup"><span data-stu-id="56955-422">The environment can be set to any value.</span></span> <span data-ttu-id="56955-423">Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`.</span><span class="sxs-lookup"><span data-stu-id="56955-423">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="56955-424">W wartościach nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="56955-424">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="56955-425">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="56955-425">ConfigureHostConfiguration</span></span>

<span data-ttu-id="56955-426"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do tworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-426"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="56955-427">Konfiguracja hosta służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-427">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="56955-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="56955-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="56955-429">Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="56955-429">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="56955-430">Żaden dostawca nie jest domyślnie uwzględniany.</span><span class="sxs-lookup"><span data-stu-id="56955-430">No providers are included by default.</span></span> <span data-ttu-id="56955-431">Należy jawnie określić dostawców konfiguracji, których wymaga aplikacja w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, w tym:</span><span class="sxs-lookup"><span data-stu-id="56955-431">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="56955-432">Konfiguracja pliku (na przykład z pliku *HostSettings. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="56955-432">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="56955-433">Konfiguracja zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="56955-433">Environment variable configuration.</span></span>
* <span data-ttu-id="56955-434">Konfiguracja argumentu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="56955-434">Command-line argument configuration.</span></span>
* <span data-ttu-id="56955-435">Każdy inny wymagany dostawca konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="56955-435">Any other required configuration providers.</span></span>

<span data-ttu-id="56955-436">Konfiguracja pliku hosta jest włączana przez określenie ścieżki podstawowej aplikacji z `SetBasePath`, a następnie wywołanie jednego z [dostawców konfiguracji plików](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="56955-436">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="56955-437">Przykładowa aplikacja używa pliku JSON, *HostSettings. JSON*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*>, aby użyć ustawień konfiguracji hosta pliku.</span><span class="sxs-lookup"><span data-stu-id="56955-437">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="56955-438">Aby dodać [konfigurację zmiennej środowiskowej](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, wywołaj <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> na konstruktorze hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-438">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="56955-439">`AddEnvironmentVariables` akceptuje opcjonalnego prefiksu zdefiniowanego przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="56955-439">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="56955-440">Przykładowa aplikacja używa prefiksu `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="56955-440">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="56955-441">Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="56955-441">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="56955-442">Po skonfigurowaniu hosta przykładowej aplikacji wartość zmiennej środowiskowej dla `PREFIX_ENVIRONMENT` będzie wartością konfiguracji hosta dla klucza `environment`.</span><span class="sxs-lookup"><span data-stu-id="56955-442">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="56955-443">Podczas programowania podczas korzystania z [programu Visual Studio](https://visualstudio.microsoft.com) lub uruchamiania aplikacji z `dotnet run`, zmienne środowiskowe można ustawić w pliku *Properties/profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="56955-443">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="56955-444">W [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe mogą być ustawiane w pliku *. programu vscode/Launch. JSON* podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="56955-444">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="56955-445">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="56955-445">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="56955-446">[Konfiguracja wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawana przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="56955-446">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="56955-447">Konfiguracja wiersza polecenia jest dodawana jako Ostatnia, aby zezwolić na argumenty wiersza polecenia w celu przesłonięcia konfiguracji udostępnionej przez wcześniejszych dostawców konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="56955-447">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="56955-448">*HostSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="56955-448">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="56955-449">Dodatkową konfigurację można uzyskać za pomocą programu [ApplicationName](#application-key-name) i kluczy [contentRoot](#content-root) .</span><span class="sxs-lookup"><span data-stu-id="56955-449">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="56955-450">Przykład konfiguracji `HostBuilder` przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="56955-450">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="56955-451">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="56955-451">ConfigureAppConfiguration</span></span>

<span data-ttu-id="56955-452">Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w implementacji <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="56955-452">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="56955-453"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do tworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-453"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="56955-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="56955-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="56955-455">Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.</span><span class="sxs-lookup"><span data-stu-id="56955-455">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="56955-456">Konfiguracja utworzona przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="56955-456">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="56955-457">Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta dostarczoną przez [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="56955-457">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="56955-458">Przykładowa konfiguracja aplikacji przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="56955-458">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="56955-459">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="56955-459">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="56955-460">*appSettings. Plik Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="56955-460">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="56955-461">*appSettings. Production. JSON*:</span><span class="sxs-lookup"><span data-stu-id="56955-461">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="56955-462">Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="56955-462">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="56955-463">Aplikacja Przykładowa przenosi pliki ustawień aplikacji JSON i *HostSettings. JSON* z następującym `<Content>` elementem:</span><span class="sxs-lookup"><span data-stu-id="56955-463">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="56955-464">Metody rozszerzenia konfiguracji, takie jak <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> i <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> wymagają dodatkowych pakietów NuGet, takich jak [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) i [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="56955-464">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="56955-465">Jeśli aplikacja nie używa [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), należy dodać te pakiety do projektu oprócz podstawowego pakietu [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) .</span><span class="sxs-lookup"><span data-stu-id="56955-465">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="56955-466">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="56955-466">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="56955-467">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="56955-467">ConfigureServices</span></span>

<span data-ttu-id="56955-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usługi do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="56955-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="56955-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="56955-470">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="56955-470">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="56955-471">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="56955-471">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="56955-472">[Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa metody rozszerzenia `AddHostedService`, aby dodać usługę dla zdarzeń okresu istnienia, `LifetimeEventsHostedService`i zadania w tle z przekroczeniem czasu, `TimedHostedService`do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="56955-472">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="56955-473">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="56955-473">ConfigureLogging</span></span>

<span data-ttu-id="56955-474"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podanej <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="56955-474"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="56955-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> mogą być wywoływane wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="56955-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="56955-476">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="56955-476">UseConsoleLifetime</span></span>

<span data-ttu-id="56955-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM i wywołań <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, aby rozpocząć proces zamykania.</span><span class="sxs-lookup"><span data-stu-id="56955-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="56955-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> odblokowywanie rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="56955-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="56955-479">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest wstępnie zarejestrowany jako domyślna implementacja okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="56955-479">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="56955-480">Używany jest ostatni zarejestrowany okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="56955-480">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="56955-481">Konfiguracja kontenera</span><span class="sxs-lookup"><span data-stu-id="56955-481">Container configuration</span></span>

<span data-ttu-id="56955-482">Aby zapewnić obsługę podłączania w innych kontenerach, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="56955-482">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="56955-483">Dostarczenie fabryki nie jest częścią rejestracji typu DI Container, ale zamiast tego jest wewnętrznym hostem używanym do tworzenia konkretnych kontenerów DI.</span><span class="sxs-lookup"><span data-stu-id="56955-483">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="56955-484">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę używaną do tworzenia dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-484">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="56955-485">Niestandardowa konfiguracja kontenera jest zarządzana przez metodę <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="56955-485">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="56955-486"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> zapewnia silnie określone środowisko do konfigurowania kontenera na podstawie podstawowego interfejsu API hosta.</span><span class="sxs-lookup"><span data-stu-id="56955-486"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="56955-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> może być wywoływana wiele razy z wynikami.</span><span class="sxs-lookup"><span data-stu-id="56955-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="56955-488">Utwórz kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="56955-488">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="56955-489">Podaj fabrykę kontenera usługi:</span><span class="sxs-lookup"><span data-stu-id="56955-489">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="56955-490">Użyj fabryki i skonfiguruj niestandardowy kontener usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="56955-490">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="56955-491">Rozszerzalność</span><span class="sxs-lookup"><span data-stu-id="56955-491">Extensibility</span></span>

<span data-ttu-id="56955-492">Rozszerzalność hosta jest wykonywana przy użyciu metod rozszerzających na <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="56955-492">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="56955-493">Poniższy przykład pokazuje, jak Metoda rozszerzania rozszerza implementację <xref:Microsoft.Extensions.Hosting.IHostBuilder> z przykładem [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przedstawionym w <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="56955-493">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="56955-494">Aplikacja ustanawia `UseHostedService` metodę rozszerzenia w celu zarejestrowania usługi hostowanej przekazaną w `T`:</span><span class="sxs-lookup"><span data-stu-id="56955-494">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="56955-495">Zarządzanie hostem</span><span class="sxs-lookup"><span data-stu-id="56955-495">Manage the host</span></span>

<span data-ttu-id="56955-496">Implementacja <xref:Microsoft.Extensions.Hosting.IHost> jest odpowiedzialna za uruchamianie i zatrzymywanie implementacji <xref:Microsoft.Extensions.Hosting.IHostedService> zarejestrowanych w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="56955-496">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="56955-497">Uruchom polecenie</span><span class="sxs-lookup"><span data-stu-id="56955-497">Run</span></span>

<span data-ttu-id="56955-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta:</span><span class="sxs-lookup"><span data-stu-id="56955-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="56955-499">RunAsync</span><span class="sxs-lookup"><span data-stu-id="56955-499">RunAsync</span></span>

<span data-ttu-id="56955-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia:</span><span class="sxs-lookup"><span data-stu-id="56955-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="56955-501">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="56955-501">RunConsoleAsync</span></span>

<span data-ttu-id="56955-502"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie klawiszy CTRL + C/SIGINT lub SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="56955-502"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="56955-503">Uruchom i StopAsync</span><span class="sxs-lookup"><span data-stu-id="56955-503">Start and StopAsync</span></span>

<span data-ttu-id="56955-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.</span><span class="sxs-lookup"><span data-stu-id="56955-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="56955-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="56955-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="56955-506">StartAsync i StopAsync</span><span class="sxs-lookup"><span data-stu-id="56955-506">StartAsync and StopAsync</span></span>

<span data-ttu-id="56955-507"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="56955-507"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="56955-508"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> zatrzymać aplikację.</span><span class="sxs-lookup"><span data-stu-id="56955-508"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="56955-509">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="56955-509">WaitForShutdown</span></span>

<span data-ttu-id="56955-510"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalane za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (nasłuchuje w przypadku kombinacji klawiszy CTRL + C/SIGINT lub SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="56955-510"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="56955-511"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> wywołań.</span><span class="sxs-lookup"><span data-stu-id="56955-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="56955-512">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="56955-512">WaitForShutdownAsync</span></span>

<span data-ttu-id="56955-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="56955-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="56955-514">Zewnętrzna kontrola</span><span class="sxs-lookup"><span data-stu-id="56955-514">External control</span></span>

<span data-ttu-id="56955-515">Kontrolę zewnętrzną hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="56955-515">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="56955-516"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który czeka na zakończenie przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="56955-516"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="56955-517">Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="56955-517">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="56955-518">IHostingEnvironment, interfejs</span><span class="sxs-lookup"><span data-stu-id="56955-518">IHostingEnvironment interface</span></span>

<span data-ttu-id="56955-519"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o środowisku hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-519"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="56955-520">Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) , aby uzyskać <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> w celu użycia jej właściwości i metod rozszerzania:</span><span class="sxs-lookup"><span data-stu-id="56955-520">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="56955-521">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="56955-521">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="56955-522">IApplicationLifetime, interfejs</span><span class="sxs-lookup"><span data-stu-id="56955-522">IApplicationLifetime interface</span></span>

<span data-ttu-id="56955-523"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> pozwala na działania po uruchomieniu i zamknięcia, w tym bezpieczne żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="56955-523"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="56955-524">Trzy właściwości interfejsu są tokenami anulowania używanymi do rejestrowania metod <xref:System.Action>, które definiują zdarzenia uruchamiania i zamykania.</span><span class="sxs-lookup"><span data-stu-id="56955-524">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="56955-525">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="56955-525">Cancellation Token</span></span> | <span data-ttu-id="56955-526">Wyzwalane, gdy&#8230;</span><span class="sxs-lookup"><span data-stu-id="56955-526">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="56955-527">Host został w pełni uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="56955-527">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="56955-528">Host kończy bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="56955-528">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="56955-529">Wszystkie żądania powinny być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="56955-529">All requests should be processed.</span></span> <span data-ttu-id="56955-530">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="56955-530">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="56955-531">Host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="56955-531">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="56955-532">Żądania mogą nadal być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="56955-532">Requests may still be processing.</span></span> <span data-ttu-id="56955-533">Bloki zamknięcia do momentu zakończenia tego zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="56955-533">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="56955-534">Konstruktor — wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> do dowolnej klasy.</span><span class="sxs-lookup"><span data-stu-id="56955-534">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="56955-535">[Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do klasy `LifetimeEventsHostedService` (implementacja <xref:Microsoft.Extensions.Hosting.IHostedService>), aby zarejestrować zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="56955-535">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="56955-536">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="56955-536">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="56955-537"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56955-537"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="56955-538">Następująca Klasa używa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, aby bezpiecznie zamknąć aplikację w przypadku wywołania metody `Shutdown` klasy:</span><span class="sxs-lookup"><span data-stu-id="56955-538">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="56955-539">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="56955-539">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
