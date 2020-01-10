---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2020
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 897ec2f012adcd325ca0472f381f129bc2b62854
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828883"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Zadania w tle z usługami hostowanymi w ASP.NET Core

Autorzy [Luke Latham](https://github.com/guardrex) i [Jeow li Huan](https://github.com/huan086)

::: moniker range=">= aspnetcore-3.0"

W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>. Ten temat zawiera trzy przykłady usługi hostowanej:

* Zadanie w tle, które jest uruchamiane na czasomierzu.
* Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie. Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).
* Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="worker-service-template"></a>Szablon usługi procesu roboczego

Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi. Aplikacja utworzona na podstawie szablonu usługi procesu roboczego określa zestaw SDK procesu roboczego w pliku projektu:

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a>Package

Aplikacja oparta na szablonie usługi procesu roboczego używa zestawu SDK `Microsoft.NET.Sdk.Worker` i ma jawne odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) . Na przykład zapoznaj się z plikiem projektu przykładowej aplikacji (*BackgroundTasksSample. csproj*).

W przypadku aplikacji sieci Web, które używają zestawu SDK `Microsoft.NET.Sdk.Web`, pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest przywoływany niejawnie z udostępnionej struktury. Jawne odwołanie do pakietu w pliku projektu aplikacji nie jest wymagane.

## <a name="ihostedservice-interface"></a>IHostedService, interfejs

Interfejs <xref:Microsoft.Extensions.Hosting.IHostedService> definiuje dwie metody dla obiektów zarządzanych przez hosta:

* [StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle. `StartAsync` jest wywoływana *przed*:

  * Skonfigurowano potok przetwarzania żądań aplikacji (`Startup.Configure`).
  * Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .

  Zachowanie domyślne można zmienić tak, aby `StartAsync` usługi hostowanej działała po skonfigurowaniu potoku aplikacji i po`ApplicationStarted` wywołaniu. Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu `ConfigureWebHostDefaults`:

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie. `StopAsync` zawiera logikę, aby zakończyć zadanie w tle. Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.

  Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny. Gdy żądanie anulowania jest wymagane na tokenie:

  * Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.
  * Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.

  Jednak zadania nie są porzucane po zażądaniu anulowania&mdash;obiekt wywołujący oczekuje na ukończenie wszystkich zadań.

  Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana. W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.

  Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego. Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web. Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/web-host#shutdown-timeout>.

Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji. Jeśli wystąpi błąd podczas wykonywania zadania w tle, `Dispose` powinien zostać wywołany, nawet jeśli `StopAsync` nie jest wywoływana.

## <a name="backgroundservice-base-class"></a>Klasa bazowa BackgroundService

<xref:Microsoft.Extensions.Hosting.BackgroundService> jest klasą bazową do implementowania długotrwałych <xref:Microsoft.Extensions.Hosting.IHostedService>.

[Wywoływanie ExecuteAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) jest wywoływana w celu uruchomienia usługi w tle. Implementacja zwraca <xref:System.Threading.Tasks.Task>, która reprezentuje cały okres istnienia usługi w tle. Żadne dalsze usługi nie są uruchamiane do momentu, gdy [wywoływanie ExecuteAsync staną się asynchroniczne](https://github.com/aspnet/Extensions/issues/2149), na przykład przez wywołanie `await`. Unikaj długotrwałych i blokowanych operacji inicjowania w `ExecuteAsync`. Bloki hosta w [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) oczekują na ukończenie `ExecuteAsync`.

Token anulowania jest wyzwalany, gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) . Implementacja `ExecuteAsync` powinna zostać zakończona natychmiast po uruchomieniu tokenu anulowania w celu łagodnego zamknięcia usługi. W przeciwnym razie usługa niebezpiecznie zamyka się po upływie limitu czasu zamykania. Aby uzyskać więcej informacji, zobacz sekcję dotyczącą [interfejsu IHostedService](#ihostedservice-interface) .

## <a name="timed-background-tasks"></a>Zadania w tle czasu

Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) . Czasomierz wyzwala metodę `DoWork` zadania. Czasomierz jest wyłączony na `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

<xref:System.Threading.Timer> nie czeka na zakończenie poprzednich wykonań `DoWork`, więc wskazane podejście może nie być odpowiednie dla każdego scenariusza. Z [blokadą. przyrost](xref:System.Threading.Interlocked.Increment*) służy do zwiększania licznika wykonywania jako operacja niepodzielna, dzięki czemu wiele wątków nie aktualizuje `executionCount` współbieżnie.

Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z metodą rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Zużywanie usługi w zakresie w zadaniu w tle

Aby użyć [usług z zakresem](xref:fundamentals/dependency-injection#service-lifetimes) w ramach [BackgroundService](#backgroundservice-base-class), Utwórz zakres. Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.

Usługa zadań w tle w zakresie zawiera logikę zadania w tle. W poniższym przykładzie:

* Usługa jest asynchroniczna. Metoda `DoWork` zwraca `Task`. W celach demonstracyjnych oczekuje się, że w metodzie `DoWork` czeka dziesięć sekund.
* <xref:Microsoft.Extensions.Logging.ILogger> jest wprowadzany do usługi.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`. `DoWork` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*). Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Zadania w kolejce w dół

