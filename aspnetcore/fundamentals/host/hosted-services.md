---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0eaa3a62370c1e413840bb65f597dc664adafc38
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688098"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="cfc56-103">Zadania w tle z usługami hostowanymi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cfc56-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="cfc56-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cfc56-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cfc56-105">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="cfc56-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="cfc56-106">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="cfc56-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="cfc56-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="cfc56-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="cfc56-108">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="cfc56-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="cfc56-109">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="cfc56-110">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cfc56-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="cfc56-111">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="cfc56-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cfc56-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cfc56-113">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="cfc56-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="cfc56-114">Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cfc56-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="cfc56-115">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="cfc56-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="cfc56-116">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="cfc56-117">Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="cfc56-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="cfc56-118">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="cfc56-119">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="cfc56-119">Worker Service template</span></span>

<span data-ttu-id="cfc56-120">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="cfc56-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="cfc56-121">Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:</span><span class="sxs-lookup"><span data-stu-id="cfc56-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="cfc56-122">Pakiet</span><span class="sxs-lookup"><span data-stu-id="cfc56-122">Package</span></span>

<span data-ttu-id="cfc56-123">Odwołanie do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest dodawane niejawnie dla aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cfc56-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="cfc56-124">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="cfc56-124">IHostedService interface</span></span>

<span data-ttu-id="cfc56-125"><xref:Microsoft.Extensions.Hosting.IHostedService> Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="cfc56-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="cfc56-126">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="cfc56-127">`StartAsync`jest wywoływana *przed*:</span><span class="sxs-lookup"><span data-stu-id="cfc56-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="cfc56-128">Skonfigurowano potok przetwarzania żądania aplikacji (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="cfc56-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="cfc56-129">Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="cfc56-130">Zachowanie domyślne można zmienić tak, aby usługa `StartAsync` hostowana działała po skonfigurowaniu potoku aplikacji i `ApplicationStarted` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="cfc56-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="cfc56-131">Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu: `ConfigureWebHostDefaults`</span><span class="sxs-lookup"><span data-stu-id="cfc56-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="cfc56-132">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="cfc56-133">`StopAsync`zawiera logikę końcową zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="cfc56-134">Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="cfc56-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="cfc56-135">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="cfc56-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="cfc56-136">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="cfc56-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="cfc56-137">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="cfc56-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="cfc56-138">Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="cfc56-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="cfc56-139">Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="cfc56-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="cfc56-140">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany.</span><span class="sxs-lookup"><span data-stu-id="cfc56-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="cfc56-141">W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="cfc56-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="cfc56-142">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="cfc56-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="cfc56-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="cfc56-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="cfc56-144">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="cfc56-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="cfc56-145">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cfc56-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="cfc56-146">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="cfc56-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="cfc56-147">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cfc56-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="cfc56-148">Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="cfc56-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="cfc56-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="cfc56-149">BackgroundService</span></span>

<span data-ttu-id="cfc56-150">`BackgroundService`jest klasą bazową do implementowania długotrwałego działania <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="cfc56-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="cfc56-151">`BackgroundService`udostępnia metodę `ExecuteAsync(CancellationToken stoppingToken)` abstrakcyjną, która będzie zawierać logikę usługi.</span><span class="sxs-lookup"><span data-stu-id="cfc56-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="cfc56-152">Jest `stoppingToken` wyzwalane, gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="cfc56-153">Implementacja tej metody zwraca `Task` wartość reprezentującą cały okres istnienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="cfc56-154">Ponadto *Opcjonalnie* Przesłoń metody zdefiniowane na `IHostedService` potrzeby uruchamiania kodu uruchamiania i zamykania dla usługi:</span><span class="sxs-lookup"><span data-stu-id="cfc56-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="cfc56-155">`StopAsync(CancellationToken cancellationToken)`&ndash; jestwywoływana,gdyhost`StopAsync` aplikacji wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="cfc56-156">Jest `cancellationToken` on sygnalizowane, gdy host zdecyduje się wymusić zakończenie usługi.</span><span class="sxs-lookup"><span data-stu-id="cfc56-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="cfc56-157">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy `await`bazowej (i), aby upewnić się, że usługa jest prawidłowo zamykana.</span><span class="sxs-lookup"><span data-stu-id="cfc56-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="cfc56-158">`StartAsync(CancellationToken cancellationToken)`&ndash; jestwywoływanawcelu`StartAsync` uruchomienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="cfc56-159">Jest `cancellationToken` on sygnalizowane, jeśli proces uruchamiania zostanie przerwany.</span><span class="sxs-lookup"><span data-stu-id="cfc56-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="cfc56-160">Implementacja zwraca `Task` , która reprezentuje proces uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="cfc56-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="cfc56-161">Żadne dalsze usługi nie są uruchamiane do `Task` momentu zakończenia tego procesu.</span><span class="sxs-lookup"><span data-stu-id="cfc56-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="cfc56-162">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy `await`bazowej (i), aby upewnić się, że usługa jest uruchamiana prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="cfc56-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="cfc56-163">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="cfc56-163">Timed background tasks</span></span>

<span data-ttu-id="cfc56-164">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="cfc56-165">Czasomierz wyzwala `DoWork` metodę zadania.</span><span class="sxs-lookup"><span data-stu-id="cfc56-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="cfc56-166">Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:</span><span class="sxs-lookup"><span data-stu-id="cfc56-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="cfc56-167">Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z `AddHostedService` metodą rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="cfc56-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="cfc56-168">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="cfc56-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="cfc56-169">Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `BackgroundService`w ramach programu, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="cfc56-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="cfc56-170">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="cfc56-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="cfc56-171">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="cfc56-172">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cfc56-172">In the following example:</span></span>

* <span data-ttu-id="cfc56-173">Usługa jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="cfc56-173">The service is asynchronous.</span></span> <span data-ttu-id="cfc56-174">`DoWork` Metoda`Task`zwraca.</span><span class="sxs-lookup"><span data-stu-id="cfc56-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="cfc56-175">W celach demonstracyjnych oczekuje się, że w `DoWork` metodzie występuje opóźnienie o dziesięć sekund.</span><span class="sxs-lookup"><span data-stu-id="cfc56-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="cfc56-176"><xref:Microsoft.Extensions.Logging.ILogger> Jest wstrzykiwana do usługi.</span><span class="sxs-lookup"><span data-stu-id="cfc56-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="cfc56-177">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać `DoWork` metodę.</span><span class="sxs-lookup"><span data-stu-id="cfc56-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="cfc56-178">`DoWork`Zwraca element `Task`, który jest oczekiwany w `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="cfc56-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="cfc56-179">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` usłudze (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="cfc56-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="cfc56-180">Usługa hostowana jest zarejestrowana przy `AddHostedService` użyciu metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="cfc56-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="cfc56-181">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="cfc56-181">Queued background tasks</span></span>

<span data-ttu-id="cfc56-182">Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="cfc56-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="cfc56-183">W poniższym `QueueHostedService` przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cfc56-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="cfc56-184">Metoda zwraca obiekt `Task`, który jest oczekiwany w `ExecuteAsync`. `BackgroundProcessing`</span><span class="sxs-lookup"><span data-stu-id="cfc56-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="cfc56-185">Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="cfc56-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="cfc56-186">Usługa obsługuje zadania umieszczenie dla usługi hostowanej za każdym razem `w` , gdy klucz zostanie wybrany na urządzeniu wejściowym: `MonitorLoop`</span><span class="sxs-lookup"><span data-stu-id="cfc56-186">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="cfc56-187">Jest wstrzykiwana do `MonitorLoop`usługi. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="cfc56-187">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="cfc56-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy.</span><span class="sxs-lookup"><span data-stu-id="cfc56-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="cfc56-189">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` usłudze (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="cfc56-189">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="cfc56-190">Usługa hostowana jest zarejestrowana przy `AddHostedService` użyciu metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="cfc56-190">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cfc56-191">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="cfc56-191">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="cfc56-192">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="cfc56-192">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="cfc56-193">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="cfc56-193">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="cfc56-194">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="cfc56-194">Background task that runs on a timer.</span></span>
* <span data-ttu-id="cfc56-195">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-195">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="cfc56-196">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="cfc56-196">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="cfc56-197">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-197">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="cfc56-198">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cfc56-198">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cfc56-199">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="cfc56-199">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="cfc56-200">Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cfc56-200">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="cfc56-201">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="cfc56-201">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="cfc56-202">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-202">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="cfc56-203">Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="cfc56-203">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="cfc56-204">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-204">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="cfc56-205">Pakiet</span><span class="sxs-lookup"><span data-stu-id="cfc56-205">Package</span></span>

<span data-ttu-id="cfc56-206">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-206">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="cfc56-207">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="cfc56-207">IHostedService interface</span></span>

<span data-ttu-id="cfc56-208">Usługi hostowane implementują <xref:Microsoft.Extensions.Hosting.IHostedService> interfejs.</span><span class="sxs-lookup"><span data-stu-id="cfc56-208">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="cfc56-209">Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="cfc56-209">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="cfc56-210">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="cfc56-211">W przypadku korzystania z `StartAsync` [hosta sieci Web](xref:fundamentals/host/web-host)jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-211">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="cfc56-212">W przypadku korzystania z `StartAsync` [hosta generycznego](xref:fundamentals/host/generic-host)jest wywoływana `ApplicationStarted` przed wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="cfc56-212">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="cfc56-213">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="cfc56-214">`StopAsync`zawiera logikę końcową zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-214">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="cfc56-215">Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="cfc56-215">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="cfc56-216">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="cfc56-216">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="cfc56-217">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="cfc56-217">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="cfc56-218">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="cfc56-218">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="cfc56-219">Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="cfc56-219">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="cfc56-220">Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="cfc56-220">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="cfc56-221">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany.</span><span class="sxs-lookup"><span data-stu-id="cfc56-221">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="cfc56-222">W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="cfc56-222">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="cfc56-223">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="cfc56-223">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="cfc56-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="cfc56-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="cfc56-225">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="cfc56-225">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="cfc56-226">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="cfc56-226">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="cfc56-227">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="cfc56-227">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="cfc56-228">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cfc56-228">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="cfc56-229">Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="cfc56-229">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="cfc56-230">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="cfc56-230">Timed background tasks</span></span>

<span data-ttu-id="cfc56-231">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="cfc56-231">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="cfc56-232">Czasomierz wyzwala `DoWork` metodę zadania.</span><span class="sxs-lookup"><span data-stu-id="cfc56-232">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="cfc56-233">Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:</span><span class="sxs-lookup"><span data-stu-id="cfc56-233">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="cfc56-234">Usługa jest zarejestrowana w `Startup.ConfigureServices` `AddHostedService` ramach metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="cfc56-234">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="cfc56-235">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="cfc56-235">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="cfc56-236">Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `IHostedService`w ramach programu, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="cfc56-236">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="cfc56-237">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="cfc56-237">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="cfc56-238">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="cfc56-238">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="cfc56-239">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:</span><span class="sxs-lookup"><span data-stu-id="cfc56-239">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="cfc56-240">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie w celu wywołania `DoWork` jej metody:</span><span class="sxs-lookup"><span data-stu-id="cfc56-240">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="cfc56-241">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="cfc56-241">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cfc56-242">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="cfc56-242">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="cfc56-243">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="cfc56-243">Queued background tasks</span></span>

<span data-ttu-id="cfc56-244">Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="cfc56-244">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="cfc56-245">W `QueueHostedService`programie zadania w tle w kolejce są odrzucane i wykonywane <xref:Microsoft.Extensions.Hosting.BackgroundService>jako, która jest klasą bazową do implementowania długotrwałego `IHostedService`działania:</span><span class="sxs-lookup"><span data-stu-id="cfc56-245">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="cfc56-246">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="cfc56-246">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cfc56-247">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="cfc56-247">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="cfc56-248">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="cfc56-248">In the Index page model class:</span></span>

* <span data-ttu-id="cfc56-249">Zostaje wstrzyknięty do konstruktora i przypisany do `Queue`. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="cfc56-249">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="cfc56-250">Jest wstrzykiwany i przypisany do `_serviceScopeFactory`. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory></span><span class="sxs-lookup"><span data-stu-id="cfc56-250">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="cfc56-251">Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie.</span><span class="sxs-lookup"><span data-stu-id="cfc56-251">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="cfc56-252">Zakres jest tworzony w celu używania aplikacji `AppDbContext` (usługi w [zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów `IBackgroundTaskQueue` bazy danych w (pojedynczej usłudze).</span><span class="sxs-lookup"><span data-stu-id="cfc56-252">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="cfc56-253">Po wybraniu przycisku **Dodaj zadanie** na stronie `OnPostAddTask` indeks jest wykonywana metoda.</span><span class="sxs-lookup"><span data-stu-id="cfc56-253">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="cfc56-254">`QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy:</span><span class="sxs-lookup"><span data-stu-id="cfc56-254">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cfc56-255">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cfc56-255">Additional resources</span></span>

* [<span data-ttu-id="cfc56-256">Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService</span><span class="sxs-lookup"><span data-stu-id="cfc56-256">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
