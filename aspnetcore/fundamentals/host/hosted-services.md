---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak zaimplementować zadania w tle za pomocą usług hostowanych w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0fdf503e4a5f6f73d5488261707180cfb5967492
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115941"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="c40ed-103">Zadania w tle z usługami hostowanymi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c40ed-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="c40ed-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Jeow li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="c40ed-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c40ed-105">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="c40ed-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="c40ed-106">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c40ed-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="c40ed-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="c40ed-108">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="c40ed-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="c40ed-109">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c40ed-110">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c40ed-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="c40ed-111">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="c40ed-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c40ed-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="c40ed-113">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="c40ed-113">Worker Service template</span></span>

<span data-ttu-id="c40ed-114">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="c40ed-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="c40ed-115">Aplikacja utworzona na podstawie szablonu usługi procesu roboczego określa zestaw SDK procesu roboczego w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="c40ed-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="c40ed-116">Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:</span><span class="sxs-lookup"><span data-stu-id="c40ed-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="c40ed-117">Package</span><span class="sxs-lookup"><span data-stu-id="c40ed-117">Package</span></span>

<span data-ttu-id="c40ed-118">Aplikacja oparta na szablonie usługi procesu roboczego używa zestawu SDK `Microsoft.NET.Sdk.Worker` i ma jawne odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="c40ed-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="c40ed-119">Na przykład zapoznaj się z plikiem projektu przykładowej aplikacji (*BackgroundTasksSample. csproj*).</span><span class="sxs-lookup"><span data-stu-id="c40ed-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="c40ed-120">W przypadku aplikacji sieci Web, które używają zestawu SDK `Microsoft.NET.Sdk.Web`, pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest przywoływany niejawnie z udostępnionej struktury.</span><span class="sxs-lookup"><span data-stu-id="c40ed-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="c40ed-121">Jawne odwołanie do pakietu w pliku projektu aplikacji nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="c40ed-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="c40ed-122">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="c40ed-122">IHostedService interface</span></span>

