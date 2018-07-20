---
title: Zadania w tle z usług hostowanych w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 087ff4e1e169e1a1f76e93d4993441e47bafc945
ms.sourcegitcommit: 7097dba14d5b858e82758ee031ac62dbe3611339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39138600"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="ce6fe-103">Zadania w tle z usług hostowanych w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce6fe-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="ce6fe-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ce6fe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ce6fe-105">W programie ASP.NET Core, można zaimplementować jako zadania w tle *usługi hostowane*.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="ce6fe-106">Usługa hostowana jest klasą z logiką zadań tła, który implementuje [pomocą interfejsu IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="ce6fe-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="ce6fe-108">Zadanie w tle wykonywana przez czasomierz.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="ce6fe-109">Usługa hostowana, które aktywuje usługę o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="ce6fe-110">Usługi o określonym zakresie służy iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="ce6fe-111">Umieszczonych w kolejce zadania w tle wykonywane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="ce6fe-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce6fe-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ce6fe-113">Przykładowa aplikacja znajduje się w dwóch wersjach:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="ce6fe-114">Sieci Web hosta &ndash; hosta sieci Web jest przydatne w przypadku hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="ce6fe-115">Kod przykładu przedstawiony w tym temacie pochodzą od hosta sieci Web wersję przykładu.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="ce6fe-116">Aby uzyskać więcej informacji, zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="ce6fe-117">Ogólny hosta &ndash; ogólnego hostów jest nowego w programie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="ce6fe-118">Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host) tematu.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="ce6fe-119">Interfejs pomocą interfejsu IHostedService</span><span class="sxs-lookup"><span data-stu-id="ce6fe-119">IHostedService interface</span></span>

<span data-ttu-id="ce6fe-120">Implementowanie usług hostowanych [pomocą interfejsu IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="ce6fe-121">Interfejs definiuje dwie metody dla obiektów, które są zarządzane przez hosta:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="ce6fe-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — nazywany po uruchomieniu serwera i [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="ce6fe-123">`StartAsync` zawiera logikę, aby uruchomić zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="ce6fe-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) — wyzwalane, gdy host działa łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="ce6fe-125">`StopAsync` zawiera logikę do zakończenia zadania w tle i usuwania niezarządzanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="ce6fe-126">Jeśli aplikacja zostanie wyłączony nieoczekiwanie (na przykład aplikacji proces zakończy się niepowodzeniem), `StopAsync` nie może być wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="ce6fe-127">Hostowana usługa została aktywowana po uruchamiania aplikacji, i bez problemu zmieniała zamykania przy zamykaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-127">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="ce6fe-128">Gdy [interfejsu IDisposable](/dotnet/api/system.idisposable) jest zaimplementowana, zasobów można usunąć po usunięciu kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="ce6fe-129">Jeśli podczas wykonywania zadania w tle, zostanie zgłoszony błąd `Dispose` powinna być wywoływana nawet wtedy, gdy `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="ce6fe-130">Zadania w tle przekroczono limit czasu</span><span class="sxs-lookup"><span data-stu-id="ce6fe-130">Timed background tasks</span></span>

<span data-ttu-id="ce6fe-131">Zadanie w tle czasu sprawia, że użycie [System.Threading.Timer](/dotnet/api/system.threading.timer) klasy.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="ce6fe-132">Czasomierz wyzwala zadanie `DoWork` metody.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="ce6fe-133">Czasomierz jest wyłączona na `StopAsync` i usunięty po usunięciu kontenera usług na `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="ce6fe-134">Usługa jest zarejestrowana w `Startup.ConfigureServices` z `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-134">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="ce6fe-135">Korzystanie z usługi o określonym zakresie w zadanie w tle</span><span class="sxs-lookup"><span data-stu-id="ce6fe-135">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="ce6fe-136">Do korzystania z usług o określonym zakresie w ramach `IHostedService`, tworzenia zakresu.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-136">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="ce6fe-137">Zakres nie jest domyślnie tworzone dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-137">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="ce6fe-138">Usługa zadań w tle o określonym zakresie zawiera logikę zadanie w tle.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-138">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="ce6fe-139">W poniższym przykładzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) są wstrzykiwane do usługi:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-139">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="ce6fe-140">Usługa hostowana tworzy zakres rozpoznawanie usługi zadania tła o określonym zakresie wywołać jej `DoWork` metody:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-140">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="ce6fe-141">Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-141">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ce6fe-142">`IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-142">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="ce6fe-143">Zadania w tle umieszczonych w kolejce</span><span class="sxs-lookup"><span data-stu-id="ce6fe-143">Queued background tasks</span></span>

<span data-ttu-id="ce6fe-144">Kolejki zadań tła opiera się na platformie .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([wstępnie zaplanowane być wbudowane dla platformy ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="ce6fe-144">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="ce6fe-145">W `QueueHostedService`, w kolejce zadania w tle są usuwane z kolejki i są stosowane jako [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice), czyli klasę bazową dla implementacji działa długo `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-145">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

<span data-ttu-id="ce6fe-146">Usługi są zarejestrowane w usłudze `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-146">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ce6fe-147">`IHostedService` Implementacja jest zarejestrowane w usłudze `AddHostedService` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-147">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="ce6fe-148">W klasie modelu strony indeksu `IBackgroundTaskQueue` wprowadzony do konstruktora i ma przypisaną do `Queue`:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-148">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="ce6fe-149">Gdy **Dodaj zadanie** przycisk jest zaznaczony na stronie indeksu `OnPostAddTask` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="ce6fe-149">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="ce6fe-150">`QueueBackgroundWorkItem` jest wywoływana, aby umieścić w kolejce elementu roboczego:</span><span class="sxs-lookup"><span data-stu-id="ce6fe-150">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="ce6fe-151">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ce6fe-151">Additional resources</span></span>

* [<span data-ttu-id="ce6fe-152">Implementowanie zadań w tle w mikrousługach za pomocą interfejsu IHostedService i klasa BackgroundService</span><span class="sxs-lookup"><span data-stu-id="ce6fe-152">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="ce6fe-153">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="ce6fe-153">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
