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
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="8a103-103">Zadania w tle z usługami hostowanymi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a103-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="8a103-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Jeow li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="8a103-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8a103-105">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="8a103-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="8a103-106">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8a103-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="8a103-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="8a103-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="8a103-108">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="8a103-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="8a103-109">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="8a103-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8a103-110">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8a103-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="8a103-111">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="8a103-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="8a103-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a103-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8a103-113">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="8a103-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="8a103-114">Host sieci Web &ndash; hosta sieci Web jest przydatne do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8a103-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="8a103-115">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="8a103-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="8a103-116">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="8a103-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="8a103-117">Hosta ogólnego &ndash; Host ogólny jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="8a103-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="8a103-118">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="8a103-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="8a103-119">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="8a103-119">Worker Service template</span></span>

<span data-ttu-id="8a103-120">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="8a103-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="8a103-121">Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:</span><span class="sxs-lookup"><span data-stu-id="8a103-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="8a103-122">Package</span><span class="sxs-lookup"><span data-stu-id="8a103-122">Package</span></span>

<span data-ttu-id="8a103-123">Odwołanie do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest dodawane niejawnie dla aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a103-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="8a103-124">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="8a103-124">IHostedService interface</span></span>

<span data-ttu-id="8a103-125">Interfejs <xref:Microsoft.Extensions.Hosting.IHostedService> definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="8a103-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="8a103-126">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="8a103-127">`StartAsync` jest wywoływana *przed*:</span><span class="sxs-lookup"><span data-stu-id="8a103-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="8a103-128">Skonfigurowano potok przetwarzania żądań aplikacji (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="8a103-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="8a103-129">Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="8a103-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="8a103-130">Zachowanie domyślne można zmienić tak, aby usługa hostowana `StartAsync` działała po skonfigurowaniu potoku aplikacji i wywołaniu `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="8a103-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="8a103-131">Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="8a103-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="8a103-132">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="8a103-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="8a103-133">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="8a103-134">Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="8a103-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="8a103-135">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="8a103-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="8a103-136">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="8a103-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="8a103-137">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="8a103-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="8a103-138">Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="8a103-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="8a103-139">Jednak zadania nie są porzucane po żądaniu anulowania żądania @ no__t-0the Caller czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="8a103-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="8a103-140">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8a103-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="8a103-141">W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="8a103-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="8a103-142">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="8a103-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="8a103-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="8a103-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="8a103-144">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8a103-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="8a103-145">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8a103-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="8a103-146">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8a103-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="8a103-147">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a103-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="8a103-148">Jeśli wystąpi błąd podczas wykonywania zadania w tle, należy wywołać `Dispose`, nawet jeśli nie zostanie wywołane `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a103-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="8a103-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="8a103-149">BackgroundService</span></span>

<span data-ttu-id="8a103-150">`BackgroundService` jest klasą bazową do implementowania długotrwałego <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8a103-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="8a103-151">`BackgroundService` zapewnia metodę abstrakcyjną `ExecuteAsync(CancellationToken stoppingToken)` w celu uwzględnienia logiki usługi.</span><span class="sxs-lookup"><span data-stu-id="8a103-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="8a103-152">@No__t-0 jest wyzwalane, gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="8a103-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="8a103-153">Implementacja tej metody zwraca `Task` reprezentujący cały okres istnienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="8a103-154">Ponadto *Opcjonalnie* Przesłoń metody zdefiniowane w `IHostedService`, aby uruchomić kod uruchamiania i zamykania dla usługi:</span><span class="sxs-lookup"><span data-stu-id="8a103-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="8a103-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` jest wywoływana, gdy host aplikacji wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="8a103-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="8a103-156">Wartość `cancellationToken` jest sygnalizowane, gdy host zdecyduje się wymusić zakończenie usługi.</span><span class="sxs-lookup"><span data-stu-id="8a103-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="8a103-157">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy bazowej (i `await`), aby upewnić się, że usługa zostanie prawidłowo ZAMKNIĘTA.</span><span class="sxs-lookup"><span data-stu-id="8a103-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="8a103-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` jest wywoływana w celu uruchomienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="8a103-159">Jeśli proces uruchamiania zostanie przerwany, sygnalizowane jest `cancellationToken`.</span><span class="sxs-lookup"><span data-stu-id="8a103-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="8a103-160">Implementacja zwraca `Task` reprezentujący proces uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="8a103-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="8a103-161">Żadne dalsze usługi nie są uruchamiane do momentu ukończenia `Task`.</span><span class="sxs-lookup"><span data-stu-id="8a103-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="8a103-162">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy bazowej (i `await`), aby upewnić się, że usługa jest uruchamiana prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="8a103-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="8a103-163">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="8a103-163">Timed background tasks</span></span>

<span data-ttu-id="8a103-164">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="8a103-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="8a103-165">Czasomierz wyzwala metodę `DoWork` zadania.</span><span class="sxs-lookup"><span data-stu-id="8a103-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="8a103-166">Czasomierz jest wyłączony w `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="8a103-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="8a103-167">Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z metodą rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8a103-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="8a103-168">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="8a103-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="8a103-169">Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `BackgroundService`, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="8a103-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="8a103-170">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="8a103-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="8a103-171">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="8a103-172">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="8a103-172">In the following example:</span></span>

* <span data-ttu-id="8a103-173">Usługa jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="8a103-173">The service is asynchronous.</span></span> <span data-ttu-id="8a103-174">Metoda `DoWork` zwraca `Task`.</span><span class="sxs-lookup"><span data-stu-id="8a103-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="8a103-175">W celach demonstracyjnych oczekuje się, że w metodzie `DoWork` występuje opóźnienie o dziesięć sekund.</span><span class="sxs-lookup"><span data-stu-id="8a103-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="8a103-176">@No__t-0 jest wstrzykiwana do usługi.</span><span class="sxs-lookup"><span data-stu-id="8a103-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="8a103-177">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="8a103-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="8a103-178">`DoWork` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="8a103-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="8a103-179">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="8a103-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="8a103-180">Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8a103-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="8a103-181">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="8a103-181">Queued background tasks</span></span>

<span data-ttu-id="8a103-182">Kolejka zadań w tle jest oparta na platformie .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="8a103-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="8a103-183">W poniższym `QueueHostedService` przykład:</span><span class="sxs-lookup"><span data-stu-id="8a103-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="8a103-184">Metoda `BackgroundProcessing` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a103-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="8a103-185">Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="8a103-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="8a103-186">Elementy robocze oczekują na zatrzymanie usługi w `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a103-186">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="8a103-187">Usługa `MonitorLoop` obsługuje zadania umieszczenie dla usługi hostowanej przy każdym wybraniu klucza `w` na urządzeniu wejściowym:</span><span class="sxs-lookup"><span data-stu-id="8a103-187">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="8a103-188">@No__t-0 jest wstrzykiwana do usługi `MonitorLoop`.</span><span class="sxs-lookup"><span data-stu-id="8a103-188">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="8a103-189">`IBackgroundTaskQueue.QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy.</span><span class="sxs-lookup"><span data-stu-id="8a103-189">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="8a103-190">Element roboczy symuluje długotrwałe zadanie w tle:</span><span class="sxs-lookup"><span data-stu-id="8a103-190">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="8a103-191">Trzy 5-sekundowe opóźnienia są wykonywane (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="8a103-191">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="8a103-192">@No__t-0 instrukcji Trap <xref:System.OperationCanceledException>, jeśli zadanie zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="8a103-192">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="8a103-193">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="8a103-193">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="8a103-194">Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8a103-194">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8a103-195">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="8a103-195">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="8a103-196">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8a103-196">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="8a103-197">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="8a103-197">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="8a103-198">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="8a103-198">Background task that runs on a timer.</span></span>
* <span data-ttu-id="8a103-199">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="8a103-199">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8a103-200">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="8a103-200">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="8a103-201">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="8a103-201">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="8a103-202">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a103-202">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8a103-203">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="8a103-203">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="8a103-204">Host sieci Web &ndash; hosta sieci Web jest przydatne do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8a103-204">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="8a103-205">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="8a103-205">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="8a103-206">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="8a103-206">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="8a103-207">Hosta ogólnego &ndash; Host ogólny jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="8a103-207">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="8a103-208">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="8a103-208">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="8a103-209">Package</span><span class="sxs-lookup"><span data-stu-id="8a103-209">Package</span></span>

<span data-ttu-id="8a103-210">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="8a103-210">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="8a103-211">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="8a103-211">IHostedService interface</span></span>

<span data-ttu-id="8a103-212">Usługi hostowane implementują interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8a103-212">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="8a103-213">Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="8a103-213">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="8a103-214">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-214">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="8a103-215">W przypadku korzystania z [hosta sieci Web](xref:fundamentals/host/web-host)`StartAsync` jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="8a103-215">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="8a103-216">W przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host)`StartAsync` jest wywoływana przed wyzwoleniem `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="8a103-216">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="8a103-217">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="8a103-217">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="8a103-218">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-218">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="8a103-219">Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="8a103-219">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="8a103-220">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="8a103-220">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="8a103-221">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="8a103-221">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="8a103-222">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="8a103-222">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="8a103-223">Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="8a103-223">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="8a103-224">Jednak zadania nie są porzucane po żądaniu anulowania żądania @ no__t-0the Caller czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="8a103-224">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="8a103-225">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8a103-225">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="8a103-226">W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="8a103-226">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="8a103-227">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="8a103-227">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="8a103-228"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="8a103-228"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="8a103-229">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8a103-229">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="8a103-230">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8a103-230">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="8a103-231">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8a103-231">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="8a103-232">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a103-232">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="8a103-233">Jeśli wystąpi błąd podczas wykonywania zadania w tle, należy wywołać `Dispose`, nawet jeśli nie zostanie wywołane `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a103-233">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="8a103-234">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="8a103-234">Timed background tasks</span></span>

