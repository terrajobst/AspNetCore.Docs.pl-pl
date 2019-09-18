---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 1db3ee1a9bcc0d41edf24df55bcd8d54fb0e9724
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081781"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="58f1e-103">Zadania w tle z usługami hostowanymi w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58f1e-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="58f1e-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="58f1e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="58f1e-105">W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="58f1e-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="58f1e-106">Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs.</span><span class="sxs-lookup"><span data-stu-id="58f1e-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="58f1e-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="58f1e-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="58f1e-108">Zadanie w tle, które jest uruchamiane na czasomierzu.</span><span class="sxs-lookup"><span data-stu-id="58f1e-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="58f1e-109">Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="58f1e-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="58f1e-110">Usługa objęta zakresem może używać iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="58f1e-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="58f1e-111">Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="58f1e-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="58f1e-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="58f1e-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="58f1e-113">Przykładowa aplikacja jest dostępna w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="58f1e-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="58f1e-114">Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="58f1e-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="58f1e-115">Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej.</span><span class="sxs-lookup"><span data-stu-id="58f1e-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="58f1e-116">Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .</span><span class="sxs-lookup"><span data-stu-id="58f1e-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="58f1e-117">Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="58f1e-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="58f1e-118">Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="58f1e-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="58f1e-119">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="58f1e-119">Worker Service template</span></span>

<span data-ttu-id="58f1e-120">Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi.</span><span class="sxs-lookup"><span data-stu-id="58f1e-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="58f1e-121">Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:</span><span class="sxs-lookup"><span data-stu-id="58f1e-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="58f1e-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58f1e-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="58f1e-123">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="58f1e-123">Create a new project.</span></span>
1. <span data-ttu-id="58f1e-124">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="58f1e-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="58f1e-125">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="58f1e-125">Select **Next**.</span></span>
1. <span data-ttu-id="58f1e-126">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="58f1e-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="58f1e-127">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="58f1e-127">Select **Create**.</span></span>
1. <span data-ttu-id="58f1e-128">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .</span><span class="sxs-lookup"><span data-stu-id="58f1e-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="58f1e-129">Wybierz szablon **usługi procesu roboczego** .</span><span class="sxs-lookup"><span data-stu-id="58f1e-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="58f1e-130">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="58f1e-130">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="58f1e-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="58f1e-131">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="58f1e-132">Użyj szablonu usługi procesu roboczego`worker`() z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="58f1e-132">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="58f1e-133">W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="58f1e-133">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="58f1e-134">Folder dla `ContosoWorkerService` aplikacji jest tworzony automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="58f1e-134">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a><span data-ttu-id="58f1e-135">Package</span><span class="sxs-lookup"><span data-stu-id="58f1e-135">Package</span></span>

<span data-ttu-id="58f1e-136">Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="58f1e-136">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="58f1e-137">IHostedService, interfejs</span><span class="sxs-lookup"><span data-stu-id="58f1e-137">IHostedService interface</span></span>

<span data-ttu-id="58f1e-138">Usługi hostowane implementują <xref:Microsoft.Extensions.Hosting.IHostedService> interfejs.</span><span class="sxs-lookup"><span data-stu-id="58f1e-138">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="58f1e-139">Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:</span><span class="sxs-lookup"><span data-stu-id="58f1e-139">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="58f1e-140">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="58f1e-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="58f1e-141">W przypadku korzystania z `StartAsync` [hosta sieci Web](xref:fundamentals/host/web-host)jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="58f1e-141">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="58f1e-142">W przypadku korzystania z `StartAsync` [hosta generycznego](xref:fundamentals/host/generic-host)jest wywoływana `ApplicationStarted` przed wywołaniem.</span><span class="sxs-lookup"><span data-stu-id="58f1e-142">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="58f1e-143">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie.</span><span class="sxs-lookup"><span data-stu-id="58f1e-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="58f1e-144">`StopAsync`zawiera logikę końcową zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="58f1e-144">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="58f1e-145">Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="58f1e-145">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="58f1e-146">Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny.</span><span class="sxs-lookup"><span data-stu-id="58f1e-146">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="58f1e-147">Gdy żądanie anulowania jest wymagane na tokenie:</span><span class="sxs-lookup"><span data-stu-id="58f1e-147">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="58f1e-148">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.</span><span class="sxs-lookup"><span data-stu-id="58f1e-148">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="58f1e-149">Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.</span><span class="sxs-lookup"><span data-stu-id="58f1e-149">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="58f1e-150">Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="58f1e-150">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="58f1e-151">Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany.</span><span class="sxs-lookup"><span data-stu-id="58f1e-151">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="58f1e-152">W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.</span><span class="sxs-lookup"><span data-stu-id="58f1e-152">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="58f1e-153">Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:</span><span class="sxs-lookup"><span data-stu-id="58f1e-153">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="58f1e-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego.</span><span class="sxs-lookup"><span data-stu-id="58f1e-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="58f1e-155">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="58f1e-155">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="58f1e-156">Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="58f1e-156">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="58f1e-157">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="58f1e-157">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="58f1e-158">Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="58f1e-158">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="58f1e-159">Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="58f1e-159">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="58f1e-160">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="58f1e-160">Timed background tasks</span></span>

