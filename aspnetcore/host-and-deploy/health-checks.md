---
title: Kontrole kondycji w ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować Sprawdzanie kondycji infrastruktury ASP.NET Core, na przykład aplikacje i bazy danych.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: cc2ee50cd887a14fba2141bee13d65e777c16232
ms.sourcegitcommit: 4b00e77f9984ce76356e829cfe7f75f0f61a7a8f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2019
ms.locfileid: "70145759"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="d38dc-103">Kontrole kondycji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d38dc-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="d38dc-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="d38dc-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="d38dc-105">ASP.NET Core oferuje oprogramowanie do sprawdzania kondycji i biblioteki do raportowania kondycji składników infrastruktury aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="d38dc-106">Kontrole kondycji są udostępniane przez aplikację jako punkty końcowe HTTP.</span><span class="sxs-lookup"><span data-stu-id="d38dc-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="d38dc-107">Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:</span><span class="sxs-lookup"><span data-stu-id="d38dc-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="d38dc-108">Sondy kondycji mogą być używane przez koordynatorów kontenerów i moduły równoważenia obciążenia w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="d38dc-109">Na przykład koordynator kontenera może odpowiedzieć na niepowodzenie sprawdzania kondycji przez zatrzymanie wdrożenia stopniowego lub ponowne uruchomienie kontenera.</span><span class="sxs-lookup"><span data-stu-id="d38dc-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="d38dc-110">Moduł równoważenia obciążenia może reagować na aplikację w złej kondycji przez kierowanie ruchu z nieprawidłowego wystąpienia do prawidłowego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="d38dc-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="d38dc-111">Użycie pamięci, dysku i innych zasobów serwera fizycznego może być monitorowane w celu zapewnienia prawidłowego stanu.</span><span class="sxs-lookup"><span data-stu-id="d38dc-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="d38dc-112">Kontrole kondycji umożliwiają testowanie zależności aplikacji, takich jak bazy danych i punkty końcowe usług zewnętrznych, w celu potwierdzenia dostępności i normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="d38dc-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d38dc-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d38dc-114">Przykładowa aplikacja zawiera przykłady scenariuszy opisanych w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="d38dc-115">Aby uruchomić przykładową aplikację dla danego scenariusza, użyj polecenia [dotnet Run](/dotnet/core/tools/dotnet-run) z folderu projektu w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="d38dc-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="d38dc-116">Zobacz plik *README.MD* aplikacji przykładowej i opisy scenariuszy w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d38dc-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d38dc-117">Prerequisites</span></span>

