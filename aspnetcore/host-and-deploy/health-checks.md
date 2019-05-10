---
title: Kontroli kondycji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować kontrole kondycji infrastruktury platformy ASP.NET Core, takich jak aplikacje i bazy danych.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 04/23/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 5119267a8da5c950989b14b7c2e818aa22806506
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901129"
---
# <a name="health-checks-in-aspnet-core"></a>Kontroli kondycji w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex) i [Glenn Condron](https://github.com/glennc)

Platforma ASP.NET Core oferuje kondycji Sprawdź oprogramowanie pośredniczące i biblioteki do raportowania kondycji składników infrastruktury aplikacji.

Kontrole kondycji są udostępniane przez aplikację jako punktów końcowych HTTP. Punkty końcowe sprawdzania kondycji można skonfigurować dla różnych scenariuszy monitorowania w czasie rzeczywistym:

* Sondy kondycji mogą być używane przez koordynatorów kontenerów i obciążenia równoważenia w celu sprawdzenia stanu aplikacji. Na przykład orkiestrator kontenerów mogą odpowiadać na kondycji niepowodzenie sprawdzenia przez zatrzymywanie stopniowego wdrażania lub ponownego uruchamiania kontenera. Moduł równoważenia obciążenia może reagować na złej kondycji aplikacji, kierując ruch od wystąpienia przechodzenia do wystąpienia dobrej kondycji.
* Można monitorować użycie pamięci, dysku i inne zasoby serwera fizycznego do stanu dobrej kondycji.
* Kontrole kondycji można przetestować zależności aplikacji, takich jak bazy danych i punktów końcowych usługi zewnętrznej, aby upewnić się, dostępność i normalnego funkcjonowania.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja zawiera przykładowe scenariusze opisane w tym temacie. Aby uruchomić przykładową aplikację w danej sytuacji, należy użyć [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia z folderu projektu w powłoce poleceń. Zobacz przykładową aplikację *README.md* plików i opisy scenariuszy, w tym temacie, aby uzyskać szczegółowe informacje na temat korzystania z przykładowej aplikacji.

## <a name="prerequisites"></a>Wymagania wstępne

Kontrole kondycji są zwykle używane z zewnętrznego monitorowania usługi lub kontener programu orchestrator do sprawdzenia stanu aplikacji. Przed dodaniem kontrole kondycji do aplikacji, zdecyduj, na które monitorowania systemowi operacyjnemu używanie. System monitorowania decyduje o jakie kontrole kondycji do tworzenia i konfigurowania ich punkty końcowe.

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) pakietu.

