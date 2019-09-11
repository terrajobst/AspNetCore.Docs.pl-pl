---
title: Kontrole kondycji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować Sprawdzanie kondycji infrastruktury ASP.NET Core, na przykład aplikacje i bazy danych.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 09/10/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: cc30b3fc67cec42eada20aed494642cf6d88b289
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878435"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="bd23d-103">Kontrole kondycji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bd23d-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="bd23d-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="bd23d-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bd23d-105">ASP.NET Core oferuje oprogramowanie do sprawdzania kondycji i biblioteki do raportowania kondycji składników infrastruktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="bd23d-106">Kontrole kondycji są udostępniane przez aplikację jako punkty końcowe HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd23d-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="bd23d-107">Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="bd23d-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="bd23d-108">Sondy kondycji mogą być używane przez koordynatorów kontenerów i moduły równoważenia obciążenia w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="bd23d-109">Na przykład koordynator kontenera może odpowiedzieć na niepowodzenie sprawdzania kondycji przez zatrzymanie wdrożenia stopniowego lub ponowne uruchomienie kontenera.</span><span class="sxs-lookup"><span data-stu-id="bd23d-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="bd23d-110">Moduł równoważenia obciążenia może reagować na aplikację w złej kondycji przez kierowanie ruchu z nieprawidłowego wystąpienia do prawidłowego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd23d-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="bd23d-111">Użycie pamięci, dysku i innych zasobów serwera fizycznego może być monitorowane w celu zapewnienia prawidłowego stanu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="bd23d-112">Kontrole kondycji umożliwiają testowanie zależności aplikacji, takich jak bazy danych i punkty końcowe usług zewnętrznych, w celu potwierdzenia dostępności i normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="bd23d-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bd23d-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bd23d-114">Przykładowa aplikacja zawiera przykłady scenariuszy opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="bd23d-115">Aby uruchomić przykładową aplikację dla danego scenariusza, użyj polecenia [dotnet Run](/dotnet/core/tools/dotnet-run) z folderu projektu w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="bd23d-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="bd23d-116">Zobacz plik *README.MD* aplikacji przykładowej i opisy scenariuszy w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd23d-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bd23d-117">Prerequisites</span></span>

