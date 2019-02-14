---
title: Kontroli kondycji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować kontrole kondycji infrastruktury platformy ASP.NET Core, takich jak aplikacje i bazy danych.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: e186a3cb484035199a8f355540c3e985db87ad98
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248578"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="a9d5c-103">Kontroli kondycji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9d5c-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="a9d5c-104">Przez [Luke Latham](https://github.com/guardrex) i [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="a9d5c-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="a9d5c-105">Platforma ASP.NET Core oferuje kondycji Sprawdź oprogramowanie pośredniczące i biblioteki do raportowania kondycji składników infrastruktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="a9d5c-106">Kontrole kondycji są udostępniane przez aplikację jako punktów końcowych HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="a9d5c-107">Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="a9d5c-108">Sondy kondycji mogą być używane przez koordynatorów kontenerów i obciążenia równoważenia w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="a9d5c-109">Na przykład orkiestrator kontenerów mogą odpowiadać na kondycji niepowodzenie sprawdzenia przez zatrzymywanie stopniowego wdrażania lub ponownego uruchamiania kontenera.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="a9d5c-110">Moduł równoważenia obciążenia może reagować na złej kondycji aplikacji, kierując ruch od wystąpienia przechodzenia do wystąpienia dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="a9d5c-111">Można monitorować użycie pamięci, dysku i inne zasoby serwera fizycznego do stanu dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="a9d5c-112">Kontrole kondycji można przetestować zależności aplikacji, takich jak bazy danych i punktów końcowych usługi zewnętrznej, aby upewnić się, dostępność i normalnego funkcjonowania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="a9d5c-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9d5c-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a9d5c-114">Przykładowa aplikacja zawiera przykładowe scenariusze opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="a9d5c-115">Aby uruchomić przykładową aplikację w danej sytuacji, należy użyć [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia z folderu projektu w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="a9d5c-116">Zobacz przykładową aplikację *README.md* plików i opisy scenariuszy, w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9d5c-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a9d5c-117">Prerequisites</span></span>

<span data-ttu-id="a9d5c-118">Kontrole kondycji są zwykle używane z zewnętrznego monitorowania usługi lub kontener programu orchestrator do sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="a9d5c-119">Przed dodaniem kontrole kondycji do aplikacji, zdecyduj, na które monitorowania systemowi operacyjnemu używanie.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="a9d5c-120">System monitorowania decyduje o jakie kontrole kondycji do tworzenia i konfigurowania ich punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="a9d5c-121">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) pakietu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="a9d5c-122">Przykładowa aplikacja zawiera kod uruchamiający wykazanie, że kontrole kondycji dla kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="a9d5c-123">[Sondowanie bazy danych](#database-probe) scenariusz sprawdza kondycję połączenia bazy danych przy użyciu [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-123">The [database probe](#database-probe) scenario checks the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="a9d5c-124">[Sondy DbContext](#entity-framework-core-dbcontext-probe) scenariusz sprawdza bazę danych przy użyciu programu EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="a9d5c-125">Aby zapoznać się w scenariuszach bazy danych dla przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="a9d5c-126">Tworzy bazę danych i zapewnia jego parametry połączenia w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="a9d5c-127">Ma następujące odwołania do pakietu w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="a9d5c-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="a9d5c-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="a9d5c-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="a9d5c-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="a9d5c-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="a9d5c-131">Inny scenariusz sprawdzania kondycji pokazuje sposób filtrowania kontrole kondycji do portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="a9d5c-132">Przykładowa aplikacja wymaga utworzenia *Properties/launchSettings.json* pliku, który zawiera adres URL zarządzania i port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="a9d5c-133">Aby uzyskać więcej informacji, zobacz [filtru według portów](#filter-by-port) sekcji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="a9d5c-134">Sonda kondycji podstawowe</span><span class="sxs-lookup"><span data-stu-id="a9d5c-134">Basic health probe</span></span>

<span data-ttu-id="a9d5c-135">W przypadku wielu aplikacji konfiguracji sondy kondycji podstawowe raporty dostępności aplikacji do przetwarzania żądań (*żywotności*) jest wystarczająca dowiedzieć się, stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="a9d5c-136">Podstawową konfigurację rejestruje usługi sprawdzania kondycji i wywołuje kondycji Sprawdź oprogramowania pośredniczącego, aby odpowiedzieć na punkt końcowy adres URL odpowiedzi kondycji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="a9d5c-137">Domyślnie żadne testy kondycji konkretnego są rejestrowane do testowania dowolnej określonej zależności lub podsystemu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="a9d5c-138">Aplikacja jest traktowany jako w dobrej kondycji, jeśli jest w stanie odpowiadać na adres URL punktu końcowego kondycji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="a9d5c-139">Moduł zapisujący odpowiedzi domyślnej zapisuje stan (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) jako zwykły tekst odpowiedź z powrotem do klienta, wskazując [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) lub [ HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) stanu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-139">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="a9d5c-140">Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-140">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9d5c-141">Dodaj oprogramowanie pośredniczące sprawdzanie kondycji za pomocą <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania żądań `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-141">Add Health Check Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="a9d5c-142">W przykładowej aplikacji, punkt końcowy kontroli kondycji jest tworzony w `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="a9d5c-143">Aby uruchomić scenariusz konfiguracji podstawowej, przy użyciu przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="a9d5c-144">Przykład platformy docker</span><span class="sxs-lookup"><span data-stu-id="a9d5c-144">Docker example</span></span>

<span data-ttu-id="a9d5c-145">[Docker](xref:host-and-deploy/docker/index) oferuje wbudowaną `HEALTHCHECK` dyrektywę, który może służyć do sprawdzania stanu aplikacji, która używa konfiguracji kontroli kondycji podstawowych:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="a9d5c-146">Utwórz kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-146">Create health checks</span></span>

<span data-ttu-id="a9d5c-147">Kontrole kondycji, są tworzone przez zaimplementowanie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-147">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="a9d5c-148"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> Metoda zwraca `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` oznacza kondycji jako `Healthy`, `Degraded`, lub `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-148">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="a9d5c-149">Wynik jest napisany w postaci zwykłego tekstu odpowiedzi z kodem stanu można konfigurować (Konfiguracja jest opisana w [opcje sprawdzania kondycji](#health-check-options) sekcji).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="a9d5c-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> może również zwracać opcjonalne pary klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="a9d5c-151">Przykład kontrola kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-151">Example health check</span></span>

<span data-ttu-id="a9d5c-152">Następujące `ExampleHealthCheck` klasy pokazuje układ sprawdzenie kondycji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context, 
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a><span data-ttu-id="a9d5c-153">Rejestrowanie usługi sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-153">Register health check services</span></span>

<span data-ttu-id="a9d5c-154">`ExampleHealthCheck` Typu jest dodawany do usług sprawdzanie kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-154">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="a9d5c-155"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> Przeciążenia pokazano w poniższym przykładzie ustawia stan błędu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) do raportu, gdy kontrola kondycji zgłosi błąd.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-155">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="a9d5c-156">Jeśli ustawiono stan niepowodzenia `null` (ustawienie domyślne), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) są zgłaszane.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-156">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="a9d5c-157">To przeciążenie jest użytecznego scenariusza autorów biblioteki, gdzie stan Niepowodzenie, wskazywanym przez bibliotekę jest wymuszana przez aplikację po wystąpieniu błędu sprawdzenia kondycji, jeśli implementacja sprawdzania kondycji honoruje ustawienie.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="a9d5c-158">*Tagi* może służyć do filtrowania kontrole kondycji (dokładniejszym opisem zawartym w [filtrowania kontrole kondycji](#filter-health-checks) sekcji).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

