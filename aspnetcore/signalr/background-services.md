---
title: Host platformy ASP.NET Core SignalR w usługi w tle
author: bradygaster
description: Dowiedz się, jak wysyłać komunikaty do klientów SignalR z klas .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739673"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="fb570-103">Host platformy ASP.NET Core SignalR w usługi w tle</span><span class="sxs-lookup"><span data-stu-id="fb570-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="fb570-104">Przez [Brady'ego Gastera](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="fb570-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="fb570-105">Ten artykuł zawiera wskazówki dotyczące:</span><span class="sxs-lookup"><span data-stu-id="fb570-105">This article provides guidance for:</span></span>

* <span data-ttu-id="fb570-106">Hosting centrów SignalR przy użyciu procesu roboczego tła hostowanych za pomocą programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb570-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="fb570-107">Wysyłanie komunikatów do połączonych klientów z w ramach platformy .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="fb570-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="fb570-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="fb570-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="fb570-109">Podłączanie SignalR podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="fb570-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="fb570-110">Hosting centrów SignalR programu ASP.NET Core w ramach procesu roboczego tła jest taka sama jak hostowania Centrum w aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb570-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="fb570-111">W `Startup.ConfigureServices` metody, wywoływania `services.AddSignalR` dodaje wymagane usługi do warstwy platformy ASP.NET Core zależności iniekcji (DI) do obsługi SignalR.</span><span class="sxs-lookup"><span data-stu-id="fb570-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="fb570-112">W `Startup.Configure`, `UseSignalR` metoda jest wywoływana, aby przewodowy się punkty końcowe: koncentratora, w potoku żądania programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb570-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="fb570-113">W powyższym przykładzie `ClockHub` klasy implementuje `Hub<T>` klasy, aby utworzyć silnie typizowaną koncentratora.</span><span class="sxs-lookup"><span data-stu-id="fb570-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="fb570-114">`ClockHub` Został skonfigurowany w `Startup` klasy, aby odpowiadać na żądania w punkcie końcowym `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="fb570-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="fb570-115">Aby uzyskać więcej informacji na silnie typizowaną Hubs, zobacz [dla platformy ASP.NET Core przy użyciu hubs w SignalR](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="fb570-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="fb570-116">Ta funkcja nie jest ograniczona do [Centrum\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) klasy.</span><span class="sxs-lookup"><span data-stu-id="fb570-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="fb570-117">Każda klasa, która dziedziczy z [Centrum](xref:Microsoft.AspNetCore.SignalR.Hub), takich jak [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), również będą działać.</span><span class="sxs-lookup"><span data-stu-id="fb570-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="fb570-118">Interfejs używany przez silnie typizowaną `ClockHub` jest `IClock` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="fb570-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="fb570-119">Wywoływanie Centrum SignalR z usługę w tle</span><span class="sxs-lookup"><span data-stu-id="fb570-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="fb570-120">Podczas uruchamiania `Worker` klasy `BackgroundService`, przewodowej przy użyciu `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="fb570-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="fb570-121">Ponieważ SignalR jest również przewodowej podczas `Startup` fazy, w którym każdy Centrum jest dołączony do poszczególnych punktu końcowego w Potok żądań HTTP platformy ASP.NET Core, każdego Centrum jest reprezentowany przez `IHubContext<T>` na serwerze.</span><span class="sxs-lookup"><span data-stu-id="fb570-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="fb570-122">Za pomocą platformy ASP.NET Core DI funkcji, innych klas, tworzone przez warstwę obsługi, takie jak `BackgroundService` klasy, klasy kontrolera MVC lub modele strony Razor, można uzyskać odwołania do koncentratorów po stronie serwera, akceptując wystąpień `IHubContext<ClockHub, IClock>` podczas konstruowania.</span><span class="sxs-lookup"><span data-stu-id="fb570-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="fb570-123">Jako `ExecuteAsync` wywoływana jest metoda iteracyjne usługi w tle, bieżącą datę i godzinę serwera są wysyłane do połączonych klientów przy użyciu `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="fb570-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="fb570-124">Reagowanie na zdarzenia SignalR przy użyciu usługi w tle</span><span class="sxs-lookup"><span data-stu-id="fb570-124">React to SignalR events with background services</span></span>

<span data-ttu-id="fb570-125">Podobnie jak aplikacja jednostronicowa przy użyciu klienta języka JavaScript dla biblioteki SignalR lub aplikacji klasycznej platformy .NET można zrobić za pomocą modułu za pomocą polecenia <xref:signalr/dotnet-client>, `BackgroundService` lub `IHostedService` implementacji można również nawiązać połączenie z centrów SignalR i reagowania na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="fb570-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="fb570-126">`ClockHubClient` Klasa implementuje interfejsy `IClock` interfejsu i `IHostedService` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="fb570-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="fb570-127">W ten sposób mogą być połączone się podczas `Startup` wykonuj ciągle i reagowania na zdarzenia Centrum z serwera.</span><span class="sxs-lookup"><span data-stu-id="fb570-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="fb570-128">Podczas inicjowania `ClockHubClient` tworzy wystąpienie `HubConnection` i tworzącej powiązania `IClock.ShowTime` metodę jako program obsługi Centrum `ShowTime` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="fb570-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="fb570-129">W `IHostedService.StartAsync` implementacji `HubConnection` jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="fb570-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="fb570-130">Podczas `IHostedService.StopAsync` metody `HubConnection` jest usunięty asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="fb570-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="fb570-131">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fb570-131">Additional resources</span></span>

* [<span data-ttu-id="fb570-132">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="fb570-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="fb570-133">Centra</span><span class="sxs-lookup"><span data-stu-id="fb570-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="fb570-134">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="fb570-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="fb570-135">Koncentratory silnie typizowane</span><span class="sxs-lookup"><span data-stu-id="fb570-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
