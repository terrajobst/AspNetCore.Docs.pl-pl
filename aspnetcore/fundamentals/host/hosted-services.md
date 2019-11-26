---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/19/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: da3c2679005714a3d82de94cf3bc3c809aa3500d
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239732"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="8d51b-103">Zadania w tle z usługami hostowanymi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d51b-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="8d51b-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Jeow li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="8d51b-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8d51b-105">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="8d51b-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="8d51b-106">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="8d51b-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="8d51b-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="8d51b-108">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="8d51b-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="8d51b-109">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="8d51b-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8d51b-110">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8d51b-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="8d51b-111">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="8d51b-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="8d51b-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8d51b-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="8d51b-113">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="8d51b-113">Worker Service template</span></span>

<span data-ttu-id="8d51b-114">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="8d51b-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="8d51b-115">Aplikacja utworzona na podstawie szablonu usługi procesu roboczego określa zestaw SDK procesu roboczego w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="8d51b-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="8d51b-116">Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:</span><span class="sxs-lookup"><span data-stu-id="8d51b-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="8d51b-117">Package</span><span class="sxs-lookup"><span data-stu-id="8d51b-117">Package</span></span>

<span data-ttu-id="8d51b-118">Aplikacja oparta na szablonie usługi procesu roboczego używa zestawu SDK `Microsoft.NET.Sdk.Worker` i ma jawne odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="8d51b-119">Na przykład zapoznaj się z plikiem projektu przykładowej aplikacji (*BackgroundTasksSample. csproj*).</span><span class="sxs-lookup"><span data-stu-id="8d51b-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="8d51b-120">W przypadku aplikacji sieci Web, które używają zestawu SDK `Microsoft.NET.Sdk.Web`, pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest przywoływany niejawnie z udostępnionej struktury.</span><span class="sxs-lookup"><span data-stu-id="8d51b-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="8d51b-121">Jawne odwołanie do pakietu w pliku projektu aplikacji nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="8d51b-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="8d51b-122">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="8d51b-122">IHostedService interface</span></span>

