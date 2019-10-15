---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak zaimplementować zadania w tle za pomocą usług hostowanych w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: c1fbb5ae8ffc4ee506f42df6a4cbbe845b2b903d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333660"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Zadania w tle z usługami hostowanymi w ASP.NET Core

Autorzy [Luke Latham](https://github.com/guardrex) i [Jeow li Huan](https://github.com/huan086)

::: moniker range=">= aspnetcore-3.0"

W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>. Ten temat zawiera trzy przykłady usługi hostowanej:

* Zadanie w tle, które jest uruchamiane na czasomierzu.
* Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie. Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).
* Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowa aplikacja jest dostępna w dwóch wersjach:

* Host sieci Web &ndash; hosta sieci Web jest przydatne do hostowania aplikacji sieci Web. Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej. Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .
* Hosta ogólnego &ndash; Host ogólny jest nowy w ASP.NET Core 2,1. Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .

## <a name="worker-service-template"></a>Szablon usługi procesu roboczego

Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi. Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a>Package

Odwołanie do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest dodawane niejawnie dla aplikacji ASP.NET Core.

## <a name="ihostedservice-interface"></a>IHostedService, interfejs

Interfejs <xref:Microsoft.Extensions.Hosting.IHostedService> definiuje dwie metody dla obiektów zarządzanych przez hosta:

* [StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle. `StartAsync` jest wywoływana *przed*:

  * Skonfigurowano potok przetwarzania żądań aplikacji (`Startup.Configure`).
  * Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .

  Zachowanie domyślne można zmienić tak, aby usługa hostowana `StartAsync` działała po skonfigurowaniu potoku aplikacji i wywołaniu `ApplicationStarted`. Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu `ConfigureWebHostDefaults`:

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

  Jednak zadania nie są porzucane po żądaniu anulowania żądania @ no__t-0the Caller czeka na ukończenie wszystkich zadań.

  Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana. W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.

  Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.

Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji. Jeśli wystąpi błąd podczas wykonywania zadania w tle, należy wywołać `Dispose`, nawet jeśli nie zostanie wywołane `StopAsync`.

## <a name="backgroundservice"></a>BackgroundService

`BackgroundService` jest klasą bazową do implementowania długotrwałego <xref:Microsoft.Extensions.Hosting.IHostedService>. `BackgroundService` zapewnia metodę abstrakcyjną `ExecuteAsync(CancellationToken stoppingToken)` w celu uwzględnienia logiki usługi. @No__t-0 jest wyzwalane, gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) . Implementacja tej metody zwraca `Task` reprezentujący cały okres istnienia usługi w tle.

Ponadto *Opcjonalnie* Przesłoń metody zdefiniowane w `IHostedService`, aby uruchomić kod uruchamiania i zamykania dla usługi:

* `StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` jest wywoływana, gdy host aplikacji wykonuje bezpieczne zamknięcie. Wartość `cancellationToken` jest sygnalizowane, gdy host zdecyduje się wymusić zakończenie usługi. Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy bazowej (i `await`), aby upewnić się, że usługa zostanie prawidłowo ZAMKNIĘTA.
* `StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` jest wywoływana w celu uruchomienia usługi w tle. Jeśli proces uruchamiania zostanie przerwany, sygnalizowane jest `cancellationToken`. Implementacja zwraca `Task` reprezentujący proces uruchamiania usługi. Żadne dalsze usługi nie są uruchamiane do momentu ukończenia `Task`. Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy bazowej (i `await`), aby upewnić się, że usługa jest uruchamiana prawidłowo.

## <a name="timed-background-tasks"></a>Zadania w tle czasu

Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) . Czasomierz wyzwala metodę `DoWork` zadania. Czasomierz jest wyłączony w `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z metodą rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Zużywanie usługi w zakresie w zadaniu w tle

Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `BackgroundService`, należy utworzyć zakres. Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.

Usługa zadań w tle w zakresie zawiera logikę zadania w tle. W poniższym przykładzie:

* Usługa jest asynchroniczna. Metoda `DoWork` zwraca `Task`. W celach demonstracyjnych oczekuje się, że w metodzie `DoWork` występuje opóźnienie o dziesięć sekund.
* @No__t-0 jest wstrzykiwana do usługi.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`. `DoWork` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*). Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Zadania w kolejce w dół

