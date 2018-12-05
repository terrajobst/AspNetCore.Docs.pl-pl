---
title: Kontroli kondycji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować kontrole kondycji infrastruktury platformy ASP.NET Core, takich jak aplikacje i bazy danych.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2018
uid: host-and-deploy/health-checks
ms.openlocfilehash: d8fd43d9d689396cf30ca371763cdf7ac9423c77
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862769"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="274e7-103">Kontroli kondycji w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="274e7-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="274e7-104">Przez [Luke Latham](https://github.com/guardrex) i [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="274e7-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="274e7-105">Platforma ASP.NET Core oferuje kondycji Sprawdź oprogramowanie pośredniczące i biblioteki do raportowania kondycji składników infrastruktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="274e7-106">Kontrole kondycji są udostępniane przez aplikację jako punktów końcowych HTTP.</span><span class="sxs-lookup"><span data-stu-id="274e7-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="274e7-107">Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="274e7-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="274e7-108">Sondy kondycji mogą być używane przez koordynatorów kontenerów i obciążenia równoważenia w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="274e7-109">Na przykład orkiestrator kontenerów mogą odpowiadać na kondycji niepowodzenie sprawdzenia przez zatrzymywanie stopniowego wdrażania lub ponownego uruchamiania kontenera.</span><span class="sxs-lookup"><span data-stu-id="274e7-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="274e7-110">Moduł równoważenia obciążenia może reagować na złej kondycji aplikacji, kierując ruch od wystąpienia przechodzenia do wystąpienia dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="274e7-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="274e7-111">Można monitorować użycie pamięci, dysku i inne zasoby serwera fizycznego do stanu dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="274e7-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="274e7-112">Kontrole kondycji można przetestować zależności aplikacji, takich jak bazy danych i punktów końcowych usługi zewnętrznej, aby upewnić się, dostępność i normalnego funkcjonowania.</span><span class="sxs-lookup"><span data-stu-id="274e7-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="274e7-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="274e7-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="274e7-114">Przykładowa aplikacja zawiera przykładowe scenariusze opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="274e7-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="274e7-115">Aby uruchomić przykładową aplikację w danej sytuacji, należy użyć [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia z folderu projektu w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="274e7-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="274e7-116">Zobacz przykładową aplikację *README.md* plików i opisy scenariuszy, w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="274e7-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="274e7-117">Prerequisites</span></span>

<span data-ttu-id="274e7-118">Kontrole kondycji są zwykle używane z zewnętrznego monitorowania usługi lub kontener programu orchestrator do sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="274e7-119">Przed dodaniem kontrole kondycji do aplikacji, zdecyduj, na które monitorowania systemowi operacyjnemu używanie.</span><span class="sxs-lookup"><span data-stu-id="274e7-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="274e7-120">System monitorowania decyduje o jakie kontrole kondycji do tworzenia i konfigurowania ich punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="274e7-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="274e7-121">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) pakietu.</span><span class="sxs-lookup"><span data-stu-id="274e7-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="274e7-122">Przykładowa aplikacja zawiera kod startowy wykazanie, że kontrole kondycji dla kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="274e7-122">The sample app provides start-up code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="274e7-123">[Sondowanie bazy danych](#database-probe) scenariusza sondy kondycji połączenia bazy danych przy użyciu [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="274e7-123">The [database probe](#database-probe) scenario probes the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="274e7-124">[Sondy DbContext](#entity-framework-core-dbcontext-probe) scenariusza sondy bazę danych przy użyciu programu EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="274e7-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario probes a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="274e7-125">Aby zapoznać się z scenariusze bazy danych za pomocą przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="274e7-125">To explore the database scenarios using the sample app:</span></span>

