---
title: Zadania w tle z usług hostowanych w ASP.NET Core
author: guardrex
description: Informacje o sposobie wykonania zadania w tle z usług hostowanych w ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: e5455e553cba817dce811391d4a909e501a20d9a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273784"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="b56e7-103">Zadania w tle z usług hostowanych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b56e7-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="b56e7-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b56e7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b56e7-105">W ASP.NET Core, można stosować jako zadania w tle *usług hostowanych*.</span><span class="sxs-lookup"><span data-stu-id="b56e7-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="b56e7-106">Hostowana usługa jest klasa z logiką zadania tła, który implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b56e7-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="b56e7-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="b56e7-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="b56e7-108">Zadania w tle uruchamiana przez czasomierz.</span><span class="sxs-lookup"><span data-stu-id="b56e7-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="b56e7-109">Usługa hostowana aktywuje zakresami usługi.</span><span class="sxs-lookup"><span data-stu-id="b56e7-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="b56e7-110">Usługa zakresie może wykorzystać iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="b56e7-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="b56e7-111">Tło w kolejce zadań, które są wykonywane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="b56e7-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="b56e7-112">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b56e7-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b56e7-113">Przykładowa aplikacja znajduje się w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="b56e7-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="b56e7-114">Sieci Web hosta &ndash; hosta sieci Web jest przydatne do obsługi aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="b56e7-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="b56e7-115">Przykładowy kod wyświetlany w tym temacie pochodzi z wersji hosta sieci Web w próbce.</span><span class="sxs-lookup"><span data-stu-id="b56e7-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="b56e7-116">Aby uzyskać więcej informacji, zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="b56e7-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="b56e7-117">Ogólny hosta &ndash; rodzajowego hosta jest nowa w programie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b56e7-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="b56e7-118">Aby uzyskać więcej informacji, zobacz [rodzajowego hosta](xref:fundamentals/host/generic-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="b56e7-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="b56e7-119">Interfejs IHostedService</span><span class="sxs-lookup"><span data-stu-id="b56e7-119">IHostedService interface</span></span>

<span data-ttu-id="b56e7-120">Wdrożenie usług hostowanych [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="b56e7-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="b56e7-121">Interfejs definiuje dwie metody dla obiektów, które są zarządzane przez hosta:</span><span class="sxs-lookup"><span data-stu-id="b56e7-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="b56e7-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — nazywany po uruchomieniu serwera i [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="b56e7-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="b56e7-123">`StartAsync` zawiera logikę można uruchomić zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="b56e7-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="b56e7-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -wyzwalane, gdy host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="b56e7-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="b56e7-125">`StopAsync` zawiera logikę do zakończenia zadania w tle i usuń wszelkie niezarządzane zasoby.</span><span class="sxs-lookup"><span data-stu-id="b56e7-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="b56e7-126">Jeśli aplikacja zostanie wyłączony nieoczekiwanie (na przykład aplikacji proces zakończy się niepowodzeniem), `StopAsync` nie może mieć nazwę.</span><span class="sxs-lookup"><span data-stu-id="b56e7-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="b56e7-127">Usługa hostowana jest uaktywnić raz uruchamiania aplikacji i bezpiecznie zamykania przy zamykaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b56e7-127">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="b56e7-128">Gdy [IDisposable](/dotnet/api/system.idisposable) jest zaimplementowana, zasobów można usunięta po usunięciu kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="b56e7-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="b56e7-129">Jeśli podczas wykonywania zadania w tle, zostanie zgłoszony błąd `Dispose` powinna być wywoływana nawet wtedy, gdy `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="b56e7-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="b56e7-130">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="b56e7-130">Timed background tasks</span></span>

<span data-ttu-id="b56e7-131">Zadanie w tle czasu sprawia, że użycie [System.Threading.Timer](/dotnet/api/system.threading.timer) klasy.</span><span class="sxs-lookup"><span data-stu-id="b56e7-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="b56e7-132">Czasomierza wyzwala zadanie `DoWork` metody.</span><span class="sxs-lookup"><span data-stu-id="b56e7-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="b56e7-133">Czasomierz jest wyłączona na `StopAsync` i usunięta po usunięciu kontenera usług na `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="b56e7-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b56e7-134">Usługa jest zarejestrowana w `Startup.ConfigureServices` z `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b56e7-134">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="b56e7-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="b56e7-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b56e7-136">Usługa jest zarejestrowana w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b56e7-136">The service is registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="b56e7-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="b56e7-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="b56e7-138">Korzystanie z zakresami usługę zadania w tle</span><span class="sxs-lookup"><span data-stu-id="b56e7-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="b56e7-139">Do użycia w zakresie usług `IHostedService`, utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="b56e7-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="b56e7-140">Zakres nie jest tworzone przez domyślny dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="b56e7-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="b56e7-141">Usługa zadań tła zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="b56e7-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="b56e7-142">W poniższym przykładzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) jest dodane do usługi:</span><span class="sxs-lookup"><span data-stu-id="b56e7-142">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="b56e7-143">Usługa hostowana tworzy zakres na rozpoznawanie usługi zadania tła o zakresie wywołać jej `DoWork` metody:</span><span class="sxs-lookup"><span data-stu-id="b56e7-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b56e7-144">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b56e7-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b56e7-145">`IHostedService` Implementacji został zarejestrowany za pomocą `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b56e7-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="b56e7-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="b56e7-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b56e7-147">Usługi są zarejestrowane w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b56e7-147">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="b56e7-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="b56e7-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

## <a name="queued-background-tasks"></a><span data-ttu-id="b56e7-149">Zadania w tle w kolejce</span><span class="sxs-lookup"><span data-stu-id="b56e7-149">Queued background tasks</span></span>

<span data-ttu-id="b56e7-150">Kolejki zadań tła jest oparta na platformie .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([wstępnie zaplanowane być wbudowane dla platformy ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="b56e7-150">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="b56e7-151">W `QueueHostedService`, zadania w tle (`workItem`) w kolejce usuniętej i wykonać:</span><span class="sxs-lookup"><span data-stu-id="b56e7-151">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b56e7-152">Usługi są zarejestrowane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b56e7-152">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b56e7-153">`IHostedService` Implementacji został zarejestrowany za pomocą `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b56e7-153">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="b56e7-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="b56e7-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b56e7-155">Usługi są zarejestrowane w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b56e7-155">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="b56e7-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="b56e7-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="b56e7-157">W klasie indeksu strony modelu `IBackgroundTaskQueue` do konstruktora i ma przypisaną do `Queue`:</span><span class="sxs-lookup"><span data-stu-id="b56e7-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="b56e7-158">Gdy **Dodaj zadanie** przycisk zostanie wybrany na stronie indeksu `OnPostAddTask` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="b56e7-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="b56e7-159">`QueueBackgroundWorkItem` jest wywoływana można umieścić w kolejce elementu roboczego:</span><span class="sxs-lookup"><span data-stu-id="b56e7-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="b56e7-160">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b56e7-160">Additional resources</span></span>

* [<span data-ttu-id="b56e7-161">Wykonania zadania w tle w mikrousług IHostedService i klasa BackgroundService</span><span class="sxs-lookup"><span data-stu-id="b56e7-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="b56e7-162">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="b56e7-162">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