Kolejka zadań w tle jest oparta na platformie .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

W poniższym `QueueHostedService` przykład:

* Metoda `BackgroundProcessing` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`.
* Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.
* Elementy robocze oczekują na zatrzymanie usługi w `StopAsync`.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

Usługa `MonitorLoop` obsługuje zadania umieszczenie dla usługi hostowanej przy każdym wybraniu klucza `w` na urządzeniu wejściowym:

* @No__t-0 jest wstrzykiwana do usługi `MonitorLoop`.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy.
* Element roboczy symuluje długotrwałe zadanie w tle:
  * Trzy 5-sekundowe opóźnienia są wykonywane (`Task.Delay`).
  * @No__t-0 instrukcji Trap <xref:System.OperationCanceledException>, jeśli zadanie zostało anulowane.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*). Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>. Ten temat zawiera trzy przykłady usługi hostowanej:

* Zadanie w tle, które jest uruchamiane na czasomierzu.
* Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie. Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)
* Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowa aplikacja jest dostępna w dwóch wersjach:

* Host sieci Web &ndash; hosta sieci Web jest przydatne do hostowania aplikacji sieci Web. Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej. Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .
* Hosta ogólnego &ndash; Host ogólny jest nowy w ASP.NET Core 2,1. Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .

## <a name="package"></a>Package

Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .

## <a name="ihostedservice-interface"></a>IHostedService, interfejs

Usługi hostowane implementują interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>. Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:

* [StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle. W przypadku korzystania z [hosta sieci Web](xref:fundamentals/host/web-host)`StartAsync` jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) . W przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host)`StartAsync` jest wywoływana przed wyzwoleniem `ApplicationStarted`.

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie. `StopAsync` zawiera logikę, aby zakończyć zadanie w tle. Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.

  Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny. Gdy żądanie anulowania jest wymagane na tokenie:

  * Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.
  * Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.

  Jednak zadania nie są porzucane po żądaniu anulowania żądania @ no__t-0the Caller czeka na ukończenie wszystkich zadań.

  Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana. W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.

  Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.

Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji. Jeśli wystąpi błąd podczas wykonywania zadania w tle, należy wywołać `Dispose`, nawet jeśli nie zostanie wywołane `StopAsync`.

## <a name="timed-background-tasks"></a>Zadania w tle czasu

Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) . Czasomierz wyzwala metodę `DoWork` zadania. Czasomierz jest wyłączony w `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Usługa jest zarejestrowana w `Startup.ConfigureServices` z metodą rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Zużywanie usługi w zakresie w zadaniu w tle

Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w `IHostedService`, należy utworzyć zakres. Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.

Usługa zadań w tle w zakresie zawiera logikę zadania w tle. W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Usługi są zarejestrowane w `Startup.ConfigureServices`. Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Zadania w kolejce w dół

Kolejka zadań w tle jest oparta na .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowano wbudowaną obsługę ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

W `QueueHostedService` zadania w tle w kolejce są dekolejkowane i wykonywane jako <xref:Microsoft.Extensions.Hosting.BackgroundService>, który jest klasą bazową do implementowania długotrwałego `IHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Usługi są zarejestrowane w `Startup.ConfigureServices`. Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

W klasie modelu strony indeksu:

* @No__t-0 jest wstrzykiwana do konstruktora i przypisany do `Queue`.
* @No__t-0 jest wstrzykiwana i przypisywana do `_serviceScopeFactory`. Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w ramach zakresu. Zakres jest tworzony w celu używania `AppDbContext` ( [Usługa w zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych w `IBackgroundTaskQueue` (pojedyncza usługa).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Po wybraniu przycisku **Dodaj zadanie** na stronie indeks jest wykonywana metoda `OnPostAddTask`. `QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