<span data-ttu-id="8d51b-123">Interfejs <xref:Microsoft.Extensions.Hosting.IHostedService> definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="8d51b-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="8d51b-124">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="8d51b-125">`StartAsync` jest wywoływana *przed*:</span><span class="sxs-lookup"><span data-stu-id="8d51b-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="8d51b-126">Skonfigurowano potok przetwarzania żądań aplikacji (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="8d51b-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="8d51b-127">Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="8d51b-128">Zachowanie domyślne można zmienić tak, aby `StartAsync` usługi hostowanej działała po skonfigurowaniu potoku aplikacji i po`ApplicationStarted` wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="8d51b-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="8d51b-129">Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="8d51b-130">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="8d51b-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="8d51b-131">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="8d51b-132">Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="8d51b-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="8d51b-133">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="8d51b-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="8d51b-134">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="8d51b-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="8d51b-135">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="8d51b-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="8d51b-136">Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="8d51b-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="8d51b-137">Jednak zadania nie są porzucane po zażądaniu anulowania&mdash;obiekt wywołujący oczekuje na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="8d51b-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="8d51b-138">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8d51b-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="8d51b-139">W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="8d51b-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="8d51b-140">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="8d51b-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="8d51b-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="8d51b-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="8d51b-142">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="8d51b-143">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d51b-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="8d51b-144">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="8d51b-145">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8d51b-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="8d51b-146">Jeśli wystąpi błąd podczas wykonywania zadania w tle, `Dispose` powinien zostać wywołany, nawet jeśli `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8d51b-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice-base-class"></a><span data-ttu-id="8d51b-147">Klasa bazowa BackgroundService</span><span class="sxs-lookup"><span data-stu-id="8d51b-147">BackgroundService base class</span></span>

<span data-ttu-id="8d51b-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> jest klasą bazową do implementowania długotrwałych <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span>

<span data-ttu-id="8d51b-149">[Wywoływanie ExecuteAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) jest wywoływana w celu uruchomienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-149">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span></span> <span data-ttu-id="8d51b-150">Implementacja zwraca <xref:System.Threading.Tasks.Task>, która reprezentuje cały okres istnienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-150">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span></span> <span data-ttu-id="8d51b-151">Żadne dalsze usługi nie są uruchamiane do momentu, gdy [wywoływanie ExecuteAsync staną się asynchroniczne](https://github.com/aspnet/Extensions/issues/2149), na przykład przez wywołanie `await`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-151">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/aspnet/Extensions/issues/2149), such as by calling `await`.</span></span> <span data-ttu-id="8d51b-152">Unikaj długotrwałych i blokowanych operacji inicjowania w `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-152">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span></span> <span data-ttu-id="8d51b-153">Bloki hosta w [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) oczekują na ukończenie `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-153">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span></span>

<span data-ttu-id="8d51b-154">Token anulowania jest wyzwalany, gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-154">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="8d51b-155">Implementacja `ExecuteAsync` powinna zostać zakończona natychmiast po uruchomieniu tokenu anulowania w celu łagodnego zamknięcia usługi.</span><span class="sxs-lookup"><span data-stu-id="8d51b-155">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span></span> <span data-ttu-id="8d51b-156">W przeciwnym razie usługa niebezpiecznie zamyka się po upływie limitu czasu zamykania.</span><span class="sxs-lookup"><span data-stu-id="8d51b-156">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span></span> <span data-ttu-id="8d51b-157">Aby uzyskać więcej informacji, zobacz sekcję dotyczącą [interfejsu IHostedService](#ihostedservice-interface) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-157">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="8d51b-158">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="8d51b-158">Timed background tasks</span></span>

<span data-ttu-id="8d51b-159">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-159">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="8d51b-160">Czasomierz wyzwala metodę `DoWork` zadania.</span><span class="sxs-lookup"><span data-stu-id="8d51b-160">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="8d51b-161">Czasomierz jest wyłączony na `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-161">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="8d51b-162">Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z metodą rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-162">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="8d51b-163">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="8d51b-163">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="8d51b-164">Aby użyć [usług z zakresem](xref:fundamentals/dependency-injection#service-lifetimes) w ramach [BackgroundService](#backgroundservice-base-class), Utwórz zakres.</span><span class="sxs-lookup"><span data-stu-id="8d51b-164">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span></span> <span data-ttu-id="8d51b-165">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="8d51b-165">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="8d51b-166">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-166">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="8d51b-167">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="8d51b-167">In the following example:</span></span>

* <span data-ttu-id="8d51b-168">Usługa jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="8d51b-168">The service is asynchronous.</span></span> <span data-ttu-id="8d51b-169">Metoda `DoWork` zwraca `Task`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-169">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="8d51b-170">W celach demonstracyjnych oczekuje się, że w metodzie `DoWork` czeka dziesięć sekund.</span><span class="sxs-lookup"><span data-stu-id="8d51b-170">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="8d51b-171"><xref:Microsoft.Extensions.Logging.ILogger> jest wprowadzany do usługi.</span><span class="sxs-lookup"><span data-stu-id="8d51b-171">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="8d51b-172">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-172">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="8d51b-173">`DoWork` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-173">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="8d51b-174">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="8d51b-174">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="8d51b-175">Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-175">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="8d51b-176">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="8d51b-176">Queued background tasks</span></span>

<span data-ttu-id="8d51b-177">Kolejka zadań w tle jest oparta na <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> .NET 4. x ([wstępnie zaplanowana do wbudowania dla ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="8d51b-177">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="8d51b-178">W poniższym przykładzie `QueueHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-178">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="8d51b-179">Metoda `BackgroundProcessing` zwraca `Task`, który jest oczekiwany w `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-179">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="8d51b-180">Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-180">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="8d51b-181">Elementy robocze są oczekiwane przed zatrzymaniem usługi w `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-181">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="8d51b-182">Usługa `MonitorLoop` obsługuje zadania umieszczenie dla usługi hostowanej przy każdym wybraniu klucza `w` na urządzeniu wejściowym:</span><span class="sxs-lookup"><span data-stu-id="8d51b-182">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="8d51b-183">`IBackgroundTaskQueue` jest wstrzykiwana do usługi `MonitorLoop`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-183">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="8d51b-184">`IBackgroundTaskQueue.QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy.</span><span class="sxs-lookup"><span data-stu-id="8d51b-184">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="8d51b-185">Element roboczy symuluje długotrwałe zadanie w tle:</span><span class="sxs-lookup"><span data-stu-id="8d51b-185">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="8d51b-186">Trzy 5-sekundowe opóźnienia są wykonywane (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="8d51b-186">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="8d51b-187">`try-catch` pułapki instrukcji <xref:System.OperationCanceledException>, jeśli zadanie zostało anulowane.</span><span class="sxs-lookup"><span data-stu-id="8d51b-187">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="8d51b-188">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="8d51b-188">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="8d51b-189">Usługa hostowana jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-189">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8d51b-190">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="8d51b-190">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="8d51b-191">Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-191">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="8d51b-192">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="8d51b-192">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="8d51b-193">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="8d51b-193">Background task that runs on a timer.</span></span>
* <span data-ttu-id="8d51b-194">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="8d51b-194">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8d51b-195">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="8d51b-195">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="8d51b-196">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="8d51b-196">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="8d51b-197">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8d51b-197">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="8d51b-198">Package</span><span class="sxs-lookup"><span data-stu-id="8d51b-198">Package</span></span>

<span data-ttu-id="8d51b-199">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-199">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="8d51b-200">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="8d51b-200">IHostedService interface</span></span>

<span data-ttu-id="8d51b-201">Usługi hostowane implementują interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-201">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="8d51b-202">Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="8d51b-202">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="8d51b-203">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę do uruchomienia zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-203">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="8d51b-204">W przypadku korzystania z [hosta sieci Web](xref:fundamentals/host/web-host)`StartAsync` jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-204">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="8d51b-205">W przypadku korzystania z [hosta ogólnego](xref:fundamentals/host/generic-host)`StartAsync` jest wywoływana przed wyzwoleniem `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-205">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="8d51b-206">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="8d51b-206">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="8d51b-207">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-207">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="8d51b-208">Implementowanie <xref:System.IDisposable> i [finalizatorów (destruktorów)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="8d51b-208">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="8d51b-209">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="8d51b-209">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="8d51b-210">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="8d51b-210">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="8d51b-211">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="8d51b-211">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="8d51b-212">Wszelkie metody wywoływane w `StopAsync` powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="8d51b-212">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="8d51b-213">Jednak zadania nie są porzucane po zażądaniu anulowania&mdash;obiekt wywołujący oczekuje na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="8d51b-213">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="8d51b-214">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji zakończy się niepowodzeniem), `StopAsync` może nie być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8d51b-214">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="8d51b-215">W związku z tym wszystkie metody wywoływane lub operacje wykonywane w `StopAsync` mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="8d51b-215">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="8d51b-216">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="8d51b-216">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="8d51b-217"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> podczas korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="8d51b-217"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="8d51b-218">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-218">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="8d51b-219">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d51b-219">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="8d51b-220">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="8d51b-220">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="8d51b-221">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8d51b-221">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="8d51b-222">Jeśli wystąpi błąd podczas wykonywania zadania w tle, `Dispose` powinien zostać wywołany, nawet jeśli `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8d51b-222">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="8d51b-223">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="8d51b-223">Timed background tasks</span></span>

<span data-ttu-id="8d51b-224">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="8d51b-224">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="8d51b-225">Czasomierz wyzwala metodę `DoWork` zadania.</span><span class="sxs-lookup"><span data-stu-id="8d51b-225">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="8d51b-226">Czasomierz jest wyłączony na `StopAsync` i zlikwidowany, gdy kontener usługi zostanie usunięty z `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-226">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="8d51b-227">Usługa jest zarejestrowana w `Startup.ConfigureServices` z metodą rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-227">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="8d51b-228">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="8d51b-228">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="8d51b-229">Aby korzystać z usług o określonym [zakresie](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `IHostedService`, Utwórz zakres.</span><span class="sxs-lookup"><span data-stu-id="8d51b-229">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="8d51b-230">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="8d51b-230">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="8d51b-231">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="8d51b-231">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="8d51b-232">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:</span><span class="sxs-lookup"><span data-stu-id="8d51b-232">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="8d51b-233">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać metodę `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-233">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="8d51b-234">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-234">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8d51b-235">Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-235">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="8d51b-236">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="8d51b-236">Queued background tasks</span></span>

<span data-ttu-id="8d51b-237">Kolejka zadań w tle jest oparta na .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowano wbudowaną obsługę ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="8d51b-237">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="8d51b-238">W `QueueHostedService`zadania w tle w kolejce są odrzucane i wykonywane jako [BackgroundService](#backgroundservice-base-class), która jest klasą bazową do implementowania długotrwałych `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-238">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](#backgroundservice-base-class), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="8d51b-239">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-239">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8d51b-240">Implementacja `IHostedService` jest zarejestrowana przy użyciu metody rozszerzenia `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="8d51b-240">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="8d51b-241">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="8d51b-241">In the Index page model class:</span></span>

* <span data-ttu-id="8d51b-242">`IBackgroundTaskQueue` jest wprowadzany do konstruktora i przypisywany do `Queue`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-242">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="8d51b-243"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> zostanie dodany i przypisany do `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-243">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="8d51b-244">Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie.</span><span class="sxs-lookup"><span data-stu-id="8d51b-244">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="8d51b-245">Zakres jest tworzony w celu używania `AppDbContext` aplikacji ( [usługi w zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych w `IBackgroundTaskQueue` (pojedynczej usłudze).</span><span class="sxs-lookup"><span data-stu-id="8d51b-245">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="8d51b-246">Po wybraniu przycisku **Dodaj zadanie** na stronie indeks jest wykonywana metoda `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="8d51b-246">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="8d51b-247">`QueueBackgroundWorkItem` jest wywoływana w celu dodawania do kolejki elementu pracy:</span><span class="sxs-lookup"><span data-stu-id="8d51b-247">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8d51b-248">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8d51b-248">Additional resources</span></span>

* [<span data-ttu-id="8d51b-249">Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService</span><span class="sxs-lookup"><span data-stu-id="8d51b-249">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