Przykładowa aplikacja zawiera kod uruchamiający wykazanie, że kontrole kondycji dla kilku scenariuszy. [Sondowanie bazy danych](#database-probe) scenariusz sprawdza kondycję połączenia bazy danych przy użyciu [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks). [Sondy DbContext](#entity-framework-core-dbcontext-probe) scenariusz sprawdza bazę danych przy użyciu programu EF Core `DbContext`. Aby zapoznać się w scenariuszach bazy danych dla przykładowej aplikacji:

* Tworzy bazę danych i zapewnia jego parametry połączenia w *appsettings.json* pliku.
* Ma następujące odwołania do pakietu w pliku projektu:
  * [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) jest port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany lub obsługiwane przez firmę Microsoft.

Inny scenariusz sprawdzania kondycji pokazuje sposób filtrowania kontrole kondycji do portu zarządzania. Przykładowa aplikacja wymaga utworzenia *Properties/launchSettings.json* pliku, który zawiera adres URL zarządzania i port zarządzania. Aby uzyskać więcej informacji, zobacz [filtru według portów](#filter-by-port) sekcji.

## <a name="basic-health-probe"></a>Sonda kondycji podstawowe

W przypadku wielu aplikacji konfiguracji sondy kondycji podstawowe raporty dostępności aplikacji do przetwarzania żądań (*żywotności*) jest wystarczająca dowiedzieć się, stan aplikacji.

Podstawową konfigurację rejestruje usługi sprawdzania kondycji i wywołuje kondycji Sprawdź oprogramowania pośredniczącego, aby odpowiedzieć na punkt końcowy adres URL odpowiedzi kondycji. Domyślnie żadne testy kondycji konkretnego są rejestrowane do testowania dowolnej określonej zależności lub podsystemu. Aplikacja jest traktowany jako w dobrej kondycji, jeśli jest w stanie odpowiadać na adres URL punktu końcowego kondycji. Moduł zapisujący odpowiedzi domyślnej zapisuje stan (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) jako zwykły tekst odpowiedź z powrotem do klienta, wskazując [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) lub [ HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) stanu.

Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`. Dodaj oprogramowanie pośredniczące sprawdzanie kondycji za pomocą <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania żądań `Startup.Configure`.

W przykładowej aplikacji, punkt końcowy kontroli kondycji jest tworzony w `/health` (*BasicStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

Aby uruchomić scenariusz konfiguracji podstawowej, przy użyciu przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a>Przykład platformy docker

[Docker](xref:host-and-deploy/docker/index) oferuje wbudowaną `HEALTHCHECK` dyrektywę, który może służyć do sprawdzania stanu aplikacji, która używa konfiguracji kontroli kondycji podstawowych:

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>Utwórz kontrole kondycji

Kontrole kondycji, są tworzone przez zaimplementowanie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejsu. <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> Metoda zwraca `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` oznacza kondycji jako `Healthy`, `Degraded`, lub `Unhealthy`. Wynik jest napisany w postaci zwykłego tekstu odpowiedzi z kodem stanu można konfigurować (Konfiguracja jest opisana w [opcje sprawdzania kondycji](#health-check-options) sekcji). <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> może również zwracać opcjonalne pary klucz wartość.

### <a name="example-health-check"></a>Przykład kontrola kondycji

Następujące `ExampleHealthCheck` klasy pokazuje układ sprawdzenie kondycji:

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

### <a name="register-health-check-services"></a>Rejestrowanie usługi sprawdzania kondycji

`ExampleHealthCheck` Typu jest dodawany do usług sprawdzanie kondycji za pomocą <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> Przeciążenia pokazano w poniższym przykładzie ustawia stan błędu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) do raportu, gdy kontrola kondycji zgłosi błąd. Jeśli ustawiono stan niepowodzenia `null` (ustawienie domyślne), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) są zgłaszane. To przeciążenie jest użytecznego scenariusza autorów biblioteki, gdzie stan Niepowodzenie, wskazywanym przez bibliotekę jest wymuszana przez aplikację po wystąpieniu błędu sprawdzenia kondycji, jeśli implementacja sprawdzania kondycji honoruje ustawienie.

*Tagi* może służyć do filtrowania kontrole kondycji (dokładniejszym opisem zawartym w [filtrowania kontrole kondycji](#filter-health-checks) sekcji).

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> można również wykonywać funkcję lambda. W poniższym przykładzie nazwa sprawdzenia kondycji jest określony jako `Example` i wyboru zawsze zwraca dobrej kondycji:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () =>
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a>Użyj oprogramowania pośredniczącego kontrole kondycji

W `Startup.Configure`, wywołaj <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> w potoku przetwarzania przy użyciu adresu URL punktu końcowego lub ścieżką względną:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

Jeśli kontrole kondycji powinna nasłuchiwać na określonym porcie, użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> można ustawić port (dokładniejszym opisem zawartym w [filtru według portów](#filter-by-port) sekcji):

```csharp
app.UseHealthChecks("/health", port: 8000);
```

Oprogramowanie pośredniczące sprawdza, czy w kondycji jest *terminalu oprogramowania pośredniczącego* w potoku przetwarzania żądań aplikacji. Pierwszy kondycji Sprawdź punkt końcowy napotkano dokładnie dopasowany do adresu URL żądania wykonuje i short-circuits pozostałego potoku oprogramowania pośredniczącego. Po wystąpieniu zwarcie wykonuje nie oprogramowanie pośredniczące po kontroli kondycji dopasowany.

## <a name="health-check-options"></a>Opcje sprawdzania kondycji

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> zapewniają możliwość dostosowywania zachowania wyboru kondycji:

* [Filtruj kontrole kondycji](#filter-health-checks)
* [Dostosowywanie kod stanu HTTP](#customize-the-http-status-code)
* [Pomiń nagłówki cache](#suppress-cache-headers)
* [Dostosowywanie danych wyjściowych](#customize-output)

### <a name="filter-health-checks"></a>Filtruj kontrole kondycji

Domyślnie Sprawdź kondycję w oprogramowaniu pośredniczącym uruchamia wszystkie kontrole kondycji zarejestrowanego. Aby uruchomić podzbiór kontroli kondycji, zapewnia funkcja, która zwraca wartość typu boolean do <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> opcji. W poniższym przykładzie `Bar` sprawdzenie kondycji jest odfiltrowana po jego znaczniku (`bar_tag`) w funkcji instrukcji warunkowej, gdzie `true` jest zwracany tylko wtedy, jeśli sprawdzenie kondycji <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> dopasowania właściwości `foo_tag` lub `baz_tag`:

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

### <a name="customize-the-http-status-code"></a>Dostosowywanie kod stanu HTTP

Użyj <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> dostosować mapowanie stanu kondycji, aby kody stanu HTTP. Następujące <xref:Microsoft.AspNetCore.Http.StatusCodes> przypisania są wartości domyślne używane przez oprogramowanie pośredniczące. Zmień wartości Kod stanu, zgodnie z wymaganiami.

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

### <a name="suppress-cache-headers"></a>Pomiń nagłówki cache

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> Określa, czy oprogramowanie pośredniczące sprawdzanie kondycji dodaje nagłówki HTTP do odpowiedzi sondowania, aby zapobiec buforowanie odpowiedzi. Jeśli wartość jest `false` (ustawienie domyślne), oprogramowanie pośredniczące Ustawia lub zastępuje `Cache-Control`, `Expires`, i `Pragma` nagłówki, aby zapobiec buforowanie odpowiedzi. Jeśli wartość jest `true`, oprogramowanie pośredniczące nie modyfikuje nagłówki cache odpowiedzi.

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

### <a name="customize-output"></a>Dostosowywanie danych wyjściowych

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> Opcja pobiera lub ustawia delegata używany do zapisywania odpowiedzi. Delegat domyślne zapisuje odpowiedź minimalnej w postaci zwykłego tekstu z wartością ciągu [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).

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

## <a name="database-probe"></a>Sondowanie bazy danych

Sprawdzenie kondycji, można określić zapytanie bazy danych, aby był uruchamiany jako testu logicznego, aby wskazać, czy baza danych odpowiada normalnie.

Ta aplikacja używa przykładowych [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), biblioteka sprawdzania kondycji dla aplikacji platformy ASP.NET Core, można sprawdzić kondycji bazy danych programu SQL Server. `AspNetCore.Diagnostics.HealthChecks` wykonuje `SELECT 1` zapytania względem bazy danych, aby upewnić się, połączenie z bazą danych jest w dobrej kondycji.

> [!WARNING]
> Podczas sprawdzania połączenia z bazą danych przy użyciu zapytania, wybierz zapytanie zwracające szybko. Podejście zapytania jest zagrożona przeciążenia bazy danych i zmniejszenie jego wydajności. W większości przypadków Uruchamianie zapytania testu nie jest konieczne. Wystarczy jedynie wprowadzania udane połączenie z bazą danych. Jeśli możesz znaleźć niezbędne do uruchamiania kwerendy, wybierz prostego zapytania typu SELECT, taki jak `SELECT 1`.

Odwołanie do pakietu [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).

Podaj prawidłową bazą danych parametry połączenia w *appsettings.json* pliku przykładowej aplikacji. Aplikacja korzysta z bazy danych programu SQL Server o nazwie `HealthCheckSample`:

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`. Wywołania aplikacji przykładowej `AddSqlServer` metody przy użyciu parametrów połączenia bazy danych (*DbHealthStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

Aby uruchomić scenariusza sondowanie bazy danych za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:

```console
dotnet run --scenario db
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) jest port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany lub obsługiwane przez firmę Microsoft.

## <a name="entity-framework-core-dbcontext-probe"></a>Entity Framework Core DbContext sondy

`DbContext` Wyboru potwierdza, że aplikacja może komunikować się z bazą danych skonfigurowane dla platformy EF Core `DbContext`. `DbContext` Wyboru jest obsługiwane w aplikacjach który:

* Użyj [platformy Entity Framework (EF) Core](/ef/core/).
* Odwołanie do pakietu [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).

`AddDbContextCheck<TContext>` rejestruje sprawdzenia kondycji `DbContext`. `DbContext` Jest dostarczany jako `TContext` do metody. Przeciążenie jest dostępna do konfigurowania stanu awarii, tagi i zapytań niestandardowych testu.

Domyślnie:

* `DbContextHealthCheck` Wywołuje programu EF Core `CanConnectAsync` metody. Można dostosować, jakie działanie jest uruchamiane podczas sprawdzania kondycji za pomocą `AddDbContextCheck` przeciążenia metody.
* Nazwa kontroli kondycji jest nazwą `TContext` typu.

W przykładowej aplikacji `AppDbContext` jest dostarczany w celu `AddDbContextCheck` i zarejestrowany jako usługa w `Startup.ConfigureServices`.

*DbContextHealthStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

W przykładowej aplikacji `UseHealthChecks` dodaje pośredniczącym sprawdzanie kondycji w `Startup.Configure`.

*DbContextHealthStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

Aby uruchomić `DbContext` sondowania scenariusz przy użyciu przykładowej aplikacji, upewnij się, że baza danych określona przez ciąg połączenia nie istnieje w wystąpieniu programu SQL Server. Jeśli baza danych istnieje, należy go usunąć.

Wykonaj następujące polecenie z folderu projektu w powłoce poleceń:

```console
dotnet run --scenario dbcontext
```

Po uruchomieniu aplikacji Sprawdź stan kondycji, kieruje żądanie do `/health` punktu końcowego w przeglądarce. Bazy danych i `AppDbContext` nie istnieje, więc aplikacja zapewnia następującą odpowiedź:

```
Unhealthy
```

Wyzwalanie przykładową aplikację w celu utworzenia bazy danych. Zgłosić wniosek o `/createdatabase`. Odpowiedź aplikacji:

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

Zgłosić wniosek o `/health` punktu końcowego. Bazy danych i kontekstu istnieje, więc aplikacja reaguje:

```
Healthy
```

Wyzwalanie przykładową aplikację w celu usunięcia bazy danych. Zgłosić wniosek o `/deletedatabase`. Odpowiedź aplikacji:

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

Zgłosić wniosek o `/health` punktu końcowego. Aplikacja zawiera odpowiedź złej kondycji:

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a>Oddzielne gotowości i sondy żywotności

W niektórych scenariuszach hostingu parę kontroli kondycji są używane, rozróżnienie dwóch stanów aplikacji:

* Aplikacja jest działa, ale nie jest jeszcze gotowy do odbierania żądań. Ten stan jest to aplikacja *gotowości*.
* Działa i odpowiada na żądania aplikacji. Ten stan jest to aplikacja *żywotności*.

Sprawdzanie gotowości zwykle wykonuje bardziej rozbudowanym i czasochłonne zestawu testów, aby sprawdzić, czy wszystkie podsystemy aplikacji i zasoby są dostępne. Sprawdzanie żywotności jedynie wykonuje szybkie sprawdzenie w celu określenia, czy aplikacja jest dostępna do przetwarzania żądań. Po jej przekazuje jej sprawdzanie gotowości, nie ma potrzeby do obciążać aplikacji dalsze kosztowne zestaw gotowości&mdash;dalsze procesy kontroli wymagają tylko sprawdzanie żywotności.

Przykładowa aplikacja zawiera sprawdzenie kondycji, aby zgłosić ukończenie długotrwałego zadania uruchamiania w [usługi hostowanej](xref:fundamentals/host/hosted-services). `StartupHostedServiceHealthCheck` Ujawnia właściwość `StartupTaskCompleted`, które można ustawić usługi hostowanej `true` po zakończeniu jego długotrwałe zadanie (*StartupHostedServiceHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

Długotrwałe zadanie w tle jest uruchamiana przez [usługi hostowanej](xref:fundamentals/host/hosted-services) (*usług/StartupHostedService*). Po zakończeniu zadania `StartupHostedServiceHealthCheck.StartupTaskCompleted` ustawiono `true`:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

Sprawdzenie kondycji jest zarejestrowane w usłudze <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> w `Startup.ConfigureServices` wraz z hostowanej usługi. Ponieważ usługa hostowana należy ustawić właściwość na sprawdzenie kondycji, sprawdzenie kondycji również jest zarejestrowany w kontenerze usługi (*LivenessProbeStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`. W przykładowej aplikacji z punktami końcowymi kontroli kondycji są tworzone na `/health/ready` sprawdzenia gotowości i `/health/live` sprawdzania żywotności. Filtry sprawdzania gotowości kondycji sprawdza kondycję skontaktuj się z `ready` tagu. Odfiltrowuje wyboru żywotności `StartupHostedServiceHealthCheck` , zwracając `false` w [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (Aby uzyskać więcej informacji, zobacz [filtrowania kontrole kondycji](#filter-health-checks)):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

Aby uruchomić gotowości/żywotności scenariusz konfiguracji, za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:

```console
dotnet run --scenario liveness
```

W przeglądarce, odwiedź stronę `/health/ready` kilka razy, aż przeszły 15 sekund. Sprawdzanie kondycji raporty *zła* przez pierwsze 15 sekund. Po 15 sekundach raporty punktu końcowego *dobra kondycja*, co odzwierciedla ukończenie długotrwałego zadania w usłudze hostowanej.

W tym przykładzie tworzy również usługi kondycji Sprawdź wydawcy (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementacji), które jest uruchamiane pierwszego sprawdzania gotowości za pomocą dwóch półsekundowym opóźnieniu. Aby uzyskać więcej informacji, zobacz [wydawcy sprawdzanie kondycji](#health-check-publisher) sekcji.

### <a name="kubernetes-example"></a>Przykład rozwiązania Kubernetes

Przy użyciu oddzielnych kontroli gotowości i żywotności przydaje się w środowisku, takie jak [Kubernetes](https://kubernetes.io/). W usłudze Kubernetes aplikacja może być wymagane do wykonywania pracy czasochłonne uruchamiania przed zaakceptowaniem żądania, takie jak testu dostępności podstawowej bazy danych. Przy użyciu oddzielnych kontroli umożliwia programu orchestrator w celu odróżnienia, czy aplikacja działa, ale nie jest jeszcze gotowy, lub jeśli aplikacja nie powiodło się. Aby uzyskać więcej informacji o gotowości i sondy żywotności w usłudze Kubernetes, zobacz [skonfigurować żywotności i sondy gotowości](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) w dokumentacji platformy Kubernetes.

W poniższym przykładzie pokazano konfigurację sondowania gotowości Kubernetes:

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a>Opartą na metryce sondowania przy użyciu składnika zapisywania niestandardowych

Przykładowa aplikacja pokazuje sprawdzenie kondycji pamięci za pomocą składnika zapisywania niestandardowych.

`MemoryHealthCheck` Raporty Stan obniżonej wydajności, jeśli aplikacja korzysta z więcej niż wartość progową pamięci (1 GB w przykładowej aplikacji). <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> Zawiera informacje moduł wyrzucania elementów bezużytecznych (GC) dla aplikacji (*MemoryHealthCheck.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`. Zamiast włączania kondycji Sprawdź przez przekazanie jej do <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` jest zarejestrowany jako usługa. Wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> zarejestrowane usługi są dostępne dla usługi sprawdzania kondycji i oprogramowania pośredniczącego. Firma Microsoft zaleca rejestrowanie usługi sprawdzania kondycji jako pojedynczego wystąpienia usługi.

*CustomWriterStartup.cs*:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

Wywoła kondycji Sprawdź oprogramowanie pośredniczące w potoku przetwarzania aplikacji w `Startup.Configure`. A `WriteResponse` delegata jest dostarczany w celu `ResponseWriter` właściwość niestandardową odpowiedź w formacie JSON dane wyjściowe, gdy wykonuje sprawdzenie kondycji:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

`WriteResponse` Formaty metoda `CompositeHealthCheckResult` do postaci JSON obiektu i zwraca dane wyjściowe JSON dla odpowiedzi z kontroli kondycji:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

Aby uruchomić funkcję badania opartą na metryce z danymi wyjściowymi modułu zapisującego niestandardowe odpowiedzi za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:

```console
dotnet run --scenario writer
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje opartą na metryce kondycji sprawdzenie scenariuszy, w tym dysku magazynu i maksymalną wartość żywotności kontroli.
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) jest port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany lub obsługiwane przez firmę Microsoft.

## <a name="filter-by-port"></a>Filtruj według portów

Wywoływanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> z portem ogranicza żądania sprawdzenia kondycji do określonego portu. Zazwyczaj służy w środowisku kontenera do udostępniania portów do monitorowania usługi.

Przykładowa aplikacja umożliwia skonfigurowanie portu przy użyciu [zmiennej dostawcę konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider). Numer portu jest ustawiana w *launchSettings.json* pliku i przekazywane do dostawcy konfiguracji za pośrednictwem zmiennej środowiskowej. Należy również skonfigurować serwer do nasłuchiwania żądań na porcie zarządzania.

Przykładowa aplikacja jest używana do zademonstrowania konfiguracji portów zarządzania, należy utworzyć *launchSettings.json* w pliku *właściwości* folderu.

Następujące *launchSettings.json* plików nie jest zawarty w plikach projektu przykładowej aplikacji i należy utworzyć je ręcznie.

*Properties/launchSettings.json*:

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

Rejestrowanie usługi sprawdzania kondycji z <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> w `Startup.ConfigureServices`. Wywołanie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> Określa port usługi zarządzania (*ManagementPortStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> Możesz uniknąć tworzenia *launchSettings.json* pliku w przykładowej aplikacji przez ustawianie adresów URL i port zarządzania jawnie w kodzie. W *Program.cs* gdzie <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> jest utworzony, dodaj wywołanie <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> i podaj punktu końcowego normalne odpowiedzi aplikacji oraz port punktu końcowego zarządzania. W *ManagementPortStartup.cs* gdzie <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> jest wywoływana, jawnie określ port zarządzania.
>
> *Program.cs*:
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
> *ManagementPortStartup.cs*:
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

Aby uruchomić scenariusz konfiguracji portów zarządzania za pomocą przykładowej aplikacji, wykonaj następujące polecenie z folderu projektu w powłoce poleceń:

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>Dystrybucja Biblioteka sprawdzania kondycji

Aby rozesłać sprawdzenie kondycji jako biblioteki:

1. Sprawdzenie kondycji, który implementuje zapisu <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interfejs jako autonomiczną klasę. Klasa może polegać na [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection), typ aktywacji i [o nazwie opcje](xref:fundamentals/configuration/options) do dostępu do danych konfiguracji.

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

1. Zapisywanie metody rozszerzenia z parametrami, które aplikacja odbierająca komunikaty wywołania w jego `Startup.Configure` metody. W poniższym przykładzie przyjęto założenie, następujący podpis metody sprawdzania kondycji:

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   Poprzedni podpisu wskazuje, że `ExampleHealthCheck` wymaga dodatkowe dane do procesu kondycji Sprawdź logikę sondowania. Dane są udostępniane do delegata, użyty do utworzenia wystąpienia sprawdzenie kondycji po zarejestrowaniu sprawdzenie kondycji za pomocą metody rozszerzenia. W poniższym przykładzie obiekt wywołujący określa opcjonalne:

   * Nazwa sprawdzenia kondycji (`name`). Jeśli `null`, `example_health_check` jest używany.
   * Ciąg punktu danych dla kontroli kondycji (`data1`).
   * punkt danych liczby całkowitej dla kontroli kondycji (`data2`). Jeśli `null`, `1` jest używany.
   * Stan błędu (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>). Wartość domyślna to `null`. Jeśli `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) jest zgłaszany stan Niepowodzenie.
   * tagi (`IEnumerable<string>`).

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

## <a name="health-check-publisher"></a>Wydawca sprawdzania kondycji

Gdy <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> zostanie dodany do kontenera usługi systemu sprawdzanie kondycji wykonuje okresowo z kontroli kondycji i wywołania `PublishAsync` z wynikiem. Jest to przydatne w scenariuszu systemu, który oczekuje, że każdy proces, aby wywołać system monitorowania okresowo w celu określenia kondycji monitorowania kondycji na podstawie wypychania.

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> Interfejs zawiera jedną metodę:

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> pozwala na ustawienie:

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; Opóźnienie początkowej zastosowane po aplikacji, który rozpoczyna się przed wykonaniem <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. Opóźnienie jest stosowana raz przy uruchamianiu i nie ma zastosowania do kolejnych iteracjach. Wartość domyślna to 5 sekund.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; Okres <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wykonywania. Wartość domyślna to 30 sekund.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Jeśli <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> jest `null` (ustawienie domyślne), wszystkie kontrole kondycji zarejestrowanego działa usługa wydawcy sprawdzania kondycji. Aby uruchomić podzbiór kontroli kondycji, należy podać funkcji filtrującej zestawu testów. Predykat obliczone każdego okresu.
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; Limit czasu wykonywania kondycji sprawdza wszystkie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpień. Użyj <xref:System.Threading.Timeout.InfiniteTimeSpan> można wykonać bez limitu czasu. Wartość domyślna to 30 sekund.

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> W wersji platformy ASP.NET Core 2.2 ustawienie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> nie są uznawane przez <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wdrożenia; ustawia wartość <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>. Ten problem zostanie rozwiązany w programie ASP.NET Core 3.0. Aby uzyskać więcej informacji, zobacz [HealthCheckPublisherOptions.Period ustawia wartość. Opóźnienie](https://github.com/aspnet/Extensions/issues/1041).

::: moniker-end

W przykładowej aplikacji `ReadinessPublisher` jest <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementacji. Sprawdź stan kondycji jest rejestrowana w `Entries` i rejestrowane przy każdym sprawdzaniu:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

Przykładowa aplikacja `LivenessProbeStartup` przykład `StartupHostedService` sprawdzanie gotowości dwóch półsekundowym opóźnieniu uruchamiania i uruchamia kontrolę co 30 sekund. Aby aktywować <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> rejestruje przykład implementacji `ReadinessPublisher` jako pojedyncza usługa w [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera:

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> Poniższe obejście pozwala na dodawanie <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> wystąpienie kontenera usługi, gdy jeden lub więcej innych usług hostowanych zostały już dodane do aplikacji. To rozwiązanie nie będzie wymagane wraz z wydaniem programu ASP.NET Core 3.0. Aby uzyskać więcej informacji, zobacz: https://github.com/aspnet/Extensions/issues/639.
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
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) obejmuje wydawcy dla kilku systemów, w tym [usługi Application Insights](/azure/application-insights/app-insights-overview).
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) jest port [BeatPulse](https://github.com/xabaril/beatpulse) i nie jest obsługiwany lub obsługiwane przez firmę Microsoft.