<span data-ttu-id="bd23d-118">Kontrole kondycji są zwykle używane z zewnętrzną usługą monitorowania lub koordynatorem kontenera w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="bd23d-119">Przed dodaniem kontroli kondycji do aplikacji należy określić system monitorowania, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="bd23d-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="bd23d-120">System monitorujący określa, jakie typy testów kondycji należy utworzyć i jak skonfigurować ich punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="bd23d-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="bd23d-121">Dodaj odwołanie do pakietu do pakietu [Microsoft. AspNetCore. Diagnostics. HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-121">Add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span> <span data-ttu-id="bd23d-122">Aby przeprowadzić kontrolę kondycji przy użyciu Entity Framework Core, Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="bd23d-123">Przykładowa aplikacja zawiera kod uruchamiania, aby zademonstrować Sprawdzanie kondycji kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="bd23d-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="bd23d-124">Scenariusz [sondowania bazy danych](#database-probe) sprawdza kondycję połączenia z bazą danych przy użyciu [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="bd23d-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="bd23d-125">Scenariusz [sondowania DbContext](#entity-framework-core-dbcontext-probe) sprawdza bazę danych przy użyciu EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="bd23d-126">Aby poznać scenariusze baz danych, przykładową aplikację:</span><span class="sxs-lookup"><span data-stu-id="bd23d-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="bd23d-127">Tworzy bazę danych i udostępnia jej parametry połączenia w pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="bd23d-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="bd23d-128">W pliku projektu znajdują się następujące odwołania do pakietów:</span><span class="sxs-lookup"><span data-stu-id="bd23d-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="bd23d-129">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="bd23d-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="bd23d-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="bd23d-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="bd23d-131">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="bd23d-132">W innym scenariuszu sprawdzania kondycji przedstawiono sposób filtrowania kontroli kondycji do portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="bd23d-133">Przykładowa aplikacja wymaga utworzenia pliku *Properties/profilu launchsettings. JSON* , który zawiera adres URL zarządzania i port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="bd23d-134">Aby uzyskać więcej informacji, zobacz sekcję [filtrowanie według portów](#filter-by-port) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="bd23d-135">Podstawowa sonda kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-135">Basic health probe</span></span>

<span data-ttu-id="bd23d-136">W przypadku wielu aplikacji Podstawowa konfiguracja sondy kondycji, która zgłasza dostępność aplikacji do żądań przetwarzania,jest wystarczająca do odnajdywania stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="bd23d-137">Konfiguracja podstawowa rejestruje usługi sprawdzania kondycji i wywołuje program do sprawdzania kondycji, aby odpowiedzieć na punkt końcowy adresu URL z odpowiedzią na kondycję.</span><span class="sxs-lookup"><span data-stu-id="bd23d-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="bd23d-138">Domyślnie żadne określone kontrole kondycji nie są rejestrowane do testowania żadnej konkretnej zależności lub podsystemu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="bd23d-139">Aplikacja jest uważana za działającą w dobrej kondycji, jeśli jest w stanie reagować na adres URL punktu końcowego kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="bd23d-140">Domyślny moduł zapisujący odpowiedzi zapisuje stan (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) jako odpowiedź w postaci zwykłego tekstu z powrotem do klienta, co oznacza, że [HealthStatus. zdrowy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus. obniżone](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) lub [HealthStatus stan złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="bd23d-141">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-142">Utwórz punkt końcowy sprawdzania kondycji, `MapHealthChecks` wywołując `Startup.Configure`w.</span><span class="sxs-lookup"><span data-stu-id="bd23d-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="bd23d-143">W przykładowej aplikacji jest tworzony `/health` punkt końcowy sprawdzania kondycji (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="bd23d-144">Aby uruchomić podstawowy scenariusz konfiguracji przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="bd23d-145">Przykład platformy Docker</span><span class="sxs-lookup"><span data-stu-id="bd23d-145">Docker example</span></span>

<span data-ttu-id="bd23d-146">Platforma [Docker](xref:host-and-deploy/docker/index) oferuje wbudowaną `HEALTHCHECK` dyrektywę, której można użyć do sprawdzenia stanu aplikacji korzystającej z podstawowej konfiguracji sprawdzania kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="bd23d-147">Utwórz kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-147">Create health checks</span></span>

<span data-ttu-id="bd23d-148">Kontrole kondycji są tworzone przez implementację <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="bd23d-149">`Healthy` `Degraded`Metoda zwraca wartość wskazującą kondycję jako, lub `Unhealthy`. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*></span><span class="sxs-lookup"><span data-stu-id="bd23d-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="bd23d-150">Wynik jest zapisywana jako odpowiedź w postaci zwykłego tekstu ze konfigurowalnym kodem stanu (Konfiguracja jest opisana w sekcji [Opcje sprawdzania kondycji](#health-check-options) ).</span><span class="sxs-lookup"><span data-stu-id="bd23d-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="bd23d-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>może również zwracać opcjonalne pary klucz-wartość.</span><span class="sxs-lookup"><span data-stu-id="bd23d-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="bd23d-152">W poniższej `ExampleHealthCheck` klasie przedstawiono układ kontroli kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="bd23d-153">Logika kontroli kondycji jest umieszczana `CheckHealthAsync` w metodzie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="bd23d-154">Poniższy przykład ustawia zmienną `healthCheckResultHealthy`fikcyjną, do. `true`</span><span class="sxs-lookup"><span data-stu-id="bd23d-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="bd23d-155">Jeśli wartość `healthCheckResultHealthy` jest `false`równa, zwracany jest stan [HealthCheckResult. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

## <a name="register-health-check-services"></a><span data-ttu-id="bd23d-156">Rejestrowanie usług sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-156">Register health check services</span></span>

<span data-ttu-id="bd23d-157">Typ jest dodawany do <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> usług sprawdzania kondycji w programie `Startup.ConfigureServices`: `ExampleHealthCheck`</span><span class="sxs-lookup"><span data-stu-id="bd23d-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="bd23d-158">Przeciążenie pokazane w poniższym przykładzie ustawia stan błędu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) w celu raportowania, kiedy Sprawdzanie kondycji zgłasza błąd. <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*></span><span class="sxs-lookup"><span data-stu-id="bd23d-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="bd23d-159">Jeśli stan niepowodzenia jest ustawiony na `null` wartość (domyślnie), zostanie zgłoszony [HealthStatus. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="bd23d-160">To przeciążenie jest przydatnym scenariuszem dla autorów biblioteki, w którym stan niepowodzenia wskazywany przez bibliotekę jest wymuszany przez aplikację w przypadku niepowodzenia sprawdzania kondycji, jeśli implementacja sprawdzania kondycji przestrzega tego ustawienia.</span><span class="sxs-lookup"><span data-stu-id="bd23d-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="bd23d-161">*Tagi* mogą służyć do filtrowania kontroli kondycji (opisanych w sekcji [Sprawdzanie kondycji filtru](#filter-health-checks) ).</span><span class="sxs-lookup"><span data-stu-id="bd23d-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="bd23d-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>można również wykonać funkcję lambda.</span><span class="sxs-lookup"><span data-stu-id="bd23d-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="bd23d-163">W poniższym przykładzie nazwa sprawdzania kondycji jest określona jako `Example` , a sprawdzanie zawsze zwraca prawidłowy stan:</span><span class="sxs-lookup"><span data-stu-id="bd23d-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="bd23d-164">Użyj routingu kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-164">Use Health Checks Routing</span></span>

<span data-ttu-id="bd23d-165">W `Startup.Configure`programie Wywołaj `MapHealthChecks` program Endpoint Builder z adresem URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="bd23d-165">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="bd23d-166">Wymagaj hosta</span><span class="sxs-lookup"><span data-stu-id="bd23d-166">Require host</span></span>

<span data-ttu-id="bd23d-167">Wywołaj `RequireHost` , aby określić co najmniej jeden dozwolony Host dla punktu końcowego sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-167">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="bd23d-168">Hosty powinny być w formacie Unicode, a nie formacie Punycode i mogą zawierać port.</span><span class="sxs-lookup"><span data-stu-id="bd23d-168">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="bd23d-169">Jeśli kolekcja nie zostanie podana, każdy host zostanie zaakceptowany.</span><span class="sxs-lookup"><span data-stu-id="bd23d-169">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="bd23d-170">Aby uzyskać więcej informacji, zobacz sekcję [filtrowanie według portów](#filter-by-port) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-170">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="bd23d-171">Wymagaj autoryzacji</span><span class="sxs-lookup"><span data-stu-id="bd23d-171">Require authorization</span></span>

<span data-ttu-id="bd23d-172">Wywołaj `RequireAuthorization` , aby uruchomić oprogramowanie pośredniczące autoryzacji w punkcie końcowym żądania sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-172">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="bd23d-173">`RequireAuthorization` Przeciążenie akceptuje co najmniej jedną zasadę autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-173">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="bd23d-174">Jeśli nie podano zasad, zostanie użyta domyślna zasada autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-174">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="bd23d-175">Włączanie żądań Cross-Origin (CORS)</span><span class="sxs-lookup"><span data-stu-id="bd23d-175">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="bd23d-176">Mimo że kontrole kondycji są wykonywane ręcznie z przeglądarki, nie jest to typowy scenariusz użycia, można włączyć oprogramowanie do `RequireCors` obsługi mechanizmu CORS, wywołując testy kondycji punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-176">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="bd23d-177">Przeciążenie akceptuje delegata konstruktora zasad CORS (`CorsPolicyBuilder`) lub nazwę zasady. `RequireCors`</span><span class="sxs-lookup"><span data-stu-id="bd23d-177">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="bd23d-178">Jeśli nie podano zasad, zostanie użyta domyślna zasada CORS.</span><span class="sxs-lookup"><span data-stu-id="bd23d-178">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="bd23d-179">Aby uzyskać więcej informacji, zobacz <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="bd23d-179">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="bd23d-180">Opcje sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-180">Health check options</span></span>

<span data-ttu-id="bd23d-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions>możliwość dostosowania zachowania kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="bd23d-182">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-182">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="bd23d-183">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="bd23d-183">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="bd23d-184">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="bd23d-184">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="bd23d-185">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="bd23d-185">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="bd23d-186">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-186">Filter health checks</span></span>

<span data-ttu-id="bd23d-187">Domyślnie kontrole kondycji oprogramowania pośredniczącego uruchamia wszystkie zarejestrowane testy kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-187">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="bd23d-188">Aby uruchomić podzestaw kontroli kondycji, podaj funkcję, która zwraca wartość logiczną dla <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> opcji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-188">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="bd23d-189">W `Bar` poniższym przykładzie Sprawdzanie kondycji jest odfiltrowane przez tag ( `true` `bar_tag`) w instrukcji warunkowej funkcji, gdzie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> jest zwracane tylko wtedy, gdy właściwość sprawdzania kondycji jest zgodna `foo_tag` lub `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="bd23d-189">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="bd23d-190">W `Startup.ConfigureServices`programie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-190">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="bd23d-191">W `Startup.Configure` programie`Predicate` program filtruje kontrolę kondycji "bar".</span><span class="sxs-lookup"><span data-stu-id="bd23d-191">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="bd23d-192">Tylko Foo i baz Execute.:</span><span class="sxs-lookup"><span data-stu-id="bd23d-192">Only Foo and Baz execute.:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="bd23d-193">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="bd23d-193">Customize the HTTP status code</span></span>

<span data-ttu-id="bd23d-194">Użyj <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> , aby dostosować mapowanie stanu kondycji do kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd23d-194">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="bd23d-195">Następujące <xref:Microsoft.AspNetCore.Http.StatusCodes> przypisania są wartościami domyślnymi używanymi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="bd23d-195">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="bd23d-196">Zmień wartości kodów stanu, aby spełniały Twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-196">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="bd23d-197">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-197">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="bd23d-198">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="bd23d-198">Suppress cache headers</span></span>

<span data-ttu-id="bd23d-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>Określa, czy kontrole kondycji powodują dodanie nagłówków HTTP do odpowiedzi sondy, aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="bd23d-200">Jeśli wartość jest `false` (domyślnie), oprogramowanie pośredniczące ustawia lub `Cache-Control`zastępuje nagłówki, `Expires`i `Pragma` , aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-200">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="bd23d-201">Jeśli wartość to `true`, oprogramowanie pośredniczące nie modyfikuje nagłówków pamięci podręcznej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-201">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="bd23d-202">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-202">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="bd23d-203">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="bd23d-203">Customize output</span></span>

<span data-ttu-id="bd23d-204"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> Opcja Pobiera lub ustawia delegata używany do zapisywania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-204">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span>

<span data-ttu-id="bd23d-205">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="bd23d-206">Domyślnym delegatem jest zapisanie minimalnej odpowiedzi w postaci zwykłego tekstu z wartością ciągu [HealthReport. status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="bd23d-206">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="bd23d-207">Następujący delegat `WriteResponse`niestandardowy,, wyprowadza niestandardową odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="bd23d-207">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="bd23d-208">System kontroli kondycji nie zapewnia wbudowanej obsługi złożonych formatów powrotu JSON, ponieważ format jest specyficzny dla wybranego systemu monitorowania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-208">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="bd23d-209">Możesz dowolnie dostosowywać `JObject` w powyższym przykładzie, aby zaspokoić Twoje potrzeby.</span><span class="sxs-lookup"><span data-stu-id="bd23d-209">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="bd23d-210">Sonda bazy danych</span><span class="sxs-lookup"><span data-stu-id="bd23d-210">Database probe</span></span>

<span data-ttu-id="bd23d-211">Kontrola kondycji może określić zapytanie bazy danych, które ma zostać uruchomione jako test logiczny, aby wskazać, czy baza danych ma zwykle odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="bd23d-211">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="bd23d-212">Przykładowa aplikacja używa [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), biblioteki sprawdzania kondycji dla aplikacji ASP.NET Core, aby przeprowadzić kontrolę kondycji SQL Server bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-212">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="bd23d-213">`AspNetCore.Diagnostics.HealthChecks``SELECT 1` wykonuje zapytanie względem bazy danych w celu potwierdzenia, że połączenie z bazą danych jest w dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-213">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="bd23d-214">Podczas sprawdzania połączenia z bazą danych za pomocą zapytania wybierz zapytanie, które zwraca szybko.</span><span class="sxs-lookup"><span data-stu-id="bd23d-214">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="bd23d-215">Podejście zapytania uruchamia ryzyko przeciążenia bazy danych i spadek jej wydajności.</span><span class="sxs-lookup"><span data-stu-id="bd23d-215">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="bd23d-216">W większości przypadków uruchomienie zapytania testowego nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="bd23d-216">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="bd23d-217">Wystarczy, że połączenie z bazą danych zostało zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-217">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="bd23d-218">Jeśli okaże się, że konieczne jest uruchomienie zapytania, wybierz proste zapytanie SELECT, takie jak `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-218">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="bd23d-219">Dołącz odwołanie do pakietu do [AspNetCore. HealthChecks. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-219">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="bd23d-220">Podaj prawidłowe parametry połączenia z bazą danych w pliku *appSettings. JSON* przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-220">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="bd23d-221">Aplikacja używa SQL Server bazy danych o nazwie `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="bd23d-221">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="bd23d-222">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-222">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-223">Przykładowa aplikacja wywołuje `AddSqlServer` metodę z parametrami połączenia bazy danych (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-223">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="bd23d-224">Punkt końcowy sprawdzania kondycji jest tworzony przez `MapHealthChecks` wywołanie `Startup.Configure`w:</span><span class="sxs-lookup"><span data-stu-id="bd23d-224">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="bd23d-225">Aby uruchomić scenariusz sondowania bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-225">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="bd23d-226">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-226">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="bd23d-227">Badanie Entity Framework Core DbContext</span><span class="sxs-lookup"><span data-stu-id="bd23d-227">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="bd23d-228">Sprawdzanie potwierdza, że aplikacja może komunikować się z bazą danych skonfigurowaną dla EF Core `DbContext`. `DbContext`</span><span class="sxs-lookup"><span data-stu-id="bd23d-228">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="bd23d-229">`DbContext` Sprawdzanie jest obsługiwane w aplikacjach, które:</span><span class="sxs-lookup"><span data-stu-id="bd23d-229">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="bd23d-230">Użyj [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-230">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="bd23d-231">Dołącz odwołanie do pakietu do [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-231">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="bd23d-232">`AddDbContextCheck<TContext>`rejestruje kontrolę kondycji dla elementu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-232">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="bd23d-233">`DbContext` Jest dostarczany`TContext` jako do metody.</span><span class="sxs-lookup"><span data-stu-id="bd23d-233">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="bd23d-234">Jest dostępne Przeciążenie umożliwiające skonfigurowanie stanu niepowodzenia, tagów i niestandardowego zapytania testowego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-234">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="bd23d-235">Domyślnie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-235">By default:</span></span>

* <span data-ttu-id="bd23d-236">`CanConnectAsync` Metoda `DbContextHealthCheck` wywołań EF Core.</span><span class="sxs-lookup"><span data-stu-id="bd23d-236">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="bd23d-237">Można dostosować, jaka operacja jest uruchamiana podczas sprawdzania kondycji przy użyciu `AddDbContextCheck` przeciążenia metod.</span><span class="sxs-lookup"><span data-stu-id="bd23d-237">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="bd23d-238">Nazwa sprawdzania kondycji jest nazwą `TContext` typu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-238">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="bd23d-239">W przykładowej aplikacji `AppDbContext` jest `AddDbContextCheck` dostarczany i zarejestrowany jako usługa w `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-239">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="bd23d-240">Punkt końcowy sprawdzania kondycji jest tworzony przez `MapHealthChecks` wywołanie `Startup.Configure`w:</span><span class="sxs-lookup"><span data-stu-id="bd23d-240">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="bd23d-241">Aby uruchomić `DbContext` scenariusz sondowania za pomocą przykładowej aplikacji, upewnij się, że baza danych określona przez parametry połączenia nie istnieje w wystąpieniu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bd23d-241">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="bd23d-242">Jeśli baza danych istnieje, usuń ją.</span><span class="sxs-lookup"><span data-stu-id="bd23d-242">If the database exists, delete it.</span></span>

<span data-ttu-id="bd23d-243">Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-243">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="bd23d-244">Po uruchomieniu aplikacji Sprawdź stan kondycji, wysyłając żądanie do `/health` punktu końcowego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bd23d-244">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="bd23d-245">Baza danych programu `AppDbContext` i nie istnieje, więc aplikacja udostępnia następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="bd23d-245">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="bd23d-246">Wyzwól przykładową aplikację, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-246">Trigger the sample app to create the database.</span></span> <span data-ttu-id="bd23d-247">Wprowadź żądanie do `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-247">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="bd23d-248">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="bd23d-248">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="bd23d-249">Utwórz żądanie do `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-249">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="bd23d-250">Istnieje baza danych i kontekst, aby aplikacja odpowiadała:</span><span class="sxs-lookup"><span data-stu-id="bd23d-250">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="bd23d-251">Wyzwól przykładową aplikację, aby usunąć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-251">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="bd23d-252">Wprowadź żądanie do `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-252">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="bd23d-253">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="bd23d-253">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="bd23d-254">Utwórz żądanie do `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-254">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="bd23d-255">Aplikacja zawiera odpowiedź w złej kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-255">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="bd23d-256">Oddzielne sondy gotowości i na żywo</span><span class="sxs-lookup"><span data-stu-id="bd23d-256">Separate readiness and liveness probes</span></span>

<span data-ttu-id="bd23d-257">W niektórych scenariuszach hostingu jest używana para kontroli kondycji, która odróżnia dwa stany aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-257">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="bd23d-258">Aplikacja działa, ale jeszcze nie jest gotowa do odbierania żądań.</span><span class="sxs-lookup"><span data-stu-id="bd23d-258">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="bd23d-259">Jest to stan *gotowości*aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-259">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="bd23d-260">Aplikacja działa i odpowiada na żądania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-260">The app is functioning and responding to requests.</span></span> <span data-ttu-id="bd23d-261">Ten stan jest *aktywny*.</span><span class="sxs-lookup"><span data-stu-id="bd23d-261">This state is the app's *liveness*.</span></span>

<span data-ttu-id="bd23d-262">Sprawdzanie gotowości zwykle wykonuje bardziej obszerny i czasochłonny zestaw kontroli w celu ustalenia, czy wszystkie podsystemy i zasoby aplikacji są dostępne.</span><span class="sxs-lookup"><span data-stu-id="bd23d-262">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="bd23d-263">Sprawdzenie na żywo powoduje jedynie szybkie sprawdzenie, czy aplikacja jest dostępna do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="bd23d-263">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="bd23d-264">Gdy aplikacja przejdzie kontrolę gotowości, nie ma potrzeby dalszej obciążania aplikacji przy użyciu kosztownego zestawu kontroli&mdash;gotowości sprawdza tylko, czy sprawdzanie dostępności jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="bd23d-264">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="bd23d-265">Przykładowa aplikacja zawiera kontrolę kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [hostowanej usłudze](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="bd23d-265">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="bd23d-266">Uwidacznia właściwość, `StartupTaskCompleted`która może zostać `true` ustawiona przez usługę hostowaną po zakończeniu długotrwałego zadania (StartupHostedServiceHealthCheck.cs): `StartupHostedServiceHealthCheck`</span><span class="sxs-lookup"><span data-stu-id="bd23d-266">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="bd23d-267">Długotrwałe zadanie w tle jest uruchamiane przez [hostowaną usługę](xref:fundamentals/host/hosted-services) (*usługi/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="bd23d-267">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="bd23d-268">Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` jest ustawiony na `true`:</span><span class="sxs-lookup"><span data-stu-id="bd23d-268">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="bd23d-269">Kontrola kondycji jest zarejestrowana <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices` programie w programie wraz z usługą hostowaną.</span><span class="sxs-lookup"><span data-stu-id="bd23d-269">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="bd23d-270">Ponieważ usługa hostowana musi ustawić właściwość na sprawdzaniu kondycji, sprawdzanie kondycji jest również rejestrowane w kontenerze usługi (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-270">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="bd23d-271">Punkt końcowy sprawdzania kondycji jest tworzony przez `MapHealthChecks` wywołanie `Startup.Configure`w.</span><span class="sxs-lookup"><span data-stu-id="bd23d-271">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="bd23d-272">W przykładowej aplikacji punkty końcowe sprawdzania kondycji są tworzone w:</span><span class="sxs-lookup"><span data-stu-id="bd23d-272">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="bd23d-273">`/health/ready`w celu sprawdzenia gotowości.</span><span class="sxs-lookup"><span data-stu-id="bd23d-273">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="bd23d-274">Sprawdzanie gotowości filtruje kontrolę kondycji w celu sprawdzenia kondycji `ready` za pomocą znacznika.</span><span class="sxs-lookup"><span data-stu-id="bd23d-274">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="bd23d-275">`/health/live`na potrzeby kontroli na żywo.</span><span class="sxs-lookup"><span data-stu-id="bd23d-275">`/health/live` for the liveness check.</span></span> <span data-ttu-id="bd23d-276">Sprawdzenie stanu na żywo odfiltruje `StartupHostedServiceHealthCheck` przez zwrócenie `false` elementu [HealthCheckOptions. predykatu](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (Aby uzyskać więcej informacji, zobacz [Sprawdzanie kondycji filtru](#filter-health-checks))</span><span class="sxs-lookup"><span data-stu-id="bd23d-276">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="bd23d-277">W poniższym przykładowym kodzie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-277">In the following example code:</span></span>

* <span data-ttu-id="bd23d-278">Sprawdzenie gotowości używa wszystkich zarejestrowanych testów ze znacznikiem "gotowe".</span><span class="sxs-lookup"><span data-stu-id="bd23d-278">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="bd23d-279">`Predicate` Wyklucza wszystkie testy i zwracają 200-OK.</span><span class="sxs-lookup"><span data-stu-id="bd23d-279">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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

<span data-ttu-id="bd23d-280">Aby uruchomić scenariusz konfiguracji gotowości/na żywo za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-280">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="bd23d-281">W przeglądarce odwiedź `/health/ready` kilka razy do 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-281">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="bd23d-282">Sprawdzanie kondycji raportuje w *złej kondycji* przez pierwsze 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-282">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="bd23d-283">Po upływie 15 sekund punkt końcowy zgłasza w *dobrej kondycji*, co odzwierciedla ukończenie długotrwałego zadania wykonywanego przez usługę hostowaną.</span><span class="sxs-lookup"><span data-stu-id="bd23d-283">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="bd23d-284">W tym przykładzie tworzony jest również Wydawca sprawdzania kondycji (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementacja), który uruchamia pierwsze sprawdzenie gotowości z dwoma drugim opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="bd23d-284">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="bd23d-285">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Sprawdzanie kondycji wydawcy](#health-check-publisher) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-285">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="bd23d-286">Przykład Kubernetes</span><span class="sxs-lookup"><span data-stu-id="bd23d-286">Kubernetes example</span></span>

<span data-ttu-id="bd23d-287">Korzystanie z odrębnych gotowości i kontroli na żywo jest przydatne w środowisku, takim jak [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-287">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="bd23d-288">W programie Kubernetes aplikacja może być wymagana do wykonywania czasochłonnych zadań uruchamiania przed zaakceptowaniem żądań, takich jak Test dostępności źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-288">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="bd23d-289">Przy użyciu osobnych kontroli program Orchestrator może rozróżnić, czy aplikacja działa, ale nie jest jeszcze gotowa lub Jeśli uruchomienie aplikacji nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="bd23d-289">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="bd23d-290">Aby uzyskać więcej informacji na temat gotowości i sondy na żywo w programie Kubernetes, zobacz [Konfigurowanie sondy na żywo i gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="bd23d-290">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="bd23d-291">Poniższy przykład demonstruje konfigurację sondowania gotowości Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="bd23d-291">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="bd23d-292">Sondowanie oparte na metrykach z niestandardowym modułem zapisywania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="bd23d-292">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="bd23d-293">Przykładowa aplikacja demonstruje kontrolę kondycji pamięci za pomocą modułu zapisywania odpowiedzi niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-293">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="bd23d-294">`MemoryHealthCheck`zgłasza stan obniżonej wydajności, jeśli aplikacja używa więcej niż danego progu pamięci (1 GB w przykładowej aplikacji).</span><span class="sxs-lookup"><span data-stu-id="bd23d-294">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="bd23d-295">Zawiera informacje dotyczące modułu wyrzucania elementów bezużytecznych (GC) dla aplikacji (MemoryHealthCheck.cs): <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult></span><span class="sxs-lookup"><span data-stu-id="bd23d-295">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="bd23d-296">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-296">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-297">Zamiast włączać kontrolę kondycji przez przekazanie jej <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>do `MemoryHealthCheck` programu jest zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="bd23d-297">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="bd23d-298">Wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> zarejestrowane usługi są dostępne dla usług sprawdzania kondycji i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-298">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="bd23d-299">Zalecamy rejestrację usług sprawdzania kondycji jako usług pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-299">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="bd23d-300">W przykładowej aplikacji (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-300">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="bd23d-301">Punkt końcowy sprawdzania kondycji jest tworzony przez `MapHealthChecks` wywołanie `Startup.Configure`w.</span><span class="sxs-lookup"><span data-stu-id="bd23d-301">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="bd23d-302">Obiekt delegowany jest dostarczany `ResponseWriter` do właściwości w celu wygenerowania niestandardowej odpowiedzi JSON po zakończeniu sprawdzania kondycji: `WriteResponse`</span><span class="sxs-lookup"><span data-stu-id="bd23d-302">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="bd23d-303">`WriteResponse` Metoda formatujedoobiektuJSONizwracadanewyjścioweJSONdla`CompositeHealthCheckResult` odpowiedzi kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-303">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="bd23d-304">Aby uruchomić sondę opartą na metrykach z niestandardowymi danymi wyjściowymi modułu zapisywania odpowiedzi przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-304">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="bd23d-305">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje scenariusze sprawdzania kondycji oparte na metrykach, w tym magazyn dyskowy i maksymalną liczbę sprawdzeń na żywo.</span><span class="sxs-lookup"><span data-stu-id="bd23d-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="bd23d-306">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-306">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="bd23d-307">Filtruj według portu</span><span class="sxs-lookup"><span data-stu-id="bd23d-307">Filter by port</span></span>

<span data-ttu-id="bd23d-308">Zadzwoń `RequireHost` do wzorca adresu URL, który określa port, aby ograniczyć żądania sprawdzania kondycji do określonego portu. `MapHealthChecks`</span><span class="sxs-lookup"><span data-stu-id="bd23d-308">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="bd23d-309">Jest to zwykle używane w środowisku kontenera w celu udostępnienia portu usług monitorowania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-309">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="bd23d-310">Przykładowa aplikacja konfiguruje port przy użyciu [zmiennej środowiskowej dostawcy konfiguracji](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bd23d-310">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="bd23d-311">Port jest ustawiany w pliku *profilu launchsettings. JSON* i przekazywać do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="bd23d-311">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="bd23d-312">Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-312">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="bd23d-313">Aby użyć przykładowej aplikacji do zademonstrowania konfiguracji portów zarządzania, Utwórz plik *profilu launchsettings. JSON* w folderze *Właściwości* .</span><span class="sxs-lookup"><span data-stu-id="bd23d-313">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="bd23d-314">Następujące *właściwości/profilu launchsettings. JSON* w przykładowej aplikacji nie znajdują się w plikach projektu przykładowej aplikacji i muszą zostać utworzone ręcznie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-314">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="bd23d-315">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-315">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-316">Utwórz punkt końcowy sprawdzania kondycji, `MapHealthChecks` wywołując `Startup.Configure`w.</span><span class="sxs-lookup"><span data-stu-id="bd23d-316">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="bd23d-317">W przykładowej aplikacji wywołanie do `RequireHost` punktu końcowego w programie `Startup.Configure` określa port zarządzania z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-317">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="bd23d-318">Punkty końcowe są tworzone w aplikacji przykładowej `Startup.Configure`w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-318">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="bd23d-319">W poniższym przykładowym kodzie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-319">In the following example code:</span></span>

* <span data-ttu-id="bd23d-320">Sprawdzenie gotowości używa wszystkich zarejestrowanych testów ze znacznikiem "gotowe".</span><span class="sxs-lookup"><span data-stu-id="bd23d-320">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="bd23d-321">`Predicate` Wyklucza wszystkie testy i zwracają 200-OK.</span><span class="sxs-lookup"><span data-stu-id="bd23d-321">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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
> <span data-ttu-id="bd23d-322">Można uniknąć tworzenia pliku *profilu launchsettings. JSON* w przykładowej aplikacji przez ustawienie portu zarządzania jawnie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-322">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="bd23d-323">W *program.cs* , gdzie <xref:Microsoft.Extensions.Hosting.HostBuilder> został utworzony, Dodaj wywołanie do <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> i Podaj punkt końcowy portu zarządzania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-323">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="bd23d-324">W `Configure` programie *ManagementPortStartup.cs*określ port zarządzania przy użyciu `RequireHost`:</span><span class="sxs-lookup"><span data-stu-id="bd23d-324">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="bd23d-325">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bd23d-325">*Program.cs*:</span></span>
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
> <span data-ttu-id="bd23d-326">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bd23d-326">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="bd23d-327">Aby uruchomić scenariusz konfiguracji portu zarządzania przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-327">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="bd23d-328">Dystrybucja biblioteki kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-328">Distribute a health check library</span></span>

<span data-ttu-id="bd23d-329">Aby rozpowszechnić kontrolę kondycji jako bibliotekę:</span><span class="sxs-lookup"><span data-stu-id="bd23d-329">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="bd23d-330">Napisz kontrolę kondycji, która implementuje <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejs jako autonomiczną klasę.</span><span class="sxs-lookup"><span data-stu-id="bd23d-330">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="bd23d-331">Klasa może polegać na [iniekcji zależności (di)](xref:fundamentals/dependency-injection), aktywacji typu i [nazwanych opcjach](xref:fundamentals/configuration/options) w celu uzyskania dostępu do danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-331">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="bd23d-332">W logice `CheckHealthAsync`kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-332">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="bd23d-333">`data1`i `data2` są używane w metodzie do uruchamiania logiki sprawdzania kondycji sondy.</span><span class="sxs-lookup"><span data-stu-id="bd23d-333">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="bd23d-334">`AccessViolationException`jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="bd23d-334">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="bd23d-335">Gdy wystąpi, zostaje zwrócony z <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> , aby umożliwić użytkownikom Konfigurowanie stanu niepowodzenia sprawdzania kondycji. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> <xref:System.AccessViolationException></span><span class="sxs-lookup"><span data-stu-id="bd23d-335">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="bd23d-336">Napisz metodę rozszerzenia z parametrami, które są używane przez zużywaną aplikację `Startup.Configure` w jej metodzie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-336">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="bd23d-337">W poniższym przykładzie przyjęto założenie, że następująca sygnatura metody kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-337">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="bd23d-338">Poprzednia sygnatura wskazuje `ExampleHealthCheck` , że wymaga dodatkowych danych do przetworzenia logiki sondowania sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-338">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="bd23d-339">Dane są przekazywane delegatom używanym do tworzenia wystąpienia kontroli kondycji, gdy Sprawdzanie kondycji jest zarejestrowane za pomocą metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bd23d-339">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="bd23d-340">W poniższym przykładzie obiekt wywołujący określa opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="bd23d-340">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="bd23d-341">Nazwa sprawdzania kondycji`name`().</span><span class="sxs-lookup"><span data-stu-id="bd23d-341">health check name (`name`).</span></span> <span data-ttu-id="bd23d-342">Jeśli `null`jestużywany. `example_health_check`</span><span class="sxs-lookup"><span data-stu-id="bd23d-342">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="bd23d-343">punkt danych ciągu dla kontroli kondycji (`data1`).</span><span class="sxs-lookup"><span data-stu-id="bd23d-343">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="bd23d-344">punkt danych liczb całkowitych dla kontroli kondycji (`data2`).</span><span class="sxs-lookup"><span data-stu-id="bd23d-344">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="bd23d-345">Jeśli `null`jestużywany. `1`</span><span class="sxs-lookup"><span data-stu-id="bd23d-345">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="bd23d-346">stan niepowodzenia<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>().</span><span class="sxs-lookup"><span data-stu-id="bd23d-346">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="bd23d-347">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-347">The default is `null`.</span></span> <span data-ttu-id="bd23d-348">Jeśli `null`dla stanu błędu zostanie zgłoszona wartość [HealthStatus. w złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-348">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="bd23d-349">Tagi (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="bd23d-349">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="bd23d-350">Wydawca kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-350">Health Check Publisher</span></span>

<span data-ttu-id="bd23d-351">Po dodaniu do kontenera usługi system kontroli kondycji okresowo wykonuje sprawdzanie kondycji i wywołuje `PublishAsync` wynik. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher></span><span class="sxs-lookup"><span data-stu-id="bd23d-351">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="bd23d-352">Jest to przydatne w scenariuszu systemu monitorowania kondycji opartej na wypychaniu, który oczekuje, że każdy proces będzie okresowo wywoływał system monitorowania w celu określenia kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-352">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="bd23d-353"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> Interfejs ma jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="bd23d-353">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="bd23d-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions>Zezwól na ustawienie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="bd23d-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>Początkowe opóźnienie stosowane po uruchomieniu aplikacji przed wykonaniem <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. &ndash;</span><span class="sxs-lookup"><span data-stu-id="bd23d-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="bd23d-356">Opóźnienie jest stosowane raz podczas uruchamiania i nie ma zastosowania do kolejnych iteracji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-356">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="bd23d-357">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-357">The default value is five seconds.</span></span>
* <span data-ttu-id="bd23d-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>&ndash; Okres wykonania.<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher></span><span class="sxs-lookup"><span data-stu-id="bd23d-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="bd23d-359">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-359">The default value is 30 seconds.</span></span>
* <span data-ttu-id="bd23d-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>&ndash; Jeśli jest(`null` domyślnie), usługa wydawcy sprawdzania kondycji uruchamia wszystkie zarejestrowane testy kondycji. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate></span><span class="sxs-lookup"><span data-stu-id="bd23d-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="bd23d-361">Aby uruchomić podzestaw kontroli kondycji, należy określić funkcję, która filtruje zestaw kontroli.</span><span class="sxs-lookup"><span data-stu-id="bd23d-361">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="bd23d-362">Predykat jest oceniany w każdym okresie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-362">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="bd23d-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>Limit czasu wykonywania kontroli kondycji dla wszystkich <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. &ndash;</span><span class="sxs-lookup"><span data-stu-id="bd23d-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="bd23d-364">Użyj <xref:System.Threading.Timeout.InfiniteTimeSpan> , aby wykonać bez limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-364">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="bd23d-365">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-365">The default value is 30 seconds.</span></span>

<span data-ttu-id="bd23d-366">W przykładowej aplikacji `ReadinessPublisher` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> jest implementacją.</span><span class="sxs-lookup"><span data-stu-id="bd23d-366">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="bd23d-367">Stan sprawdzania kondycji jest rejestrowany w `Entries` i rejestrowany dla każdej kontroli:</span><span class="sxs-lookup"><span data-stu-id="bd23d-367">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="bd23d-368">W `LivenessProbeStartup` przykładzie przykładowej `StartupHostedService` aplikacji sprawdzanie gotowości ma dwa drugie opóźnienie uruchamiania i sprawdza, co 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-368">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="bd23d-369">Aby uaktywnić <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementację, przykład rejestruje `ReadinessPublisher` się jako usługa pojedyncza w kontenerze [iniekcji zależności (di)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="bd23d-369">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="bd23d-370">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje wydawców dla kilku systemów, w tym [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="bd23d-370">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="bd23d-371">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-371">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="bd23d-372">Ogranicz kontrolę kondycji za pomocą MapWhen</span><span class="sxs-lookup"><span data-stu-id="bd23d-372">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="bd23d-373">Użyj <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> , aby warunkowo rozgałęziać potok żądania dla punktów końcowych sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-373">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="bd23d-374">W poniższym przykładzie `MapWhen` rozgałęzienie potoku żądania w celu aktywowania kontroli kondycji oprogramowania pośredniczącego w przypadku otrzymania żądania `api/HealthCheck` Get dla punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="bd23d-374">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

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

<span data-ttu-id="bd23d-375">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="bd23d-375">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bd23d-376">ASP.NET Core oferuje oprogramowanie do sprawdzania kondycji i biblioteki do raportowania kondycji składników infrastruktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-376">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="bd23d-377">Kontrole kondycji są udostępniane przez aplikację jako punkty końcowe HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd23d-377">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="bd23d-378">Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="bd23d-378">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="bd23d-379">Sondy kondycji mogą być używane przez koordynatorów kontenerów i moduły równoważenia obciążenia w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-379">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="bd23d-380">Na przykład koordynator kontenera może odpowiedzieć na niepowodzenie sprawdzania kondycji przez zatrzymanie wdrożenia stopniowego lub ponowne uruchomienie kontenera.</span><span class="sxs-lookup"><span data-stu-id="bd23d-380">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="bd23d-381">Moduł równoważenia obciążenia może reagować na aplikację w złej kondycji przez kierowanie ruchu z nieprawidłowego wystąpienia do prawidłowego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="bd23d-381">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="bd23d-382">Użycie pamięci, dysku i innych zasobów serwera fizycznego może być monitorowane w celu zapewnienia prawidłowego stanu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-382">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="bd23d-383">Kontrole kondycji umożliwiają testowanie zależności aplikacji, takich jak bazy danych i punkty końcowe usług zewnętrznych, w celu potwierdzenia dostępności i normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-383">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="bd23d-384">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bd23d-384">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bd23d-385">Przykładowa aplikacja zawiera przykłady scenariuszy opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-385">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="bd23d-386">Aby uruchomić przykładową aplikację dla danego scenariusza, użyj polecenia [dotnet Run](/dotnet/core/tools/dotnet-run) z folderu projektu w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="bd23d-386">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="bd23d-387">Zobacz plik *README.MD* aplikacji przykładowej i opisy scenariuszy w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-387">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd23d-388">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bd23d-388">Prerequisites</span></span>

<span data-ttu-id="bd23d-389">Kontrole kondycji są zwykle używane z zewnętrzną usługą monitorowania lub koordynatorem kontenera w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-389">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="bd23d-390">Przed dodaniem kontroli kondycji do aplikacji należy określić system monitorowania, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="bd23d-390">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="bd23d-391">System monitorujący określa, jakie typy testów kondycji należy utworzyć i jak skonfigurować ich punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="bd23d-391">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="bd23d-392">Odwołującego się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu w pakiecie [Microsoft. AspNetCore. Diagnostics. HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-392">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="bd23d-393">Przykładowa aplikacja zawiera kod uruchamiania, aby zademonstrować Sprawdzanie kondycji kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="bd23d-393">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="bd23d-394">Scenariusz [sondowania bazy danych](#database-probe) sprawdza kondycję połączenia z bazą danych przy użyciu [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="bd23d-394">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="bd23d-395">Scenariusz [sondowania DbContext](#entity-framework-core-dbcontext-probe) sprawdza bazę danych przy użyciu EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-395">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="bd23d-396">Aby poznać scenariusze baz danych, przykładową aplikację:</span><span class="sxs-lookup"><span data-stu-id="bd23d-396">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="bd23d-397">Tworzy bazę danych i udostępnia jej parametry połączenia w pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="bd23d-397">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="bd23d-398">W pliku projektu znajdują się następujące odwołania do pakietów:</span><span class="sxs-lookup"><span data-stu-id="bd23d-398">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="bd23d-399">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="bd23d-399">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="bd23d-400">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="bd23d-400">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="bd23d-401">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-401">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="bd23d-402">W innym scenariuszu sprawdzania kondycji przedstawiono sposób filtrowania kontroli kondycji do portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-402">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="bd23d-403">Przykładowa aplikacja wymaga utworzenia pliku *Properties/profilu launchsettings. JSON* , który zawiera adres URL zarządzania i port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-403">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="bd23d-404">Aby uzyskać więcej informacji, zobacz sekcję [filtrowanie według portów](#filter-by-port) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-404">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="bd23d-405">Podstawowa sonda kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-405">Basic health probe</span></span>

<span data-ttu-id="bd23d-406">W przypadku wielu aplikacji Podstawowa konfiguracja sondy kondycji, która zgłasza dostępność aplikacji do żądań przetwarzania,jest wystarczająca do odnajdywania stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-406">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="bd23d-407">Konfiguracja podstawowa rejestruje usługi sprawdzania kondycji i wywołuje program do sprawdzania kondycji, aby odpowiedzieć na punkt końcowy adresu URL z odpowiedzią na kondycję.</span><span class="sxs-lookup"><span data-stu-id="bd23d-407">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="bd23d-408">Domyślnie żadne określone kontrole kondycji nie są rejestrowane do testowania żadnej konkretnej zależności lub podsystemu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-408">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="bd23d-409">Aplikacja jest uważana za działającą w dobrej kondycji, jeśli jest w stanie reagować na adres URL punktu końcowego kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-409">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="bd23d-410">Domyślny moduł zapisujący odpowiedzi zapisuje stan (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) jako odpowiedź w postaci zwykłego tekstu z powrotem do klienta, co oznacza, że [HealthStatus. zdrowy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus. obniżone](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) lub [HealthStatus stan złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-410">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="bd23d-411">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-411">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-412">Dodaj punkt końcowy dla programu testowego kondycji <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> programu z programu w `Startup.Configure`potoku przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-412">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="bd23d-413">W przykładowej aplikacji jest tworzony `/health` punkt końcowy sprawdzania kondycji (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-413">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="bd23d-414">Aby uruchomić podstawowy scenariusz konfiguracji przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-414">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="bd23d-415">Przykład platformy Docker</span><span class="sxs-lookup"><span data-stu-id="bd23d-415">Docker example</span></span>

<span data-ttu-id="bd23d-416">Platforma [Docker](xref:host-and-deploy/docker/index) oferuje wbudowaną `HEALTHCHECK` dyrektywę, której można użyć do sprawdzenia stanu aplikacji korzystającej z podstawowej konfiguracji sprawdzania kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-416">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="bd23d-417">Utwórz kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-417">Create health checks</span></span>

<span data-ttu-id="bd23d-418">Kontrole kondycji są tworzone przez implementację <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-418">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="bd23d-419">`Healthy` `Degraded`Metoda zwraca wartość wskazującą kondycję jako, lub `Unhealthy`. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*></span><span class="sxs-lookup"><span data-stu-id="bd23d-419">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="bd23d-420">Wynik jest zapisywana jako odpowiedź w postaci zwykłego tekstu ze konfigurowalnym kodem stanu (Konfiguracja jest opisana w sekcji [Opcje sprawdzania kondycji](#health-check-options) ).</span><span class="sxs-lookup"><span data-stu-id="bd23d-420">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="bd23d-421"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>może również zwracać opcjonalne pary klucz-wartość.</span><span class="sxs-lookup"><span data-stu-id="bd23d-421"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="bd23d-422">Przykładowe Sprawdzenie kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-422">Example health check</span></span>

<span data-ttu-id="bd23d-423">W poniższej `ExampleHealthCheck` klasie przedstawiono układ kontroli kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-423">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="bd23d-424">Logika kontroli kondycji jest umieszczana `CheckHealthAsync` w metodzie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-424">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="bd23d-425">Poniższy przykład ustawia zmienną `healthCheckResultHealthy`fikcyjną, do. `true`</span><span class="sxs-lookup"><span data-stu-id="bd23d-425">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="bd23d-426">Jeśli wartość `healthCheckResultHealthy` jest `false`równa, zwracany jest stan [HealthCheckResult. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-426">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="bd23d-427">Rejestrowanie usług sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-427">Register health check services</span></span>

<span data-ttu-id="bd23d-428">Typ jest dodawany do usług sprawdzania kondycji w `Startup.ConfigureServices` programie w programie <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>: `ExampleHealthCheck`</span><span class="sxs-lookup"><span data-stu-id="bd23d-428">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="bd23d-429">Przeciążenie pokazane w poniższym przykładzie ustawia stan błędu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) w celu raportowania, kiedy Sprawdzanie kondycji zgłasza błąd. <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*></span><span class="sxs-lookup"><span data-stu-id="bd23d-429">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="bd23d-430">Jeśli stan niepowodzenia jest ustawiony na `null` wartość (domyślnie), zostanie zgłoszony [HealthStatus. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-430">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="bd23d-431">To przeciążenie jest przydatnym scenariuszem dla autorów biblioteki, w którym stan niepowodzenia wskazywany przez bibliotekę jest wymuszany przez aplikację w przypadku niepowodzenia sprawdzania kondycji, jeśli implementacja sprawdzania kondycji przestrzega tego ustawienia.</span><span class="sxs-lookup"><span data-stu-id="bd23d-431">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="bd23d-432">*Tagi* mogą służyć do filtrowania kontroli kondycji (opisanych w sekcji [Sprawdzanie kondycji filtru](#filter-health-checks) ).</span><span class="sxs-lookup"><span data-stu-id="bd23d-432">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="bd23d-433"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>można również wykonać funkcję lambda.</span><span class="sxs-lookup"><span data-stu-id="bd23d-433"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="bd23d-434">W poniższym `Startup.ConfigureServices` przykładzie nazwa sprawdzania kondycji jest określona jako `Example` , a sprawdzanie zawsze zwraca prawidłowy stan:</span><span class="sxs-lookup"><span data-stu-id="bd23d-434">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="bd23d-435">Używanie oprogramowania do sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-435">Use Health Checks Middleware</span></span>

<span data-ttu-id="bd23d-436">W `Startup.Configure`programie Wywołaj <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania z adresem URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="bd23d-436">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="bd23d-437">Jeśli kontrole kondycji powinny nasłuchiwać określonego portu, Użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> , aby ustawić port (dokładniej opisane w sekcji [Filtruj według portu](#filter-by-port) ):</span><span class="sxs-lookup"><span data-stu-id="bd23d-437">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="bd23d-438">Opcje sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-438">Health check options</span></span>

<span data-ttu-id="bd23d-439"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions>możliwość dostosowania zachowania kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-439"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="bd23d-440">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-440">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="bd23d-441">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="bd23d-441">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="bd23d-442">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="bd23d-442">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="bd23d-443">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="bd23d-443">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="bd23d-444">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-444">Filter health checks</span></span>

<span data-ttu-id="bd23d-445">Domyślnie kontrole kondycji oprogramowania pośredniczącego uruchamia wszystkie zarejestrowane testy kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-445">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="bd23d-446">Aby uruchomić podzestaw kontroli kondycji, podaj funkcję, która zwraca wartość logiczną dla <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> opcji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-446">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="bd23d-447">W `Bar` poniższym przykładzie Sprawdzanie kondycji jest odfiltrowane przez tag ( `true` `bar_tag`) w instrukcji warunkowej funkcji, gdzie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> jest zwracane tylko wtedy, gdy właściwość sprawdzania kondycji jest zgodna `foo_tag` lub `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="bd23d-447">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="bd23d-448">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="bd23d-448">Customize the HTTP status code</span></span>

<span data-ttu-id="bd23d-449">Użyj <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> , aby dostosować mapowanie stanu kondycji do kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd23d-449">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="bd23d-450">Następujące <xref:Microsoft.AspNetCore.Http.StatusCodes> przypisania są wartościami domyślnymi używanymi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="bd23d-450">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="bd23d-451">Zmień wartości kodów stanu, aby spełniały Twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-451">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="bd23d-452">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-452">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="bd23d-453">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="bd23d-453">Suppress cache headers</span></span>

<span data-ttu-id="bd23d-454"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>Określa, czy kontrole kondycji powodują dodanie nagłówków HTTP do odpowiedzi sondy, aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-454"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="bd23d-455">Jeśli wartość jest `false` (domyślnie), oprogramowanie pośredniczące ustawia lub `Cache-Control`zastępuje nagłówki, `Expires`i `Pragma` , aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-455">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="bd23d-456">Jeśli wartość to `true`, oprogramowanie pośredniczące nie modyfikuje nagłówków pamięci podręcznej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-456">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="bd23d-457">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-457">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="bd23d-458">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="bd23d-458">Customize output</span></span>

<span data-ttu-id="bd23d-459"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> Opcja Pobiera lub ustawia delegata używany do zapisywania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bd23d-459">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="bd23d-460">Domyślnym delegatem jest zapisanie minimalnej odpowiedzi w postaci zwykłego tekstu z wartością ciągu [HealthReport. status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="bd23d-460">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="bd23d-461">W `Startup.Configure`programie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-461">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="bd23d-462">Domyślnym delegatem jest zapisanie minimalnej odpowiedzi w postaci zwykłego tekstu z wartością ciągu [HealthReport. status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="bd23d-462">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="bd23d-463">Następujący delegat `WriteResponse`niestandardowy,, wyprowadza niestandardową odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="bd23d-463">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="bd23d-464">System kontroli kondycji nie zapewnia wbudowanej obsługi złożonych formatów powrotu JSON, ponieważ format jest specyficzny dla wybranego systemu monitorowania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-464">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="bd23d-465">Możesz dowolnie dostosowywać `JObject` w powyższym przykładzie, aby zaspokoić Twoje potrzeby.</span><span class="sxs-lookup"><span data-stu-id="bd23d-465">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="bd23d-466">Sonda bazy danych</span><span class="sxs-lookup"><span data-stu-id="bd23d-466">Database probe</span></span>

<span data-ttu-id="bd23d-467">Kontrola kondycji może określić zapytanie bazy danych, które ma zostać uruchomione jako test logiczny, aby wskazać, czy baza danych ma zwykle odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="bd23d-467">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="bd23d-468">Przykładowa aplikacja używa [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), biblioteki sprawdzania kondycji dla aplikacji ASP.NET Core, aby przeprowadzić kontrolę kondycji SQL Server bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-468">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="bd23d-469">`AspNetCore.Diagnostics.HealthChecks``SELECT 1` wykonuje zapytanie względem bazy danych w celu potwierdzenia, że połączenie z bazą danych jest w dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-469">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="bd23d-470">Podczas sprawdzania połączenia z bazą danych za pomocą zapytania wybierz zapytanie, które zwraca szybko.</span><span class="sxs-lookup"><span data-stu-id="bd23d-470">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="bd23d-471">Podejście zapytania uruchamia ryzyko przeciążenia bazy danych i spadek jej wydajności.</span><span class="sxs-lookup"><span data-stu-id="bd23d-471">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="bd23d-472">W większości przypadków uruchomienie zapytania testowego nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="bd23d-472">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="bd23d-473">Wystarczy, że połączenie z bazą danych zostało zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-473">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="bd23d-474">Jeśli okaże się, że konieczne jest uruchomienie zapytania, wybierz proste zapytanie SELECT, takie jak `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-474">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="bd23d-475">Dołącz odwołanie do pakietu do [AspNetCore. HealthChecks. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-475">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="bd23d-476">Podaj prawidłowe parametry połączenia z bazą danych w pliku *appSettings. JSON* przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-476">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="bd23d-477">Aplikacja używa SQL Server bazy danych o nazwie `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="bd23d-477">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="bd23d-478">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-478">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-479">Przykładowa aplikacja wywołuje `AddSqlServer` metodę z parametrami połączenia bazy danych (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-479">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="bd23d-480">Wywołaj kontrolę kondycji oprogramowania pośredniczącego w potoku `Startup.Configure`przetwarzania aplikacji w:</span><span class="sxs-lookup"><span data-stu-id="bd23d-480">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="bd23d-481">Aby uruchomić scenariusz sondowania bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-481">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="bd23d-482">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-482">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="bd23d-483">Badanie Entity Framework Core DbContext</span><span class="sxs-lookup"><span data-stu-id="bd23d-483">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="bd23d-484">Sprawdzanie potwierdza, że aplikacja może komunikować się z bazą danych skonfigurowaną dla EF Core `DbContext`. `DbContext`</span><span class="sxs-lookup"><span data-stu-id="bd23d-484">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="bd23d-485">`DbContext` Sprawdzanie jest obsługiwane w aplikacjach, które:</span><span class="sxs-lookup"><span data-stu-id="bd23d-485">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="bd23d-486">Użyj [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-486">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="bd23d-487">Dołącz odwołanie do pakietu do [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-487">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="bd23d-488">`AddDbContextCheck<TContext>`rejestruje kontrolę kondycji dla elementu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-488">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="bd23d-489">`DbContext` Jest dostarczany`TContext` jako do metody.</span><span class="sxs-lookup"><span data-stu-id="bd23d-489">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="bd23d-490">Jest dostępne Przeciążenie umożliwiające skonfigurowanie stanu niepowodzenia, tagów i niestandardowego zapytania testowego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-490">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="bd23d-491">Domyślnie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-491">By default:</span></span>

* <span data-ttu-id="bd23d-492">`CanConnectAsync` Metoda `DbContextHealthCheck` wywołań EF Core.</span><span class="sxs-lookup"><span data-stu-id="bd23d-492">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="bd23d-493">Można dostosować, jaka operacja jest uruchamiana podczas sprawdzania kondycji przy użyciu `AddDbContextCheck` przeciążenia metod.</span><span class="sxs-lookup"><span data-stu-id="bd23d-493">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="bd23d-494">Nazwa sprawdzania kondycji jest nazwą `TContext` typu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-494">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="bd23d-495">W przykładowej aplikacji `AppDbContext` jest `AddDbContextCheck` dostarczany i zarejestrowany jako usługa w `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-495">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="bd23d-496">W przykładowej aplikacji program `UseHealthChecks` dodaje do programu oprogramowanie `Startup.Configure`do sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-496">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="bd23d-497">Aby uruchomić `DbContext` scenariusz sondowania za pomocą przykładowej aplikacji, upewnij się, że baza danych określona przez parametry połączenia nie istnieje w wystąpieniu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bd23d-497">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="bd23d-498">Jeśli baza danych istnieje, usuń ją.</span><span class="sxs-lookup"><span data-stu-id="bd23d-498">If the database exists, delete it.</span></span>

<span data-ttu-id="bd23d-499">Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-499">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="bd23d-500">Po uruchomieniu aplikacji Sprawdź stan kondycji, wysyłając żądanie do `/health` punktu końcowego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bd23d-500">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="bd23d-501">Baza danych programu `AppDbContext` i nie istnieje, więc aplikacja udostępnia następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="bd23d-501">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="bd23d-502">Wyzwól przykładową aplikację, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-502">Trigger the sample app to create the database.</span></span> <span data-ttu-id="bd23d-503">Wprowadź żądanie do `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-503">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="bd23d-504">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="bd23d-504">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="bd23d-505">Utwórz żądanie do `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-505">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="bd23d-506">Istnieje baza danych i kontekst, aby aplikacja odpowiadała:</span><span class="sxs-lookup"><span data-stu-id="bd23d-506">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="bd23d-507">Wyzwól przykładową aplikację, aby usunąć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-507">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="bd23d-508">Wprowadź żądanie do `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-508">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="bd23d-509">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="bd23d-509">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="bd23d-510">Utwórz żądanie do `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-510">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="bd23d-511">Aplikacja zawiera odpowiedź w złej kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-511">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="bd23d-512">Oddzielne sondy gotowości i na żywo</span><span class="sxs-lookup"><span data-stu-id="bd23d-512">Separate readiness and liveness probes</span></span>

<span data-ttu-id="bd23d-513">W niektórych scenariuszach hostingu jest używana para kontroli kondycji, która odróżnia dwa stany aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-513">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="bd23d-514">Aplikacja działa, ale jeszcze nie jest gotowa do odbierania żądań.</span><span class="sxs-lookup"><span data-stu-id="bd23d-514">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="bd23d-515">Jest to stan *gotowości*aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-515">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="bd23d-516">Aplikacja działa i odpowiada na żądania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-516">The app is functioning and responding to requests.</span></span> <span data-ttu-id="bd23d-517">Ten stan jest *aktywny*.</span><span class="sxs-lookup"><span data-stu-id="bd23d-517">This state is the app's *liveness*.</span></span>

<span data-ttu-id="bd23d-518">Sprawdzanie gotowości zwykle wykonuje bardziej obszerny i czasochłonny zestaw kontroli w celu ustalenia, czy wszystkie podsystemy i zasoby aplikacji są dostępne.</span><span class="sxs-lookup"><span data-stu-id="bd23d-518">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="bd23d-519">Sprawdzenie na żywo powoduje jedynie szybkie sprawdzenie, czy aplikacja jest dostępna do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="bd23d-519">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="bd23d-520">Gdy aplikacja przejdzie kontrolę gotowości, nie ma potrzeby dalszej obciążania aplikacji przy użyciu kosztownego zestawu kontroli&mdash;gotowości sprawdza tylko, czy sprawdzanie dostępności jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="bd23d-520">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="bd23d-521">Przykładowa aplikacja zawiera kontrolę kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [hostowanej usłudze](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="bd23d-521">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="bd23d-522">Uwidacznia właściwość, `StartupTaskCompleted`która może zostać `true` ustawiona przez usługę hostowaną po zakończeniu długotrwałego zadania (StartupHostedServiceHealthCheck.cs): `StartupHostedServiceHealthCheck`</span><span class="sxs-lookup"><span data-stu-id="bd23d-522">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="bd23d-523">Długotrwałe zadanie w tle jest uruchamiane przez [hostowaną usługę](xref:fundamentals/host/hosted-services) (*usługi/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="bd23d-523">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="bd23d-524">Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` jest ustawiony na `true`:</span><span class="sxs-lookup"><span data-stu-id="bd23d-524">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="bd23d-525">Kontrola kondycji jest zarejestrowana <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices` programie w programie wraz z usługą hostowaną.</span><span class="sxs-lookup"><span data-stu-id="bd23d-525">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="bd23d-526">Ponieważ usługa hostowana musi ustawić właściwość na sprawdzaniu kondycji, sprawdzanie kondycji jest również rejestrowane w kontenerze usługi (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-526">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="bd23d-527">Wywołaj Sprawdzanie kondycji oprogramowania pośredniczącego w potoku `Startup.Configure`przetwarzania aplikacji w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-527">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="bd23d-528">W przykładowej aplikacji punkty końcowe sprawdzania kondycji są tworzone na `/health/ready` potrzeby kontroli gotowości i `/health/live` kontroli stanu na żywo.</span><span class="sxs-lookup"><span data-stu-id="bd23d-528">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="bd23d-529">Sprawdzanie gotowości filtruje kontrolę kondycji w celu sprawdzenia kondycji `ready` za pomocą znacznika.</span><span class="sxs-lookup"><span data-stu-id="bd23d-529">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="bd23d-530">Sprawdzenie stanu na żywo odfiltruje `StartupHostedServiceHealthCheck` przez zwrócenie `false` elementu [HealthCheckOptions. predykatu](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (Aby uzyskać więcej informacji, zobacz [Sprawdzanie kondycji filtru](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="bd23d-530">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

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

<span data-ttu-id="bd23d-531">Aby uruchomić scenariusz konfiguracji gotowości/na żywo za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-531">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="bd23d-532">W przeglądarce odwiedź `/health/ready` kilka razy do 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-532">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="bd23d-533">Sprawdzanie kondycji raportuje w *złej kondycji* przez pierwsze 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-533">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="bd23d-534">Po upływie 15 sekund punkt końcowy zgłasza w *dobrej kondycji*, co odzwierciedla ukończenie długotrwałego zadania wykonywanego przez usługę hostowaną.</span><span class="sxs-lookup"><span data-stu-id="bd23d-534">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="bd23d-535">W tym przykładzie tworzony jest również Wydawca sprawdzania kondycji (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementacja), który uruchamia pierwsze sprawdzenie gotowości z dwoma drugim opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="bd23d-535">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="bd23d-536">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Sprawdzanie kondycji wydawcy](#health-check-publisher) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-536">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="bd23d-537">Przykład Kubernetes</span><span class="sxs-lookup"><span data-stu-id="bd23d-537">Kubernetes example</span></span>

<span data-ttu-id="bd23d-538">Korzystanie z odrębnych gotowości i kontroli na żywo jest przydatne w środowisku, takim jak [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="bd23d-538">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="bd23d-539">W programie Kubernetes aplikacja może być wymagana do wykonywania czasochłonnych zadań uruchamiania przed zaakceptowaniem żądań, takich jak Test dostępności źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-539">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="bd23d-540">Przy użyciu osobnych kontroli program Orchestrator może rozróżnić, czy aplikacja działa, ale nie jest jeszcze gotowa lub Jeśli uruchomienie aplikacji nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="bd23d-540">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="bd23d-541">Aby uzyskać więcej informacji na temat gotowości i sondy na żywo w programie Kubernetes, zobacz [Konfigurowanie sondy na żywo i gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="bd23d-541">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="bd23d-542">Poniższy przykład demonstruje konfigurację sondowania gotowości Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="bd23d-542">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="bd23d-543">Sondowanie oparte na metrykach z niestandardowym modułem zapisywania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="bd23d-543">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="bd23d-544">Przykładowa aplikacja demonstruje kontrolę kondycji pamięci za pomocą modułu zapisywania odpowiedzi niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-544">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="bd23d-545">`MemoryHealthCheck`zgłasza stan złej kondycji, jeśli aplikacja używa więcej niż danego progu pamięci (1 GB w przykładowej aplikacji).</span><span class="sxs-lookup"><span data-stu-id="bd23d-545">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="bd23d-546">Zawiera informacje dotyczące modułu wyrzucania elementów bezużytecznych (GC) dla aplikacji (MemoryHealthCheck.cs): <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult></span><span class="sxs-lookup"><span data-stu-id="bd23d-546">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="bd23d-547">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-547">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-548">Zamiast włączać kontrolę kondycji przez przekazanie jej <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>do `MemoryHealthCheck` programu jest zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="bd23d-548">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="bd23d-549">Wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> zarejestrowane usługi są dostępne dla usług sprawdzania kondycji i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="bd23d-549">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="bd23d-550">Zalecamy rejestrację usług sprawdzania kondycji jako usług pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-550">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="bd23d-551">W przykładowej aplikacji (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-551">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="bd23d-552">Wywołaj Sprawdzanie kondycji oprogramowania pośredniczącego w potoku `Startup.Configure`przetwarzania aplikacji w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-552">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="bd23d-553">Obiekt delegowany jest dostarczany `ResponseWriter` do właściwości w celu wygenerowania niestandardowej odpowiedzi JSON po zakończeniu sprawdzania kondycji: `WriteResponse`</span><span class="sxs-lookup"><span data-stu-id="bd23d-553">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

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

<span data-ttu-id="bd23d-554">`WriteResponse` Metoda formatujedoobiektuJSONizwracadanewyjścioweJSONdla`CompositeHealthCheckResult` odpowiedzi kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-554">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="bd23d-555">Aby uruchomić sondę opartą na metrykach z niestandardowymi danymi wyjściowymi modułu zapisywania odpowiedzi przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-555">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="bd23d-556">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje scenariusze sprawdzania kondycji oparte na metrykach, w tym magazyn dyskowy i maksymalną liczbę sprawdzeń na żywo.</span><span class="sxs-lookup"><span data-stu-id="bd23d-556">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="bd23d-557">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-557">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="bd23d-558">Filtruj według portu</span><span class="sxs-lookup"><span data-stu-id="bd23d-558">Filter by port</span></span>

<span data-ttu-id="bd23d-559">Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> z portem ogranicza liczbę żądań sprawdzania kondycji do określonego portu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-559">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="bd23d-560">Jest to zwykle używane w środowisku kontenera w celu udostępnienia portu usług monitorowania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-560">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="bd23d-561">Przykładowa aplikacja konfiguruje port przy użyciu [zmiennej środowiskowej dostawcy konfiguracji](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bd23d-561">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="bd23d-562">Port jest ustawiany w pliku *profilu launchsettings. JSON* i przekazywać do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="bd23d-562">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="bd23d-563">Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-563">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="bd23d-564">Aby użyć przykładowej aplikacji do zademonstrowania konfiguracji portów zarządzania, Utwórz plik *profilu launchsettings. JSON* w folderze *Właściwości* .</span><span class="sxs-lookup"><span data-stu-id="bd23d-564">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="bd23d-565">Następujące *właściwości/profilu launchsettings. JSON* w przykładowej aplikacji nie znajdują się w plikach projektu przykładowej aplikacji i muszą zostać utworzone ręcznie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-565">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="bd23d-566">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-566">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bd23d-567">Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> określające port zarządzania (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="bd23d-567">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="bd23d-568">Możesz uniknąć tworzenia pliku *profilu launchsettings. JSON* w przykładowej aplikacji, ustawiając adresy URL i port zarządzania jawnie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-568">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="bd23d-569">W *program.cs* , w <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> którym został utworzony, Dodaj wywołanie do <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> i podaj normalny punkt końcowy odpowiedzi aplikacji oraz punkt końcowy portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="bd23d-569">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="bd23d-570">W *ManagementPortStartup.cs* , <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> gdzie jest wywoływana, określ port zarządzania jawnie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-570">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="bd23d-571">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bd23d-571">*Program.cs*:</span></span>
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
> <span data-ttu-id="bd23d-572">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bd23d-572">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="bd23d-573">Aby uruchomić scenariusz konfiguracji portu zarządzania przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="bd23d-573">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="bd23d-574">Dystrybucja biblioteki kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-574">Distribute a health check library</span></span>

<span data-ttu-id="bd23d-575">Aby rozpowszechnić kontrolę kondycji jako bibliotekę:</span><span class="sxs-lookup"><span data-stu-id="bd23d-575">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="bd23d-576">Napisz kontrolę kondycji, która implementuje <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejs jako autonomiczną klasę.</span><span class="sxs-lookup"><span data-stu-id="bd23d-576">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="bd23d-577">Klasa może polegać na [iniekcji zależności (di)](xref:fundamentals/dependency-injection), aktywacji typu i [nazwanych opcjach](xref:fundamentals/configuration/options) w celu uzyskania dostępu do danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="bd23d-577">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="bd23d-578">W logice `CheckHealthAsync`kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-578">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="bd23d-579">`data1`i `data2` są używane w metodzie do uruchamiania logiki sprawdzania kondycji sondy.</span><span class="sxs-lookup"><span data-stu-id="bd23d-579">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="bd23d-580">`AccessViolationException`jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="bd23d-580">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="bd23d-581">Gdy wystąpi, zostaje zwrócony z <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> , aby umożliwić użytkownikom Konfigurowanie stanu niepowodzenia sprawdzania kondycji. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> <xref:System.AccessViolationException></span><span class="sxs-lookup"><span data-stu-id="bd23d-581">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="bd23d-582">Napisz metodę rozszerzenia z parametrami, które są używane przez zużywaną aplikację `Startup.Configure` w jej metodzie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-582">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="bd23d-583">W poniższym przykładzie przyjęto założenie, że następująca sygnatura metody kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="bd23d-583">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="bd23d-584">Poprzednia sygnatura wskazuje `ExampleHealthCheck` , że wymaga dodatkowych danych do przetworzenia logiki sondowania sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-584">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="bd23d-585">Dane są przekazywane delegatom używanym do tworzenia wystąpienia kontroli kondycji, gdy Sprawdzanie kondycji jest zarejestrowane za pomocą metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="bd23d-585">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="bd23d-586">W poniższym przykładzie obiekt wywołujący określa opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="bd23d-586">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="bd23d-587">Nazwa sprawdzania kondycji`name`().</span><span class="sxs-lookup"><span data-stu-id="bd23d-587">health check name (`name`).</span></span> <span data-ttu-id="bd23d-588">Jeśli `null`jestużywany. `example_health_check`</span><span class="sxs-lookup"><span data-stu-id="bd23d-588">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="bd23d-589">punkt danych ciągu dla kontroli kondycji (`data1`).</span><span class="sxs-lookup"><span data-stu-id="bd23d-589">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="bd23d-590">punkt danych liczb całkowitych dla kontroli kondycji (`data2`).</span><span class="sxs-lookup"><span data-stu-id="bd23d-590">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="bd23d-591">Jeśli `null`jestużywany. `1`</span><span class="sxs-lookup"><span data-stu-id="bd23d-591">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="bd23d-592">stan niepowodzenia<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>().</span><span class="sxs-lookup"><span data-stu-id="bd23d-592">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="bd23d-593">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="bd23d-593">The default is `null`.</span></span> <span data-ttu-id="bd23d-594">Jeśli `null`dla stanu błędu zostanie zgłoszona wartość [HealthStatus. w złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="bd23d-594">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="bd23d-595">Tagi (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="bd23d-595">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="bd23d-596">Wydawca kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="bd23d-596">Health Check Publisher</span></span>

<span data-ttu-id="bd23d-597">Po dodaniu do kontenera usługi system kontroli kondycji okresowo wykonuje sprawdzanie kondycji i wywołuje `PublishAsync` wynik. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher></span><span class="sxs-lookup"><span data-stu-id="bd23d-597">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="bd23d-598">Jest to przydatne w scenariuszu systemu monitorowania kondycji opartej na wypychaniu, który oczekuje, że każdy proces będzie okresowo wywoływał system monitorowania w celu określenia kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-598">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="bd23d-599"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> Interfejs ma jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="bd23d-599">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="bd23d-600"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions>Zezwól na ustawienie:</span><span class="sxs-lookup"><span data-stu-id="bd23d-600"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="bd23d-601"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>Początkowe opóźnienie stosowane po uruchomieniu aplikacji przed wykonaniem <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. &ndash;</span><span class="sxs-lookup"><span data-stu-id="bd23d-601"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="bd23d-602">Opóźnienie jest stosowane raz podczas uruchamiania i nie ma zastosowania do kolejnych iteracji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-602">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="bd23d-603">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-603">The default value is five seconds.</span></span>
* <span data-ttu-id="bd23d-604"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>&ndash; Okres wykonania.<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher></span><span class="sxs-lookup"><span data-stu-id="bd23d-604"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="bd23d-605">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-605">The default value is 30 seconds.</span></span>
* <span data-ttu-id="bd23d-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>&ndash; Jeśli jest(`null` domyślnie), usługa wydawcy sprawdzania kondycji uruchamia wszystkie zarejestrowane testy kondycji. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate></span><span class="sxs-lookup"><span data-stu-id="bd23d-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="bd23d-607">Aby uruchomić podzestaw kontroli kondycji, należy określić funkcję, która filtruje zestaw kontroli.</span><span class="sxs-lookup"><span data-stu-id="bd23d-607">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="bd23d-608">Predykat jest oceniany w każdym okresie.</span><span class="sxs-lookup"><span data-stu-id="bd23d-608">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="bd23d-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>Limit czasu wykonywania kontroli kondycji dla wszystkich <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. &ndash;</span><span class="sxs-lookup"><span data-stu-id="bd23d-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="bd23d-610">Użyj <xref:System.Threading.Timeout.InfiniteTimeSpan> , aby wykonać bez limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="bd23d-610">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="bd23d-611">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-611">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="bd23d-612">W wersji 2,2 ASP.NET Core ustawienie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> nie jest honorowane <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> przez implementację; ustawia wartość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="bd23d-612">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="bd23d-613">Ten problem został rozwiązany w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="bd23d-613">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="bd23d-614">W przykładowej aplikacji `ReadinessPublisher` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> jest implementacją.</span><span class="sxs-lookup"><span data-stu-id="bd23d-614">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="bd23d-615">Stan sprawdzania kondycji jest rejestrowany w `Entries` i rejestrowany dla każdej kontroli:</span><span class="sxs-lookup"><span data-stu-id="bd23d-615">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="bd23d-616">W `LivenessProbeStartup` przykładzie przykładowej `StartupHostedService` aplikacji sprawdzanie gotowości ma dwa drugie opóźnienie uruchamiania i sprawdza, co 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="bd23d-616">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="bd23d-617">Aby uaktywnić <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementację, przykład rejestruje `ReadinessPublisher` się jako usługa pojedyncza w kontenerze [iniekcji zależności (di)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="bd23d-617">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="bd23d-618">Następujące obejście umożliwia dodanie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpienia do kontenera usługi, gdy co najmniej jedna inna usługa hostowana została już dodana do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-618">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="bd23d-619">To obejście nie będzie wymagane w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="bd23d-619">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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
> <span data-ttu-id="bd23d-620">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje wydawców dla kilku systemów, w tym [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="bd23d-620">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="bd23d-621">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bd23d-621">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="bd23d-622">Ogranicz kontrolę kondycji za pomocą MapWhen</span><span class="sxs-lookup"><span data-stu-id="bd23d-622">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="bd23d-623">Użyj <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> , aby warunkowo rozgałęziać potok żądania dla punktów końcowych sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="bd23d-623">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="bd23d-624">W poniższym przykładzie `MapWhen` rozgałęzienie potoku żądania w celu aktywowania kontroli kondycji oprogramowania pośredniczącego w przypadku otrzymania żądania `api/HealthCheck` Get dla punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="bd23d-624">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="bd23d-625">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="bd23d-625">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