Kolejka zadań w tle jest oparta na <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> .NET 4. x ([wstępnie zaplanowana do wbudowania dla ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

W poniższym przykładzie `QueueHostedService`:

* Metoda `BackgroundProcessing` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`.
* Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.
* Elementy robocze są oczekiwane przed zatrzymaniem usługi w `StopAsync`.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

Usługa `MonitorLoop` obsługuje zadania umieszczenie dla usługi hostowanej przy każdym wybraniu klucza `w` na urządzeniu wejściowym:

* `IBackgroundTaskQueue` jest wstrzykiwana do usługi `MonitorLoop`.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy.
* Element roboczy symuluje długotrwałe zadanie w tle:
  * Trzy 5-sekundowe opóźnienia są wykonywane (`Task.Delay`).
  * `try-catch` pułapki instrukcji <xref:System.OperationCanceledException>, jeśli zadanie zostało anulowane.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*). Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>. Ten temat zawiera trzy przykłady usługi hostowanej:

* Zadanie w tle, które jest uruchamiane na czasomierzu.
* Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie. Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)
* Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="package"></a>Package

Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .

## <a name="ihostedservice-interface"></a>IHostedService, interfejs

Usługi hostowane implementują interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>. Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:

* [StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle. W przypadku korzystania z [hosta sieci Web](xref:fundamentals/host/web-host)`StartAsync` jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) . W przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host)`StartAsync` jest wywoływana przed wyzwoleniem `ApplicationStarted`.

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie. `StopAsync` zawiera logikę, aby zakończyć zadanie w tle. Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.

  Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny. Gdy żądanie anulowania jest wymagane na tokenie:

  * Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.
  * Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.

  Jednak zadania nie są porzucane po zażądaniu anulowania&mdash;obiekt wywołujący oczekuje na ukończenie wszystkich zadań.

  Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana. W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.

  Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego. Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web. Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/web-host#shutdown-timeout>.

Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji. Jeśli wystąpi błąd podczas wykonywania zadania w tle, `Dispose` powinien zostać wywołany, nawet jeśli `StopAsync` nie jest wywoływana.

## <a name="timed-background-tasks"></a>Zadania w tle czasu

Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) . Czasomierz wyzwala metodę `DoWork` zadania. Czasomierz jest wyłączony na `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<xref:System.Threading.Timer> nie czeka na zakończenie poprzednich wykonań `DoWork`, więc wskazane podejście może nie być odpowiednie dla każdego scenariusza.

Usługa jest zarejestrowana w `Startup.ConfigureServices` z metodą rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Zużywanie usługi w zakresie w zadaniu w tle

Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `IHostedService`, Utwórz zakres. Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.

Usługa zadań w tle w zakresie zawiera logikę zadania w tle. W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Usługi są zarejestrowane w `Startup.ConfigureServices`. Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Zadania w kolejce w dół

Kolejka zadań w tle jest oparta na .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowano wbudowaną obsługę ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

W `QueueHostedService`zadania w tle w kolejce są odrzucane i wykonywane jako [BackgroundService](#backgroundservice-base-class), która jest klasą bazową do implementowania długotrwałych `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Usługi są zarejestrowane w `Startup.ConfigureServices`. Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

W klasie modelu strony indeksu:

* `IBackgroundTaskQueue` jest wprowadzany do konstruktora i przypisywany do `Queue`.
* <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> zostanie dodany i przypisany do `_serviceScopeFactory`. Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie. Zakres jest tworzony w celu używania `AppDbContext` aplikacji ( [usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych w `IBackgroundTaskQueue` (pojedynczej usłudze).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Po wybraniu przycisku **Dodaj zadanie** na stronie indeks jest wykonywana metoda `OnPostAddTask`. `QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
