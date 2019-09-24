---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/18/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8df86b10d7ba853edb3265df0e02eabbf8a2c058
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205711"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="c6d2b-103">Zadania w tle z usługami hostowanymi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6d2b-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="c6d2b-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c6d2b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c6d2b-105">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="c6d2b-106">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c6d2b-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="c6d2b-108">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="c6d2b-109">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c6d2b-110">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c6d2b-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="c6d2b-111">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="c6d2b-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c6d2b-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c6d2b-113">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="c6d2b-114">Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="c6d2b-115">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="c6d2b-116">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="c6d2b-117">Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="c6d2b-118">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="c6d2b-119">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="c6d2b-119">Worker Service template</span></span>

<span data-ttu-id="c6d2b-120">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="c6d2b-121">Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6d2b-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6d2b-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c6d2b-123">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-123">Create a new project.</span></span>
1. <span data-ttu-id="c6d2b-124">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="c6d2b-125">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-125">Select **Next**.</span></span>
1. <span data-ttu-id="c6d2b-126">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="c6d2b-127">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-127">Select **Create**.</span></span>
1. <span data-ttu-id="c6d2b-128">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="c6d2b-129">Wybierz szablon **usługi procesu roboczego** .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="c6d2b-130">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c6d2b-131">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="c6d2b-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="c6d2b-132">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-132">Create a new project.</span></span>
1. <span data-ttu-id="c6d2b-133">Na pasku bocznym wybierz pozycję **aplikacja** na **platformie .NET Core** .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="c6d2b-134">Wybierz pozycję **proces roboczy** w obszarze **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="c6d2b-135">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-135">Select **Next**.</span></span>
1. <span data-ttu-id="c6d2b-136">Wybierz pozycję **.NET Core 3,0** dla **platformy docelowej**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="c6d2b-137">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-137">Select **Next**.</span></span>
1. <span data-ttu-id="c6d2b-138">Podaj nazwę w polu **Nazwa projektu** .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="c6d2b-139">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c6d2b-140">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c6d2b-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c6d2b-141">Użyj szablonu usługi procesu roboczego`worker`() z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="c6d2b-142">W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="c6d2b-143">Folder dla `ContosoWorker` aplikacji jest tworzony automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="c6d2b-144">Package</span><span class="sxs-lookup"><span data-stu-id="c6d2b-144">Package</span></span>

<span data-ttu-id="c6d2b-145">Odwołanie do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) jest dodawane niejawnie dla aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="c6d2b-146">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="c6d2b-146">IHostedService interface</span></span>

