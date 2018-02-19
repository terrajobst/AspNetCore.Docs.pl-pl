---
title: "Zadania w tle z usług hostowanych w ASP.NET Core"
author: guardrex
description: "Informacje o sposobie wykonania zadania w tle z usług hostowanych w ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosted-services
ms.openlocfilehash: 49d7780184e915bfbfd48cc9a2285f23cac20b34
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="ad1e9-103">Zadania w tle z usług hostowanych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad1e9-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="ad1e9-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ad1e9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ad1e9-105">W ASP.NET Core, można stosować jako zadania w tle *usług hostowanych*.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="ad1e9-106">Hostowana usługa jest klasa z logiką zadania tła, który implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="ad1e9-107">Ten temat zawiera trzy przykłady usługi hostowanej:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="ad1e9-108">Zadania w tle uruchamiana przez czasomierz.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="ad1e9-109">Usługa hostowana aktywuje zakresami usługi.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="ad1e9-110">Usługa zakresie może wykorzystać iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="ad1e9-111">Tło w kolejce zadań, które są wykonywane sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="ad1e9-112">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ad1e9-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="ad1e9-113">Interfejs IHostedService</span><span class="sxs-lookup"><span data-stu-id="ad1e9-113">IHostedService interface</span></span>

<span data-ttu-id="ad1e9-114">Wdrożenie usług hostowanych [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="ad1e9-115">Interfejs definiuje dwie metody dla obiektów, które są zarządzane przez hosta:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="ad1e9-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — nazywany po uruchomieniu serwera i [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="ad1e9-117">`StartAsync` zawiera logikę można uruchomić zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="ad1e9-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -wyzwalane, gdy host wykonuje łagodne zamykanie.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="ad1e9-119">`StopAsync` zawiera logikę do zakończenia zadania w tle i usuń wszelkie niezarządzane zasoby.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="ad1e9-120">Jeśli aplikacja zostanie wyłączony nieoczekiwanie (na przykład aplikacji proces zakończy się niepowodzeniem), `StopAsync` nie może mieć nazwę.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="ad1e9-121">Hostowana usługa jest pojedyncza po aktywowaniu uruchamiania aplikacji i bezpiecznie zamknięcia przy zamykaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="ad1e9-122">Gdy [IDisposable](/dotnet/api/system.idisposable) jest zaimplementowana, zasobów można usunięta po usunięciu kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="ad1e9-123">Jeśli podczas wykonywania zadania w tle, zostanie zgłoszony błąd `Dispose` powinna być wywoływana nawet wtedy, gdy `StopAsync` nie jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="ad1e9-124">Zadania w tle czasu</span><span class="sxs-lookup"><span data-stu-id="ad1e9-124">Timed background tasks</span></span>

<span data-ttu-id="ad1e9-125">Zadanie w tle czasu sprawia, że użycie [System.Threading.Timer](/dotnet/api/system.threading.timer) klasy.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="ad1e9-126">Czasomierza wyzwala zadanie `DoWork` metody.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="ad1e9-127">Czasomierz jest wyłączona na `StopAsync` i usunięta po usunięciu kontenera usług na `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/sample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="ad1e9-128">Usługa jest zarejestrowana w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/sample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="ad1e9-129">Korzystanie z zakresami usługę zadania w tle</span><span class="sxs-lookup"><span data-stu-id="ad1e9-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="ad1e9-130">Do użycia w zakresie usług `IHostedService`, utworzyć zakres.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="ad1e9-131">Zakres nie jest tworzone przez domyślny dla usługi hostowanej.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="ad1e9-132">Usługa zadań tła zakresie zawiera logikę zadania w tle.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="ad1e9-133">W poniższym przykładzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) jest dodane do usługi:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/sample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="ad1e9-134">Usługa hostowana tworzy zakres na rozpoznawanie usługi zadania tła o zakresie wywołać jej `DoWork` metody:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/sample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="ad1e9-135">Usługi są zarejestrowane w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/sample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="ad1e9-136">Zadania w tle w kolejce</span><span class="sxs-lookup"><span data-stu-id="ad1e9-136">Queued background tasks</span></span>

<span data-ttu-id="ad1e9-137">Kolejki zadań tła jest oparta na platformie .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([wstępnie zaplanowane być wbudowane dla platformy ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="ad1e9-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/sample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="ad1e9-138">W `QueueHostedService`, zadania w tle (`workItem`) w kolejce usuniętej i wykonać:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/sample/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="ad1e9-139">Usługi są zarejestrowane w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="ad1e9-140">W klasie indeksu strony modelu `IBackgroundTaskQueue` do konstruktora i ma przypisaną do `Queue`:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="ad1e9-141">Gdy **Dodaj zadanie** przycisk zostanie wybrany na stronie indeksu `OnPostAddTask` metoda jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="ad1e9-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="ad1e9-142">`QueueBackgroundWorkItem` jest wywoływana można umieścić w kolejce elementu roboczego:</span><span class="sxs-lookup"><span data-stu-id="ad1e9-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/sample/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="ad1e9-143">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ad1e9-143">Additional resources</span></span>

* [<span data-ttu-id="ad1e9-144">Wykonania zadania w tle w mikrousług IHostedService i klasa BackgroundService</span><span class="sxs-lookup"><span data-stu-id="ad1e9-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="ad1e9-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="ad1e9-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
