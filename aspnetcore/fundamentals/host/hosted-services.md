---
title: Zadania w tle z usług hostowanych w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 2dbb1a84a380ab06a4be7ecf628799a070afc9e3
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692514"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="4e542-103">Zadania w tle z usług hostowanych w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e542-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="4e542-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4e542-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4e542-105">W programie ASP.NET Core, można zaimplementować jako zadania w tle *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="4e542-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="4e542-106">Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="4e542-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="4e542-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="4e542-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="4e542-108">Zadanie w tle wykonywana przez czasomierz.</span><span class="sxs-lookup"><span data-stu-id="4e542-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="4e542-109">Hostowana usługa, która aktywuje [zakresu usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="4e542-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="4e542-110">Usługi o określonym zakresie służy iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="4e542-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="4e542-111">Umieszczonych w kolejce zadania w tle wykonywane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="4e542-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="4e542-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4e542-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4e542-113">Przykładowa aplikacja znajduje się w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="4e542-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="4e542-114">Sieci Web hosta &ndash; hosta sieci Web jest przydatne w przypadku hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4e542-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="4e542-115">Kod przykładu przedstawiony w tym temacie pochodzą od hosta sieci Web wersję przykładu.</span><span class="sxs-lookup"><span data-stu-id="4e542-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="4e542-116">Aby uzyskać więcej informacji, zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="4e542-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="4e542-117">Ogólny hosta &ndash; ogólnego Host jest nowego w programie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4e542-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="4e542-118">Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="4e542-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="4e542-119">Szablon usługi procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="4e542-119">Worker Service template</span></span>

<span data-ttu-id="4e542-120">Szablon usługi procesu roboczego programu ASP.NET Core stanowi punkt wyjścia do pisania długo działające usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="4e542-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="4e542-121">Aby użyć szablonu jako podstawy dla aplikacji hostowanych usług:</span><span class="sxs-lookup"><span data-stu-id="4e542-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e542-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e542-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="4e542-123">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="4e542-123">Create a new project.</span></span>
1. <span data-ttu-id="4e542-124">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4e542-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4e542-125">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="4e542-125">Select **Next**.</span></span>
1. <span data-ttu-id="4e542-126">Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="4e542-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="4e542-127">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4e542-127">Select **Create**.</span></span>
1. <span data-ttu-id="4e542-128">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="4e542-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="4e542-129">Wybierz **Usługa procesu roboczego** szablonu.</span><span class="sxs-lookup"><span data-stu-id="4e542-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="4e542-130">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="4e542-130">Select **Create**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="4e542-131">Program Visual Studio Code / .NET Core interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="4e542-131">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="4e542-132">Usługa procesu roboczego (`worker`) szablon [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="4e542-132">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="4e542-133">W poniższym przykładzie tworzony jest aplikacją usługi procesu roboczego o nazwie `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="4e542-133">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="4e542-134">Folder `ContosoWorkerService` aplikacji jest tworzona automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="4e542-134">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a><span data-ttu-id="4e542-135">Package</span><span class="sxs-lookup"><span data-stu-id="4e542-135">Package</span></span>

<span data-ttu-id="4e542-136">Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4e542-136">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="4e542-137">Interfejs pomocą interfejsu IHostedService</span><span class="sxs-lookup"><span data-stu-id="4e542-137">IHostedService interface</span></span>

<span data-ttu-id="4e542-138">Implementowanie usług hostowanych <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="4e542-138">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="4e542-139">Interfejs definiuje dwie metody dla obiektów, które są zarządzane przez hosta:</span><span class="sxs-lookup"><span data-stu-id="4e542-139">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="4e542-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` zawiera logikę, aby uruchomić zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="4e542-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="4e542-141">Korzystając z [hosta sieci Web](xref:fundamentals/host/web-host), `StartAsync` jest wywoływana po uruchomieniu serwera i [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="4e542-141">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="4e542-142">Korzystając z [ogólnego hosta](xref:fundamentals/host/generic-host), `StartAsync` jest wywoływana przed `ApplicationStarted` zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="4e542-142">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="4e542-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; wyzwalane, gdy host działa łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="4e542-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="4e542-144">`StopAsync` zawiera logikę, aby zakończyć zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="4e542-144">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="4e542-145">Implementowanie <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) usuwania niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="4e542-145">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="4e542-146">Token anulowania ma domyślny pięciu drugi limit czasu do wskazania, że proces zamykania nie powinny już być bezpieczne.</span><span class="sxs-lookup"><span data-stu-id="4e542-146">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="4e542-147">Jeśli zażądano anulowania w tokenie:</span><span class="sxs-lookup"><span data-stu-id="4e542-147">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="4e542-148">Wszystkie pozostałe operacje w tle, które wykonuje aplikacja powinna zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="4e542-148">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="4e542-149">Wszystkie metody o nazwie w `StopAsync` powinna zwrócić natychmiast.</span><span class="sxs-lookup"><span data-stu-id="4e542-149">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="4e542-150">Jednak zadania nie są porzucona po żądania anulowania&mdash;obiekt wywołujący czeka na ukończenie wszystkich zadań.</span><span class="sxs-lookup"><span data-stu-id="4e542-150">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="4e542-151">Jeśli aplikacja zostanie wyłączony nieoczekiwanie (na przykład aplikacji proces zakończy się niepowodzeniem), `StopAsync` nie może być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="4e542-151">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="4e542-152">W związku z tym, wszystkie metody o nazwie lub operacje przeprowadzane w `StopAsync` nie mogą wystąpić.</span><span class="sxs-lookup"><span data-stu-id="4e542-152">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="4e542-153">Aby rozszerzyć domyślna pięć drugi zamykania wartość limitu czasu, należy ustawić:</span><span class="sxs-lookup"><span data-stu-id="4e542-153">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="4e542-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Korzystając z ogólnego hosta.</span><span class="sxs-lookup"><span data-stu-id="4e542-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="4e542-155">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="4e542-155">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="4e542-156">Zamknięcie ustawienie limitu czasu hosta konfiguracji przy użyciu hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4e542-156">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="4e542-157">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="4e542-157">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="4e542-158">Usługa hostowana jest aktywowany po przy uruchamianiu aplikacji i zostanie poprawnie zamknięte przy zamykaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4e542-158">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="4e542-159">Jeśli podczas wykonywania zadania w tle, zostanie zgłoszony błąd `Dispose` powinna być wywoływana nawet wtedy, gdy `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="4e542-159">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="4e542-160">Zadania w tle przekroczono limit czasu</span><span class="sxs-lookup"><span data-stu-id="4e542-160">Timed background tasks</span></span>

<span data-ttu-id="4e542-161">Zadanie w tle czasu sprawia, że użycie [System.Threading.Timer](xref:System.Threading.Timer) klasy.</span><span class="sxs-lookup"><span data-stu-id="4e542-161">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="4e542-162">Czasomierz wyzwala zadanie `DoWork` metody.</span><span class="sxs-lookup"><span data-stu-id="4e542-162">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="4e542-163">Czasomierz jest wyłączona na `StopAsync` i usunięty po usunięciu kontenera usług na `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="4e542-163">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="4e542-164">Usługa jest zarejestrowana w `Startup.ConfigureServices` z `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="4e542-164">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="4e542-165">Korzystanie z usługi o określonym zakresie w zadanie w tle</span><span class="sxs-lookup"><span data-stu-id="4e542-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="4e542-166">Aby użyć [zakresu usług](xref:fundamentals/dependency-injection#service-lifetimes) w ramach `IHostedService`, utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="4e542-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="4e542-167">Zakres nie jest domyślnie tworzone dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="4e542-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="4e542-168">Usługa zadań w tle o określonym zakresie zawiera logikę zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="4e542-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="4e542-169">W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> są wstrzykiwane do usługi:</span><span class="sxs-lookup"><span data-stu-id="4e542-169">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="4e542-170">Usługa hostowana tworzy zakres rozpoznawanie usługi zadania tła o określonym zakresie wywołać jej `DoWork` metody:</span><span class="sxs-lookup"><span data-stu-id="4e542-170">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="4e542-171">Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4e542-171">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4e542-172">`IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="4e542-172">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="4e542-173">Zadania w tle umieszczonych w kolejce</span><span class="sxs-lookup"><span data-stu-id="4e542-173">Queued background tasks</span></span>

<span data-ttu-id="4e542-174">Kolejki zadań tła opiera się na platformie .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([wstępnie zaplanowane być wbudowane dla platformy ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="4e542-174">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="4e542-175">W `QueueHostedService`, w kolejce zadania w tle są usuwane z kolejki i są stosowane jako <xref:Microsoft.Extensions.Hosting.BackgroundService>, czyli klasę bazową dla implementacji działa długo `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="4e542-175">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="4e542-176">Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4e542-176">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4e542-177">`IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="4e542-177">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="4e542-178">W klasie modelu strony indeksu:</span><span class="sxs-lookup"><span data-stu-id="4e542-178">In the Index page model class:</span></span>

* <span data-ttu-id="4e542-179">`IBackgroundTaskQueue` Wprowadzony do konstruktora i ma przypisaną do `Queue`.</span><span class="sxs-lookup"><span data-stu-id="4e542-179">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="4e542-180"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> Wprowadzony i ma przypisaną do `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="4e542-180">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="4e542-181">Fabryka jest używany do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, który jest używany do tworzenia usług w obrębie zakresu.</span><span class="sxs-lookup"><span data-stu-id="4e542-181">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="4e542-182">Zakres jest utworzone w celu korzystania z aplikacji `AppDbContext` ( [zakresu usługi](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów bazy danych `IBackgroundTaskQueue` (usługi singleton).</span><span class="sxs-lookup"><span data-stu-id="4e542-182">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="4e542-183">Gdy **Dodaj zadanie** przycisk jest zaznaczony na stronie indeksu `OnPostAddTask` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="4e542-183">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="4e542-184">`QueueBackgroundWorkItem` jest wywoływana, aby umieścić w kolejce elementu roboczego:</span><span class="sxs-lookup"><span data-stu-id="4e542-184">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="4e542-185">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4e542-185">Additional resources</span></span>

* [<span data-ttu-id="4e542-186">Implementowanie zadań w tle w mikrousługach za pomocą interfejsu IHostedService i klasa BackgroundService</span><span class="sxs-lookup"><span data-stu-id="4e542-186">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