<span data-ttu-id="c40ed-123">Interfejs <xref:Microsoft.Extensions.Hosting.IHostedService> definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="c40ed-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="c40ed-124">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="c40ed-125">`StartAsync` jest wywoływana *przed*:</span><span class="sxs-lookup"><span data-stu-id="c40ed-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="c40ed-126">Skonfigurowano potok przetwarzania żądań aplikacji (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="c40ed-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="c40ed-127">Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="c40ed-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="c40ed-128">Zachowanie domyślne można zmienić tak, aby `StartAsync` usługi hostowanej działała po skonfigurowaniu potoku aplikacji i po`ApplicationStarted` wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="c40ed-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="c40ed-129">Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="c40ed-130">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="c40ed-131">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="c40ed-132">Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="c40ed-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="c40ed-133">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="c40ed-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="c40ed-134">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="c40ed-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="c40ed-135">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="c40ed-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="c40ed-136">Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="c40ed-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="c40ed-137">Jednak zadania nie są porzucane po zażądaniu anulowania&mdash;obiekt wywołujący oczekuje na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="c40ed-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="c40ed-138">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c40ed-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="c40ed-139">W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="c40ed-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="c40ed-140">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="c40ed-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="c40ed-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="c40ed-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="c40ed-142">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="c40ed-143">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c40ed-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="c40ed-144">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="c40ed-145">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c40ed-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="c40ed-146">Jeśli wystąpi błąd podczas wykonywania zadania w tle, `Dispose` powinien zostać wywołany, nawet jeśli `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c40ed-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="c40ed-147">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="c40ed-147">BackgroundService</span></span>

<span data-ttu-id="c40ed-148">`BackgroundService` jest klasą bazową do implementowania długotrwałych <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-148">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="c40ed-149">`BackgroundService` udostępnia metodę abstrakcyjną `ExecuteAsync(CancellationToken stoppingToken)` w celu uwzględnienia logiki usługi.</span><span class="sxs-lookup"><span data-stu-id="c40ed-149">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="c40ed-150">`stoppingToken` jest wyzwalana, gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="c40ed-150">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="c40ed-151">Implementacja tej metody zwraca `Task`, który reprezentuje cały okres istnienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-151">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="c40ed-152">Ponadto *Opcjonalnie* Przesłoń metody zdefiniowane w `IHostedService`, aby uruchomić kod uruchamiania i zamykania dla usługi:</span><span class="sxs-lookup"><span data-stu-id="c40ed-152">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="c40ed-153">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` jest wywoływana, gdy host aplikacji wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-153">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="c40ed-154">`cancellationToken` jest sygnalizowane, gdy host zdecyduje się wymusić zakończenie usługi.</span><span class="sxs-lookup"><span data-stu-id="c40ed-154">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="c40ed-155">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy bazowej (i `await`), aby upewnić się, że usługa zostanie prawidłowo ZAMKNIĘTA.</span><span class="sxs-lookup"><span data-stu-id="c40ed-155">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="c40ed-156">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` jest wywoływana w celu uruchomienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-156">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="c40ed-157">Jeśli proces uruchamiania zostanie przerwany, należy uzyskać `cancellationToken`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-157">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="c40ed-158">Implementacja zwraca `Task`, która reprezentuje proces uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="c40ed-158">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="c40ed-159">Żadne dalsze usługi nie są uruchamiane, dopóki ta `Task` nie zostanie ukończona.</span><span class="sxs-lookup"><span data-stu-id="c40ed-159">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="c40ed-160">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy bazowej (i `await`), aby upewnić się, że usługa jest uruchamiana prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="c40ed-160">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="c40ed-161">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="c40ed-161">Timed background tasks</span></span>

<span data-ttu-id="c40ed-162">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="c40ed-162">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="c40ed-163">Czasomierz wyzwala metodę `DoWork` zadania.</span><span class="sxs-lookup"><span data-stu-id="c40ed-163">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="c40ed-164">Czasomierz jest wyłączony na `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-164">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="c40ed-165">Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z metodą rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-165">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="c40ed-166">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="c40ed-166">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="c40ed-167">Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `BackgroundService`, Utwórz zakres.</span><span class="sxs-lookup"><span data-stu-id="c40ed-167">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="c40ed-168">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="c40ed-168">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="c40ed-169">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-169">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="c40ed-170">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c40ed-170">In the following example:</span></span>

* <span data-ttu-id="c40ed-171">Usługa jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="c40ed-171">The service is asynchronous.</span></span> <span data-ttu-id="c40ed-172">Metoda `DoWork` zwraca `Task`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-172">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="c40ed-173">W celach demonstracyjnych oczekuje się, że w metodzie `DoWork` czeka dziesięć sekund.</span><span class="sxs-lookup"><span data-stu-id="c40ed-173">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="c40ed-174"><xref:Microsoft.Extensions.Logging.ILogger> jest wprowadzany do usługi.</span><span class="sxs-lookup"><span data-stu-id="c40ed-174">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="c40ed-175">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-175">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="c40ed-176">`DoWork` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-176">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="c40ed-177">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="c40ed-177">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="c40ed-178">Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-178">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="c40ed-179">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="c40ed-179">Queued background tasks</span></span>

<span data-ttu-id="c40ed-180">Kolejka zadań w tle jest oparta na <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> .NET 4. x ([wstępnie zaplanowana do wbudowania dla ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="c40ed-180">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="c40ed-181">W poniższym przykładzie `QueueHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-181">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="c40ed-182">Metoda `BackgroundProcessing` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-182">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="c40ed-183">Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-183">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="c40ed-184">Elementy robocze są oczekiwane przed zatrzymaniem usługi w `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-184">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="c40ed-185">Usługa `MonitorLoop` obsługuje zadania umieszczenie dla usługi hostowanej przy każdym wybraniu klucza `w` na urządzeniu wejściowym:</span><span class="sxs-lookup"><span data-stu-id="c40ed-185">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="c40ed-186">`IBackgroundTaskQueue` jest wstrzykiwana do usługi `MonitorLoop`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-186">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="c40ed-187">`IBackgroundTaskQueue.QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy.</span><span class="sxs-lookup"><span data-stu-id="c40ed-187">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="c40ed-188">Element roboczy symuluje długotrwałe zadanie w tle:</span><span class="sxs-lookup"><span data-stu-id="c40ed-188">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="c40ed-189">Trzy 5-sekundowe opóźnienia są wykonywane (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="c40ed-189">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="c40ed-190">`try-catch` pułapki instrukcji <xref:System.OperationCanceledException>, jeśli zadanie zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="c40ed-190">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="c40ed-191">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="c40ed-191">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="c40ed-192">Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-192">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c40ed-193">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="c40ed-193">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="c40ed-194">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-194">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c40ed-195">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="c40ed-195">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="c40ed-196">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="c40ed-196">Background task that runs on a timer.</span></span>
* <span data-ttu-id="c40ed-197">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-197">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c40ed-198">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="c40ed-198">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="c40ed-199">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-199">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="c40ed-200">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c40ed-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="c40ed-201">Package</span><span class="sxs-lookup"><span data-stu-id="c40ed-201">Package</span></span>

<span data-ttu-id="c40ed-202">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="c40ed-202">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="c40ed-203">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="c40ed-203">IHostedService interface</span></span>

<span data-ttu-id="c40ed-204">Usługi hostowane implementują interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-204">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c40ed-205">Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="c40ed-205">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="c40ed-206">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-206">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="c40ed-207">W przypadku korzystania z [hosta sieci Web](xref:fundamentals/host/web-host)`StartAsync` jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="c40ed-207">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="c40ed-208">W przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host)`StartAsync` jest wywoływana przed wyzwoleniem `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-208">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="c40ed-209">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-209">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="c40ed-210">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-210">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="c40ed-211">Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="c40ed-211">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="c40ed-212">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="c40ed-212">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="c40ed-213">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="c40ed-213">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="c40ed-214">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="c40ed-214">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="c40ed-215">Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="c40ed-215">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="c40ed-216">Jednak zadania nie są porzucane po zażądaniu anulowania&mdash;obiekt wywołujący oczekuje na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="c40ed-216">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="c40ed-217">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c40ed-217">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="c40ed-218">W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="c40ed-218">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="c40ed-219">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="c40ed-219">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="c40ed-220"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="c40ed-220"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="c40ed-221">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-221">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="c40ed-222">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c40ed-222">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="c40ed-223">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c40ed-223">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="c40ed-224">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c40ed-224">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="c40ed-225">Jeśli wystąpi błąd podczas wykonywania zadania w tle, `Dispose` powinien zostać wywołany, nawet jeśli `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c40ed-225">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="c40ed-226">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="c40ed-226">Timed background tasks</span></span>

<span data-ttu-id="c40ed-227">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="c40ed-227">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="c40ed-228">Czasomierz wyzwala metodę `DoWork` zadania.</span><span class="sxs-lookup"><span data-stu-id="c40ed-228">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="c40ed-229">Czasomierz jest wyłączony na `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-229">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="c40ed-230">Usługa jest zarejestrowana w `Startup.ConfigureServices` z metodą rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-230">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="c40ed-231">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="c40ed-231">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="c40ed-232">Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `IHostedService`, Utwórz zakres.</span><span class="sxs-lookup"><span data-stu-id="c40ed-232">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="c40ed-233">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="c40ed-233">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="c40ed-234">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c40ed-234">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="c40ed-235">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:</span><span class="sxs-lookup"><span data-stu-id="c40ed-235">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="c40ed-236">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-236">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="c40ed-237">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-237">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c40ed-238">Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-238">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="c40ed-239">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="c40ed-239">Queued background tasks</span></span>

<span data-ttu-id="c40ed-240">Kolejka zadań w tle jest oparta na .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowano wbudowaną obsługę ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="c40ed-240">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="c40ed-241">W `QueueHostedService`zadania w tle w kolejce są odrzucane i wykonywane jako <xref:Microsoft.Extensions.Hosting.BackgroundService>, która jest klasą bazową do implementowania długotrwałego `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-241">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="c40ed-242">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-242">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c40ed-243">Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c40ed-243">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="c40ed-244">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="c40ed-244">In the Index page model class:</span></span>

* <span data-ttu-id="c40ed-245">`IBackgroundTaskQueue` jest wprowadzany do konstruktora i przypisywany do `Queue`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-245">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="c40ed-246"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> zostanie dodany i przypisany do `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-246">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="c40ed-247">Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie.</span><span class="sxs-lookup"><span data-stu-id="c40ed-247">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="c40ed-248">Zakres jest tworzony w celu używania `AppDbContext` aplikacji ( [usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych w `IBackgroundTaskQueue` (pojedynczej usłudze).</span><span class="sxs-lookup"><span data-stu-id="c40ed-248">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="c40ed-249">Po wybraniu przycisku **Dodaj zadanie** na stronie indeks jest wykonywana metoda `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="c40ed-249">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="c40ed-250">`QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy:</span><span class="sxs-lookup"><span data-stu-id="c40ed-250">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c40ed-251">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c40ed-251">Additional resources</span></span>

* [<span data-ttu-id="c40ed-252">Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService</span><span class="sxs-lookup"><span data-stu-id="c40ed-252">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
