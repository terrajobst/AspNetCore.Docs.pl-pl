---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 5a29952c4e50edb953fa03c6ea1a1ae27b728bb0
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317720"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="5a87e-103">Zadania w tle z usługami hostowanymi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a87e-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="5a87e-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5a87e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5a87e-105">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="5a87e-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="5a87e-106">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="5a87e-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="5a87e-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="5a87e-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="5a87e-108">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="5a87e-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="5a87e-109">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="5a87e-110">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5a87e-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="5a87e-111">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="5a87e-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a87e-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5a87e-113">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="5a87e-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="5a87e-114">Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5a87e-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="5a87e-115">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a87e-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="5a87e-116">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="5a87e-117">Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="5a87e-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="5a87e-118">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="5a87e-119">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="5a87e-119">Worker Service template</span></span>

<span data-ttu-id="5a87e-120">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="5a87e-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="5a87e-121">Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:</span><span class="sxs-lookup"><span data-stu-id="5a87e-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5a87e-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5a87e-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="5a87e-123">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="5a87e-123">Create a new project.</span></span>
1. <span data-ttu-id="5a87e-124">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5a87e-125">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-125">Select **Next**.</span></span>
1. <span data-ttu-id="5a87e-126">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="5a87e-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="5a87e-127">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-127">Select **Create**.</span></span>
1. <span data-ttu-id="5a87e-128">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="5a87e-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="5a87e-129">Wybierz szablon **usługi procesu roboczego** .</span><span class="sxs-lookup"><span data-stu-id="5a87e-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="5a87e-130">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5a87e-131">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5a87e-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="5a87e-132">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="5a87e-132">Create a new project.</span></span>
1. <span data-ttu-id="5a87e-133">Na pasku bocznym wybierz pozycję **aplikacja** na **platformie .NET Core** .</span><span class="sxs-lookup"><span data-stu-id="5a87e-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="5a87e-134">Wybierz pozycję **proces roboczy** w obszarze **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="5a87e-135">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-135">Select **Next**.</span></span>
1. <span data-ttu-id="5a87e-136">Wybierz pozycję **.NET Core 3,0** dla **platformy docelowej**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="5a87e-137">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-137">Select **Next**.</span></span>
1. <span data-ttu-id="5a87e-138">Podaj nazwę w polu **Nazwa projektu** .</span><span class="sxs-lookup"><span data-stu-id="5a87e-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="5a87e-139">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5a87e-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="5a87e-140">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="5a87e-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="5a87e-141">Użyj szablonu usługi procesu roboczego`worker`() z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="5a87e-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="5a87e-142">W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="5a87e-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="5a87e-143">Folder dla `ContosoWorker` aplikacji jest tworzony automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="5a87e-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="5a87e-144">Pakiet</span><span class="sxs-lookup"><span data-stu-id="5a87e-144">Package</span></span>

<span data-ttu-id="5a87e-145">Odwołanie do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest dodawane niejawnie dla aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a87e-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="5a87e-146">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="5a87e-146">IHostedService interface</span></span>

