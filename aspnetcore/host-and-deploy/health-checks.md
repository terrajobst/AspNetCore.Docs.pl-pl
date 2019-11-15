---
title: Kontrole kondycji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować Sprawdzanie kondycji infrastruktury ASP.NET Core, na przykład aplikacje i bazy danych.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 11/13/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 4a4606a58178018f0d71d467d4c8b6c9982c09dc
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115996"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="9f63f-103">Kontrole kondycji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f63f-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="9f63f-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="9f63f-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9f63f-105">ASP.NET Core oferuje oprogramowanie do sprawdzania kondycji i biblioteki do raportowania kondycji składników infrastruktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="9f63f-106">Kontrole kondycji są udostępniane przez aplikację jako punkty końcowe HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f63f-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="9f63f-107">Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="9f63f-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="9f63f-108">Sondy kondycji mogą być używane przez koordynatorów kontenerów i moduły równoważenia obciążenia w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="9f63f-109">Na przykład koordynator kontenera może odpowiedzieć na niepowodzenie sprawdzania kondycji przez zatrzymanie wdrożenia stopniowego lub ponowne uruchomienie kontenera.</span><span class="sxs-lookup"><span data-stu-id="9f63f-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="9f63f-110">Moduł równoważenia obciążenia może reagować na aplikację w złej kondycji przez kierowanie ruchu z nieprawidłowego wystąpienia do prawidłowego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="9f63f-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="9f63f-111">Użycie pamięci, dysku i innych zasobów serwera fizycznego może być monitorowane w celu zapewnienia prawidłowego stanu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="9f63f-112">Kontrole kondycji umożliwiają testowanie zależności aplikacji, takich jak bazy danych i punkty końcowe usług zewnętrznych, w celu potwierdzenia dostępności i normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="9f63f-113">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f63f-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9f63f-114">Przykładowa aplikacja zawiera przykłady scenariuszy opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="9f63f-115">Aby uruchomić przykładową aplikację dla danego scenariusza, użyj polecenia [dotnet Run](/dotnet/core/tools/dotnet-run) z folderu projektu w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="9f63f-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="9f63f-116">Zobacz plik *README.MD* aplikacji przykładowej i opisy scenariuszy w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f63f-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9f63f-117">Prerequisites</span></span>

<span data-ttu-id="9f63f-118">Kontrole kondycji są zwykle używane z zewnętrzną usługą monitorowania lub koordynatorem kontenera w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="9f63f-119">Przed dodaniem kontroli kondycji do aplikacji należy określić system monitorowania, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="9f63f-120">System monitorujący określa, jakie typy testów kondycji należy utworzyć i jak skonfigurować ich punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="9f63f-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="9f63f-121">Pakiet [Microsoft. AspNetCore. Diagnostics. HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) jest wywoływany niejawnie dla aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f63f-121">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="9f63f-122">Aby przeprowadzić kontrolę kondycji przy użyciu Entity Framework Core, Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="9f63f-123">Przykładowa aplikacja zawiera kod uruchamiania, aby zademonstrować Sprawdzanie kondycji kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="9f63f-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="9f63f-124">Scenariusz [sondowania bazy danych](#database-probe) sprawdza kondycję połączenia z bazą danych przy użyciu [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="9f63f-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="9f63f-125">Scenariusz [sondowania DbContext](#entity-framework-core-dbcontext-probe) sprawdza bazę danych przy użyciu `DbContext`EF Core.</span><span class="sxs-lookup"><span data-stu-id="9f63f-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="9f63f-126">Aby poznać scenariusze baz danych, przykładową aplikację:</span><span class="sxs-lookup"><span data-stu-id="9f63f-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="9f63f-127">Tworzy bazę danych i udostępnia jej parametry połączenia w pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9f63f-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="9f63f-128">W pliku projektu znajdują się następujące odwołania do pakietów:</span><span class="sxs-lookup"><span data-stu-id="9f63f-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="9f63f-129">AspNetCore. HealthChecks. SqlServer</span><span class="sxs-lookup"><span data-stu-id="9f63f-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="9f63f-130">Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="9f63f-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="9f63f-131">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="9f63f-132">W innym scenariuszu sprawdzania kondycji przedstawiono sposób filtrowania kontroli kondycji do portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="9f63f-133">Przykładowa aplikacja wymaga utworzenia pliku *Properties/profilu launchsettings. JSON* , który zawiera adres URL zarządzania i port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="9f63f-134">Aby uzyskać więcej informacji, zobacz sekcję [filtrowanie według portów](#filter-by-port) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="9f63f-135">Podstawowa sonda kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-135">Basic health probe</span></span>

<span data-ttu-id="9f63f-136">W przypadku wielu aplikacji Podstawowa konfiguracja sondy kondycji, która zgłasza dostępność aplikacji do żądań przetwarzania,jest wystarczająca do odnajdywania stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="9f63f-137">Konfiguracja podstawowa rejestruje usługi sprawdzania kondycji i wywołuje program do sprawdzania kondycji, aby odpowiedzieć na punkt końcowy adresu URL z odpowiedzią na kondycję.</span><span class="sxs-lookup"><span data-stu-id="9f63f-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="9f63f-138">Domyślnie żadne określone kontrole kondycji nie są rejestrowane do testowania żadnej konkretnej zależności lub podsystemu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="9f63f-139">Aplikacja jest uważana za działającą w dobrej kondycji, jeśli jest w stanie reagować na adres URL punktu końcowego kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="9f63f-140">Domyślny moduł zapisujący odpowiedzi zapisuje stan (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) jako odpowiedź w postaci zwykłego tekstu z powrotem do klienta, co oznacza, że jest to [HealthStatus. zdrowy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus. obniżona](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) [kondycja lub HealthStatus.](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)</span><span class="sxs-lookup"><span data-stu-id="9f63f-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="9f63f-141">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-142">Utwórz punkt końcowy sprawdzania kondycji, wywołując `MapHealthChecks` w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="9f63f-143">W przykładowej aplikacji punkt końcowy sprawdzania kondycji jest tworzony w `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHealthChecks("/health");
        });
    }
}
```

<span data-ttu-id="9f63f-144">Aby uruchomić podstawowy scenariusz konfiguracji przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="9f63f-145">Przykład platformy Docker</span><span class="sxs-lookup"><span data-stu-id="9f63f-145">Docker example</span></span>

<span data-ttu-id="9f63f-146">Platforma [Docker](xref:host-and-deploy/docker/index) oferuje wbudowaną `HEALTHCHECK` dyrektywę, której można użyć do sprawdzenia stanu aplikacji korzystającej z podstawowej konfiguracji sprawdzania kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="9f63f-147">Utwórz kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-147">Create health checks</span></span>

<span data-ttu-id="9f63f-148">Kontrole kondycji są tworzone przez implementację interfejsu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="9f63f-149">Metoda <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> zwraca <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, który wskazuje na kondycję jako `Healthy`, `Degraded`lub `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="9f63f-150">Wynik jest zapisywana jako odpowiedź w postaci zwykłego tekstu ze konfigurowalnym kodem stanu (Konfiguracja jest opisana w sekcji [Opcje sprawdzania kondycji](#health-check-options) ).</span><span class="sxs-lookup"><span data-stu-id="9f63f-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="9f63f-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> może również zwracać opcjonalne pary klucz-wartość.</span><span class="sxs-lookup"><span data-stu-id="9f63f-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="9f63f-152">W poniższej klasie `ExampleHealthCheck` zademonstrowano układ kontroli kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="9f63f-153">Logika kontroli kondycji jest umieszczana w metodzie `CheckHealthAsync`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="9f63f-154">Poniższy przykład ustawia zmienną fikcyjną `healthCheckResultHealthy`, aby `true`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="9f63f-155">Jeśli wartość `healthCheckResultHealthy` jest ustawiona na `false`, zwracany jest stan [HealthCheckResult. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("A healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("An unhealthy result."));
    }
}
```

## <a name="register-health-check-services"></a><span data-ttu-id="9f63f-156">Rejestrowanie usług sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-156">Register health check services</span></span>

<span data-ttu-id="9f63f-157">Typ `ExampleHealthCheck` jest dodawany do usług sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="9f63f-158">Przeciążenie <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> pokazane w poniższym przykładzie ustawia stan awarii (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) do raportowania, gdy Sprawdzanie kondycji zgłosi błąd.</span><span class="sxs-lookup"><span data-stu-id="9f63f-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="9f63f-159">Jeśli stan niepowodzenia jest ustawiony na wartość `null` (domyślnie), zostanie zgłoszony [HealthStatus. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="9f63f-160">To przeciążenie jest przydatnym scenariuszem dla autorów biblioteki, w którym stan niepowodzenia wskazywany przez bibliotekę jest wymuszany przez aplikację w przypadku niepowodzenia sprawdzania kondycji, jeśli implementacja sprawdzania kondycji przestrzega tego ustawienia.</span><span class="sxs-lookup"><span data-stu-id="9f63f-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="9f63f-161">*Tagi* mogą służyć do filtrowania kontroli kondycji (opisanych w sekcji [Sprawdzanie kondycji filtru](#filter-health-checks) ).</span><span class="sxs-lookup"><span data-stu-id="9f63f-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="9f63f-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> może również wykonać funkcję lambda.</span><span class="sxs-lookup"><span data-stu-id="9f63f-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="9f63f-163">W poniższym przykładzie nazwa sprawdzania kondycji jest określona jako `Example` i sprawdzanie zawsze zwraca prawidłowy stan:</span><span class="sxs-lookup"><span data-stu-id="9f63f-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

<span data-ttu-id="9f63f-164">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*>, aby przekazać argumenty do implementacji sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-164">Call <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> to pass arugments to a health check implementation.</span></span> <span data-ttu-id="9f63f-165">W poniższym przykładzie `TestHealthCheckWithArgs` akceptuje liczbę całkowitą i ciąg, który ma być używany, gdy <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="9f63f-165">In the following example, `TestHealthCheckWithArgs` accepts an integer and a string for use when <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> is called:</span></span>

```csharp
private class TestHealthCheckWithArgs : IHealthCheck
{
    public TestHealthCheckWithArgs(int i, string s)
    {
        I = i;
        S = s;
    }