<span data-ttu-id="d38dc-118">Kontrole kondycji są zwykle używane z zewnętrzną usługą monitorowania lub koordynatorem kontenera w celu sprawdzenia stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="d38dc-119">Przed dodaniem kontroli kondycji do aplikacji należy określić system monitorowania, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="d38dc-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="d38dc-120">System monitorujący określa, jakie typy testów kondycji należy utworzyć i jak skonfigurować ich punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="d38dc-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="d38dc-121">Odwołującego się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu w pakiecie [Microsoft. AspNetCore. Diagnostics. HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) .</span><span class="sxs-lookup"><span data-stu-id="d38dc-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="d38dc-122">Przykładowa aplikacja zawiera kod uruchamiania, aby zademonstrować Sprawdzanie kondycji kilku scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="d38dc-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="d38dc-123">Scenariusz [sondowania bazy danych](#database-probe) sprawdza kondycję połączenia z bazą danych przy użyciu [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="d38dc-123">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="d38dc-124">Scenariusz [sondowania DbContext](#entity-framework-core-dbcontext-probe) sprawdza bazę danych przy użyciu EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d38dc-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="d38dc-125">Aby poznać scenariusze baz danych, przykładową aplikację:</span><span class="sxs-lookup"><span data-stu-id="d38dc-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="d38dc-126">Tworzy bazę danych i udostępnia jej parametry połączenia w pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d38dc-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="d38dc-127">W pliku projektu znajdują się następujące odwołania do pakietów:</span><span class="sxs-lookup"><span data-stu-id="d38dc-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="d38dc-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="d38dc-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="d38dc-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="d38dc-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="d38dc-130">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d38dc-130">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="d38dc-131">W innym scenariuszu sprawdzania kondycji przedstawiono sposób filtrowania kontroli kondycji do portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="d38dc-132">Przykładowa aplikacja wymaga utworzenia pliku *Properties/profilu launchsettings. JSON* , który zawiera adres URL zarządzania i port zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="d38dc-133">Aby uzyskać więcej informacji, zobacz sekcję [filtrowanie według portów](#filter-by-port) .</span><span class="sxs-lookup"><span data-stu-id="d38dc-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="d38dc-134">Podstawowa sonda kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-134">Basic health probe</span></span>

<span data-ttu-id="d38dc-135">W przypadku wielu aplikacji Podstawowa konfiguracja sondy kondycji, która zgłasza dostępność aplikacji do żądań przetwarzania,jest wystarczająca do odnajdywania stanu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="d38dc-136">Konfiguracja podstawowa rejestruje usługi sprawdzania kondycji i wywołuje oprogramowanie do sprawdzania kondycji, aby odpowiedzieć na punkt końcowy adresu URL z odpowiedzią na kondycję.</span><span class="sxs-lookup"><span data-stu-id="d38dc-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="d38dc-137">Domyślnie żadne określone kontrole kondycji nie są rejestrowane do testowania żadnej konkretnej zależności lub podsystemu.</span><span class="sxs-lookup"><span data-stu-id="d38dc-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="d38dc-138">Aplikacja jest uważana za działającą w dobrej kondycji, jeśli jest w stanie reagować na adres URL punktu końcowego kondycji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="d38dc-139">Domyślny moduł zapisujący odpowiedzi zapisuje stan (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) jako odpowiedź w postaci zwykłego tekstu z powrotem do klienta, co oznacza, że [HealthStatus. zdrowy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus. obniżone](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) lub [HealthStatus stan złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="d38dc-139">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="d38dc-140">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-140">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d38dc-141">Dodaj oprogramowanie do sprawdzania kondycji <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> z programu w `Startup.Configure`potoku przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-141">Add Health Check Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="d38dc-142">W przykładowej aplikacji jest tworzony `/health` punkt końcowy sprawdzania kondycji (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="d38dc-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="d38dc-143">Aby uruchomić podstawowy scenariusz konfiguracji przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d38dc-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="d38dc-144">Przykład platformy Docker</span><span class="sxs-lookup"><span data-stu-id="d38dc-144">Docker example</span></span>

<span data-ttu-id="d38dc-145">[](xref:host-and-deploy/docker/index) Platforma Docker oferuje wbudowaną `HEALTHCHECK` dyrektywę, której można użyć do sprawdzenia stanu aplikacji korzystającej z podstawowej konfiguracji sprawdzania kondycji:</span><span class="sxs-lookup"><span data-stu-id="d38dc-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="d38dc-146">Utwórz kontrole kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-146">Create health checks</span></span>

<span data-ttu-id="d38dc-147">Kontrole kondycji są tworzone przez implementację <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="d38dc-147">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="d38dc-148"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> Metoda `Task<` zwracawartośćwskazującą`Healthy`kondycję jako ,`Degraded`lub `Unhealthy`. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>`</span><span class="sxs-lookup"><span data-stu-id="d38dc-148">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="d38dc-149">Wynik jest zapisywana jako odpowiedź w postaci zwykłego tekstu ze konfigurowalnym kodem stanu (Konfiguracja jest opisana w sekcji [Opcje sprawdzania kondycji](#health-check-options) ).</span><span class="sxs-lookup"><span data-stu-id="d38dc-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="d38dc-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>może również zwracać opcjonalne pary klucz-wartość.</span><span class="sxs-lookup"><span data-stu-id="d38dc-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="d38dc-151">Przykładowe Sprawdzenie kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-151">Example health check</span></span>

<span data-ttu-id="d38dc-152">W poniższej `ExampleHealthCheck` klasie przedstawiono układ kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="d38dc-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="d38dc-153">Rejestrowanie usług sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-153">Register health check services</span></span>

<span data-ttu-id="d38dc-154">Typ zostanie dodany do usług sprawdzania kondycji przy <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>użyciu: `ExampleHealthCheck`</span><span class="sxs-lookup"><span data-stu-id="d38dc-154">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="d38dc-155">Przeciążenie pokazane w poniższym przykładzie ustawia stan błędu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) w celu raportowania, kiedy Sprawdzanie kondycji zgłasza błąd. <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*></span><span class="sxs-lookup"><span data-stu-id="d38dc-155">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="d38dc-156">Jeśli stan niepowodzenia jest ustawiony na `null` wartość (domyślnie), zostanie zgłoszony [HealthStatus. złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="d38dc-156">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="d38dc-157">To przeciążenie jest przydatnym scenariuszem dla autorów biblioteki, w którym stan niepowodzenia wskazywany przez bibliotekę jest wymuszany przez aplikację w przypadku niepowodzenia sprawdzania kondycji, jeśli implementacja sprawdzania kondycji przestrzega tego ustawienia.</span><span class="sxs-lookup"><span data-stu-id="d38dc-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="d38dc-158">*Tagi* mogą służyć do filtrowania kontroli kondycji (opisanych w sekcji [Sprawdzanie kondycji filtru](#filter-health-checks) ).</span><span class="sxs-lookup"><span data-stu-id="d38dc-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="d38dc-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>można również wykonać funkcję lambda.</span><span class="sxs-lookup"><span data-stu-id="d38dc-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="d38dc-160">W poniższym przykładzie nazwa sprawdzania kondycji jest określona jako `Example` , a sprawdzanie zawsze zwraca prawidłowy stan:</span><span class="sxs-lookup"><span data-stu-id="d38dc-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () =>
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="d38dc-161">Używanie oprogramowania do sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="d38dc-162">W `Startup.Configure`programie Wywołaj <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania z adresem URL punktu końcowego lub ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="d38dc-162">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="d38dc-163">Jeśli kontrole kondycji powinny nasłuchiwać określonego portu, Użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> , aby ustawić port (dokładniej opisane w sekcji [Filtruj według portu](#filter-by-port) ):</span><span class="sxs-lookup"><span data-stu-id="d38dc-163">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="d38dc-164">Oprogramowanie do sprawdzania kondycji jest *terminalem pośredniczącym* w potoku przetwarzania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="d38dc-165">Pierwszy punkt końcowy sprawdzania kondycji napotkał dokładne dopasowanie do adresu URL żądania i krótkiego obwodu pozostałej części potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d38dc-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="d38dc-166">Podczas krótkiego obwodu nie jest wykonywane żadne oprogramowanie pośredniczące po dopasowanym sprawdzaniu kondycji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="d38dc-167">Opcje sprawdzania kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-167">Health check options</span></span>

<span data-ttu-id="d38dc-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions>możliwość dostosowania zachowania kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="d38dc-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="d38dc-169">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="d38dc-170">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="d38dc-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="d38dc-171">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="d38dc-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="d38dc-172">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="d38dc-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="d38dc-173">Filtrowanie kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-173">Filter health checks</span></span>

<span data-ttu-id="d38dc-174">Domyślnie oprogramowanie do sprawdzania kondycji uruchamia wszystkie zarejestrowane testy kondycji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="d38dc-175">Aby uruchomić podzestaw kontroli kondycji, podaj funkcję, która zwraca wartość logiczną dla <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> opcji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-175">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="d38dc-176">W `Bar` poniższym przykładzie Sprawdzanie kondycji jest odfiltrowane przez tag ( `true` `bar_tag`) w instrukcji warunkowej funkcji, gdzie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> jest zwracane tylko wtedy, gdy właściwość sprawdzania kondycji jest zgodna `foo_tag` lub `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="d38dc-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="d38dc-177">Dostosowywanie kodu stanu HTTP</span><span class="sxs-lookup"><span data-stu-id="d38dc-177">Customize the HTTP status code</span></span>

<span data-ttu-id="d38dc-178">Użyj <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> , aby dostosować mapowanie stanu kondycji do kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="d38dc-178">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="d38dc-179">Następujące <xref:Microsoft.AspNetCore.Http.StatusCodes> przypisania są wartościami domyślnymi używanymi przez oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="d38dc-179">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="d38dc-180">Zmień wartości kodów stanu, aby spełniały Twoje wymagania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-180">Change the status code values to meet your requirements.</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="d38dc-181">Pomiń nagłówki pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="d38dc-181">Suppress cache headers</span></span>

<span data-ttu-id="d38dc-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>Określa, czy oprogramowanie do sprawdzania kondycji dodaje nagłówki HTTP do odpowiedzi sondy, aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d38dc-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="d38dc-183">Jeśli wartość jest `false` (domyślnie), oprogramowanie pośredniczące ustawia lub `Cache-Control`zastępuje nagłówki, `Expires`i `Pragma` , aby zapobiec buforowaniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d38dc-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="d38dc-184">Jeśli wartość to `true`, oprogramowanie pośredniczące nie modyfikuje nagłówków pamięci podręcznej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d38dc-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

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

### <a name="customize-output"></a><span data-ttu-id="d38dc-185">Dostosuj dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="d38dc-185">Customize output</span></span>

<span data-ttu-id="d38dc-186"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> Opcja Pobiera lub ustawia delegata używany do zapisywania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d38dc-186">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="d38dc-187">Domyślnym delegatem jest zapisanie minimalnej odpowiedzi w postaci zwykłego tekstu z wartością ciągu [HealthReport. status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="d38dc-187">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

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

## <a name="database-probe"></a><span data-ttu-id="d38dc-188">Sonda bazy danych</span><span class="sxs-lookup"><span data-stu-id="d38dc-188">Database probe</span></span>

<span data-ttu-id="d38dc-189">Kontrola kondycji może określić zapytanie bazy danych, które ma zostać uruchomione jako test logiczny, aby wskazać, czy baza danych ma zwykle odpowiadać.</span><span class="sxs-lookup"><span data-stu-id="d38dc-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="d38dc-190">Przykładowa aplikacja używa [AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), biblioteki sprawdzania kondycji dla aplikacji ASP.NET Core, aby przeprowadzić kontrolę kondycji SQL Server bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d38dc-190">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="d38dc-191">`AspNetCore.Diagnostics.HealthChecks``SELECT 1` wykonuje zapytanie względem bazy danych w celu potwierdzenia, że połączenie z bazą danych jest w dobrej kondycji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-191">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="d38dc-192">Podczas sprawdzania połączenia z bazą danych za pomocą zapytania wybierz zapytanie, które zwraca szybko.</span><span class="sxs-lookup"><span data-stu-id="d38dc-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="d38dc-193">Podejście zapytania uruchamia ryzyko przeciążenia bazy danych i spadek jej wydajności.</span><span class="sxs-lookup"><span data-stu-id="d38dc-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="d38dc-194">W większości przypadków uruchomienie zapytania testowego nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="d38dc-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="d38dc-195">Wystarczy, że połączenie z bazą danych zostało zakończone pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="d38dc-196">Jeśli okaże się, że konieczne jest uruchomienie zapytania, wybierz proste zapytanie SELECT, takie jak `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="d38dc-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="d38dc-197">Dołącz odwołanie do pakietu do [AspNetCore. HealthChecks. SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="d38dc-197">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="d38dc-198">Podaj prawidłowe parametry połączenia z bazą danych w pliku *appSettings. JSON* przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="d38dc-199">Aplikacja używa SQL Server bazy danych o nazwie `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="d38dc-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="d38dc-200">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-200">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d38dc-201">Przykładowa aplikacja wywołuje `AddSqlServer` metodę z parametrami połączenia bazy danych (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="d38dc-201">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="d38dc-202">Wywołaj oprogramowanie pośredniczące sprawdzania kondycji w potoku `Startup.Configure`przetwarzania aplikacji w:</span><span class="sxs-lookup"><span data-stu-id="d38dc-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="d38dc-203">Aby uruchomić scenariusz sondowania bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d38dc-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="d38dc-204">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d38dc-204">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="d38dc-205">Badanie Entity Framework Core DbContext</span><span class="sxs-lookup"><span data-stu-id="d38dc-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="d38dc-206">Sprawdzanie potwierdza, że aplikacja może komunikować się z bazą danych skonfigurowaną dla EF Core `DbContext`. `DbContext`</span><span class="sxs-lookup"><span data-stu-id="d38dc-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="d38dc-207">`DbContext` Sprawdzanie jest obsługiwane w aplikacjach, które:</span><span class="sxs-lookup"><span data-stu-id="d38dc-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="d38dc-208">Użyj [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="d38dc-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="d38dc-209">Dołącz odwołanie do pakietu do [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="d38dc-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="d38dc-210">`AddDbContextCheck<TContext>`rejestruje kontrolę kondycji dla elementu `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d38dc-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="d38dc-211">`DbContext` Jest dostarczany`TContext` jako do metody.</span><span class="sxs-lookup"><span data-stu-id="d38dc-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="d38dc-212">Jest dostępne Przeciążenie umożliwiające skonfigurowanie stanu niepowodzenia, tagów i niestandardowego zapytania testowego.</span><span class="sxs-lookup"><span data-stu-id="d38dc-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="d38dc-213">Domyślnie:</span><span class="sxs-lookup"><span data-stu-id="d38dc-213">By default:</span></span>

* <span data-ttu-id="d38dc-214">`CanConnectAsync` Metoda `DbContextHealthCheck` wywołań EF Core.</span><span class="sxs-lookup"><span data-stu-id="d38dc-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="d38dc-215">Można dostosować, jaka operacja jest uruchamiana podczas sprawdzania kondycji przy użyciu `AddDbContextCheck` przeciążenia metod.</span><span class="sxs-lookup"><span data-stu-id="d38dc-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="d38dc-216">Nazwa sprawdzania kondycji jest nazwą `TContext` typu.</span><span class="sxs-lookup"><span data-stu-id="d38dc-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="d38dc-217">W przykładowej aplikacji `AppDbContext` jest `AddDbContextCheck` dostępna i zarejestrowana jako usługa w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d38dc-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="d38dc-218">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d38dc-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="d38dc-219">W przykładowej aplikacji program `UseHealthChecks` dodaje oprogramowanie do sprawdzania kondycji w `Startup.Configure`programie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="d38dc-220">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d38dc-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="d38dc-221">Aby uruchomić `DbContext` scenariusz sondowania za pomocą przykładowej aplikacji, upewnij się, że baza danych określona przez parametry połączenia nie istnieje w wystąpieniu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d38dc-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="d38dc-222">Jeśli baza danych istnieje, usuń ją.</span><span class="sxs-lookup"><span data-stu-id="d38dc-222">If the database exists, delete it.</span></span>

<span data-ttu-id="d38dc-223">Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d38dc-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="d38dc-224">Po uruchomieniu aplikacji Sprawdź stan kondycji, wysyłając żądanie do `/health` punktu końcowego w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d38dc-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="d38dc-225">Baza danych programu `AppDbContext` i nie istnieje, więc aplikacja udostępnia następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="d38dc-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="d38dc-226">Wyzwól przykładową aplikację, aby utworzyć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d38dc-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="d38dc-227">Wprowadź żądanie do `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="d38dc-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="d38dc-228">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="d38dc-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="d38dc-229">Utwórz żądanie do `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d38dc-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="d38dc-230">Istnieje baza danych i kontekst, aby aplikacja odpowiadała:</span><span class="sxs-lookup"><span data-stu-id="d38dc-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="d38dc-231">Wyzwól przykładową aplikację, aby usunąć bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d38dc-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="d38dc-232">Wprowadź żądanie do `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="d38dc-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="d38dc-233">Aplikacja odpowiada:</span><span class="sxs-lookup"><span data-stu-id="d38dc-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="d38dc-234">Utwórz żądanie do `/health` punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d38dc-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="d38dc-235">Aplikacja zawiera odpowiedź w złej kondycji:</span><span class="sxs-lookup"><span data-stu-id="d38dc-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="d38dc-236">Oddzielne sondy gotowości i na żywo</span><span class="sxs-lookup"><span data-stu-id="d38dc-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="d38dc-237">W niektórych scenariuszach hostingu jest używana para kontroli kondycji, która odróżnia dwa stany aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d38dc-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="d38dc-238">Aplikacja działa, ale jeszcze nie jest gotowa do odbierania żądań.</span><span class="sxs-lookup"><span data-stu-id="d38dc-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="d38dc-239">Jest to stan gotowości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="d38dc-240">Aplikacja działa i odpowiada na żądania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="d38dc-241">Ten stan jest *aktywny*.</span><span class="sxs-lookup"><span data-stu-id="d38dc-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="d38dc-242">Sprawdzanie gotowości zwykle wykonuje bardziej obszerny i czasochłonny zestaw kontroli w celu ustalenia, czy wszystkie podsystemy i zasoby aplikacji są dostępne.</span><span class="sxs-lookup"><span data-stu-id="d38dc-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="d38dc-243">Sprawdzenie na żywo powoduje jedynie szybkie sprawdzenie, czy aplikacja jest dostępna do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="d38dc-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="d38dc-244">Gdy aplikacja przejdzie kontrolę gotowości, nie ma potrzeby dalszej obciążania aplikacji przy użyciu kosztownego zestawu kontroli&mdash;gotowości sprawdza tylko, czy sprawdzanie dostępności jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="d38dc-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="d38dc-245">Przykładowa aplikacja zawiera kontrolę kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [hostowanej usłudze](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="d38dc-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="d38dc-246">Uwidacznia właściwość, `StartupTaskCompleted`która może zostać `true` ustawiona przez usługę hostowaną po zakończeniu długotrwałego zadania (StartupHostedServiceHealthCheck.cs): `StartupHostedServiceHealthCheck`</span><span class="sxs-lookup"><span data-stu-id="d38dc-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="d38dc-247">Długotrwałe zadanie w tle jest uruchamiane przez hostowaną [usługę](xref:fundamentals/host/hosted-services) (*usługi/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="d38dc-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="d38dc-248">Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` jest ustawiony na `true`:</span><span class="sxs-lookup"><span data-stu-id="d38dc-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="d38dc-249">Kontrola kondycji jest zarejestrowana <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices` programie w programie wraz z usługą hostowaną.</span><span class="sxs-lookup"><span data-stu-id="d38dc-249">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="d38dc-250">Ponieważ usługa hostowana musi ustawić właściwość na sprawdzaniu kondycji, sprawdzanie kondycji jest również rejestrowane w kontenerze usługi (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="d38dc-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="d38dc-251">Wywołaj test kondycji oprogramowania pośredniczącego w potoku `Startup.Configure`przetwarzania aplikacji w.</span><span class="sxs-lookup"><span data-stu-id="d38dc-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="d38dc-252">W przykładowej aplikacji punkty końcowe sprawdzania kondycji są tworzone na `/health/ready` potrzeby kontroli gotowości i `/health/live` kontroli stanu na żywo.</span><span class="sxs-lookup"><span data-stu-id="d38dc-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="d38dc-253">Sprawdzanie gotowości filtruje kontrolę kondycji w celu sprawdzenia kondycji `ready` za pomocą znacznika.</span><span class="sxs-lookup"><span data-stu-id="d38dc-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="d38dc-254">Sprawdzenie stanu na żywo odfiltruje `StartupHostedServiceHealthCheck` przez zwrócenie `false` elementu [HealthCheckOptions. predykatu](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (Aby uzyskać więcej informacji, zobacz [Sprawdzanie kondycji filtru](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="d38dc-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="d38dc-255">Aby uruchomić scenariusz konfiguracji gotowości/na żywo za pomocą przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d38dc-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="d38dc-256">W przeglądarce odwiedź `/health/ready` kilka razy do 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="d38dc-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="d38dc-257">Sprawdzanie kondycji raportuje w *złej kondycji* przez pierwsze 15 sekund.</span><span class="sxs-lookup"><span data-stu-id="d38dc-257">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="d38dc-258">Po upływie 15 sekund punkt końcowy zgłasza w *dobrej kondycji*, co odzwierciedla ukończenie długotrwałego zadania wykonywanego przez usługę hostowaną.</span><span class="sxs-lookup"><span data-stu-id="d38dc-258">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="d38dc-259">W tym przykładzie tworzony jest również Wydawca sprawdzania kondycji (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementacja), który uruchamia pierwsze sprawdzenie gotowości z dwoma drugim opóźnieniem.</span><span class="sxs-lookup"><span data-stu-id="d38dc-259">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="d38dc-260">Aby uzyskać więcej informacji, zapoznaj się z sekcją [Sprawdzanie kondycji wydawcy](#health-check-publisher) .</span><span class="sxs-lookup"><span data-stu-id="d38dc-260">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="d38dc-261">Przykład Kubernetes</span><span class="sxs-lookup"><span data-stu-id="d38dc-261">Kubernetes example</span></span>

<span data-ttu-id="d38dc-262">Korzystanie z odrębnych gotowości i kontroli na żywo jest przydatne w środowisku, takim jak [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="d38dc-262">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="d38dc-263">W programie Kubernetes aplikacja może być wymagana do wykonywania czasochłonnych zadań uruchamiania przed zaakceptowaniem żądań, takich jak Test dostępności źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d38dc-263">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="d38dc-264">Przy użyciu osobnych kontroli program Orchestrator może rozróżnić, czy aplikacja działa, ale nie jest jeszcze gotowa lub Jeśli uruchomienie aplikacji nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="d38dc-264">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="d38dc-265">Aby uzyskać więcej informacji na temat gotowości i sondy na żywo w programie Kubernetes, zobacz [Konfigurowanie sondy na żywo i gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="d38dc-265">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="d38dc-266">Poniższy przykład demonstruje konfigurację sondowania gotowości Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="d38dc-266">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="d38dc-267">Sondowanie oparte na metrykach z niestandardowym modułem zapisywania odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d38dc-267">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="d38dc-268">Przykładowa aplikacja demonstruje kontrolę kondycji pamięci za pomocą modułu zapisywania odpowiedzi niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="d38dc-268">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="d38dc-269">`MemoryHealthCheck`zgłasza stan złej kondycji, jeśli aplikacja używa więcej niż danego progu pamięci (1 GB w przykładowej aplikacji).</span><span class="sxs-lookup"><span data-stu-id="d38dc-269">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="d38dc-270">Zawiera informacje dotyczące modułu wyrzucania elementów bezużytecznych (GC) dla aplikacji (MemoryHealthCheck.cs): <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult></span><span class="sxs-lookup"><span data-stu-id="d38dc-270">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="d38dc-271">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-271">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d38dc-272">Zamiast włączać kontrolę kondycji przez przekazanie jej <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>do `MemoryHealthCheck` programu jest zarejestrowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="d38dc-272">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="d38dc-273">Wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> zarejestrowane usługi są dostępne dla usług sprawdzania kondycji i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d38dc-273">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="d38dc-274">Zalecamy rejestrację usług sprawdzania kondycji jako usług pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="d38dc-274">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="d38dc-275">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d38dc-275">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="d38dc-276">Wywołaj test kondycji oprogramowania pośredniczącego w potoku `Startup.Configure`przetwarzania aplikacji w.</span><span class="sxs-lookup"><span data-stu-id="d38dc-276">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="d38dc-277">Obiekt delegowany jest dostarczany `ResponseWriter` do właściwości w celu wygenerowania niestandardowej odpowiedzi JSON po zakończeniu sprawdzania kondycji: `WriteResponse`</span><span class="sxs-lookup"><span data-stu-id="d38dc-277">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="d38dc-278">`WriteResponse` Metoda formatujedoobiektuJSONizwracadanewyjścioweJSONdla`CompositeHealthCheckResult` odpowiedzi kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="d38dc-278">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="d38dc-279">Aby uruchomić sondę opartą na metrykach z niestandardowymi danymi wyjściowymi modułu zapisywania odpowiedzi przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d38dc-279">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="d38dc-280">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje scenariusze sprawdzania kondycji oparte na metrykach, w tym magazyn dyskowy i maksymalną liczbę sprawdzeń na żywo.</span><span class="sxs-lookup"><span data-stu-id="d38dc-280">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="d38dc-281">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d38dc-281">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="d38dc-282">Filtruj według portu</span><span class="sxs-lookup"><span data-stu-id="d38dc-282">Filter by port</span></span>

<span data-ttu-id="d38dc-283">Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> z portem ogranicza liczbę żądań sprawdzania kondycji do określonego portu.</span><span class="sxs-lookup"><span data-stu-id="d38dc-283">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="d38dc-284">Jest to zwykle używane w środowisku kontenera w celu udostępnienia portu usług monitorowania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-284">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="d38dc-285">Przykładowa aplikacja konfiguruje port przy użyciu [zmiennej środowiskowej dostawcy konfiguracji](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d38dc-285">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="d38dc-286">Port jest ustawiany w pliku *profilu launchsettings. JSON* i przekazywać do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="d38dc-286">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="d38dc-287">Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-287">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="d38dc-288">Aby użyć przykładowej aplikacji do zademonstrowania konfiguracji portów zarządzania, Utwórz plik *profilu launchsettings. JSON* w folderze *Właściwości* .</span><span class="sxs-lookup"><span data-stu-id="d38dc-288">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="d38dc-289">Następujący plik *profilu launchsettings. JSON* nie jest zawarty w plikach projektu przykładowej aplikacji i musi być utworzony ręcznie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-289">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="d38dc-290">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d38dc-290">*Properties/launchSettings.json*:</span></span>

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

<span data-ttu-id="d38dc-291">Zarejestruj usługi <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> sprawdzania kondycji w `Startup.ConfigureServices`programie w programie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-291">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d38dc-292">Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> określające port zarządzania (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="d38dc-292">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="d38dc-293">Możesz uniknąć tworzenia pliku *profilu launchsettings. JSON* w przykładowej aplikacji, ustawiając adresy URL i port zarządzania jawnie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-293">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="d38dc-294">W *program.cs* , w <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> którym został utworzony, Dodaj wywołanie do <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> i podaj normalny punkt końcowy odpowiedzi aplikacji oraz punkt końcowy portu zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d38dc-294">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="d38dc-295">W *ManagementPortStartup.cs* , <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> gdzie jest wywoływana, określ port zarządzania jawnie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-295">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="d38dc-296">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d38dc-296">*Program.cs*:</span></span>
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
> <span data-ttu-id="d38dc-297">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d38dc-297">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="d38dc-298">Aby uruchomić scenariusz konfiguracji portu zarządzania przy użyciu przykładowej aplikacji, wykonaj następujące polecenie w folderze projektu w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="d38dc-298">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="d38dc-299">Dystrybucja biblioteki kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-299">Distribute a health check library</span></span>

<span data-ttu-id="d38dc-300">Aby rozpowszechnić kontrolę kondycji jako bibliotekę:</span><span class="sxs-lookup"><span data-stu-id="d38dc-300">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="d38dc-301">Napisz kontrolę kondycji, która implementuje <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejs jako autonomiczną klasę.</span><span class="sxs-lookup"><span data-stu-id="d38dc-301">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="d38dc-302">Klasa może polegać na [iniekcji zależności (di)](xref:fundamentals/dependency-injection), aktywacji typu i [nazwanych opcjach](xref:fundamentals/configuration/options) w celu uzyskania dostępu do danych konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="d38dc-302">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

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

1. <span data-ttu-id="d38dc-303">Napisz metodę rozszerzenia z parametrami, które są używane przez zużywaną aplikację `Startup.Configure` w jej metodzie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-303">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="d38dc-304">W poniższym przykładzie przyjęto założenie, że następująca sygnatura metody kontroli kondycji:</span><span class="sxs-lookup"><span data-stu-id="d38dc-304">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="d38dc-305">Poprzednia sygnatura wskazuje `ExampleHealthCheck` , że wymaga dodatkowych danych do przetworzenia logiki sondowania sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-305">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="d38dc-306">Dane są przekazywane delegatom używanym do tworzenia wystąpienia kontroli kondycji, gdy Sprawdzanie kondycji jest zarejestrowane za pomocą metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="d38dc-306">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="d38dc-307">W poniższym przykładzie obiekt wywołujący określa opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="d38dc-307">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="d38dc-308">Nazwa sprawdzania kondycji`name`().</span><span class="sxs-lookup"><span data-stu-id="d38dc-308">health check name (`name`).</span></span> <span data-ttu-id="d38dc-309">Jeśli `null`jestużywany. `example_health_check`</span><span class="sxs-lookup"><span data-stu-id="d38dc-309">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="d38dc-310">punkt danych ciągu dla kontroli kondycji (`data1`).</span><span class="sxs-lookup"><span data-stu-id="d38dc-310">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="d38dc-311">punkt danych liczb całkowitych dla kontroli kondycji (`data2`).</span><span class="sxs-lookup"><span data-stu-id="d38dc-311">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="d38dc-312">Jeśli `null`jestużywany. `1`</span><span class="sxs-lookup"><span data-stu-id="d38dc-312">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="d38dc-313">stan niepowodzenia<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>().</span><span class="sxs-lookup"><span data-stu-id="d38dc-313">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="d38dc-314">Wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="d38dc-314">The default is `null`.</span></span> <span data-ttu-id="d38dc-315">Jeśli `null`dla stanu błędu zostanie zgłoszona wartość [HealthStatus. w złej kondycji](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) .</span><span class="sxs-lookup"><span data-stu-id="d38dc-315">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="d38dc-316">Tagi (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="d38dc-316">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="d38dc-317">Wydawca kontroli kondycji</span><span class="sxs-lookup"><span data-stu-id="d38dc-317">Health Check Publisher</span></span>

<span data-ttu-id="d38dc-318">Po dodaniu do kontenera usługi system kontroli kondycji okresowo wykonuje sprawdzanie kondycji i wywołuje `PublishAsync` wynik. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher></span><span class="sxs-lookup"><span data-stu-id="d38dc-318">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="d38dc-319">Jest to przydatne w scenariuszu systemu monitorowania kondycji opartej na wypychaniu, który oczekuje, że każdy proces będzie okresowo wywoływał system monitorowania w celu określenia kondycji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-319">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="d38dc-320"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> Interfejs ma jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="d38dc-320">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="d38dc-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions>Zezwól na ustawienie:</span><span class="sxs-lookup"><span data-stu-id="d38dc-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="d38dc-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>Początkowe opóźnienie stosowane po uruchomieniu aplikacji przed wykonaniem <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. &ndash;</span><span class="sxs-lookup"><span data-stu-id="d38dc-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="d38dc-323">Opóźnienie jest stosowane raz podczas uruchamiania i nie ma zastosowania do kolejnych iteracji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-323">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="d38dc-324">Wartość domyślna to pięć sekund.</span><span class="sxs-lookup"><span data-stu-id="d38dc-324">The default value is five seconds.</span></span>
* <span data-ttu-id="d38dc-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>&ndash; Okres wykonania.<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher></span><span class="sxs-lookup"><span data-stu-id="d38dc-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="d38dc-326">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="d38dc-326">The default value is 30 seconds.</span></span>
* <span data-ttu-id="d38dc-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>&ndash; Jeśli jest(`null` domyślnie), usługa wydawcy sprawdzania kondycji uruchamia wszystkie zarejestrowane testy kondycji. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate></span><span class="sxs-lookup"><span data-stu-id="d38dc-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="d38dc-328">Aby uruchomić podzestaw kontroli kondycji, należy określić funkcję, która filtruje zestaw kontroli.</span><span class="sxs-lookup"><span data-stu-id="d38dc-328">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="d38dc-329">Predykat jest oceniany w każdym okresie.</span><span class="sxs-lookup"><span data-stu-id="d38dc-329">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="d38dc-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>Limit czasu wykonywania kontroli kondycji dla wszystkich <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. &ndash;</span><span class="sxs-lookup"><span data-stu-id="d38dc-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="d38dc-331">Użyj <xref:System.Threading.Timeout.InfiniteTimeSpan> , aby wykonać bez limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="d38dc-331">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="d38dc-332">Wartość domyślna to 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="d38dc-332">The default value is 30 seconds.</span></span>

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> <span data-ttu-id="d38dc-333">W wersji 2,2 ASP.NET Core ustawienie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> nie jest honorowane <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> przez implementację; ustawia wartość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="d38dc-333">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="d38dc-334">Ten problem zostanie rozwiązany w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="d38dc-334">This issue will be fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="d38dc-335">Aby uzyskać więcej informacji, zobacz [HealthCheckPublisherOptions. period ustawia wartość. Opóźnienie](https://github.com/aspnet/Extensions/issues/1041).</span><span class="sxs-lookup"><span data-stu-id="d38dc-335">For more information, see [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041).</span></span>

::: moniker-end

<span data-ttu-id="d38dc-336">W przykładowej aplikacji `ReadinessPublisher` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> jest implementacją.</span><span class="sxs-lookup"><span data-stu-id="d38dc-336">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="d38dc-337">Stan sprawdzania kondycji jest rejestrowany w `Entries` i rejestrowany dla każdej kontroli:</span><span class="sxs-lookup"><span data-stu-id="d38dc-337">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="d38dc-338">W `LivenessProbeStartup` przykładzie przykładowej `StartupHostedService` aplikacji sprawdzanie gotowości ma dwa drugie opóźnienie uruchamiania i sprawdza, co 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="d38dc-338">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="d38dc-339">Aby uaktywnić <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementację, przykład rejestruje `ReadinessPublisher` się jako usługa pojedyncza w kontenerze [iniekcji zależności (di)](xref:fundamentals/dependency-injection) :</span><span class="sxs-lookup"><span data-stu-id="d38dc-339">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="d38dc-340">Następujące obejście umożliwia dodanie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpienia do kontenera usługi, gdy co najmniej jedna inna usługa hostowana została już dodana do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-340">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="d38dc-341">To obejście nie będzie wymagane w przypadku wersji ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="d38dc-341">This workaround won't be required with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="d38dc-342">Aby uzyskać więcej informacji, zobacz https://github.com/aspnet/Extensions/issues/639:.</span><span class="sxs-lookup"><span data-stu-id="d38dc-342">For more information, see: https://github.com/aspnet/Extensions/issues/639.</span></span>
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
> <span data-ttu-id="d38dc-343">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje wydawców dla kilku systemów, w tym [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="d38dc-343">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="d38dc-344">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) to port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany ani wspierany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d38dc-344">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="d38dc-345">Ogranicz kontrolę kondycji za pomocą MapWhen</span><span class="sxs-lookup"><span data-stu-id="d38dc-345">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="d38dc-346">Użyj <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> , aby warunkowo rozgałęziać potok żądania dla punktów końcowych sprawdzania kondycji.</span><span class="sxs-lookup"><span data-stu-id="d38dc-346">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="d38dc-347">W poniższym przykładzie rozgałęzimy potok żądania, `MapWhen` aby aktywować oprogramowanie do sprawdzania kondycji w przypadku otrzymania żądania get `api/HealthCheck` dla punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="d38dc-347">In the following example, `MapWhen` branches the request pipeline to activate Health Check Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="d38dc-348">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="d38dc-348">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>