<span data-ttu-id="a9d5c-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> można również wykonywać funkcję lambda.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="a9d5c-160">W poniższym przykładzie nazwa sprawdzenia kondycji jest określony jako `Example` i wyboru zawsze zwraca dobrej kondycji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="a9d5c-161">Użyj oprogramowania pośredniczącego kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="a9d5c-162">W `Startup.Configure`, wywołaj <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania przy użyciu adresu URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-162">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="a9d5c-163">Jeśli kontrole kondycji powinna nasłuchiwać na określonym porcie, użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> można ustawić port (dokładniejszym opisem zawartym w [filtru według portów](#filter-by-port) sekcji):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-163">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="a9d5c-164">Oprogramowanie pośredniczące sprawdza, czy w kondycji jest *terminalu oprogramowania pośredniczącego* w potoku przetwarzania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="a9d5c-165">Pierwszy kondycji Sprawdź punkt końcowy napotkano dokładnie dopasowany do adresu URL żądania wykonuje i short-circuits pozostałego potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="a9d5c-166">Po wystąpieniu zwarcie wykonuje nie oprogramowanie pośredniczące po kontroli kondycji dopasowany.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="a9d5c-167">Opcje sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-167">Health check options</span></span>

<span data-ttu-id="a9d5c-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> zapewniają możliwość dostosowywania zachowania wyboru kondycji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="a9d5c-169">Filtruj kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="a9d5c-170">Dostosowywanie kod stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="a9d5c-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="a9d5c-171">Pomiń nagłówki cache</span><span class="sxs-lookup"><span data-stu-id="a9d5c-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="a9d5c-172">Dostosowywanie danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="a9d5c-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="a9d5c-173">Filtruj kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-173">Filter health checks</span></span>

<span data-ttu-id="a9d5c-174">Domyślnie Sprawdź kondycję w oprogramowaniu pośredniczącym uruchamia wszystkie kontrole kondycji zarejestrowanego.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="a9d5c-175">Aby uruchomić podzbiór kontroli kondycji, zapewnia funkcja, która zwraca wartość typu boolean do <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> opcji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-175">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="a9d5c-176">W poniższym przykładzie `Bar` sprawdzenie kondycji jest odfiltrowana po jego znaczniku (`bar_tag`) w funkcji instrukcji warunkowej, gdzie `true` jest zwracany tylko wtedy, jeśli sprawdzenie kondycji <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> dopasowania właściwości `foo_tag` lub `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () => 
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () => 
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () => 
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") || 
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="a9d5c-177">Dostosowywanie kod stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="a9d5c-177">Customize the HTTP status code</span></span>

<span data-ttu-id="a9d5c-178">Użyj <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> dostosować mapowanie stanu kondycji, aby kody stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-178">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="a9d5c-179">Następujące <xref:Microsoft.AspNetCore.Http.StatusCodes> przypisania są wartości domyślne używane przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-179">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="a9d5c-180">Zmień wartości Kod stanu, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-180">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthStatus properties.
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="a9d5c-181">Pomiń nagłówki cache</span><span class="sxs-lookup"><span data-stu-id="a9d5c-181">Suppress cache headers</span></span>

<span data-ttu-id="a9d5c-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> Określa, czy oprogramowanie pośredniczące sprawdzanie kondycji dodaje nagłówki HTTP do odpowiedzi sondowania, aby zapobiec buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="a9d5c-183">Jeśli wartość jest `false` (ustawienie domyślne), oprogramowanie pośredniczące Ustawia lub zastępuje `Cache-Control`, `Expires`, i `Pragma` nagłówki, aby zapobiec buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="a9d5c-184">Jeśli wartość jest `true`, oprogramowanie pośredniczące nie modyfikuje nagłówki cache odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="a9d5c-185">Dostosowywanie danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="a9d5c-185">Customize output</span></span>

<span data-ttu-id="a9d5c-186"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> Opcja pobiera lub ustawia delegata używany do zapisywania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-186">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="a9d5c-187">Delegat domyślne zapisuje odpowiedź minimalnej w postaci zwykłego tekstu z wartością ciągu [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-187">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext, 
    HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

## <a name="database-probe"></a><span data-ttu-id="a9d5c-188">Sondowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="a9d5c-188">Database probe</span></span>

<span data-ttu-id="a9d5c-189">Sprawdzenie kondycji, można określić zapytanie bazy danych, aby był uruchamiany jako testu logicznego, aby wskazać, czy baza danych odpowiada normalnie.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="a9d5c-190">Ta aplikacja używa przykładowych [BeatPulse](https://github.com/Xabaril/BeatPulse), biblioteka sprawdzania kondycji dla aplikacji platformy ASP.NET Core, można sprawdzić kondycji bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-190">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="a9d5c-191">Wykonuje BeatPulse `SELECT 1` zapytania względem bazy danych, aby upewnić się, połączenie z bazą danych jest w dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-191">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="a9d5c-192">Podczas sprawdzania połączenia z bazą danych przy użyciu zapytania, wybierz zapytanie zwracające szybko.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="a9d5c-193">Podejście zapytania jest zagrożona przeciążenia bazy danych i zmniejszenie jego wydajności.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="a9d5c-194">W większości przypadków Uruchamianie zapytania testu nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="a9d5c-195">Wystarczy jedynie wprowadzania udane połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="a9d5c-196">Jeśli możesz znaleźć niezbędne do uruchamiania kwerendy, wybierz prostego zapytania typu SELECT, taki jak `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="a9d5c-197">Aby można było używać biblioteki BeatPulse, zawierają odwołania do pakietu do [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-197">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="a9d5c-198">Podaj prawidłową bazą danych parametry połączenia w *appsettings.json* pliku przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="a9d5c-199">Aplikacja korzysta z bazy danych programu SQL Server o nazwie `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="a9d5c-200">Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-200">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9d5c-201">Przykładowa aplikacja wywołuje firmy BeatPulse `AddSqlServer` metody przy użyciu parametrów połączenia bazy danych (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-201">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="a9d5c-202">Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="a9d5c-203">Aby uruchomić scenariusza sondowanie bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="a9d5c-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="a9d5c-205">Entity Framework Core DbContext sondy</span><span class="sxs-lookup"><span data-stu-id="a9d5c-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="a9d5c-206">`DbContext` Wyboru potwierdza, że aplikacja może komunikować się z bazą danych skonfigurowane dla platformy EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="a9d5c-207">`DbContext` Wyboru jest obsługiwane w aplikacjach który:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="a9d5c-208">Użyj [platformy Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="a9d5c-209">Odwołanie do pakietu [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="a9d5c-210">`AddDbContextCheck<TContext>` rejestruje sprawdzenia kondycji `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="a9d5c-211">`DbContext` Jest dostarczany jako `TContext` do metody.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="a9d5c-212">Przeciążenie jest dostępna do konfigurowania stanu awarii, tagi i zapytań niestandardowych testu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="a9d5c-213">Domyślnie:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-213">By default:</span></span>

* <span data-ttu-id="a9d5c-214">`DbContextHealthCheck` Wywołuje programu EF Core `CanConnectAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="a9d5c-215">Można dostosować, jakie działanie jest uruchamiane podczas sprawdzania kondycji za pomocą `AddDbContextCheck` przeciążenia metody.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="a9d5c-216">Nazwa kontroli kondycji jest nazwą `TContext` typu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="a9d5c-217">W przykładowej aplikacji `AppDbContext` jest dostarczany w celu `AddDbContextCheck` i zarejestrowany jako usługa w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="a9d5c-218">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="a9d5c-219">W przykładowej aplikacji `UseHealthChecks` dodaje pośredniczącym sprawdzanie kondycji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="a9d5c-220">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="a9d5c-221">Aby uruchomić `DbContext` sondowania scenariusz przy użyciu przykładowej aplikacji, upewnij się, że baza danych określona przez ciąg połączenia nie istnieje w wystąpieniu programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="a9d5c-222">Jeśli baza danych istnieje, należy go usunąć.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-222">If the database exists, delete it.</span></span>

<span data-ttu-id="a9d5c-223">Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="a9d5c-224">Po uruchomieniu aplikacji Sprawdź stan kondycji, kieruje żądanie do `/health` punktu końcowego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="a9d5c-225">Bazy danych i `AppDbContext` nie istnieje, więc aplikacja zapewnia następującą odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="a9d5c-226">Wyzwalanie przykładową aplikację w celu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="a9d5c-227">Zgłosić wniosek o `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="a9d5c-228">Odpowiedź aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="a9d5c-229">Zgłosić wniosek o `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="a9d5c-230">Bazy danych i kontekstu istnieje, więc aplikacja reaguje:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="a9d5c-231">Wyzwalanie przykładową aplikację w celu usunięcia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="a9d5c-232">Zgłosić wniosek o `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="a9d5c-233">Odpowiedź aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="a9d5c-234">Zgłosić wniosek o `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="a9d5c-235">Aplikacja zawiera odpowiedź złej kondycji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="a9d5c-236">Oddzielne gotowości i sondy żywotności</span><span class="sxs-lookup"><span data-stu-id="a9d5c-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="a9d5c-237">W niektórych scenariuszach hostingu parę kontroli kondycji są używane, rozróżnienie dwóch stanów aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="a9d5c-238">Aplikacja jest działa, ale nie jest jeszcze gotowy do odbierania żądań.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="a9d5c-239">Ten stan jest to aplikacja *gotowości*.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="a9d5c-240">Działa i odpowiada na żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="a9d5c-241">Ten stan jest to aplikacja *żywotności*.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="a9d5c-242">Sprawdzanie gotowości zwykle wykonuje bardziej rozbudowanym i czasochłonne zestawu testów, aby sprawdzić, czy wszystkie podsystemy aplikacji i zasoby są dostępne.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="a9d5c-243">Sprawdzanie żywotności jedynie wykonuje szybkie sprawdzenie w celu określenia, czy aplikacja jest dostępna do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="a9d5c-244">Po jej przekazuje jej sprawdzanie gotowości, nie ma potrzeby do obciążać aplikacji dalsze kosztowne zestaw gotowości&mdash;dalsze procesy kontroli wymagają tylko sprawdzanie żywotności.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="a9d5c-245">Przykładowa aplikacja zawiera sprawdzenie kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [usługi hostowanej](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="a9d5c-246">`StartupHostedServiceHealthCheck` Ujawnia właściwość `StartupTaskCompleted`, które można ustawić usługi hostowanej `true` po zakończeniu jego długotrwałe zadanie (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a9d5c-247">Długotrwałe zadanie w tle jest uruchamiana przez [usługi hostowanej](xref:fundamentals/host/hosted-services) (*usług/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="a9d5c-248">Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` ustawiono `true`:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="a9d5c-249">Sprawdzenie kondycji jest zarejestrowane w usłudze <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices` wraz z hostowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-249">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="a9d5c-250">Ponieważ usługa hostowana należy ustawić właściwość na sprawdzenie kondycji, sprawdzenie kondycji również jest zarejestrowany w kontenerze usługi (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="a9d5c-251">Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="a9d5c-252">W przykładowej aplikacji z punktami końcowymi kontroli kondycji są tworzone na `/health/ready` sprawdzenia gotowości i `/health/live` sprawdzania żywotności.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="a9d5c-253">Filtry sprawdzania gotowości kondycji sprawdza kondycję skontaktuj się z `ready` tagu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="a9d5c-254">Odfiltrowuje wyboru żywotności `StartupHostedServiceHealthCheck` , zwracając `false` w [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (Aby uzyskać więcej informacji, zobacz [filtrowania kontrole kondycji](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="a9d5c-255">Aby uruchomić gotowości/żywotności scenariusz konfiguracji, za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="a9d5c-256">W przeglądarce, odwiedź stronę `/health/ready` kilka razy, aż przeszły 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="a9d5c-257">Sprawdzanie kondycji raporty *zła* przez pierwsze 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-257">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="a9d5c-258">Po 15 sekundach raporty punktu końcowego *dobra kondycja*, co odzwierciedla ukończenie długotrwałego zadania w usłudze hostowanej.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-258">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="a9d5c-259">W tym przykładzie tworzy również usługi kondycji Sprawdź wydawcy (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementacji), które jest uruchamiane pierwszego sprawdzania gotowości za pomocą dwóch półsekundowym opóźnieniu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-259">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="a9d5c-260">Aby uzyskać więcej informacji, zobacz [wydawcy sprawdzanie kondycji](#health-check-publisher) sekcji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-260">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="a9d5c-261">Przykład rozwiązania Kubernetes</span><span class="sxs-lookup"><span data-stu-id="a9d5c-261">Kubernetes example</span></span>

<span data-ttu-id="a9d5c-262">Przy użyciu oddzielnych kontroli gotowości i żywotności przydaje się w środowisku, takie jak [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-262">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="a9d5c-263">W usłudze Kubernetes aplikacja może być wymagane do wykonywania pracy czasochłonne uruchamiania przed zaakceptowaniem żądania, takie jak testu dostępności podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-263">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="a9d5c-264">Przy użyciu oddzielnych kontroli umożliwia programu orchestrator w celu odróżnienia, czy aplikacja działa, ale nie jest jeszcze gotowy, lub jeśli aplikacja nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-264">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="a9d5c-265">Aby uzyskać więcej informacji o gotowości i sondy żywotności w usłudze Kubernetes, zobacz [skonfigurować żywotności i sondy gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji platformy Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-265">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="a9d5c-266">W poniższym przykładzie pokazano konfigurację sondowania gotowości Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-266">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="a9d5c-267">Opartą na metryce sondowania przy użyciu składnika zapisywania niestandardowych</span><span class="sxs-lookup"><span data-stu-id="a9d5c-267">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="a9d5c-268">Przykładowa aplikacja pokazuje sprawdzenie kondycji pamięci za pomocą składnika zapisywania niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-268">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="a9d5c-269">`MemoryHealthCheck` Raporty Stan obniżonej wydajności, jeśli aplikacja korzysta z więcej niż wartość progową pamięci (1 GB w przykładowej aplikacji).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-269">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="a9d5c-270"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> Zawiera informacje moduł wyrzucania elementów bezużytecznych (GC) dla aplikacji (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-270">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="a9d5c-271">Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-271">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9d5c-272">Zamiast włączania kondycji Sprawdź przez przekazanie jej do <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` jest zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-272">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="a9d5c-273">Wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> zarejestrowane usługi są dostępne dla usługi sprawdzania kondycji i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-273">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="a9d5c-274">Firma Microsoft zaleca rejestrowanie usługi sprawdzania kondycji jako pojedynczego wystąpienia usługi.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-274">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="a9d5c-275">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-275">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="a9d5c-276">Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-276">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="a9d5c-277">A `WriteResponse` delegata jest dostarczany w celu `ResponseWriter` właściwość niestandardową odpowiedź w formacie JSON dane wyjściowe, gdy wykonuje sprawdzenie kondycji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-277">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="a9d5c-278">`WriteResponse` Formaty metoda `CompositeHealthCheckResult` do postaci JSON obiektu i zwraca dane wyjściowe JSON dla odpowiedzi z kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-278">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="a9d5c-279">Aby uruchomić funkcję badania opartą na metryce z danymi wyjściowymi modułu zapisującego niestandardowe odpowiedzi za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-279">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="a9d5c-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) obejmuje opartą na metryce kondycji sprawdzenie scenariuszy, w tym dysku magazynu i maksymalną wartość żywotności kontroli.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="a9d5c-281">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-281">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="a9d5c-282">Filtruj według portów</span><span class="sxs-lookup"><span data-stu-id="a9d5c-282">Filter by port</span></span>

<span data-ttu-id="a9d5c-283">Wywoływanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> z portem ogranicza żądania sprawdzenia kondycji do określonego portu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-283">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="a9d5c-284">Zazwyczaj służy w środowisku kontenera do udostępniania portów do monitorowania usługi.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-284">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="a9d5c-285">Przykładowa aplikacja umożliwia skonfigurowanie portu przy użyciu [zmiennej dostawcę konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-285">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="a9d5c-286">Numer portu jest ustawiana w *launchSettings.json* pliku i przekazywane do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-286">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="a9d5c-287">Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-287">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="a9d5c-288">Przykładowa aplikacja jest używana do zademonstrowania konfiguracji portów zarządzania, należy utworzyć *launchSettings.json* w pliku *właściwości* folderu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-288">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="a9d5c-289">Następujące *launchSettings.json* plików nie jest zawarty w plikach projektu przykładowej aplikacji i należy utworzyć je ręcznie.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-289">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="a9d5c-290">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-290">*Properties/launchSettings.json*:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="a9d5c-291">Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-291">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9d5c-292">Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> Określa port usługi zarządzania (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="a9d5c-292">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="a9d5c-293">Możesz uniknąć tworzenia *launchSettings.json* pliku w przykładowej aplikacji przez ustawianie adresów URL i port zarządzania jawnie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-293">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="a9d5c-294">W *Program.cs* gdzie <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> jest utworzony, dodaj wywołanie <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> i podaj punktu końcowego normalne odpowiedzi aplikacji oraz port punktu końcowego zarządzania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-294">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="a9d5c-295">W *ManagementPortStartup.cs* gdzie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> jest wywoływana, jawnie określ port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-295">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="a9d5c-296">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-296">*Program.cs*:</span></span>
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> <span data-ttu-id="a9d5c-297">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-297">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="a9d5c-298">Aby uruchomić scenariusz konfiguracji portów zarządzania za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-298">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="a9d5c-299">Dystrybucja Biblioteka sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-299">Distribute a health check library</span></span>

<span data-ttu-id="a9d5c-300">Aby rozesłać sprawdzenie kondycji jako biblioteki:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-300">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="a9d5c-301">Sprawdzenie kondycji, który implementuje zapisu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejs jako autonomiczną klasę.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-301">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="a9d5c-302">Klasa może polegać na [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection), typ aktywacji i [o nazwie opcje](xref:fundamentals/configuration/options) do dostępu do danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-302">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. <span data-ttu-id="a9d5c-303">Zapisywanie metody rozszerzenia z parametrami, które aplikacja odbierająca komunikaty wywołania w jego `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-303">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="a9d5c-304">W poniższym przykładzie przyjęto założenie, następujący podpis metody sprawdzania kondycji:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-304">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="a9d5c-305">Poprzedni podpisu wskazuje, że `ExampleHealthCheck` wymaga dodatkowe dane do procesu kondycji Sprawdź logikę sondowania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-305">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="a9d5c-306">Dane są udostępniane do delegata, użyty do utworzenia wystąpienia sprawdzenie kondycji po zarejestrowaniu sprawdzenie kondycji za pomocą metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-306">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="a9d5c-307">W poniższym przykładzie obiekt wywołujący określa opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-307">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="a9d5c-308">Nazwa sprawdzenia kondycji (`name`).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-308">health check name (`name`).</span></span> <span data-ttu-id="a9d5c-309">Jeśli `null`, `example_health_check` jest używany.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-309">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="a9d5c-310">Ciąg punktu danych dla kontroli kondycji (`data1`).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-310">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="a9d5c-311">punkt danych liczby całkowitej dla kontroli kondycji (`data2`).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-311">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="a9d5c-312">Jeśli `null`, `1` jest używany.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-312">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="a9d5c-313">Stan błędu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-313">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="a9d5c-314">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-314">The default is `null`.</span></span> <span data-ttu-id="a9d5c-315">Jeśli `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) jest zgłaszany stan Niepowodzenie.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-315">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="a9d5c-316">tagi (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-316">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder, 
           string name = default, 
           string data1, 
           int data2 = 1, 
           HealthStatus? failureStatus = default, 
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="a9d5c-317">Wydawca sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="a9d5c-317">Health Check Publisher</span></span>

<span data-ttu-id="a9d5c-318">Gdy <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> zostanie dodany do kontenera usługi systemu sprawdzanie kondycji wykonuje okresowo z kontroli kondycji i wywołania `PublishAsync` z wynikiem.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-318">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="a9d5c-319">Jest to przydatne w scenariuszu systemu, który oczekuje, że każdy proces, aby wywołać system monitorowania okresowo w celu określenia kondycji monitorowania kondycji na podstawie wypychania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-319">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="a9d5c-320"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> Interfejs zawiera jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-320">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="a9d5c-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> pozwala na ustawienie:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="a9d5c-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; Opóźnienie początkowej zastosowane po aplikacji, który rozpoczyna się przed wykonaniem <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="a9d5c-323">Opóźnienie jest stosowana raz przy uruchamianiu i nie ma zastosowania do kolejnych iteracjach.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-323">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="a9d5c-324">Wartość domyślna to 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-324">The default value is five seconds.</span></span>
* <span data-ttu-id="a9d5c-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; Okres <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="a9d5c-326">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-326">The default value is 30 seconds.</span></span>
* <span data-ttu-id="a9d5c-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Jeśli <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> jest `null` (ustawienie domyślne), wszystkie kontrole kondycji zarejestrowanego działa usługa wydawcy sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="a9d5c-328">Aby uruchomić podzbiór kontroli kondycji, należy podać funkcji filtrującej zestawu testów.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-328">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="a9d5c-329">Predykat obliczone każdego okresu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-329">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="a9d5c-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; Limit czasu wykonywania kondycji sprawdza wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="a9d5c-331">Użyj <xref:System.Threading.Timeout.InfiniteTimeSpan> można wykonać bez limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-331">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="a9d5c-332">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-332">The default value is 30 seconds.</span></span>

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> <span data-ttu-id="a9d5c-333">W wersji platformy ASP.NET Core 2.2 ustawienie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> nie są uznawane przez <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wdrożenia; ustawia wartość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-333">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="a9d5c-334">Ten problem zostanie rozwiązany w programie ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-334">This issue will be fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="a9d5c-335">Aby uzyskać więcej informacji, zobacz [HealthCheckPublisherOptions.Period ustawia wartość. Opóźnienie](https://github.com/aspnet/Extensions/issues/1041).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-335">For more information, see [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041).</span></span>

::: moniker-end

<span data-ttu-id="a9d5c-336">W przykładowej aplikacji `ReadinessPublisher` jest <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-336">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="a9d5c-337">Sprawdź stan kondycji jest rejestrowana w `Entries` i rejestrowane przy każdym sprawdzaniu:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-337">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="a9d5c-338">Przykładowa aplikacja `LivenessProbeStartup` przykład `StartupHostedService` sprawdzanie gotowości dwóch półsekundowym opóźnieniu uruchamiania i uruchamia kontrolę co 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-338">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="a9d5c-339">Aby aktywować <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> rejestruje przykład implementacji `ReadinessPublisher` jako pojedyncza usługa w [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera:</span><span class="sxs-lookup"><span data-stu-id="a9d5c-339">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="a9d5c-340">Poniższe obejście pozwala na dodawanie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpienie kontenera usługi, gdy jeden lub więcej innych usług hostowanych zostały już dodane do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-340">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="a9d5c-341">To rozwiązanie nie będzie wymagane wraz z wydaniem programu ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-341">This workaround won't be required with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="a9d5c-342">Aby uzyskać więcej informacji, zobacz: https://github.com/aspnet/Extensions/issues/639.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-342">For more information, see: https://github.com/aspnet/Extensions/issues/639.</span></span>
>
> ```csharp
> private const string HealthCheckServiceAssembly = 
>     "Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherHostedService";
>
> services.TryAddEnumerable(
>     ServiceDescriptor.Singleton(typeof(IHostedService), 
>         typeof(HealthCheckPublisherOptions).Assembly
>             .GetType(HealthCheckServiceAssembly)));
> ```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="a9d5c-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) obejmuje wydawcy dla kilku systemów, w tym [usługi Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="a9d5c-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="a9d5c-344">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a9d5c-344">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