    public int I { get; set; }

    public string S { get; set; }

    public Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, 
        CancellationToken cancellationToken = default)
    {
        ...
    }
}
```

<span data-ttu-id="9f63f-166">`TestHealthCheckWithArgs` jest zarejestrowany przez wywołanie `AddTypeActivatedCheck` z liczbą całkowitą i ciągiem przekazaną do implementacji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-166">`TestHealthCheckWithArgs` is registered by calling `AddTypeActivatedCheck` with the integer and string passed to the implementation:</span></span>

```csharp
services.AddHealthChecks()
    .AddTypeActivatedCheck<TestHealthCheckWithArgs>(
        "test", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" }, 
        args: new object[] { 5, "string" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="9f63f-167">Użyj routingu kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-167">Use Health Checks Routing</span></span>

<span data-ttu-id="9f63f-168">W `Startup.Configure`Wywołaj `MapHealthChecks` na konstruktorze punktów końcowych z adresem URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="9f63f-168">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="9f63f-169">Wymagaj hosta</span><span class="sxs-lookup"><span data-stu-id="9f63f-169">Require host</span></span>

<span data-ttu-id="9f63f-170">Wywołaj `RequireHost`, aby określić co najmniej jeden dozwolony Host dla punktu końcowego sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-170">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="9f63f-171">Hosty powinny być w formacie Unicode, a nie formacie Punycode i mogą zawierać port.</span><span class="sxs-lookup"><span data-stu-id="9f63f-171">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="9f63f-172">Jeśli kolekcja nie zostanie podana, każdy host zostanie zaakceptowany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-172">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="9f63f-173">Aby uzyskać więcej informacji, zobacz sekcję [filtrowanie według portów](#filter-by-port) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-173">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="9f63f-174">Wymagaj autoryzacji</span><span class="sxs-lookup"><span data-stu-id="9f63f-174">Require authorization</span></span>

<span data-ttu-id="9f63f-175">Wywołaj `RequireAuthorization`, aby uruchomić oprogramowanie pośredniczące autoryzacji w punkcie końcowym żądania sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-175">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="9f63f-176">Przeciążenie `RequireAuthorization` akceptuje co najmniej jedną zasadę autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-176">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="9f63f-177">Jeśli nie podano zasad, zostanie użyta domyślna zasada autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-177">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="9f63f-178">Włączanie żądań Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="9f63f-178">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="9f63f-179">Mimo że sprawdzanie kondycji jest wykonywane ręcznie z przeglądarki nie jest typowym scenariuszem użycia, oprogramowanie do obsługi mechanizmu CORS można włączyć, wywołując `RequireCors` w punktach końcowych sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-179">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="9f63f-180">Przeciążenie `RequireCors` akceptuje delegata konstruktora zasad CORS (`CorsPolicyBuilder`) lub nazwę zasady.</span><span class="sxs-lookup"><span data-stu-id="9f63f-180">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="9f63f-181">Jeśli nie podano zasad, zostanie użyta domyślna zasada CORS.</span><span class="sxs-lookup"><span data-stu-id="9f63f-181">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="9f63f-182">Aby uzyskać więcej informacji, zobacz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-182">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="9f63f-183">Opcje sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-183">Health check options</span></span>

<span data-ttu-id="9f63f-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> zapewnić możliwość dostosowania zachowania kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="9f63f-185">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-185">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="9f63f-186">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="9f63f-186">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="9f63f-187">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9f63f-187">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="9f63f-188">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="9f63f-188">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="9f63f-189">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-189">Filter health checks</span></span>

<span data-ttu-id="9f63f-190">Domyślnie kontrole kondycji oprogramowania pośredniczącego uruchamia wszystkie zarejestrowane testy kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-190">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="9f63f-191">Aby uruchomić podzestaw kontroli kondycji, podaj funkcję, która zwraca wartość logiczną do <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> opcji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-191">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="9f63f-192">W poniższym przykładzie Sprawdzanie kondycji `Bar` jest odfiltrowane przez tag (`bar_tag`) w instrukcji warunkowej funkcji, gdzie `true` jest zwracany tylko wtedy, gdy właściwość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> kontroli kondycji pasuje do `foo_tag` lub `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-192">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="9f63f-193">W `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-193">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="9f63f-194">W `Startup.Configure``Predicate` filtruje kontrolę kondycji "bar".</span><span class="sxs-lookup"><span data-stu-id="9f63f-194">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="9f63f-195">Tylko Foo i baz Execute.:</span><span class="sxs-lookup"><span data-stu-id="9f63f-195">Only Foo and Baz execute.:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
});
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="9f63f-196">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="9f63f-196">Customize the HTTP status code</span></span>

<span data-ttu-id="9f63f-197">Użyj <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes>, aby dostosować mapowanie stanu kondycji do kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f63f-197">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="9f63f-198">Następujące przypisania <xref:Microsoft.AspNetCore.Http.StatusCodes> są wartościami domyślnymi używanymi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="9f63f-198">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="9f63f-199">Zmień wartości kodów stanu, aby spełniały Twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-199">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="9f63f-200">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-200">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="9f63f-201">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9f63f-201">Suppress cache headers</span></span>

<span data-ttu-id="9f63f-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> kontroluje, czy program do sprawdzania kondycji dodaje do odpowiedzi sondy nagłówki HTTP, aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="9f63f-203">Jeśli wartość jest `false` (domyślnie), oprogramowanie pośredniczące ustawia lub zastępuje nagłówki `Cache-Control`, `Expires`i `Pragma`, aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-203">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="9f63f-204">Jeśli wartość jest `true`, oprogramowanie pośredniczące nie zmodyfikuje nagłówków pamięci podręcznej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-204">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="9f63f-205">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="9f63f-206">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="9f63f-206">Customize output</span></span>

<span data-ttu-id="9f63f-207">Opcja <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> Pobiera lub ustawia delegata używany do zapisywania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-207">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span>

<span data-ttu-id="9f63f-208">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-208">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="9f63f-209">Domyślnym delegatem jest zapisanie minimalnej odpowiedzi w postaci zwykłego tekstu z wartością ciągu [HealthReport. status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="9f63f-209">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="9f63f-210">Następujący delegat niestandardowy `WriteResponse`, wyprowadza niestandardową odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="9f63f-210">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
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

<span data-ttu-id="9f63f-211">System kontroli kondycji nie zapewnia wbudowanej obsługi złożonych formatów powrotu JSON, ponieważ format jest specyficzny dla wybranego systemu monitorowania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-211">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="9f63f-212">Możesz dowolnie dostosowywać `JObject` w powyższym przykładzie, aby zaspokoić Twoje potrzeby.</span><span class="sxs-lookup"><span data-stu-id="9f63f-212">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="9f63f-213">Sonda bazy danych</span><span class="sxs-lookup"><span data-stu-id="9f63f-213">Database probe</span></span>

<span data-ttu-id="9f63f-214">Kontrola kondycji może określić zapytanie bazy danych, które ma zostać uruchomione jako test logiczny, aby wskazać, czy baza danych ma zwykle odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="9f63f-214">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="9f63f-215">Przykładowa aplikacja używa [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), biblioteki sprawdzania kondycji dla aplikacji ASP.NET Core, aby przeprowadzić kontrolę kondycji SQL Server bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-215">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="9f63f-216">`AspNetCore.Diagnostics.HealthChecks` wykonuje `SELECT 1` zapytanie względem bazy danych w celu potwierdzenia, że połączenie z bazą danych jest w dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-216">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="9f63f-217">Podczas sprawdzania połączenia z bazą danych za pomocą zapytania wybierz zapytanie, które zwraca szybko.</span><span class="sxs-lookup"><span data-stu-id="9f63f-217">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="9f63f-218">Podejście zapytania uruchamia ryzyko przeciążenia bazy danych i spadek jej wydajności.</span><span class="sxs-lookup"><span data-stu-id="9f63f-218">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="9f63f-219">W większości przypadków uruchomienie zapytania testowego nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="9f63f-219">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="9f63f-220">Wystarczy, że połączenie z bazą danych zostało zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-220">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="9f63f-221">Jeśli okaże się, że konieczne jest uruchomienie zapytania, wybierz proste zapytanie SELECT, takie jak `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-221">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="9f63f-222">Dołącz odwołanie do pakietu do [AspNetCore. HealthChecks. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-222">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="9f63f-223">Podaj prawidłowe parametry połączenia z bazą danych w pliku *appSettings. JSON* przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-223">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="9f63f-224">Aplikacja używa SQL Server bazy danych o nazwie `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-224">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="9f63f-225">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-225">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-226">Przykładowa aplikacja wywołuje metodę `AddSqlServer` przy użyciu parametrów połączenia bazy danych (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-226">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9f63f-227">Punkt końcowy sprawdzania kondycji jest tworzony przez wywołanie `MapHealthChecks` w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-227">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="9f63f-228">Aby uruchomić scenariusz sondowania bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-228">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="9f63f-229">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-229">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="9f63f-230">Badanie Entity Framework Core DbContext</span><span class="sxs-lookup"><span data-stu-id="9f63f-230">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="9f63f-231">`DbContext` sprawdza, czy aplikacja może komunikować się z bazą danych skonfigurowaną dla EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-231">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="9f63f-232">Sprawdzanie `DbContext` jest obsługiwane w aplikacjach, które:</span><span class="sxs-lookup"><span data-stu-id="9f63f-232">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="9f63f-233">Użyj [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-233">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="9f63f-234">Dołącz odwołanie do pakietu do [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-234">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="9f63f-235">`AddDbContextCheck<TContext>` rejestruje Sprawdzanie kondycji `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-235">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="9f63f-236">`DbContext` jest dostarczany jako `TContext` do metody.</span><span class="sxs-lookup"><span data-stu-id="9f63f-236">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="9f63f-237">Jest dostępne Przeciążenie umożliwiające skonfigurowanie stanu niepowodzenia, tagów i niestandardowego zapytania testowego.</span><span class="sxs-lookup"><span data-stu-id="9f63f-237">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="9f63f-238">Domyślnie:</span><span class="sxs-lookup"><span data-stu-id="9f63f-238">By default:</span></span>

* <span data-ttu-id="9f63f-239">`DbContextHealthCheck` wywołuje EF Core metodę `CanConnectAsync`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-239">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="9f63f-240">Można dostosować, jaka operacja jest uruchamiana podczas sprawdzania kondycji przy użyciu przeciążeń metody `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-240">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="9f63f-241">Nazwa sprawdzania kondycji jest nazwą typu `TContext`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-241">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="9f63f-242">W przykładowej aplikacji `AppDbContext` są udostępniane `AddDbContextCheck` i rejestrowane jako usługa w `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-242">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9f63f-243">Punkt końcowy sprawdzania kondycji jest tworzony przez wywołanie `MapHealthChecks` w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-243">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="9f63f-244">Aby uruchomić scenariusz sondowania `DbContext` za pomocą przykładowej aplikacji, upewnij się, że baza danych określona przez parametry połączenia nie istnieje w wystąpieniu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f63f-244">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="9f63f-245">Jeśli baza danych istnieje, usuń ją.</span><span class="sxs-lookup"><span data-stu-id="9f63f-245">If the database exists, delete it.</span></span>

<span data-ttu-id="9f63f-246">Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-246">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="9f63f-247">Po uruchomieniu aplikacji Sprawdź stan kondycji, wysyłając żądanie do punktu końcowego `/health` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="9f63f-247">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="9f63f-248">Baza danych i `AppDbContext` nie istnieją, dlatego aplikacja oferuje następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="9f63f-248">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="9f63f-249">Wyzwól przykładową aplikację, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-249">Trigger the sample app to create the database.</span></span> <span data-ttu-id="9f63f-250">Utwórz żądanie `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-250">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="9f63f-251">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="9f63f-251">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="9f63f-252">Utwórz żądanie do punktu końcowego `/health`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-252">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="9f63f-253">Istnieje baza danych i kontekst, aby aplikacja odpowiadała:</span><span class="sxs-lookup"><span data-stu-id="9f63f-253">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="9f63f-254">Wyzwól przykładową aplikację, aby usunąć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-254">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="9f63f-255">Utwórz żądanie `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-255">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="9f63f-256">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="9f63f-256">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="9f63f-257">Utwórz żądanie do punktu końcowego `/health`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-257">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="9f63f-258">Aplikacja zawiera odpowiedź w złej kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-258">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="9f63f-259">Oddzielne sondy gotowości i na żywo</span><span class="sxs-lookup"><span data-stu-id="9f63f-259">Separate readiness and liveness probes</span></span>

<span data-ttu-id="9f63f-260">W niektórych scenariuszach hostingu jest używana para kontroli kondycji, która odróżnia dwa stany aplikacji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-260">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="9f63f-261">Aplikacja działa, ale jeszcze nie jest gotowa do odbierania żądań.</span><span class="sxs-lookup"><span data-stu-id="9f63f-261">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="9f63f-262">Jest to stan *gotowości*aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-262">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="9f63f-263">Aplikacja działa i odpowiada na żądania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-263">The app is functioning and responding to requests.</span></span> <span data-ttu-id="9f63f-264">Ten stan jest *aktywny*.</span><span class="sxs-lookup"><span data-stu-id="9f63f-264">This state is the app's *liveness*.</span></span>

<span data-ttu-id="9f63f-265">Sprawdzanie gotowości zwykle wykonuje bardziej obszerny i czasochłonny zestaw kontroli w celu ustalenia, czy wszystkie podsystemy i zasoby aplikacji są dostępne.</span><span class="sxs-lookup"><span data-stu-id="9f63f-265">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="9f63f-266">Sprawdzenie na żywo powoduje jedynie szybkie sprawdzenie, czy aplikacja jest dostępna do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="9f63f-266">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="9f63f-267">Gdy aplikacja przejdzie kontrolę gotowości, nie ma potrzeby dalszej obciążeń aplikacji przy użyciu kosztownego zestawu kontroli gotowości&mdash;dalsze sprawdzenia wymagają tylko sprawdzenia dostępności.</span><span class="sxs-lookup"><span data-stu-id="9f63f-267">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="9f63f-268">Przykładowa aplikacja zawiera kontrolę kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [hostowanej usłudze](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="9f63f-268">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="9f63f-269">`StartupHostedServiceHealthCheck` uwidacznia właściwość, `StartupTaskCompleted`, że usługa hostowana może zostać ustawiona na `true` po zakończeniu długotrwałego zadania (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-269">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="9f63f-270">Długotrwałe zadanie w tle jest uruchamiane przez [hostowaną usługę](xref:fundamentals/host/hosted-services) (*usługi/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="9f63f-270">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="9f63f-271">Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` jest ustawiona na `true`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-271">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="9f63f-272">Kontrola kondycji jest zarejestrowana w <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices` wraz z usługą hostowaną.</span><span class="sxs-lookup"><span data-stu-id="9f63f-272">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="9f63f-273">Ponieważ usługa hostowana musi ustawić właściwość na sprawdzaniu kondycji, sprawdzanie kondycji jest również rejestrowane w kontenerze usługi (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-273">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9f63f-274">Punkt końcowy sprawdzania kondycji jest tworzony przez wywołanie `MapHealthChecks` w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-274">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="9f63f-275">W przykładowej aplikacji punkty końcowe sprawdzania kondycji są tworzone w:</span><span class="sxs-lookup"><span data-stu-id="9f63f-275">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="9f63f-276">`/health/ready` kontroli gotowości.</span><span class="sxs-lookup"><span data-stu-id="9f63f-276">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="9f63f-277">Sprawdzanie gotowości filtruje kontrolę kondycji w celu sprawdzenia kondycji za pomocą tagu `ready`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-277">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="9f63f-278">`/health/live` do sprawdzenia na żywo.</span><span class="sxs-lookup"><span data-stu-id="9f63f-278">`/health/live` for the liveness check.</span></span> <span data-ttu-id="9f63f-279">Sprawdzenie stanu na żywo filtruje `StartupHostedServiceHealthCheck` przez zwrócenie `false` w [predykacie HealthCheckOptions.](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (Aby uzyskać więcej informacji, zobacz [Sprawdzanie kondycji filtru](#filter-health-checks))</span><span class="sxs-lookup"><span data-stu-id="9f63f-279">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="9f63f-280">W poniższym przykładowym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9f63f-280">In the following example code:</span></span>

* <span data-ttu-id="9f63f-281">Sprawdzenie gotowości używa wszystkich zarejestrowanych testów ze znacznikiem "gotowe".</span><span class="sxs-lookup"><span data-stu-id="9f63f-281">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="9f63f-282">`Predicate` wyklucza wszystkie testy i zwróci 200-OK.</span><span class="sxs-lookup"><span data-stu-id="9f63f-282">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

<span data-ttu-id="9f63f-283">Aby uruchomić scenariusz konfiguracji gotowości/na żywo za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-283">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="9f63f-284">W przeglądarce odwiedź `/health/ready` kilka razy do 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-284">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="9f63f-285">Sprawdzanie kondycji raportuje w *złej kondycji* przez pierwsze 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-285">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="9f63f-286">Po upływie 15 sekund punkt końcowy zgłasza w *dobrej kondycji*, co odzwierciedla ukończenie długotrwałego zadania wykonywanego przez usługę hostowaną.</span><span class="sxs-lookup"><span data-stu-id="9f63f-286">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="9f63f-287">Ten przykład tworzy również Wydawca sprawdzania kondycji (implementacja<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>), który uruchamia pierwsze sprawdzenie gotowości z dwoma drugim opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="9f63f-287">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="9f63f-288">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Sprawdzanie kondycji wydawcy](#health-check-publisher) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-288">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="9f63f-289">Przykład Kubernetes</span><span class="sxs-lookup"><span data-stu-id="9f63f-289">Kubernetes example</span></span>

<span data-ttu-id="9f63f-290">Korzystanie z odrębnych gotowości i kontroli na żywo jest przydatne w środowisku, takim jak [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-290">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="9f63f-291">W programie Kubernetes aplikacja może być wymagana do wykonywania czasochłonnych zadań uruchamiania przed zaakceptowaniem żądań, takich jak Test dostępności źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-291">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="9f63f-292">Przy użyciu osobnych kontroli program Orchestrator może rozróżnić, czy aplikacja działa, ale nie jest jeszcze gotowa lub Jeśli uruchomienie aplikacji nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="9f63f-292">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="9f63f-293">Aby uzyskać więcej informacji na temat gotowości i sondy na żywo w programie Kubernetes, zobacz [Konfigurowanie sondy na żywo i gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="9f63f-293">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="9f63f-294">Poniższy przykład demonstruje konfigurację sondowania gotowości Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="9f63f-294">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="9f63f-295">Sondowanie oparte na metrykach z niestandardowym modułem zapisywania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="9f63f-295">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="9f63f-296">Przykładowa aplikacja demonstruje kontrolę kondycji pamięci za pomocą modułu zapisywania odpowiedzi niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-296">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="9f63f-297">`MemoryHealthCheck` zgłasza stan obniżonej wydajności, jeśli aplikacja używa więcej niż danego progu pamięci (1 GB w przykładowej aplikacji).</span><span class="sxs-lookup"><span data-stu-id="9f63f-297">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="9f63f-298"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> obejmuje informacje dotyczące modułu wyrzucania elementów bezużytecznych (GC) dla aplikacji (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-298">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="9f63f-299">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-299">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-300">Zamiast włączać kontrolę kondycji przez przekazanie jej do <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` jest zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="9f63f-300">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="9f63f-301">Wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> zarejestrowane usługi są dostępne dla usług sprawdzania kondycji i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9f63f-301">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="9f63f-302">Zalecamy rejestrację usług sprawdzania kondycji jako usług pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-302">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="9f63f-303">W przykładowej aplikacji (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-303">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="9f63f-304">Punkt końcowy sprawdzania kondycji jest tworzony przez wywołanie `MapHealthChecks` w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-304">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="9f63f-305">Do właściwości `ResponseWriter` jest dostarczany delegat `WriteResponse`, który będzie wyprowadzał niestandardową odpowiedź JSON, gdy jest wykonywane sprawdzanie kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-305">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="9f63f-306">Metoda `WriteResponse` formatuje `CompositeHealthCheckResult` do obiektu JSON i generuje dane wyjściowe JSON dla odpowiedzi kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-306">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="9f63f-307">Aby uruchomić sondę opartą na metrykach z niestandardowymi danymi wyjściowymi modułu zapisywania odpowiedzi przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-307">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="9f63f-308">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje scenariusze sprawdzania kondycji oparte na metrykach, w tym magazyn dyskowy i maksymalną liczbę sprawdzeń na żywo.</span><span class="sxs-lookup"><span data-stu-id="9f63f-308">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="9f63f-309">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-309">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="9f63f-310">Filtruj według portu</span><span class="sxs-lookup"><span data-stu-id="9f63f-310">Filter by port</span></span>

<span data-ttu-id="9f63f-311">Wywołaj `RequireHost` na `MapHealthChecks` za pomocą wzorca adresu URL, który określa port, aby ograniczyć żądania sprawdzania kondycji do określonego portu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-311">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="9f63f-312">Jest to zwykle używane w środowisku kontenera w celu udostępnienia portu usług monitorowania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-312">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="9f63f-313">Przykładowa aplikacja konfiguruje port przy użyciu [zmiennej środowiskowej dostawcy konfiguracji](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="9f63f-313">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="9f63f-314">Port jest ustawiany w pliku *profilu launchsettings. JSON* i przekazywać do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="9f63f-314">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="9f63f-315">Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-315">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="9f63f-316">Aby użyć przykładowej aplikacji do zademonstrowania konfiguracji portów zarządzania, Utwórz plik *profilu launchsettings. JSON* w folderze *Właściwości* .</span><span class="sxs-lookup"><span data-stu-id="9f63f-316">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="9f63f-317">Następujące *właściwości/profilu launchsettings. JSON* w przykładowej aplikacji nie znajdują się w plikach projektu przykładowej aplikacji i muszą zostać utworzone ręcznie:</span><span class="sxs-lookup"><span data-stu-id="9f63f-317">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="9f63f-318">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-318">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-319">Utwórz punkt końcowy sprawdzania kondycji, wywołując `MapHealthChecks` w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-319">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="9f63f-320">W przykładowej aplikacji wywołanie `RequireHost` w punkcie końcowym w `Startup.Configure` określa port zarządzania z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-320">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="9f63f-321">Punkty końcowe są tworzone w przykładowej aplikacji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-321">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="9f63f-322">W poniższym przykładowym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9f63f-322">In the following example code:</span></span>

* <span data-ttu-id="9f63f-323">Sprawdzenie gotowości używa wszystkich zarejestrowanych testów ze znacznikiem "gotowe".</span><span class="sxs-lookup"><span data-stu-id="9f63f-323">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="9f63f-324">`Predicate` wyklucza wszystkie testy i zwróci 200-OK.</span><span class="sxs-lookup"><span data-stu-id="9f63f-324">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

> [!NOTE]
> <span data-ttu-id="9f63f-325">Można uniknąć tworzenia pliku *profilu launchsettings. JSON* w przykładowej aplikacji przez ustawienie portu zarządzania jawnie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-325">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="9f63f-326">W *program.cs* , w którym utworzono <xref:Microsoft.Extensions.Hosting.HostBuilder>, Dodaj wywołanie do <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> i Podaj punkt końcowy portu zarządzania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-326">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="9f63f-327">W `Configure` *ManagementPortStartup.cs*należy określić port zarządzania z `RequireHost`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-327">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="9f63f-328">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f63f-328">*Program.cs*:</span></span>
>
> ```csharp
> return new HostBuilder()
>     .ConfigureWebHostDefaults(webBuilder =>
>     {
>         webBuilder.UseKestrel()
>             .ConfigureKestrel(serverOptions =>
>             {
>                 serverOptions.ListenAnyIP(5001);
>             })
>             .UseStartup(startupType);
>     })
>     .Build();
> ```
>
> <span data-ttu-id="9f63f-329">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f63f-329">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="9f63f-330">Aby uruchomić scenariusz konfiguracji portu zarządzania przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-330">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="9f63f-331">Dystrybucja biblioteki kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-331">Distribute a health check library</span></span>

<span data-ttu-id="9f63f-332">Aby rozpowszechnić kontrolę kondycji jako bibliotekę:</span><span class="sxs-lookup"><span data-stu-id="9f63f-332">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="9f63f-333">Napisz kontrolę kondycji implementującą interfejs <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> jako autonomiczną klasę.</span><span class="sxs-lookup"><span data-stu-id="9f63f-333">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="9f63f-334">Klasa może polegać na [iniekcji zależności (di)](xref:fundamentals/dependency-injection), aktywacji typu i [nazwanych opcjach](xref:fundamentals/configuration/options) w celu uzyskania dostępu do danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-334">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="9f63f-335">W logice kontroli kondycji `CheckHealthAsync`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-335">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="9f63f-336">`data1` i `data2` są używane w metodzie do uruchamiania logiki sprawdzania kondycji sondy.</span><span class="sxs-lookup"><span data-stu-id="9f63f-336">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="9f63f-337">`AccessViolationException` jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-337">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="9f63f-338">Gdy wystąpi <xref:System.AccessViolationException>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> zostanie zwrócona z <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, aby umożliwić użytkownikom Konfigurowanie stanu niepowodzenia sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-338">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="9f63f-339">Napisz metodę rozszerzenia z parametrami, które zużywają aplikację, w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-339">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="9f63f-340">W poniższym przykładzie przyjęto założenie, że następująca sygnatura metody kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-340">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="9f63f-341">Poprzednia sygnatura wskazuje, że `ExampleHealthCheck` wymaga dodatkowych danych do przetworzenia logiki sondowania sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-341">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="9f63f-342">Dane są przekazywane delegatom używanym do tworzenia wystąpienia kontroli kondycji, gdy Sprawdzanie kondycji jest zarejestrowane za pomocą metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="9f63f-342">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="9f63f-343">W poniższym przykładzie obiekt wywołujący określa opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="9f63f-343">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="9f63f-344">Nazwa sprawdzania kondycji (`name`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-344">health check name (`name`).</span></span> <span data-ttu-id="9f63f-345">Jeśli `null`, `example_health_check` jest używany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-345">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="9f63f-346">punkt danych ciągu dla kontroli kondycji (`data1`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-346">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="9f63f-347">punkt danych liczb całkowitych dla kontroli kondycji (`data2`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-347">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="9f63f-348">Jeśli `null`, `1` jest używany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-348">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="9f63f-349">stan niepowodzenia (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="9f63f-349">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="9f63f-350">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-350">The default is `null`.</span></span> <span data-ttu-id="9f63f-351">Jeśli `null`, [HealthStatus. zła kondycja](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) jest raportowany pod kątem stanu błędu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-351">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="9f63f-352">Tagi (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-352">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="9f63f-353">Wydawca kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-353">Health Check Publisher</span></span>

<span data-ttu-id="9f63f-354">Gdy <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> zostanie dodana do kontenera usługi, system sprawdzania kondycji okresowo wykonuje sprawdzanie kondycji i wywołuje `PublishAsync` z wynikiem.</span><span class="sxs-lookup"><span data-stu-id="9f63f-354">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="9f63f-355">Jest to przydatne w scenariuszu systemu monitorowania kondycji opartej na wypychaniu, który oczekuje, że każdy proces będzie okresowo wywoływał system monitorowania w celu określenia kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-355">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="9f63f-356">Interfejs <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> ma jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="9f63f-356">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="9f63f-357"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> można ustawić:</span><span class="sxs-lookup"><span data-stu-id="9f63f-357"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="9f63f-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; początkowe opóźnienie zastosowane po uruchomieniu aplikacji przed wykonaniem <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9f63f-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="9f63f-359">Opóźnienie jest stosowane raz podczas uruchamiania i nie ma zastosowania do kolejnych iteracji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-359">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="9f63f-360">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-360">The default value is five seconds.</span></span>
* <span data-ttu-id="9f63f-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; czasie wykonywania <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="9f63f-362">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-362">The default value is 30 seconds.</span></span>
* <span data-ttu-id="9f63f-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Jeśli <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> jest `null` (domyślnie), usługa wydawcy sprawdzania kondycji uruchamia wszystkie zarejestrowane testy kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="9f63f-364">Aby uruchomić podzestaw kontroli kondycji, należy określić funkcję, która filtruje zestaw kontroli.</span><span class="sxs-lookup"><span data-stu-id="9f63f-364">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="9f63f-365">Predykat jest oceniany w każdym okresie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-365">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="9f63f-366"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; limit czasu wykonywania kontroli kondycji dla wszystkich wystąpień <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-366"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="9f63f-367">Użyj <xref:System.Threading.Timeout.InfiniteTimeSpan>, aby wykonać bez limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-367">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="9f63f-368">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-368">The default value is 30 seconds.</span></span>

<span data-ttu-id="9f63f-369">W przykładowej aplikacji `ReadinessPublisher` jest implementacją <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-369">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="9f63f-370">Stan sprawdzania kondycji jest rejestrowany dla każdego sprawdzenia na poziomie dziennika:</span><span class="sxs-lookup"><span data-stu-id="9f63f-370">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="9f63f-371">Informacje (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>), jeśli stan sprawdzania kondycji jest <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-371">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="9f63f-372">Błąd (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>), jeśli stan ma wartość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> lub <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-372">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="9f63f-373">W przykładzie `LivenessProbeStartup` przykładowej aplikacji sprawdzanie gotowości `StartupHostedService` ma dwa drugie opóźnienie uruchamiania i sprawdza co 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-373">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="9f63f-374">Aby uaktywnić implementację <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, przykład rejestruje `ReadinessPublisher` jako usługę pojedynczą w kontenerze [iniekcji zależności (di)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="9f63f-374">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="9f63f-375">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje wydawców dla kilku systemów, w tym [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="9f63f-375">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="9f63f-376">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-376">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="9f63f-377">Ogranicz kontrolę kondycji za pomocą MapWhen</span><span class="sxs-lookup"><span data-stu-id="9f63f-377">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="9f63f-378">Użyj <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*>, aby warunkowo rozgałęziać potok żądania dla punktów końcowych sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-378">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="9f63f-379">W poniższym przykładzie `MapWhen` rozgałęziać potok żądania, aby aktywować program do sprawdzania kondycji w przypadku odebrania żądania GET dla punktu końcowego `api/HealthCheck`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-379">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();
});
```

<span data-ttu-id="9f63f-380">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-380">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9f63f-381">ASP.NET Core oferuje oprogramowanie do sprawdzania kondycji i biblioteki do raportowania kondycji składników infrastruktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-381">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="9f63f-382">Kontrole kondycji są udostępniane przez aplikację jako punkty końcowe HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f63f-382">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="9f63f-383">Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="9f63f-383">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="9f63f-384">Sondy kondycji mogą być używane przez koordynatorów kontenerów i moduły równoważenia obciążenia w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-384">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="9f63f-385">Na przykład koordynator kontenera może odpowiedzieć na niepowodzenie sprawdzania kondycji przez zatrzymanie wdrożenia stopniowego lub ponowne uruchomienie kontenera.</span><span class="sxs-lookup"><span data-stu-id="9f63f-385">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="9f63f-386">Moduł równoważenia obciążenia może reagować na aplikację w złej kondycji przez kierowanie ruchu z nieprawidłowego wystąpienia do prawidłowego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="9f63f-386">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="9f63f-387">Użycie pamięci, dysku i innych zasobów serwera fizycznego może być monitorowane w celu zapewnienia prawidłowego stanu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-387">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="9f63f-388">Kontrole kondycji umożliwiają testowanie zależności aplikacji, takich jak bazy danych i punkty końcowe usług zewnętrznych, w celu potwierdzenia dostępności i normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-388">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="9f63f-389">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f63f-389">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9f63f-390">Przykładowa aplikacja zawiera przykłady scenariuszy opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-390">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="9f63f-391">Aby uruchomić przykładową aplikację dla danego scenariusza, użyj polecenia [dotnet Run](/dotnet/core/tools/dotnet-run) z folderu projektu w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="9f63f-391">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="9f63f-392">Zobacz plik *README.MD* aplikacji przykładowej i opisy scenariuszy w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-392">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f63f-393">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9f63f-393">Prerequisites</span></span>

<span data-ttu-id="9f63f-394">Kontrole kondycji są zwykle używane z zewnętrzną usługą monitorowania lub koordynatorem kontenera w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-394">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="9f63f-395">Przed dodaniem kontroli kondycji do aplikacji należy określić system monitorowania, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-395">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="9f63f-396">System monitorujący określa, jakie typy testów kondycji należy utworzyć i jak skonfigurować ich punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="9f63f-396">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="9f63f-397">Odwołującego się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu w pakiecie [Microsoft. AspNetCore. Diagnostics. HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-397">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="9f63f-398">Przykładowa aplikacja zawiera kod uruchamiania, aby zademonstrować Sprawdzanie kondycji kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="9f63f-398">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="9f63f-399">Scenariusz [sondowania bazy danych](#database-probe) sprawdza kondycję połączenia z bazą danych przy użyciu [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="9f63f-399">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="9f63f-400">Scenariusz [sondowania DbContext](#entity-framework-core-dbcontext-probe) sprawdza bazę danych przy użyciu `DbContext`EF Core.</span><span class="sxs-lookup"><span data-stu-id="9f63f-400">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="9f63f-401">Aby poznać scenariusze baz danych, przykładową aplikację:</span><span class="sxs-lookup"><span data-stu-id="9f63f-401">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="9f63f-402">Tworzy bazę danych i udostępnia jej parametry połączenia w pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="9f63f-402">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="9f63f-403">W pliku projektu znajdują się następujące odwołania do pakietów:</span><span class="sxs-lookup"><span data-stu-id="9f63f-403">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="9f63f-404">AspNetCore. HealthChecks. SqlServer</span><span class="sxs-lookup"><span data-stu-id="9f63f-404">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="9f63f-405">Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="9f63f-405">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="9f63f-406">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-406">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="9f63f-407">W innym scenariuszu sprawdzania kondycji przedstawiono sposób filtrowania kontroli kondycji do portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-407">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="9f63f-408">Przykładowa aplikacja wymaga utworzenia pliku *Properties/profilu launchsettings. JSON* , który zawiera adres URL zarządzania i port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-408">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="9f63f-409">Aby uzyskać więcej informacji, zobacz sekcję [filtrowanie według portów](#filter-by-port) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-409">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="9f63f-410">Podstawowa sonda kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-410">Basic health probe</span></span>

<span data-ttu-id="9f63f-411">W przypadku wielu aplikacji Podstawowa konfiguracja sondy kondycji, która zgłasza dostępność aplikacji do żądań przetwarzania,jest wystarczająca do odnajdywania stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-411">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="9f63f-412">Konfiguracja podstawowa rejestruje usługi sprawdzania kondycji i wywołuje program do sprawdzania kondycji, aby odpowiedzieć na punkt końcowy adresu URL z odpowiedzią na kondycję.</span><span class="sxs-lookup"><span data-stu-id="9f63f-412">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="9f63f-413">Domyślnie żadne określone kontrole kondycji nie są rejestrowane do testowania żadnej konkretnej zależności lub podsystemu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-413">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="9f63f-414">Aplikacja jest uważana za działającą w dobrej kondycji, jeśli jest w stanie reagować na adres URL punktu końcowego kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-414">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="9f63f-415">Domyślny moduł zapisujący odpowiedzi zapisuje stan (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) jako odpowiedź w postaci zwykłego tekstu z powrotem do klienta, co oznacza, że jest to [HealthStatus. zdrowy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus. obniżona](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) [kondycja lub HealthStatus.](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)</span><span class="sxs-lookup"><span data-stu-id="9f63f-415">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="9f63f-416">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-416">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-417">Dodaj punkt końcowy dla oprogramowania do sprawdzania kondycji z <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania żądań `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-417">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="9f63f-418">W przykładowej aplikacji punkt końcowy sprawdzania kondycji jest tworzony w `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-418">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseHealthChecks("/health");
    }
}
```

<span data-ttu-id="9f63f-419">Aby uruchomić podstawowy scenariusz konfiguracji przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-419">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="9f63f-420">Przykład platformy Docker</span><span class="sxs-lookup"><span data-stu-id="9f63f-420">Docker example</span></span>

<span data-ttu-id="9f63f-421">Platforma [Docker](xref:host-and-deploy/docker/index) oferuje wbudowaną `HEALTHCHECK` dyrektywę, której można użyć do sprawdzenia stanu aplikacji korzystającej z podstawowej konfiguracji sprawdzania kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-421">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="9f63f-422">Utwórz kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-422">Create health checks</span></span>

<span data-ttu-id="9f63f-423">Kontrole kondycji są tworzone przez implementację interfejsu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-423">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="9f63f-424">Metoda <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> zwraca <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, który wskazuje na kondycję jako `Healthy`, `Degraded`lub `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-424">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="9f63f-425">Wynik jest zapisywana jako odpowiedź w postaci zwykłego tekstu ze konfigurowalnym kodem stanu (Konfiguracja jest opisana w sekcji [Opcje sprawdzania kondycji](#health-check-options) ).</span><span class="sxs-lookup"><span data-stu-id="9f63f-425">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="9f63f-426"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> może również zwracać opcjonalne pary klucz-wartość.</span><span class="sxs-lookup"><span data-stu-id="9f63f-426"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="9f63f-427">Przykładowe Sprawdzenie kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-427">Example health check</span></span>

<span data-ttu-id="9f63f-428">W poniższej klasie `ExampleHealthCheck` zademonstrowano układ kontroli kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-428">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="9f63f-429">Logika kontroli kondycji jest umieszczana w metodzie `CheckHealthAsync`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-429">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="9f63f-430">Poniższy przykład ustawia zmienną fikcyjną `healthCheckResultHealthy`, aby `true`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-430">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="9f63f-431">Jeśli wartość `healthCheckResultHealthy` jest ustawiona na `false`, zwracany jest stan [HealthCheckResult. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-431">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
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

### <a name="register-health-check-services"></a><span data-ttu-id="9f63f-432">Rejestrowanie usług sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-432">Register health check services</span></span>

<span data-ttu-id="9f63f-433">Typ `ExampleHealthCheck` jest dodawany do usług sprawdzania kondycji w usłudze `Startup.ConfigureServices` z <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span><span class="sxs-lookup"><span data-stu-id="9f63f-433">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="9f63f-434">Przeciążenie <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> pokazane w poniższym przykładzie ustawia stan awarii (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) do raportowania, gdy Sprawdzanie kondycji zgłosi błąd.</span><span class="sxs-lookup"><span data-stu-id="9f63f-434">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="9f63f-435">Jeśli stan niepowodzenia jest ustawiony na wartość `null` (domyślnie), zostanie zgłoszony [HealthStatus. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-435">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="9f63f-436">To przeciążenie jest przydatnym scenariuszem dla autorów biblioteki, w którym stan niepowodzenia wskazywany przez bibliotekę jest wymuszany przez aplikację w przypadku niepowodzenia sprawdzania kondycji, jeśli implementacja sprawdzania kondycji przestrzega tego ustawienia.</span><span class="sxs-lookup"><span data-stu-id="9f63f-436">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="9f63f-437">*Tagi* mogą służyć do filtrowania kontroli kondycji (opisanych w sekcji [Sprawdzanie kondycji filtru](#filter-health-checks) ).</span><span class="sxs-lookup"><span data-stu-id="9f63f-437">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="9f63f-438"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> może również wykonać funkcję lambda.</span><span class="sxs-lookup"><span data-stu-id="9f63f-438"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="9f63f-439">W poniższym przykładzie `Startup.ConfigureServices` nazwa sprawdzania kondycji jest określana jako `Example` i sprawdzanie zawsze zwraca prawidłowy stan:</span><span class="sxs-lookup"><span data-stu-id="9f63f-439">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="9f63f-440">Używanie oprogramowania do sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-440">Use Health Checks Middleware</span></span>

<span data-ttu-id="9f63f-441">W `Startup.Configure`Wywołaj <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania z adresem URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="9f63f-441">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="9f63f-442">Jeśli kontrole kondycji powinny nasłuchiwać określonego portu, Użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, aby ustawić port (opisany dokładniej w sekcji [Filtruj według portu](#filter-by-port) ):</span><span class="sxs-lookup"><span data-stu-id="9f63f-442">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="9f63f-443">Opcje sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-443">Health check options</span></span>

<span data-ttu-id="9f63f-444"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> zapewnić możliwość dostosowania zachowania kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-444"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="9f63f-445">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-445">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="9f63f-446">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="9f63f-446">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="9f63f-447">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9f63f-447">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="9f63f-448">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="9f63f-448">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="9f63f-449">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-449">Filter health checks</span></span>

<span data-ttu-id="9f63f-450">Domyślnie kontrole kondycji oprogramowania pośredniczącego uruchamia wszystkie zarejestrowane testy kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-450">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="9f63f-451">Aby uruchomić podzestaw kontroli kondycji, podaj funkcję, która zwraca wartość logiczną do <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> opcji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-451">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="9f63f-452">W poniższym przykładzie Sprawdzanie kondycji `Bar` jest odfiltrowane przez tag (`bar_tag`) w instrukcji warunkowej funkcji, gdzie `true` jest zwracany tylko wtedy, gdy właściwość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> kontroli kondycji pasuje do `foo_tag` lub `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-452">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), 
                tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="9f63f-453">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="9f63f-453">Customize the HTTP status code</span></span>

<span data-ttu-id="9f63f-454">Użyj <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes>, aby dostosować mapowanie stanu kondycji do kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f63f-454">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="9f63f-455">Następujące przypisania <xref:Microsoft.AspNetCore.Http.StatusCodes> są wartościami domyślnymi używanymi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="9f63f-455">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="9f63f-456">Zmień wartości kodów stanu, aby spełniały Twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-456">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="9f63f-457">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-457">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResultStatusCodes =
    {
        [HealthStatus.Healthy] = StatusCodes.Status200OK,
        [HealthStatus.Degraded] = StatusCodes.Status200OK,
        [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
    }
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="9f63f-458">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="9f63f-458">Suppress cache headers</span></span>

<span data-ttu-id="9f63f-459"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> kontroluje, czy program do sprawdzania kondycji dodaje do odpowiedzi sondy nagłówki HTTP, aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-459"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="9f63f-460">Jeśli wartość jest `false` (domyślnie), oprogramowanie pośredniczące ustawia lub zastępuje nagłówki `Cache-Control`, `Expires`i `Pragma`, aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-460">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="9f63f-461">Jeśli wartość jest `true`, oprogramowanie pośredniczące nie zmodyfikuje nagłówków pamięci podręcznej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-461">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="9f63f-462">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-462">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="9f63f-463">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="9f63f-463">Customize output</span></span>

<span data-ttu-id="9f63f-464">Opcja <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> Pobiera lub ustawia delegata używany do zapisywania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9f63f-464">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="9f63f-465">Domyślnym delegatem jest zapisanie minimalnej odpowiedzi w postaci zwykłego tekstu z wartością ciągu [HealthReport. status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="9f63f-465">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="9f63f-466">W `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-466">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="9f63f-467">Domyślnym delegatem jest zapisanie minimalnej odpowiedzi w postaci zwykłego tekstu z wartością ciągu [HealthReport. status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="9f63f-467">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="9f63f-468">Następujący delegat niestandardowy `WriteResponse`, wyprowadza niestandardową odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="9f63f-468">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
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

<span data-ttu-id="9f63f-469">System kontroli kondycji nie zapewnia wbudowanej obsługi złożonych formatów powrotu JSON, ponieważ format jest specyficzny dla wybranego systemu monitorowania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-469">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="9f63f-470">Możesz dowolnie dostosowywać `JObject` w powyższym przykładzie, aby zaspokoić Twoje potrzeby.</span><span class="sxs-lookup"><span data-stu-id="9f63f-470">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="9f63f-471">Sonda bazy danych</span><span class="sxs-lookup"><span data-stu-id="9f63f-471">Database probe</span></span>

<span data-ttu-id="9f63f-472">Kontrola kondycji może określić zapytanie bazy danych, które ma zostać uruchomione jako test logiczny, aby wskazać, czy baza danych ma zwykle odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="9f63f-472">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="9f63f-473">Przykładowa aplikacja używa [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), biblioteki sprawdzania kondycji dla aplikacji ASP.NET Core, aby przeprowadzić kontrolę kondycji SQL Server bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-473">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="9f63f-474">`AspNetCore.Diagnostics.HealthChecks` wykonuje `SELECT 1` zapytanie względem bazy danych w celu potwierdzenia, że połączenie z bazą danych jest w dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-474">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="9f63f-475">Podczas sprawdzania połączenia z bazą danych za pomocą zapytania wybierz zapytanie, które zwraca szybko.</span><span class="sxs-lookup"><span data-stu-id="9f63f-475">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="9f63f-476">Podejście zapytania uruchamia ryzyko przeciążenia bazy danych i spadek jej wydajności.</span><span class="sxs-lookup"><span data-stu-id="9f63f-476">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="9f63f-477">W większości przypadków uruchomienie zapytania testowego nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="9f63f-477">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="9f63f-478">Wystarczy, że połączenie z bazą danych zostało zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-478">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="9f63f-479">Jeśli okaże się, że konieczne jest uruchomienie zapytania, wybierz proste zapytanie SELECT, takie jak `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-479">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="9f63f-480">Dołącz odwołanie do pakietu do [AspNetCore. HealthChecks. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-480">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="9f63f-481">Podaj prawidłowe parametry połączenia z bazą danych w pliku *appSettings. JSON* przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-481">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="9f63f-482">Aplikacja używa SQL Server bazy danych o nazwie `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-482">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="9f63f-483">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-483">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-484">Przykładowa aplikacja wywołuje metodę `AddSqlServer` przy użyciu parametrów połączenia bazy danych (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-484">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9f63f-485">Wywołaj kontrolę kondycji oprogramowania pośredniczącego w potoku przetwarzania aplikacji w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-485">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="9f63f-486">Aby uruchomić scenariusz sondowania bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-486">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="9f63f-487">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-487">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="9f63f-488">Badanie Entity Framework Core DbContext</span><span class="sxs-lookup"><span data-stu-id="9f63f-488">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="9f63f-489">`DbContext` sprawdza, czy aplikacja może komunikować się z bazą danych skonfigurowaną dla EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-489">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="9f63f-490">Sprawdzanie `DbContext` jest obsługiwane w aplikacjach, które:</span><span class="sxs-lookup"><span data-stu-id="9f63f-490">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="9f63f-491">Użyj [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-491">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="9f63f-492">Dołącz odwołanie do pakietu do [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-492">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="9f63f-493">`AddDbContextCheck<TContext>` rejestruje Sprawdzanie kondycji `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-493">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="9f63f-494">`DbContext` jest dostarczany jako `TContext` do metody.</span><span class="sxs-lookup"><span data-stu-id="9f63f-494">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="9f63f-495">Jest dostępne Przeciążenie umożliwiające skonfigurowanie stanu niepowodzenia, tagów i niestandardowego zapytania testowego.</span><span class="sxs-lookup"><span data-stu-id="9f63f-495">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="9f63f-496">Domyślnie:</span><span class="sxs-lookup"><span data-stu-id="9f63f-496">By default:</span></span>

* <span data-ttu-id="9f63f-497">`DbContextHealthCheck` wywołuje EF Core metodę `CanConnectAsync`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-497">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="9f63f-498">Można dostosować, jaka operacja jest uruchamiana podczas sprawdzania kondycji przy użyciu przeciążeń metody `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-498">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="9f63f-499">Nazwa sprawdzania kondycji jest nazwą typu `TContext`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-499">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="9f63f-500">W przykładowej aplikacji `AppDbContext` są udostępniane `AddDbContextCheck` i rejestrowane jako usługa w `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-500">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9f63f-501">W przykładowej aplikacji `UseHealthChecks` dodaje oprogramowanie do sprawdzania kondycji w programie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-501">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="9f63f-502">Aby uruchomić scenariusz sondowania `DbContext` za pomocą przykładowej aplikacji, upewnij się, że baza danych określona przez parametry połączenia nie istnieje w wystąpieniu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f63f-502">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="9f63f-503">Jeśli baza danych istnieje, usuń ją.</span><span class="sxs-lookup"><span data-stu-id="9f63f-503">If the database exists, delete it.</span></span>

<span data-ttu-id="9f63f-504">Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-504">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="9f63f-505">Po uruchomieniu aplikacji Sprawdź stan kondycji, wysyłając żądanie do punktu końcowego `/health` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="9f63f-505">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="9f63f-506">Baza danych i `AppDbContext` nie istnieją, dlatego aplikacja oferuje następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="9f63f-506">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="9f63f-507">Wyzwól przykładową aplikację, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-507">Trigger the sample app to create the database.</span></span> <span data-ttu-id="9f63f-508">Utwórz żądanie `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-508">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="9f63f-509">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="9f63f-509">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="9f63f-510">Utwórz żądanie do punktu końcowego `/health`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-510">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="9f63f-511">Istnieje baza danych i kontekst, aby aplikacja odpowiadała:</span><span class="sxs-lookup"><span data-stu-id="9f63f-511">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="9f63f-512">Wyzwól przykładową aplikację, aby usunąć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-512">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="9f63f-513">Utwórz żądanie `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-513">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="9f63f-514">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="9f63f-514">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="9f63f-515">Utwórz żądanie do punktu końcowego `/health`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-515">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="9f63f-516">Aplikacja zawiera odpowiedź w złej kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-516">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="9f63f-517">Oddzielne sondy gotowości i na żywo</span><span class="sxs-lookup"><span data-stu-id="9f63f-517">Separate readiness and liveness probes</span></span>

<span data-ttu-id="9f63f-518">W niektórych scenariuszach hostingu jest używana para kontroli kondycji, która odróżnia dwa stany aplikacji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-518">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="9f63f-519">Aplikacja działa, ale jeszcze nie jest gotowa do odbierania żądań.</span><span class="sxs-lookup"><span data-stu-id="9f63f-519">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="9f63f-520">Jest to stan *gotowości*aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-520">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="9f63f-521">Aplikacja działa i odpowiada na żądania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-521">The app is functioning and responding to requests.</span></span> <span data-ttu-id="9f63f-522">Ten stan jest *aktywny*.</span><span class="sxs-lookup"><span data-stu-id="9f63f-522">This state is the app's *liveness*.</span></span>

<span data-ttu-id="9f63f-523">Sprawdzanie gotowości zwykle wykonuje bardziej obszerny i czasochłonny zestaw kontroli w celu ustalenia, czy wszystkie podsystemy i zasoby aplikacji są dostępne.</span><span class="sxs-lookup"><span data-stu-id="9f63f-523">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="9f63f-524">Sprawdzenie na żywo powoduje jedynie szybkie sprawdzenie, czy aplikacja jest dostępna do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="9f63f-524">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="9f63f-525">Gdy aplikacja przejdzie kontrolę gotowości, nie ma potrzeby dalszej obciążeń aplikacji przy użyciu kosztownego zestawu kontroli gotowości&mdash;dalsze sprawdzenia wymagają tylko sprawdzenia dostępności.</span><span class="sxs-lookup"><span data-stu-id="9f63f-525">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="9f63f-526">Przykładowa aplikacja zawiera kontrolę kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [hostowanej usłudze](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="9f63f-526">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="9f63f-527">`StartupHostedServiceHealthCheck` uwidacznia właściwość, `StartupTaskCompleted`, że usługa hostowana może zostać ustawiona na `true` po zakończeniu długotrwałego zadania (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-527">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="9f63f-528">Długotrwałe zadanie w tle jest uruchamiane przez [hostowaną usługę](xref:fundamentals/host/hosted-services) (*usługi/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="9f63f-528">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="9f63f-529">Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` jest ustawiona na `true`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-529">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="9f63f-530">Kontrola kondycji jest zarejestrowana w <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices` wraz z usługą hostowaną.</span><span class="sxs-lookup"><span data-stu-id="9f63f-530">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="9f63f-531">Ponieważ usługa hostowana musi ustawić właściwość na sprawdzaniu kondycji, sprawdzanie kondycji jest również rejestrowane w kontenerze usługi (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-531">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="9f63f-532">Wywołaj Sprawdzanie kondycji oprogramowania pośredniczącego w potoku przetwarzania aplikacji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-532">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="9f63f-533">W przykładowej aplikacji punkty końcowe sprawdzania kondycji są tworzone w `/health/ready` na potrzeby sprawdzania gotowości i `/health/live` na potrzeby kontroli stanu na żywo.</span><span class="sxs-lookup"><span data-stu-id="9f63f-533">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="9f63f-534">Sprawdzanie gotowości filtruje kontrolę kondycji w celu sprawdzenia kondycji za pomocą tagu `ready`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-534">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="9f63f-535">Sprawdzenie stanu na żywo filtruje `StartupHostedServiceHealthCheck` przez zwrócenie `false` do [HealthCheckOptions. predykatu](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (Aby uzyskać więcej informacji, zobacz [Sprawdzanie kondycji filtru](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="9f63f-535">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

```csharp
app.UseHealthChecks("/health/ready", new HealthCheckOptions()
{
    Predicate = (check) => check.Tags.Contains("ready"), 
});

app.UseHealthChecks("/health/live", new HealthCheckOptions()
{
    Predicate = (_) => false
});
```

<span data-ttu-id="9f63f-536">Aby uruchomić scenariusz konfiguracji gotowości/na żywo za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-536">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="9f63f-537">W przeglądarce odwiedź `/health/ready` kilka razy do 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-537">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="9f63f-538">Sprawdzanie kondycji raportuje w *złej kondycji* przez pierwsze 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-538">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="9f63f-539">Po upływie 15 sekund punkt końcowy zgłasza w *dobrej kondycji*, co odzwierciedla ukończenie długotrwałego zadania wykonywanego przez usługę hostowaną.</span><span class="sxs-lookup"><span data-stu-id="9f63f-539">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="9f63f-540">Ten przykład tworzy również Wydawca sprawdzania kondycji (implementacja<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>), który uruchamia pierwsze sprawdzenie gotowości z dwoma drugim opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="9f63f-540">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="9f63f-541">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Sprawdzanie kondycji wydawcy](#health-check-publisher) .</span><span class="sxs-lookup"><span data-stu-id="9f63f-541">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="9f63f-542">Przykład Kubernetes</span><span class="sxs-lookup"><span data-stu-id="9f63f-542">Kubernetes example</span></span>

<span data-ttu-id="9f63f-543">Korzystanie z odrębnych gotowości i kontroli na żywo jest przydatne w środowisku, takim jak [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="9f63f-543">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="9f63f-544">W programie Kubernetes aplikacja może być wymagana do wykonywania czasochłonnych zadań uruchamiania przed zaakceptowaniem żądań, takich jak Test dostępności źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-544">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="9f63f-545">Przy użyciu osobnych kontroli program Orchestrator może rozróżnić, czy aplikacja działa, ale nie jest jeszcze gotowa lub Jeśli uruchomienie aplikacji nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="9f63f-545">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="9f63f-546">Aby uzyskać więcej informacji na temat gotowości i sondy na żywo w programie Kubernetes, zobacz [Konfigurowanie sondy na żywo i gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="9f63f-546">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="9f63f-547">Poniższy przykład demonstruje konfigurację sondowania gotowości Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="9f63f-547">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="9f63f-548">Sondowanie oparte na metrykach z niestandardowym modułem zapisywania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="9f63f-548">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="9f63f-549">Przykładowa aplikacja demonstruje kontrolę kondycji pamięci za pomocą modułu zapisywania odpowiedzi niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-549">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="9f63f-550">`MemoryHealthCheck` zgłasza stan złej kondycji, jeśli aplikacja używa więcej niż danego progu pamięci (1 GB w przykładowej aplikacji).</span><span class="sxs-lookup"><span data-stu-id="9f63f-550">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="9f63f-551"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> obejmuje informacje dotyczące modułu wyrzucania elementów bezużytecznych (GC) dla aplikacji (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-551">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="9f63f-552">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-552">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-553">Zamiast włączać kontrolę kondycji przez przekazanie jej do <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` jest zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="9f63f-553">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="9f63f-554">Wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> zarejestrowane usługi są dostępne dla usług sprawdzania kondycji i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="9f63f-554">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="9f63f-555">Zalecamy rejestrację usług sprawdzania kondycji jako usług pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-555">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="9f63f-556">W przykładowej aplikacji (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-556">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="9f63f-557">Wywołaj Sprawdzanie kondycji oprogramowania pośredniczącego w potoku przetwarzania aplikacji w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-557">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="9f63f-558">Do właściwości `ResponseWriter` jest dostarczany delegat `WriteResponse`, który będzie wyprowadzał niestandardową odpowiedź JSON, gdy jest wykonywane sprawdzanie kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-558">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // This custom writer formats the detailed status as JSON.
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="9f63f-559">Metoda `WriteResponse` formatuje `CompositeHealthCheckResult` do obiektu JSON i generuje dane wyjściowe JSON dla odpowiedzi kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-559">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="9f63f-560">Aby uruchomić sondę opartą na metrykach z niestandardowymi danymi wyjściowymi modułu zapisywania odpowiedzi przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-560">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="9f63f-561">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje scenariusze sprawdzania kondycji oparte na metrykach, w tym magazyn dyskowy i maksymalną liczbę sprawdzeń na żywo.</span><span class="sxs-lookup"><span data-stu-id="9f63f-561">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="9f63f-562">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-562">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="9f63f-563">Filtruj według portu</span><span class="sxs-lookup"><span data-stu-id="9f63f-563">Filter by port</span></span>

<span data-ttu-id="9f63f-564">Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> z portem ogranicza liczbę żądań sprawdzania kondycji do określonego portu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-564">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="9f63f-565">Jest to zwykle używane w środowisku kontenera w celu udostępnienia portu usług monitorowania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-565">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="9f63f-566">Przykładowa aplikacja konfiguruje port przy użyciu [zmiennej środowiskowej dostawcy konfiguracji](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="9f63f-566">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="9f63f-567">Port jest ustawiany w pliku *profilu launchsettings. JSON* i przekazywać do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="9f63f-567">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="9f63f-568">Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-568">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="9f63f-569">Aby użyć przykładowej aplikacji do zademonstrowania konfiguracji portów zarządzania, Utwórz plik *profilu launchsettings. JSON* w folderze *Właściwości* .</span><span class="sxs-lookup"><span data-stu-id="9f63f-569">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="9f63f-570">Następujące *właściwości/profilu launchsettings. JSON* w przykładowej aplikacji nie znajdują się w plikach projektu przykładowej aplikacji i muszą zostać utworzone ręcznie:</span><span class="sxs-lookup"><span data-stu-id="9f63f-570">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="9f63f-571">Rejestrowanie usług sprawdzania kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-571">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9f63f-572">Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> określa port zarządzania (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9f63f-572">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="9f63f-573">Możesz uniknąć tworzenia pliku *profilu launchsettings. JSON* w przykładowej aplikacji, ustawiając adresy URL i port zarządzania jawnie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-573">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="9f63f-574">W *program.cs* , w którym utworzono <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, Dodaj wywołanie do <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> i podaj normalny punkt końcowy odpowiedzi aplikacji oraz punkt końcowy portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="9f63f-574">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="9f63f-575">W *ManagementPortStartup.cs* , gdzie jest wywoływana <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, określ port zarządzania jawnie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-575">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="9f63f-576">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f63f-576">*Program.cs*:</span></span>
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
> <span data-ttu-id="9f63f-577">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f63f-577">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="9f63f-578">Aby uruchomić scenariusz konfiguracji portu zarządzania przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="9f63f-578">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="9f63f-579">Dystrybucja biblioteki kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-579">Distribute a health check library</span></span>

<span data-ttu-id="9f63f-580">Aby rozpowszechnić kontrolę kondycji jako bibliotekę:</span><span class="sxs-lookup"><span data-stu-id="9f63f-580">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="9f63f-581">Napisz kontrolę kondycji implementującą interfejs <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> jako autonomiczną klasę.</span><span class="sxs-lookup"><span data-stu-id="9f63f-581">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="9f63f-582">Klasa może polegać na [iniekcji zależności (di)](xref:fundamentals/dependency-injection), aktywacji typu i [nazwanych opcjach](xref:fundamentals/configuration/options) w celu uzyskania dostępu do danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="9f63f-582">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="9f63f-583">W logice kontroli kondycji `CheckHealthAsync`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-583">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="9f63f-584">`data1` i `data2` są używane w metodzie do uruchamiania logiki sprawdzania kondycji sondy.</span><span class="sxs-lookup"><span data-stu-id="9f63f-584">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="9f63f-585">`AccessViolationException` jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-585">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="9f63f-586">Gdy wystąpi <xref:System.AccessViolationException>, <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> zostanie zwrócona z <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>, aby umożliwić użytkownikom Konfigurowanie stanu niepowodzenia sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-586">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

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
   ```

1. <span data-ttu-id="9f63f-587">Napisz metodę rozszerzenia z parametrami, które zużywają aplikację, w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-587">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="9f63f-588">W poniższym przykładzie przyjęto założenie, że następująca sygnatura metody kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="9f63f-588">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="9f63f-589">Poprzednia sygnatura wskazuje, że `ExampleHealthCheck` wymaga dodatkowych danych do przetworzenia logiki sondowania sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-589">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="9f63f-590">Dane są przekazywane delegatom używanym do tworzenia wystąpienia kontroli kondycji, gdy Sprawdzanie kondycji jest zarejestrowane za pomocą metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="9f63f-590">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="9f63f-591">W poniższym przykładzie obiekt wywołujący określa opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="9f63f-591">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="9f63f-592">Nazwa sprawdzania kondycji (`name`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-592">health check name (`name`).</span></span> <span data-ttu-id="9f63f-593">Jeśli `null`, `example_health_check` jest używany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-593">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="9f63f-594">punkt danych ciągu dla kontroli kondycji (`data1`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-594">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="9f63f-595">punkt danych liczb całkowitych dla kontroli kondycji (`data2`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-595">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="9f63f-596">Jeśli `null`, `1` jest używany.</span><span class="sxs-lookup"><span data-stu-id="9f63f-596">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="9f63f-597">stan niepowodzenia (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="9f63f-597">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="9f63f-598">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="9f63f-598">The default is `null`.</span></span> <span data-ttu-id="9f63f-599">Jeśli `null`, [HealthStatus. zła kondycja](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) jest raportowany pod kątem stanu błędu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-599">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="9f63f-600">Tagi (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="9f63f-600">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="9f63f-601">Wydawca kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="9f63f-601">Health Check Publisher</span></span>

<span data-ttu-id="9f63f-602">Gdy <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> zostanie dodana do kontenera usługi, system sprawdzania kondycji okresowo wykonuje sprawdzanie kondycji i wywołuje `PublishAsync` z wynikiem.</span><span class="sxs-lookup"><span data-stu-id="9f63f-602">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="9f63f-603">Jest to przydatne w scenariuszu systemu monitorowania kondycji opartej na wypychaniu, który oczekuje, że każdy proces będzie okresowo wywoływał system monitorowania w celu określenia kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-603">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="9f63f-604">Interfejs <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> ma jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="9f63f-604">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="9f63f-605"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> można ustawić:</span><span class="sxs-lookup"><span data-stu-id="9f63f-605"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="9f63f-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; początkowe opóźnienie zastosowane po uruchomieniu aplikacji przed wykonaniem <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień.</span><span class="sxs-lookup"><span data-stu-id="9f63f-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="9f63f-607">Opóźnienie jest stosowane raz podczas uruchamiania i nie ma zastosowania do kolejnych iteracji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-607">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="9f63f-608">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-608">The default value is five seconds.</span></span>
* <span data-ttu-id="9f63f-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; czasie wykonywania <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="9f63f-610">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-610">The default value is 30 seconds.</span></span>
* <span data-ttu-id="9f63f-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Jeśli <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> jest `null` (domyślnie), usługa wydawcy sprawdzania kondycji uruchamia wszystkie zarejestrowane testy kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="9f63f-612">Aby uruchomić podzestaw kontroli kondycji, należy określić funkcję, która filtruje zestaw kontroli.</span><span class="sxs-lookup"><span data-stu-id="9f63f-612">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="9f63f-613">Predykat jest oceniany w każdym okresie.</span><span class="sxs-lookup"><span data-stu-id="9f63f-613">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="9f63f-614"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; limit czasu wykonywania kontroli kondycji dla wszystkich wystąpień <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-614"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="9f63f-615">Użyj <xref:System.Threading.Timeout.InfiniteTimeSpan>, aby wykonać bez limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="9f63f-615">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="9f63f-616">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-616">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="9f63f-617">W wersji 2,2 ASP.NET Core ustawienie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> nie jest honorowane przez implementację <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>; ustawia wartość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-617">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="9f63f-618">Ten problem został rozwiązany w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="9f63f-618">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="9f63f-619">W przykładowej aplikacji `ReadinessPublisher` jest implementacją <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-619">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="9f63f-620">Stan sprawdzania kondycji jest rejestrowany dla każdego sprawdzenia jako:</span><span class="sxs-lookup"><span data-stu-id="9f63f-620">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="9f63f-621">Informacje (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>), jeśli stan sprawdzania kondycji jest <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-621">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="9f63f-622">Błąd (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>), jeśli stan ma wartość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> lub <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-622">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="9f63f-623">W przykładzie `LivenessProbeStartup` przykładowej aplikacji sprawdzanie gotowości `StartupHostedService` ma dwa drugie opóźnienie uruchamiania i sprawdza co 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="9f63f-623">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="9f63f-624">Aby uaktywnić implementację <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, przykład rejestruje `ReadinessPublisher` jako usługę pojedynczą w kontenerze [iniekcji zależności (di)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="9f63f-624">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="9f63f-625">Następujące obejście umożliwia dodanie wystąpienia <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> do kontenera usługi, gdy co najmniej jedna inna usługa hostowana została już dodana do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-625">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="9f63f-626">To obejście nie będzie wymagane w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="9f63f-626">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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

> [!NOTE]
> <span data-ttu-id="9f63f-627">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje wydawców dla kilku systemów, w tym [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="9f63f-627">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="9f63f-628">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) nie jest obsługiwana przez firmę Microsoft lub nie są przez nią obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9f63f-628">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="9f63f-629">Ogranicz kontrolę kondycji za pomocą MapWhen</span><span class="sxs-lookup"><span data-stu-id="9f63f-629">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="9f63f-630">Użyj <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*>, aby warunkowo rozgałęziać potok żądania dla punktów końcowych sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="9f63f-630">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="9f63f-631">W poniższym przykładzie `MapWhen` rozgałęziać potok żądania, aby aktywować program do sprawdzania kondycji w przypadku odebrania żądania GET dla punktu końcowego `api/HealthCheck`:</span><span class="sxs-lookup"><span data-stu-id="9f63f-631">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="9f63f-632">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="9f63f-632">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