<span data-ttu-id="c6d2b-147"><xref:Microsoft.Extensions.Hosting.IHostedService> Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="c6d2b-148">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="c6d2b-149">`StartAsync`jest wywoływana *przed*:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="c6d2b-150">Skonfigurowano potok przetwarzania żądania aplikacji (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="c6d2b-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="c6d2b-151">Serwer został uruchomiony i zostanie wyzwolony [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="c6d2b-152">Zachowanie domyślne można zmienić tak, aby usługa `StartAsync` hostowana działała po skonfigurowaniu potoku aplikacji i `ApplicationStarted` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="c6d2b-153">Aby zmienić zachowanie domyślne, należy dodać usługę hostowaną (`VideosWatcher` w poniższym przykładzie) po wywołaniu: `ConfigureWebHostDefaults`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="c6d2b-154">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="c6d2b-155">`StopAsync`zawiera logikę końcową zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="c6d2b-156">Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="c6d2b-157">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="c6d2b-158">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="c6d2b-159">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="c6d2b-160">Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="c6d2b-161">Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="c6d2b-162">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="c6d2b-163">W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="c6d2b-164">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="c6d2b-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="c6d2b-166">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="c6d2b-167">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="c6d2b-168">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="c6d2b-169">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="c6d2b-170">Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="c6d2b-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="c6d2b-171">BackgroundService</span></span>

<span data-ttu-id="c6d2b-172">`BackgroundService`jest klasą bazową do implementowania długotrwałego działania <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="c6d2b-173">`BackgroundService`definiuje dwie metody dla operacji w tle:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-173">`BackgroundService` defines two methods for background operations:</span></span>

* <span data-ttu-id="c6d2b-174">`ExecuteAsync(CancellationToken stoppingToken)`Wywoływana, gdyzostanie<xref:Microsoft.Extensions.Hosting.IHostedService>uruchomiony. &ndash; `ExecuteAsync`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-174">`ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync` Called when the <xref:Microsoft.Extensions.Hosting.IHostedService> starts.</span></span> <span data-ttu-id="c6d2b-175">Implementacja powinna zwrócić `Task` , która reprezentuje okres istnienia długotrwałych operacji wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-175">The implementation should return a `Task` that represents the lifetime of the long running operations performed.</span></span> <span data-ttu-id="c6d2b-176">Wyzwalane, `stoppingToken` gdy wywoływana jest [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-176">The `stoppingToken` Triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span>
* <span data-ttu-id="c6d2b-177">`StopAsync(CancellationToken stoppingToken)`&ndash; jest wyzwalany,gdyhostaplikacji`StopAsync` wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` is triggered when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="c6d2b-178">`stoppingToken` Wskazuje, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-178">The `stoppingToken` indicates that the shutdown process should no longer be graceful.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="c6d2b-179">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="c6d2b-179">Timed background tasks</span></span>

<span data-ttu-id="c6d2b-180">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-180">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="c6d2b-181">Czasomierz wyzwala `DoWork` metodę zadania.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-181">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="c6d2b-182">Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-182">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="c6d2b-183">Usługa jest zarejestrowana w `IHostBuilder.ConfigureServices` (*program.cs*) z `AddHostedService` metodą rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-183">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="c6d2b-184">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="c6d2b-184">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="c6d2b-185">Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `BackgroundService`w ramach programu, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-185">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="c6d2b-186">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-186">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="c6d2b-187">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-187">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="c6d2b-188">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-188">In the following example:</span></span>

* <span data-ttu-id="c6d2b-189">Usługa jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-189">The service is asynchronous.</span></span> <span data-ttu-id="c6d2b-190">`DoWork` Metoda`Task`zwraca.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-190">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="c6d2b-191">W celach demonstracyjnych oczekuje się, że w `DoWork` metodzie występuje opóźnienie o dziesięć sekund.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-191">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="c6d2b-192"><xref:Microsoft.Extensions.Logging.ILogger> Jest wstrzykiwana do usługi.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-192">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="c6d2b-193">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie, aby wywołać `DoWork` metodę.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-193">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="c6d2b-194">`DoWork`Zwraca element `Task`, który jest oczekiwany w `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-194">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="c6d2b-195">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` usłudze (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="c6d2b-195">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="c6d2b-196">Usługa hostowana jest zarejestrowana przy `AddHostedService` użyciu metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-196">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="c6d2b-197">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="c6d2b-197">Queued background tasks</span></span>

<span data-ttu-id="c6d2b-198">Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="c6d2b-198">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="c6d2b-199">W poniższym `QueueHostedService` przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-199">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="c6d2b-200">Metoda zwraca obiekt `Task`, który jest oczekiwany w `ExecuteAsync`. `BackgroundProcessing`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-200">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="c6d2b-201">Zadania w tle w kolejce są dekolejkowane i wykonywane w `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-201">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="c6d2b-202">Usługa obsługuje zadania umieszczenie dla usługi hostowanej za każdym razem `w` , gdy klucz zostanie wybrany na urządzeniu wejściowym: `MonitorLoop`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-202">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="c6d2b-203">Jest wstrzykiwana do `MonitorLoop`usługi. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-203">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="c6d2b-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="c6d2b-205">Usługi są zarejestrowane w `IHostBuilder.ConfigureServices` usłudze (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="c6d2b-205">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="c6d2b-206">Usługa hostowana jest zarejestrowana przy `AddHostedService` użyciu metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-206">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c6d2b-207">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-207">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="c6d2b-208">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-208">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c6d2b-209">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-209">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="c6d2b-210">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-210">Background task that runs on a timer.</span></span>
* <span data-ttu-id="c6d2b-211">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-211">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c6d2b-212">Usługa objęta zakresem może używać [iniekcji zależności (di)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="c6d2b-212">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="c6d2b-213">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-213">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="c6d2b-214">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c6d2b-214">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c6d2b-215">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-215">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="c6d2b-216">Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-216">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="c6d2b-217">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-217">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="c6d2b-218">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-218">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="c6d2b-219">Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-219">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="c6d2b-220">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-220">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="c6d2b-221">Package</span><span class="sxs-lookup"><span data-stu-id="c6d2b-221">Package</span></span>

<span data-ttu-id="c6d2b-222">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-222">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="c6d2b-223">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="c6d2b-223">IHostedService interface</span></span>

<span data-ttu-id="c6d2b-224">Usługi hostowane implementują <xref:Microsoft.Extensions.Hosting.IHostedService> interfejs.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-224">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c6d2b-225">Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-225">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="c6d2b-226">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="c6d2b-227">W przypadku korzystania z `StartAsync` [hosta sieci Web](xref:fundamentals/host/web-host)jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-227">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="c6d2b-228">W przypadku korzystania z `StartAsync` [hosta generycznego](xref:fundamentals/host/generic-host)jest wywoływana `ApplicationStarted` przed wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-228">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="c6d2b-229">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="c6d2b-230">`StopAsync`zawiera logikę końcową zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-230">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="c6d2b-231">Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-231">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="c6d2b-232">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-232">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="c6d2b-233">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-233">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="c6d2b-234">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-234">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="c6d2b-235">Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-235">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="c6d2b-236">Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-236">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="c6d2b-237">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-237">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="c6d2b-238">W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-238">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="c6d2b-239">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-239">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="c6d2b-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="c6d2b-241">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-241">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="c6d2b-242">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-242">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="c6d2b-243">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-243">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="c6d2b-244">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-244">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="c6d2b-245">Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-245">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="c6d2b-246">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="c6d2b-246">Timed background tasks</span></span>

<span data-ttu-id="c6d2b-247">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="c6d2b-247">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="c6d2b-248">Czasomierz wyzwala `DoWork` metodę zadania.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-248">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="c6d2b-249">Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-249">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="c6d2b-250">Usługa jest zarejestrowana w `Startup.ConfigureServices` `AddHostedService` ramach metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-250">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="c6d2b-251">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="c6d2b-251">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="c6d2b-252">Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `IHostedService`w ramach programu, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-252">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="c6d2b-253">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-253">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="c6d2b-254">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-254">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="c6d2b-255">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-255">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="c6d2b-256">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie w celu wywołania `DoWork` jej metody:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-256">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="c6d2b-257">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-257">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6d2b-258">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-258">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="c6d2b-259">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="c6d2b-259">Queued background tasks</span></span>

<span data-ttu-id="c6d2b-260">Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowano wbudowaną ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="c6d2b-260">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="c6d2b-261">W `QueueHostedService`programie zadania w tle w kolejce są odrzucane i wykonywane <xref:Microsoft.Extensions.Hosting.BackgroundService>jako, która jest klasą bazową do implementowania długotrwałego `IHostedService`działania:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-261">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="c6d2b-262">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-262">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6d2b-263">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-263">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="c6d2b-264">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-264">In the Index page model class:</span></span>

* <span data-ttu-id="c6d2b-265">Zostaje wstrzyknięty do konstruktora i przypisany do `Queue`. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="c6d2b-265">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="c6d2b-266">Jest wstrzykiwany i przypisany do `_serviceScopeFactory`. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory></span><span class="sxs-lookup"><span data-stu-id="c6d2b-266">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="c6d2b-267">Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-267">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="c6d2b-268">Zakres jest tworzony w celu używania aplikacji `AppDbContext` (usługi w [zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów `IBackgroundTaskQueue` bazy danych w (pojedynczej usłudze).</span><span class="sxs-lookup"><span data-stu-id="c6d2b-268">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="c6d2b-269">Po wybraniu przycisku **Dodaj zadanie** na stronie `OnPostAddTask` indeks jest wykonywana metoda.</span><span class="sxs-lookup"><span data-stu-id="c6d2b-269">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="c6d2b-270">`QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy:</span><span class="sxs-lookup"><span data-stu-id="c6d2b-270">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c6d2b-271">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c6d2b-271">Additional resources</span></span>

* [<span data-ttu-id="c6d2b-272">Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService</span><span class="sxs-lookup"><span data-stu-id="c6d2b-272">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