<span data-ttu-id="8a103-235">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="8a103-235">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="8a103-236">Czasomierz wyzwala metodę `DoWork` zadania.</span><span class="sxs-lookup"><span data-stu-id="8a103-236">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="8a103-237">Czasomierz jest wyłączony w `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="8a103-237">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="8a103-238">Usługa jest zarejestrowana w `Startup.ConfigureServices` z metodą rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8a103-238">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="8a103-239">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="8a103-239">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="8a103-240">Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w `IHostedService`, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="8a103-240">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="8a103-241">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="8a103-241">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="8a103-242">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8a103-242">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="8a103-243">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:</span><span class="sxs-lookup"><span data-stu-id="8a103-243">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="8a103-244">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="8a103-244">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="8a103-245">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8a103-245">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8a103-246">Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8a103-246">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="8a103-247">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="8a103-247">Queued background tasks</span></span>

<span data-ttu-id="8a103-248">Kolejka zadań w tle jest oparta na .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowano wbudowaną obsługę ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="8a103-248">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="8a103-249">W `QueueHostedService` zadania w tle w kolejce są dekolejkowane i wykonywane jako <xref:Microsoft.Extensions.Hosting.BackgroundService>, który jest klasą bazową do implementowania długotrwałego `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8a103-249">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="8a103-250">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8a103-250">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8a103-251">Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8a103-251">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="8a103-252">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="8a103-252">In the Index page model class:</span></span>

* <span data-ttu-id="8a103-253">@No__t-0 jest wstrzykiwana do konstruktora i przypisany do `Queue`.</span><span class="sxs-lookup"><span data-stu-id="8a103-253">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="8a103-254">@No__t-0 jest wstrzykiwana i przypisywana do `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="8a103-254">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="8a103-255">Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w ramach zakresu.</span><span class="sxs-lookup"><span data-stu-id="8a103-255">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="8a103-256">Zakres jest tworzony w celu używania `AppDbContext` ( [Usługa w zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych w `IBackgroundTaskQueue` (pojedyncza usługa).</span><span class="sxs-lookup"><span data-stu-id="8a103-256">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="8a103-257">Po wybraniu przycisku **Dodaj zadanie** na stronie indeks jest wykonywana metoda `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="8a103-257">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="8a103-258">`QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy:</span><span class="sxs-lookup"><span data-stu-id="8a103-258">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8a103-259">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8a103-259">Additional resources</span></span>

* [<span data-ttu-id="8a103-260">Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService</span><span class="sxs-lookup"><span data-stu-id="8a103-260">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