<span data-ttu-id="5a87e-147"><xref:Microsoft.Extensions.Hosting.IHostedService> Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="5a87e-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="5a87e-148">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="5a87e-149">`StartAsync`jest wywoływana *przed*:</span><span class="sxs-lookup"><span data-stu-id="5a87e-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="5a87e-150">Skonfigurowano potok przetwarzania żądania aplikacji (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="5a87e-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="5a87e-151">Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="5a87e-152">Zachowanie domyślne można zmienić tak, aby usługa `StartAsync` hostowana działała po skonfigurowaniu potoku aplikacji i `ApplicationStarted` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="5a87e-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="5a87e-153">Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu: `ConfigureWebHostDefaults`</span><span class="sxs-lookup"><span data-stu-id="5a87e-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="5a87e-154">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="5a87e-155">`StopAsync`zawiera logikę końcową zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="5a87e-156">Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="5a87e-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="5a87e-157">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="5a87e-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="5a87e-158">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="5a87e-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="5a87e-159">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="5a87e-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="5a87e-160">Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="5a87e-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="5a87e-161">Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="5a87e-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="5a87e-162">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany.</span><span class="sxs-lookup"><span data-stu-id="5a87e-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="5a87e-163">W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="5a87e-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="5a87e-164">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="5a87e-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="5a87e-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="5a87e-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="5a87e-166">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5a87e-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="5a87e-167">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5a87e-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="5a87e-168">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5a87e-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="5a87e-169">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a87e-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="5a87e-170">Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="5a87e-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="5a87e-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="5a87e-171">BackgroundService</span></span>

<span data-ttu-id="5a87e-172">`BackgroundService`jest klasą bazową do implementowania długotrwałego działania <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="5a87e-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="5a87e-173">`BackgroundService`udostępnia metodę `ExecuteAsync(CancellationToken stoppingToken)` abstrakcyjną, która będzie zawierać logikę usługi.</span><span class="sxs-lookup"><span data-stu-id="5a87e-173">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="5a87e-174">Jest `stoppingToken` wyzwalane, gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-174">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="5a87e-175">Implementacja tej metody zwraca `Task` wartość reprezentującą cały okres istnienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-175">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="5a87e-176">Ponadto *Opcjonalnie* Przesłoń metody zdefiniowane na `IHostedService` potrzeby uruchamiania kodu uruchamiania i zamykania dla usługi:</span><span class="sxs-lookup"><span data-stu-id="5a87e-176">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="5a87e-177">`StopAsync(CancellationToken cancellationToken)`&ndash; jestwywoływana,gdyhost`StopAsync` aplikacji wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-177">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="5a87e-178">Jest `cancellationToken` on sygnalizowane, gdy host zdecyduje się wymusić zakończenie usługi.</span><span class="sxs-lookup"><span data-stu-id="5a87e-178">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="5a87e-179">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy `await`bazowej (i), aby upewnić się, że usługa jest prawidłowo zamykana.</span><span class="sxs-lookup"><span data-stu-id="5a87e-179">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="5a87e-180">`StartAsync(CancellationToken cancellationToken)`&ndash; jestwywoływanawcelu`StartAsync` uruchomienia usługi w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-180">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="5a87e-181">Jest `cancellationToken` on sygnalizowane, jeśli proces uruchamiania zostanie przerwany.</span><span class="sxs-lookup"><span data-stu-id="5a87e-181">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="5a87e-182">Implementacja zwraca `Task` , która reprezentuje proces uruchamiania usługi.</span><span class="sxs-lookup"><span data-stu-id="5a87e-182">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="5a87e-183">Żadne dalsze usługi nie są uruchamiane do `Task` momentu zakończenia tego procesu.</span><span class="sxs-lookup"><span data-stu-id="5a87e-183">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="5a87e-184">Jeśli ta metoda zostanie zastąpiona, **należy** wywołać metodę klasy `await`bazowej (i), aby upewnić się, że usługa jest uruchamiana prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="5a87e-184">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="5a87e-185">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="5a87e-185">Timed background tasks</span></span>

<span data-ttu-id="5a87e-186">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-186">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="5a87e-187">Czasomierz wyzwala `DoWork` metodę zadania.</span><span class="sxs-lookup"><span data-stu-id="5a87e-187">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="5a87e-188">Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:</span><span class="sxs-lookup"><span data-stu-id="5a87e-188">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="5a87e-189">Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z `AddHostedService` metodą rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="5a87e-189">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="5a87e-190">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="5a87e-190">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="5a87e-191">Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `BackgroundService`w ramach programu, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="5a87e-191">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="5a87e-192">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="5a87e-192">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="5a87e-193">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-193">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="5a87e-194">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="5a87e-194">In the following example:</span></span>

* <span data-ttu-id="5a87e-195">Usługa jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="5a87e-195">The service is asynchronous.</span></span> <span data-ttu-id="5a87e-196">`DoWork` Metoda`Task`zwraca.</span><span class="sxs-lookup"><span data-stu-id="5a87e-196">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="5a87e-197">W celach demonstracyjnych oczekuje się, że w `DoWork` metodzie występuje opóźnienie o dziesięć sekund.</span><span class="sxs-lookup"><span data-stu-id="5a87e-197">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="5a87e-198"><xref:Microsoft.Extensions.Logging.ILogger> Jest wstrzykiwana do usługi.</span><span class="sxs-lookup"><span data-stu-id="5a87e-198">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="5a87e-199">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać `DoWork` metodę.</span><span class="sxs-lookup"><span data-stu-id="5a87e-199">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="5a87e-200">`DoWork`Zwraca element `Task`, który jest oczekiwany w `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="5a87e-200">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="5a87e-201">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` usłudze (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="5a87e-201">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="5a87e-202">Usługa hostowana jest zarejestrowana przy `AddHostedService` użyciu metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="5a87e-202">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="5a87e-203">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="5a87e-203">Queued background tasks</span></span>

<span data-ttu-id="5a87e-204">Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="5a87e-204">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="5a87e-205">W poniższym `QueueHostedService` przykładzie:</span><span class="sxs-lookup"><span data-stu-id="5a87e-205">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="5a87e-206">Metoda zwraca obiekt `Task`, który jest oczekiwany w `ExecuteAsync`. `BackgroundProcessing`</span><span class="sxs-lookup"><span data-stu-id="5a87e-206">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="5a87e-207">Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="5a87e-207">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="5a87e-208">Usługa obsługuje zadania umieszczenie dla usługi hostowanej za każdym razem `w` , gdy klucz zostanie wybrany na urządzeniu wejściowym: `MonitorLoop`</span><span class="sxs-lookup"><span data-stu-id="5a87e-208">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="5a87e-209">Jest wstrzykiwana do `MonitorLoop`usługi. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="5a87e-209">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="5a87e-210">`IBackgroundTaskQueue.QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy.</span><span class="sxs-lookup"><span data-stu-id="5a87e-210">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="5a87e-211">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` usłudze (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="5a87e-211">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="5a87e-212">Usługa hostowana jest zarejestrowana przy `AddHostedService` użyciu metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="5a87e-212">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5a87e-213">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="5a87e-213">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="5a87e-214">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="5a87e-214">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="5a87e-215">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="5a87e-215">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="5a87e-216">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="5a87e-216">Background task that runs on a timer.</span></span>
* <span data-ttu-id="5a87e-217">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-217">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="5a87e-218">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="5a87e-218">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="5a87e-219">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-219">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="5a87e-220">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a87e-220">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5a87e-221">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="5a87e-221">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="5a87e-222">Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5a87e-222">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="5a87e-223">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="5a87e-223">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="5a87e-224">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-224">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="5a87e-225">Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="5a87e-225">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="5a87e-226">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-226">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="5a87e-227">Pakiet</span><span class="sxs-lookup"><span data-stu-id="5a87e-227">Package</span></span>

<span data-ttu-id="5a87e-228">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-228">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="5a87e-229">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="5a87e-229">IHostedService interface</span></span>

<span data-ttu-id="5a87e-230">Usługi hostowane implementują <xref:Microsoft.Extensions.Hosting.IHostedService> interfejs.</span><span class="sxs-lookup"><span data-stu-id="5a87e-230">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="5a87e-231">Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="5a87e-231">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="5a87e-232">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-232">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="5a87e-233">W przypadku korzystania z `StartAsync` [hosta sieci Web](xref:fundamentals/host/web-host)jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-233">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="5a87e-234">W przypadku korzystania z `StartAsync` [hosta generycznego](xref:fundamentals/host/generic-host)jest wywoływana `ApplicationStarted` przed wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="5a87e-234">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="5a87e-235">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-235">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="5a87e-236">`StopAsync`zawiera logikę końcową zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-236">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="5a87e-237">Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="5a87e-237">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="5a87e-238">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="5a87e-238">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="5a87e-239">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="5a87e-239">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="5a87e-240">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="5a87e-240">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="5a87e-241">Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="5a87e-241">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="5a87e-242">Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="5a87e-242">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="5a87e-243">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany.</span><span class="sxs-lookup"><span data-stu-id="5a87e-243">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="5a87e-244">W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="5a87e-244">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="5a87e-245">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="5a87e-245">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="5a87e-246"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="5a87e-246"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="5a87e-247">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5a87e-247">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="5a87e-248">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5a87e-248">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="5a87e-249">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5a87e-249">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="5a87e-250">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a87e-250">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="5a87e-251">Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="5a87e-251">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="5a87e-252">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="5a87e-252">Timed background tasks</span></span>

<span data-ttu-id="5a87e-253">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="5a87e-253">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="5a87e-254">Czasomierz wyzwala `DoWork` metodę zadania.</span><span class="sxs-lookup"><span data-stu-id="5a87e-254">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="5a87e-255">Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:</span><span class="sxs-lookup"><span data-stu-id="5a87e-255">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="5a87e-256">Usługa jest zarejestrowana w `Startup.ConfigureServices` `AddHostedService` ramach metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="5a87e-256">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="5a87e-257">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="5a87e-257">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="5a87e-258">Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `IHostedService`w ramach programu, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="5a87e-258">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="5a87e-259">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="5a87e-259">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="5a87e-260">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="5a87e-260">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="5a87e-261">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:</span><span class="sxs-lookup"><span data-stu-id="5a87e-261">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="5a87e-262">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie w celu wywołania `DoWork` jej metody:</span><span class="sxs-lookup"><span data-stu-id="5a87e-262">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="5a87e-263">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="5a87e-263">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5a87e-264">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="5a87e-264">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="5a87e-265">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="5a87e-265">Queued background tasks</span></span>

<span data-ttu-id="5a87e-266">Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="5a87e-266">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="5a87e-267">W `QueueHostedService`programie zadania w tle w kolejce są odrzucane i wykonywane <xref:Microsoft.Extensions.Hosting.BackgroundService>jako, która jest klasą bazową do implementowania długotrwałego `IHostedService`działania:</span><span class="sxs-lookup"><span data-stu-id="5a87e-267">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="5a87e-268">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="5a87e-268">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5a87e-269">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="5a87e-269">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="5a87e-270">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="5a87e-270">In the Index page model class:</span></span>

* <span data-ttu-id="5a87e-271">Zostaje wstrzyknięty do konstruktora i przypisany do `Queue`. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="5a87e-271">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="5a87e-272">Jest wstrzykiwany i przypisany do `_serviceScopeFactory`. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory></span><span class="sxs-lookup"><span data-stu-id="5a87e-272">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="5a87e-273">Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie.</span><span class="sxs-lookup"><span data-stu-id="5a87e-273">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="5a87e-274">Zakres jest tworzony w celu używania aplikacji `AppDbContext` (usługi w [zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów `IBackgroundTaskQueue` bazy danych w (pojedynczej usłudze).</span><span class="sxs-lookup"><span data-stu-id="5a87e-274">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="5a87e-275">Po wybraniu przycisku **Dodaj zadanie** na stronie `OnPostAddTask` indeks jest wykonywana metoda.</span><span class="sxs-lookup"><span data-stu-id="5a87e-275">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="5a87e-276">`QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy:</span><span class="sxs-lookup"><span data-stu-id="5a87e-276">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5a87e-277">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5a87e-277">Additional resources</span></span>

* [<span data-ttu-id="5a87e-278">Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService</span><span class="sxs-lookup"><span data-stu-id="5a87e-278">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