* <span data-ttu-id="274e7-126">Tworzenie bazy danych oraz zapewnienia jego parametry połączenia w *appsettings.json* pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-126">Create a database and provide its connection string in the *appsettings.json* file of the app.</span></span>
* <span data-ttu-id="274e7-127">Dodaj odwołanie do pakietu [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="274e7-127">Add a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

> [!NOTE]
> <span data-ttu-id="274e7-128">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="274e7-128">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="274e7-129">Inny scenariusz sprawdzania kondycji pokazuje sposób filtrowania kontrole kondycji do portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="274e7-129">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="274e7-130">Przykładowa aplikacja wymaga utworzenia *Properties/launchSettings.json* pliku, który zawiera adres URL zarządzania i port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="274e7-130">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="274e7-131">Aby uzyskać więcej informacji, zobacz [filtru według portów](#filter-by-port) sekcji.</span><span class="sxs-lookup"><span data-stu-id="274e7-131">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="274e7-132">Sonda kondycji podstawowe</span><span class="sxs-lookup"><span data-stu-id="274e7-132">Basic health probe</span></span>

<span data-ttu-id="274e7-133">W przypadku wielu aplikacji konfiguracji sondy kondycji podstawowe raporty dostępności aplikacji do przetwarzania żądań (*żywotności*) jest wystarczająca dowiedzieć się, stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-133">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="274e7-134">Podstawową konfigurację rejestruje usługi sprawdzania kondycji i wywołuje kondycji Sprawdź oprogramowania pośredniczącego, aby odpowiedzieć na punkt końcowy adres URL odpowiedzi kondycji.</span><span class="sxs-lookup"><span data-stu-id="274e7-134">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="274e7-135">Domyślnie żadne testy kondycji konkretnego są rejestrowane do testowania dowolnej określonej zależności lub podsystemu.</span><span class="sxs-lookup"><span data-stu-id="274e7-135">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="274e7-136">Aplikacja jest traktowany jako w dobrej kondycji, jeśli jest w stanie odpowiadać na adres URL punktu końcowego kondycji.</span><span class="sxs-lookup"><span data-stu-id="274e7-136">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="274e7-137">Moduł zapisujący odpowiedzi domyślnej zapisuje stan (`HealthCheckStatus`) jako zwykły tekst odpowiedź z powrotem do klienta, wskazując `HealthCheckResult.Healthy` lub `HealthCheckResult.Unhealthy` stanu.</span><span class="sxs-lookup"><span data-stu-id="274e7-137">The default response writer writes the status (`HealthCheckStatus`) as a plaintext response back to the client, indicating either a `HealthCheckResult.Healthy` or `HealthCheckResult.Unhealthy` status.</span></span>

<span data-ttu-id="274e7-138">Rejestrowanie usługi sprawdzania kondycji z `AddHealthChecks` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="274e7-138">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="274e7-139">Dodaj oprogramowanie pośredniczące sprawdzanie kondycji za pomocą `UseHealthChecks` w potoku przetwarzania żądań `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="274e7-139">Add Health Check Middleware with `UseHealthChecks` in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="274e7-140">W przykładowej aplikacji, punkt końcowy kontroli kondycji jest tworzony w `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="274e7-140">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="274e7-141">Aby uruchomić scenariusz konfiguracji podstawowej, przy użyciu przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="274e7-141">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="274e7-142">Przykład platformy docker</span><span class="sxs-lookup"><span data-stu-id="274e7-142">Docker example</span></span>

<span data-ttu-id="274e7-143">[Docker](xref:host-and-deploy/docker/index) oferuje wbudowaną `HEALTHCHECK` dyrektywę, który może służyć do sprawdzania stanu aplikacji, która używa konfiguracji kontroli kondycji podstawowych:</span><span class="sxs-lookup"><span data-stu-id="274e7-143">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="274e7-144">Utwórz kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-144">Create health checks</span></span>

<span data-ttu-id="274e7-145">Kontrole kondycji, są tworzone przez zaimplementowanie `IHealthCheck` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="274e7-145">Health checks are created by implementing the `IHealthCheck` interface.</span></span> <span data-ttu-id="274e7-146">`IHealthCheck.CheckHealthAsync` Metoda zwraca `Task<HealthCheckResult>` oznacza kondycji jako `Healthy`, `Degraded`, lub `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="274e7-146">The `IHealthCheck.CheckHealthAsync` method returns a `Task<HealthCheckResult>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="274e7-147">Wynik jest napisany w postaci zwykłego tekstu odpowiedzi z kodem stanu można konfigurować (Konfiguracja jest opisana w [opcje sprawdzania kondycji](#health-check-options) sekcji).</span><span class="sxs-lookup"><span data-stu-id="274e7-147">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="274e7-148">`HealthCheckResult` może również zwracać opcjonalne pary klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="274e7-148">`HealthCheckResult` can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="274e7-149">Przykład kontrola kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-149">Example health check</span></span>

<span data-ttu-id="274e7-150">Następujące `ExampleHealthCheck` klasy pokazuje układ sprawdzenie kondycji:</span><span class="sxs-lookup"><span data-stu-id="274e7-150">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="274e7-151">Rejestrowanie usługi sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-151">Register health check services</span></span>

<span data-ttu-id="274e7-152">`ExampleHealthCheck` Typu jest dodawany do usług sprawdzanie kondycji za pomocą `AddCheck`:</span><span class="sxs-lookup"><span data-stu-id="274e7-152">The `ExampleHealthCheck` type is added to health check services with `AddCheck`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="274e7-153">`AddCheck` Przeciążenia pokazano w poniższym przykładzie ustawia stan błędu (`HealthStatus`) do raportu, gdy kontrola kondycji zgłosi błąd.</span><span class="sxs-lookup"><span data-stu-id="274e7-153">The `AddCheck` overload shown in the following example sets the failure status (`HealthStatus`) to report when the health check reports a failure.</span></span> <span data-ttu-id="274e7-154">Jeśli ustawiono stan niepowodzenia `null` (ustawienie domyślne), `HealthStatus.Unhealthy` są zgłaszane.</span><span class="sxs-lookup"><span data-stu-id="274e7-154">If the failure status is set to `null` (default), `HealthStatus.Unhealthy` is reported.</span></span> <span data-ttu-id="274e7-155">To przeciążenie jest użytecznego scenariusza autorów biblioteki, gdzie stan Niepowodzenie, wskazywanym przez bibliotekę jest wymuszana przez aplikację po wystąpieniu błędu sprawdzenia kondycji, jeśli implementacja sprawdzania kondycji honoruje ustawienie.</span><span class="sxs-lookup"><span data-stu-id="274e7-155">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="274e7-156">*Tagi* może służyć do filtrowania kontrole kondycji (dokładniejszym opisem zawartym w [filtrowania kontrole kondycji](#filter-health-checks) sekcji).</span><span class="sxs-lookup"><span data-stu-id="274e7-156">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

<span data-ttu-id="274e7-157">`AddCheck` można również wykonywać funkcję lambda.</span><span class="sxs-lookup"><span data-stu-id="274e7-157">`AddCheck` can also execute a lambda function.</span></span> <span data-ttu-id="274e7-158">W poniższym przykładzie nazwa sprawdzenia kondycji jest określony jako `Example` i wyboru zawsze zwraca dobrej kondycji:</span><span class="sxs-lookup"><span data-stu-id="274e7-158">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="274e7-159">Użyj oprogramowania pośredniczącego kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-159">Use Health Checks Middleware</span></span>

<span data-ttu-id="274e7-160">W `Startup.Configure`, wywołaj `UseHealthChecks` w potoku przetwarzania przy użyciu adresu URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="274e7-160">In `Startup.Configure`, call `UseHealthChecks` in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="274e7-161">Jeśli kontrole kondycji powinna nasłuchiwać na określonym porcie, użyj przeciążenia `UseHealthChecks` można ustawić port (dokładniejszym opisem zawartym w [filtru według portów](#filter-by-port) sekcji):</span><span class="sxs-lookup"><span data-stu-id="274e7-161">If the health checks should listen on a specific port, use an overload of `UseHealthChecks` to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="274e7-162">Oprogramowanie pośredniczące sprawdza, czy w kondycji jest *terminalu oprogramowania pośredniczącego* w potoku przetwarzania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-162">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="274e7-163">Pierwszy kondycji Sprawdź punkt końcowy napotkano dokładnie dopasowany do adresu URL żądania wykonuje i short-circuits pozostałego potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="274e7-163">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="274e7-164">Po wystąpieniu zwarcie wykonuje nie oprogramowanie pośredniczące po kontroli kondycji dopasowany.</span><span class="sxs-lookup"><span data-stu-id="274e7-164">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="274e7-165">Opcje sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-165">Health check options</span></span>

<span data-ttu-id="274e7-166">`HealthCheckOptions` zapewniają możliwość dostosowywania zachowania wyboru kondycji:</span><span class="sxs-lookup"><span data-stu-id="274e7-166">`HealthCheckOptions` provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="274e7-167">Filtruj kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-167">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="274e7-168">Dostosowywanie kod stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="274e7-168">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="274e7-169">Pomiń nagłówki cache</span><span class="sxs-lookup"><span data-stu-id="274e7-169">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="274e7-170">Dostosowywanie danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="274e7-170">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="274e7-171">Filtruj kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-171">Filter health checks</span></span>

<span data-ttu-id="274e7-172">Domyślnie Sprawdź kondycję w oprogramowaniu pośredniczącym uruchamia wszystkie kontrole kondycji zarejestrowanego.</span><span class="sxs-lookup"><span data-stu-id="274e7-172">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="274e7-173">Aby uruchomić podzbiór kontroli kondycji, zapewnia funkcja, która zwraca wartość typu boolean do `Predicate` opcji.</span><span class="sxs-lookup"><span data-stu-id="274e7-173">To run a subset of health checks, provide a function that returns a boolean to the `Predicate` option.</span></span> <span data-ttu-id="274e7-174">W poniższym przykładzie `Bar` sprawdzenie kondycji jest odfiltrowana po jego znaczniku (`bar_tag`) w funkcji instrukcji warunkowej, gdzie `true` jest zwracany tylko wtedy, jeśli sprawdzenie kondycji `Tag` dopasowania właściwości `foo_tag` lub `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="274e7-174">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's `Tag` property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="274e7-175">Dostosowywanie kod stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="274e7-175">Customize the HTTP status code</span></span>

<span data-ttu-id="274e7-176">Użyj `ResultStatusCodes` dostosować mapowanie stanu kondycji, aby kody stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="274e7-176">Use `ResultStatusCodes` to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="274e7-177">Następujące `StatusCode` przypisania są wartości domyślne używane przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="274e7-177">The following `StatusCode` assignments are the default values used by the middleware.</span></span> <span data-ttu-id="274e7-178">Zmień wartości Kod stanu, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="274e7-178">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthCheckStatus properties.
        ResultStatusCodes =
        {
            [HealthCheckStatus.Healthy] = StatusCodes.Status200OK,
            [HealthCheckStatus.Degraded] = StatusCodes.Status200OK,
            [HealthCheckStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="274e7-179">Pomiń nagłówki cache</span><span class="sxs-lookup"><span data-stu-id="274e7-179">Suppress cache headers</span></span>

<span data-ttu-id="274e7-180">`AllowCachingResponses` Określa, czy oprogramowanie pośredniczące sprawdzanie kondycji dodaje nagłówki HTTP do odpowiedzi sondowania, aby zapobiec buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="274e7-180">`AllowCachingResponses` controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="274e7-181">Jeśli wartość jest `false` (ustawienie domyślne), oprogramowanie pośredniczące Ustawia lub zastępuje `Cache-Control`, `Expires`, i `Pragma` nagłówki, aby zapobiec buforowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="274e7-181">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="274e7-182">Jeśli wartość jest `true`, oprogramowanie pośredniczące nie modyfikuje nagłówki cache odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="274e7-182">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

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

### <a name="customize-output"></a><span data-ttu-id="274e7-183">Dostosowywanie danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="274e7-183">Customize output</span></span>

<span data-ttu-id="274e7-184">`ResponseWriter` Opcja pobiera lub ustawia delegata używany do zapisywania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="274e7-184">The `ResponseWriter` option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="274e7-185">Delegat domyślne zapisuje odpowiedź minimalnej w postaci zwykłego tekstu z wartością ciągu `HealthReport.Status`.</span><span class="sxs-lookup"><span data-stu-id="274e7-185">The default delegate writes a minimal plaintext response with the string value of `HealthReport.Status`.</span></span>

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

## <a name="database-probe"></a><span data-ttu-id="274e7-186">Sondowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="274e7-186">Database probe</span></span>

<span data-ttu-id="274e7-187">Sprawdzenie kondycji, można określić zapytanie bazy danych, aby był uruchamiany jako testu logicznego, aby wskazać, czy baza danych odpowiada normalnie.</span><span class="sxs-lookup"><span data-stu-id="274e7-187">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="274e7-188">Ta aplikacja używa przykładowych [BeatPulse](https://github.com/Xabaril/BeatPulse), biblioteka sprawdzania kondycji dla aplikacji platformy ASP.NET Core, można sprawdzić kondycji bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="274e7-188">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="274e7-189">Wykonuje BeatPulse `SELECT 1` zapytania względem bazy danych, aby upewnić się, połączenie z bazą danych jest w dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="274e7-189">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="274e7-190">Podczas sprawdzania połączenia z bazą danych przy użyciu zapytania, wybierz zapytanie zwracające szybko.</span><span class="sxs-lookup"><span data-stu-id="274e7-190">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="274e7-191">Podejście zapytania jest zagrożona przeciążenia bazy danych i zmniejszenie jego wydajności.</span><span class="sxs-lookup"><span data-stu-id="274e7-191">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="274e7-192">W większości przypadków Uruchamianie zapytania testu nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="274e7-192">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="274e7-193">Wystarczy jedynie wprowadzania udane połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="274e7-193">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="274e7-194">Jeśli możesz znaleźć niezbędne do uruchamiania kwerendy, wybierz prostego zapytania typu SELECT, taki jak `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="274e7-194">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="274e7-195">Aby można było używać biblioteki BeatPulse, zawierają odwołania do pakietu do [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="274e7-195">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="274e7-196">Podaj prawidłową bazą danych parametry połączenia w *appsettings.json* pliku przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-196">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="274e7-197">Aplikacja korzysta z bazy danych programu SQL Server o nazwie `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="274e7-197">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="274e7-198">Rejestrowanie usługi sprawdzania kondycji z `AddHealthChecks` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="274e7-198">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="274e7-199">Przykładowa aplikacja wywołuje firmy BeatPulse `AddSqlServer` metody przy użyciu parametrów połączenia bazy danych (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="274e7-199">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="274e7-200">Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="274e7-200">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="274e7-201">Aby uruchomić scenariusza sondowanie bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="274e7-201">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="274e7-202">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="274e7-202">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="274e7-203">Entity Framework Core DbContext sondy</span><span class="sxs-lookup"><span data-stu-id="274e7-203">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="274e7-204">`DbContext` Sprawdzanie jest obsługiwane w aplikacjach, które używają [Core Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="274e7-204">The `DbContext` check is supported in apps that use [Entity Framework (EF) Core](/ef/core/).</span></span> <span data-ttu-id="274e7-205">To sprawdzenie potwierdza, że aplikacja może komunikować się z bazą danych skonfigurowane dla platformy EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="274e7-205">This check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="274e7-206">Domyślnie `DbContextHealthCheck` wywołuje programu EF Core `CanConnectAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="274e7-206">By default, the `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="274e7-207">Można dostosować, jakie działanie jest uruchamiany, gdy sprawdzanie kondycji za pomocą przeciążenia `AddDbContextCheck` metody.</span><span class="sxs-lookup"><span data-stu-id="274e7-207">You can customize what operation is run when checking health using overloads of the `AddDbContextCheck` method.</span></span>

<span data-ttu-id="274e7-208">`AddDbContextCheck<TContext>` rejestruje sprawdzenia kondycji `DbContext` (`TContext`).</span><span class="sxs-lookup"><span data-stu-id="274e7-208">`AddDbContextCheck<TContext>` registers a health check for a `DbContext` (`TContext`).</span></span> <span data-ttu-id="274e7-209">Domyślnie nazwa sprawdzenie kondycji jest nazwą `TContext` typu.</span><span class="sxs-lookup"><span data-stu-id="274e7-209">By default, the name of the health check is the name of the `TContext` type.</span></span> <span data-ttu-id="274e7-210">Przeciążenie jest dostępna do konfigurowania stanu awarii, tagi i zapytań niestandardowych testu.</span><span class="sxs-lookup"><span data-stu-id="274e7-210">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="274e7-211">W przykładowej aplikacji `AppDbContext` jest dostarczany w celu `AddDbContextCheck` i zarejestrowany jako usługa w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="274e7-211">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="274e7-212">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="274e7-212">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="274e7-213">W przykładowej aplikacji `UseHealthChecks` dodaje pośredniczącym sprawdzanie kondycji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="274e7-213">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="274e7-214">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="274e7-214">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="274e7-215">Aby uruchomić `DbContext` sondowania scenariusz przy użyciu przykładowej aplikacji, upewnij się, że baza danych określona przez ciąg połączenia nie istnieje w wystąpieniu programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="274e7-215">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="274e7-216">Jeśli baza danych istnieje, należy go usunąć.</span><span class="sxs-lookup"><span data-stu-id="274e7-216">If the database exists, delete it.</span></span>

<span data-ttu-id="274e7-217">Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="274e7-217">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="274e7-218">Po uruchomieniu aplikacji Sprawdź stan kondycji, kieruje żądanie do `/health` punktu końcowego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="274e7-218">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="274e7-219">Bazy danych i `AppDbContext` nie istnieje, więc aplikacja zapewnia następującą odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="274e7-219">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="274e7-220">Wyzwalanie przykładową aplikację w celu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="274e7-220">Trigger the sample app to create the database.</span></span> <span data-ttu-id="274e7-221">Zgłosić wniosek o `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="274e7-221">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="274e7-222">Odpowiedź aplikacji:</span><span class="sxs-lookup"><span data-stu-id="274e7-222">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="274e7-223">Zgłosić wniosek o `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="274e7-223">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="274e7-224">Bazy danych i kontekstu istnieje, więc aplikacja reaguje:</span><span class="sxs-lookup"><span data-stu-id="274e7-224">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="274e7-225">Wyzwalanie przykładową aplikację w celu usunięcia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="274e7-225">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="274e7-226">Zgłosić wniosek o `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="274e7-226">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="274e7-227">Odpowiedź aplikacji:</span><span class="sxs-lookup"><span data-stu-id="274e7-227">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="274e7-228">Zgłosić wniosek o `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="274e7-228">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="274e7-229">Aplikacja zawiera odpowiedź złej kondycji:</span><span class="sxs-lookup"><span data-stu-id="274e7-229">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="274e7-230">Oddzielne gotowości i sondy żywotności</span><span class="sxs-lookup"><span data-stu-id="274e7-230">Separate readiness and liveness probes</span></span>

<span data-ttu-id="274e7-231">W niektórych scenariuszach hostingu parę kontroli kondycji są używane, rozróżnienie dwóch stanów aplikacji:</span><span class="sxs-lookup"><span data-stu-id="274e7-231">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="274e7-232">Aplikacja jest działa, ale nie jest jeszcze gotowy do odbierania żądań.</span><span class="sxs-lookup"><span data-stu-id="274e7-232">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="274e7-233">Ten stan jest to aplikacja *gotowości*.</span><span class="sxs-lookup"><span data-stu-id="274e7-233">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="274e7-234">Działa i odpowiada na żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="274e7-234">The app is functioning and responding to requests.</span></span> <span data-ttu-id="274e7-235">Ten stan jest to aplikacja *żywotności*.</span><span class="sxs-lookup"><span data-stu-id="274e7-235">This state is the app's *liveness*.</span></span>

<span data-ttu-id="274e7-236">Sprawdzanie gotowości zwykle wykonuje bardziej rozbudowanym i czasochłonne zestawu testów, aby sprawdzić, czy wszystkie podsystemy aplikacji i zasoby są dostępne.</span><span class="sxs-lookup"><span data-stu-id="274e7-236">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="274e7-237">Sprawdzanie żywotności jedynie wykonuje szybkie sprawdzenie w celu określenia, czy aplikacja jest dostępna do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="274e7-237">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="274e7-238">Po jej przekazuje jej sprawdzanie gotowości, nie ma potrzeby do obciążać aplikacji dalsze kosztowne zestaw gotowości&mdash;dalsze procesy kontroli wymagają tylko sprawdzanie żywotności.</span><span class="sxs-lookup"><span data-stu-id="274e7-238">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="274e7-239">Przykładowa aplikacja zawiera sprawdzenie kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [usługi hostowanej](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="274e7-239">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="274e7-240">`StartupHostedServiceHealthCheck` Ujawnia właściwość `StartupTaskCompleted`, które można ustawić usługi hostowanej `true` po zakończeniu jego długotrwałe zadanie (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="274e7-240">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="274e7-241">Długotrwałe zadanie w tle jest uruchamiana przez [usługi hostowanej](xref:fundamentals/host/hosted-services) (*usług/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="274e7-241">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="274e7-242">Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` ustawiono `true`:</span><span class="sxs-lookup"><span data-stu-id="274e7-242">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="274e7-243">Sprawdzenie kondycji jest zarejestrowane w usłudze `AddCheck` w `Startup.ConfigureServices` wraz z hostowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="274e7-243">The health check is registered with `AddCheck` in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="274e7-244">Ponieważ usługa hostowana należy ustawić właściwość na sprawdzenie kondycji, sprawdzenie kondycji również jest zarejestrowany w kontenerze usługi (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="274e7-244">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="274e7-245">Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="274e7-245">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="274e7-246">W przykładowej aplikacji z punktami końcowymi kontroli kondycji są tworzone na `/health/ready` sprawdzenia gotowości i `/health/live` sprawdzania żywotności.</span><span class="sxs-lookup"><span data-stu-id="274e7-246">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="274e7-247">Filtry sprawdzania gotowości kondycji sprawdza kondycję skontaktuj się z `ready` tagu.</span><span class="sxs-lookup"><span data-stu-id="274e7-247">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="274e7-248">Odfiltrowuje wyboru żywotności `StartupHostedServiceHealthCheck` , zwracając `false` w `HealthCheckOptions.Predicate` (Aby uzyskać więcej informacji, zobacz [filtrowania kontrole kondycji](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="274e7-248">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the `HealthCheckOptions.Predicate` (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="274e7-249">Aby uruchomić gotowości/żywotności scenariusz konfiguracji, za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="274e7-249">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="274e7-250">W przeglądarce, odwiedź stronę `/health/ready` kilka razy, aż przeszły 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="274e7-250">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="274e7-251">Sprawdzanie kondycji raporty `Unhealthy` przez pierwsze 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="274e7-251">The health check reports `Unhealthy` for the first 15 seconds.</span></span> <span data-ttu-id="274e7-252">Po 15 sekundach raporty punktu końcowego `Healthy`, co odzwierciedla ukończenie długotrwałego zadania w usłudze hostowanej.</span><span class="sxs-lookup"><span data-stu-id="274e7-252">After 15 seconds, the endpoint reports `Healthy`, which reflects the completion of the long-running task by the hosted service.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="274e7-253">Przykład rozwiązania Kubernetes</span><span class="sxs-lookup"><span data-stu-id="274e7-253">Kubernetes example</span></span>

<span data-ttu-id="274e7-254">Przy użyciu oddzielnych kontroli gotowości i żywotności przydaje się w środowisku, takie jak [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="274e7-254">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="274e7-255">W usłudze Kubernetes aplikacja może być wymagane do wykonywania pracy czasochłonne uruchamiania przed zaakceptowaniem żądania, takie jak testu dostępności podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="274e7-255">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="274e7-256">Przy użyciu oddzielnych kontroli umożliwia programu orchestrator w celu odróżnienia, czy aplikacja działa, ale nie jest jeszcze gotowy, lub jeśli aplikacja nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="274e7-256">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="274e7-257">Aby uzyskać więcej informacji o gotowości i sondy żywotności w usłudze Kubernetes, zobacz [skonfigurować żywotności i sondy gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji platformy Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="274e7-257">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="274e7-258">W poniższym przykładzie pokazano konfigurację sondowania gotowości Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="274e7-258">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="274e7-259">Opartą na metryce sondowania przy użyciu składnika zapisywania niestandardowych</span><span class="sxs-lookup"><span data-stu-id="274e7-259">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="274e7-260">Przykładowa aplikacja pokazuje sprawdzenie kondycji pamięci za pomocą składnika zapisywania niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="274e7-260">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="274e7-261">`MemoryHealthCheck` Raporty Stan obniżonej wydajności, jeśli aplikacja korzysta z więcej niż wartość progową pamięci (1 GB w przykładowej aplikacji).</span><span class="sxs-lookup"><span data-stu-id="274e7-261">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="274e7-262">`HealthCheckResult` Zawiera informacje moduł wyrzucania elementów bezużytecznych (GC) dla aplikacji (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="274e7-262">The `HealthCheckResult` includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="274e7-263">Rejestrowanie usługi sprawdzania kondycji z `AddHealthChecks` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="274e7-263">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="274e7-264">Zamiast włączania kondycji Sprawdź przez przekazanie jej do `AddCheck`, `MemoryHealthCheck` jest zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="274e7-264">Instead of enabling the health check by passing it to `AddCheck`, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="274e7-265">Wszystkie `IHealthCheck` zarejestrowane usługi są dostępne dla usługi sprawdzania kondycji i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="274e7-265">All `IHealthCheck` registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="274e7-266">Firma Microsoft zaleca rejestrowanie usługi sprawdzania kondycji jako pojedynczego wystąpienia usługi.</span><span class="sxs-lookup"><span data-stu-id="274e7-266">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="274e7-267">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="274e7-267">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="274e7-268">Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="274e7-268">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="274e7-269">A `WriteResponse` delegata jest dostarczany w celu `ResponseWriter` właściwość niestandardową odpowiedź w formacie JSON dane wyjściowe, gdy wykonuje sprawdzenie kondycji:</span><span class="sxs-lookup"><span data-stu-id="274e7-269">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="274e7-270">`WriteResponse` Formaty metoda `CompositeHealthCheckResult` do postaci JSON obiektu i zwraca dane wyjściowe JSON dla odpowiedzi z kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="274e7-270">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="274e7-271">Aby uruchomić funkcję badania opartą na metryce z danymi wyjściowymi modułu zapisującego niestandardowe odpowiedzi za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="274e7-271">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="274e7-272">[BeatPulse](https://github.com/Xabaril/BeatPulse) obejmuje opartą na metryce kondycji sprawdzenie scenariuszy, w tym dysku magazynu i maksymalną wartość żywotności kontroli.</span><span class="sxs-lookup"><span data-stu-id="274e7-272">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="274e7-273">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="274e7-273">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="274e7-274">Filtruj według portów</span><span class="sxs-lookup"><span data-stu-id="274e7-274">Filter by port</span></span>

<span data-ttu-id="274e7-275">Wywoływanie `UseHealthChecks` z portem ogranicza żądania sprawdzenia kondycji do określonego portu.</span><span class="sxs-lookup"><span data-stu-id="274e7-275">Calling `UseHealthChecks` with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="274e7-276">Zazwyczaj służy w środowisku kontenera do udostępniania portów do monitorowania usługi.</span><span class="sxs-lookup"><span data-stu-id="274e7-276">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="274e7-277">Przykładowa aplikacja umożliwia skonfigurowanie portu przy użyciu [zmiennej dostawcę konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="274e7-277">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="274e7-278">Numer portu jest ustawiana w *launchSettings.json* pliku i przekazywane do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="274e7-278">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="274e7-279">Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.</span><span class="sxs-lookup"><span data-stu-id="274e7-279">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="274e7-280">Przykładowa aplikacja jest używana do zademonstrowania konfiguracji portów zarządzania, należy utworzyć *launchSettings.json* w pliku *właściwości* folderu.</span><span class="sxs-lookup"><span data-stu-id="274e7-280">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="274e7-281">Następujące *launchSettings.json* plików nie jest zawarty w plikach projektu przykładowej aplikacji i należy utworzyć je ręcznie.</span><span class="sxs-lookup"><span data-stu-id="274e7-281">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="274e7-282">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="274e7-282">*Properties/launchSettings.json*:</span></span>

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

<span data-ttu-id="274e7-283">Rejestrowanie usługi sprawdzania kondycji z `AddHealthChecks` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="274e7-283">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="274e7-284">Wywołanie `UseHealthChecks` Określa port usługi zarządzania (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="274e7-284">The call to `UseHealthChecks` specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="274e7-285">Możesz uniknąć tworzenia *launchSettings.json* pliku w przykładowej aplikacji przez ustawianie adresów URL i port zarządzania jawnie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="274e7-285">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="274e7-286">W *Program.cs* gdzie `WebHostBuilder` jest utworzony, dodaj wywołanie `UseUrls` i podaj punktu końcowego normalne odpowiedzi aplikacji oraz port punktu końcowego zarządzania.</span><span class="sxs-lookup"><span data-stu-id="274e7-286">In *Program.cs* where the `WebHostBuilder` is created, add a call to `UseUrls` and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="274e7-287">W *ManagementPortStartup.cs* gdzie `UseHealthChecks` jest wywoływana, jawnie określ port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="274e7-287">In *ManagementPortStartup.cs* where `UseHealthChecks` is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="274e7-288">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="274e7-288">*Program.cs*:</span></span>
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
> <span data-ttu-id="274e7-289">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="274e7-289">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="274e7-290">Aby uruchomić scenariusz konfiguracji portów zarządzania za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="274e7-290">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="274e7-291">Dystrybucja Biblioteka sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-291">Distribute a health check library</span></span>

<span data-ttu-id="274e7-292">Aby rozesłać sprawdzenie kondycji jako biblioteki:</span><span class="sxs-lookup"><span data-stu-id="274e7-292">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="274e7-293">Sprawdzenie kondycji, który implementuje zapisu `IHealthCheck` interfejs jako autonomiczną klasę.</span><span class="sxs-lookup"><span data-stu-id="274e7-293">Write a health check that implements the `IHealthCheck` interface as a standalone class.</span></span> <span data-ttu-id="274e7-294">Klasa może polegać na [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection), typ aktywacji i [o nazwie opcje](xref:fundamentals/configuration/options) do dostępu do danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="274e7-294">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

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

1. <span data-ttu-id="274e7-295">Zapisywanie metody rozszerzenia z parametrami, które aplikacja odbierająca komunikaty wywołania w jego `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="274e7-295">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="274e7-296">W poniższym przykładzie przyjęto założenie, następujący podpis metody sprawdzania kondycji:</span><span class="sxs-lookup"><span data-stu-id="274e7-296">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="274e7-297">Poprzedni podpisu wskazuje, że `ExampleHealthCheck` wymaga dodatkowe dane do procesu kondycji Sprawdź logikę sondowania.</span><span class="sxs-lookup"><span data-stu-id="274e7-297">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="274e7-298">Dane są udostępniane do delegata, użyty do utworzenia wystąpienia sprawdzenie kondycji po zarejestrowaniu sprawdzenie kondycji za pomocą metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="274e7-298">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="274e7-299">W poniższym przykładzie obiekt wywołujący określa opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="274e7-299">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="274e7-300">Nazwa sprawdzenia kondycji (`name`).</span><span class="sxs-lookup"><span data-stu-id="274e7-300">health check name (`name`).</span></span> <span data-ttu-id="274e7-301">Jeśli `null`, `example_health_check` jest używany.</span><span class="sxs-lookup"><span data-stu-id="274e7-301">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="274e7-302">Ciąg punktu danych dla kontroli kondycji (`data1`).</span><span class="sxs-lookup"><span data-stu-id="274e7-302">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="274e7-303">punkt danych liczby całkowitej dla kontroli kondycji (`data2`).</span><span class="sxs-lookup"><span data-stu-id="274e7-303">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="274e7-304">Jeśli `null`, `1` jest używany.</span><span class="sxs-lookup"><span data-stu-id="274e7-304">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="274e7-305">Stan błędu (`HealthStatus`).</span><span class="sxs-lookup"><span data-stu-id="274e7-305">failure status (`HealthStatus`).</span></span> <span data-ttu-id="274e7-306">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="274e7-306">The default is `null`.</span></span> <span data-ttu-id="274e7-307">Jeśli `null`, `HealthStatus.Unhealthy` jest zgłaszany stan Niepowodzenie.</span><span class="sxs-lookup"><span data-stu-id="274e7-307">If `null`, `HealthStatus.Unhealthy` is reported for a failure status.</span></span>
   * <span data-ttu-id="274e7-308">tagi (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="274e7-308">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="274e7-309">Wydawca sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="274e7-309">Health Check Publisher</span></span>

<span data-ttu-id="274e7-310">Gdy `IHealthCheckPublisher` zostanie dodany do kontenera usługi systemu sprawdzanie kondycji wykonuje okresowo z kontroli kondycji i wywołania `PublishAsync` z wynikiem.</span><span class="sxs-lookup"><span data-stu-id="274e7-310">When an `IHealthCheckPublisher` is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="274e7-311">Jest to przydatne w scenariuszu systemu, który oczekuje, że każdy proces, aby wywołać system monitorowania okresowo w celu określenia kondycji monitorowania kondycji na podstawie wypychania.</span><span class="sxs-lookup"><span data-stu-id="274e7-311">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="274e7-312">`IHealthCheckPublisher` Interfejs zawiera jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="274e7-312">The `IHealthCheckPublisher` interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

> [!NOTE]
> <span data-ttu-id="274e7-313">[BeatPulse](https://github.com/Xabaril/BeatPulse) obejmuje wydawcy dla kilku systemów, w tym [usługi Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="274e7-313">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="274e7-314">[BeatPulse](https://github.com/Xabaril/BeatPulse) nie jest obsługiwany lub obsługiwane przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="274e7-314">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