<span data-ttu-id="58f1e-161">Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) .</span><span class="sxs-lookup"><span data-stu-id="58f1e-161">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="58f1e-162">Czasomierz wyzwala `DoWork` metodę zadania.</span><span class="sxs-lookup"><span data-stu-id="58f1e-162">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="58f1e-163">Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:</span><span class="sxs-lookup"><span data-stu-id="58f1e-163">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="58f1e-164">Usługa jest zarejestrowana w `Startup.ConfigureServices` `AddHostedService` ramach metody rozszerzającej:</span><span class="sxs-lookup"><span data-stu-id="58f1e-164">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="58f1e-165">Zużywanie usługi w zakresie w zadaniu w tle</span><span class="sxs-lookup"><span data-stu-id="58f1e-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="58f1e-166">Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `IHostedService`w ramach programu, należy utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="58f1e-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="58f1e-167">Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="58f1e-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="58f1e-168">Usługa zadań w tle w zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="58f1e-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="58f1e-169">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:</span><span class="sxs-lookup"><span data-stu-id="58f1e-169">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="58f1e-170">Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie w celu wywołania `DoWork` jej metody:</span><span class="sxs-lookup"><span data-stu-id="58f1e-170">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="58f1e-171">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="58f1e-171">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="58f1e-172">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="58f1e-172">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="58f1e-173">Zadania w kolejce w dół</span><span class="sxs-lookup"><span data-stu-id="58f1e-173">Queued background tasks</span></span>

<span data-ttu-id="58f1e-174">Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowana do wbudowania dla ASP.NET Core 3,0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="58f1e-174">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="58f1e-175">W `QueueHostedService`programie zadania w tle w kolejce są odrzucane i wykonywane <xref:Microsoft.Extensions.Hosting.BackgroundService>jako, która jest klasą bazową do implementowania długotrwałego `IHostedService`działania:</span><span class="sxs-lookup"><span data-stu-id="58f1e-175">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="58f1e-176">Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze.</span><span class="sxs-lookup"><span data-stu-id="58f1e-176">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="58f1e-177">Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="58f1e-177">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="58f1e-178">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="58f1e-178">In the Index page model class:</span></span>

* <span data-ttu-id="58f1e-179">Zostaje wstrzyknięty do konstruktora i przypisany do `Queue`. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="58f1e-179">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="58f1e-180">Jest wstrzykiwany i przypisany do `_serviceScopeFactory`. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory></span><span class="sxs-lookup"><span data-stu-id="58f1e-180">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="58f1e-181">Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie.</span><span class="sxs-lookup"><span data-stu-id="58f1e-181">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="58f1e-182">Zakres jest tworzony w celu używania aplikacji `AppDbContext` (usługi w [zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów `IBackgroundTaskQueue` bazy danych w (pojedynczej usłudze).</span><span class="sxs-lookup"><span data-stu-id="58f1e-182">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="58f1e-183">Po wybraniu przycisku **Dodaj zadanie** na stronie `OnPostAddTask` indeks jest wykonywana metoda.</span><span class="sxs-lookup"><span data-stu-id="58f1e-183">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="58f1e-184">`QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy:</span><span class="sxs-lookup"><span data-stu-id="58f1e-184">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="58f1e-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="58f1e-185">Additional resources</span></span>

* [<span data-ttu-id="58f1e-186">Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService</span><span class="sxs-lookup"><span data-stu-id="58f1e-186">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
