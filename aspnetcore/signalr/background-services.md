---
title: Hostowanie ASP.NET Core sygnalizacji w usługach w tle
author: bradygaster
description: Dowiedz się, jak wysyłać komunikaty do klientów sygnalizujących z klas programu .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773934"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="ebc5f-103">Hostowanie ASP.NET Core sygnalizacji w usługach w tle</span><span class="sxs-lookup"><span data-stu-id="ebc5f-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="ebc5f-104">Autor [Brady gastera](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="ebc5f-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="ebc5f-105">Ten artykuł zawiera wskazówki dotyczące:</span><span class="sxs-lookup"><span data-stu-id="ebc5f-105">This article provides guidance for:</span></span>

* <span data-ttu-id="ebc5f-106">Hosting centrów sygnałów przy użyciu procesu roboczego w tle hostowanego z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="ebc5f-107">Wysyłanie komunikatów do podłączonych klientów z poziomu platformy .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="ebc5f-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="ebc5f-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="ebc5f-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="ebc5f-109">Załącz sygnalizujący podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="ebc5f-109">Wire up SignalR during startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ebc5f-110">Hostowanie ASP.NET Core centrów sygnałów w kontekście procesu roboczego w tle jest identyczne z hostem centrum w aplikacji sieci Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="ebc5f-111">W metodzie wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy iniekcja (ASP.NET Core zależność) do obsługi sygnałów. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="ebc5f-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="ebc5f-112">W programie `Startup.Configure` Metodajestwywoływanawwywołaniuzwrotnym,abypołączyćpunktykońcowecentrumwpotokużądaniaASP.NETCore.`MapHub` `UseEndpoints`</span><span class="sxs-lookup"><span data-stu-id="ebc5f-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ebc5f-113">Hostowanie ASP.NET Core centrów sygnałów w kontekście procesu roboczego w tle jest identyczne z hostem centrum w aplikacji sieci Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="ebc5f-114">W metodzie wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy iniekcja (ASP.NET Core zależność) do obsługi sygnałów. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="ebc5f-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="ebc5f-115">W `Startup.Configure`programie `UseSignalR` Metoda jest wywoływana, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-115">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="ebc5f-116">W poprzednim przykładzie `ClockHub` Klasa `Hub<T>` implementuje klasę, aby utworzyć koncentrator o jednoznacznie określonym typie.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="ebc5f-117">Program został skonfigurowany `Startup` w klasie w celu reagowania na żądania w punkcie końcowym `/hubs/clock`. `ClockHub`</span><span class="sxs-lookup"><span data-stu-id="ebc5f-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="ebc5f-118">Aby uzyskać więcej informacji na temat silnych typach centrów, zobacz [Korzystanie z centrów w programie sygnalizującym dla ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="ebc5f-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="ebc5f-119">Ta funkcja nie jest ograniczona do [klasy\<Hub t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="ebc5f-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="ebc5f-120">Każda klasa, która dziedziczy z [koncentratora](xref:Microsoft.AspNetCore.SignalR.Hub), na przykład [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), również będzie działała.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="ebc5f-121">Interfejs używany przez silnie typ `ClockHub` `IClock` jest interfejsem.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="ebc5f-122">Wywoływanie centrum sygnałów z usługi w tle</span><span class="sxs-lookup"><span data-stu-id="ebc5f-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="ebc5f-123">Podczas uruchamiania `Worker` Klasa, a `BackgroundService`, jest przewodna przy użyciu `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-123">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="ebc5f-124">Ponieważ sygnalizujący jest również koncentrator w trakcie `Startup` fazy, w którym każde centrum jest dołączone do pojedynczego punktu końcowego w potoku żądania HTTP ASP.NET Core, każde centrum jest reprezentowane `IHubContext<T>` przez serwer.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-124">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="ebc5f-125">Korzystając z ASP.NET Core di Features, inne klasy tworzone przez warstwę hostingu, takie jak `BackgroundService` klasy, klasy kontrolerów MVC lub modele stron Razor, mogą uzyskać odwołania do centrów po stronie serwera, akceptując `IHubContext<ClockHub, IClock>` wystąpienia podczas konstruowania.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="ebc5f-126">Ponieważ metoda jest wywoływana iteracyjnie w usłudze w tle, bieżąca data i godzina serwera są wysyłane do podłączonych klientów `ClockHub`przy użyciu. `ExecuteAsync`</span><span class="sxs-lookup"><span data-stu-id="ebc5f-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="ebc5f-127">Reagowanie na zdarzenia sygnalizujące za pomocą usług w tle</span><span class="sxs-lookup"><span data-stu-id="ebc5f-127">React to SignalR events with background services</span></span>

<span data-ttu-id="ebc5f-128">Podobnie jak w przypadku aplikacji jednostronicowej korzystającej z klienta języka JavaScript dla programu sygnalizującego lub aplikacji klasycznej platformy <xref:signalr/dotnet-client>.NET, `BackgroundService` można `IHostedService` użyć, a lub wykonać implementację, aby nawiązać połączenie z centrami sygnałów i odpowiadać na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="ebc5f-129">Klasa implementuje `IClock` Interfejs i`IHostedService` interfejs. `ClockHubClient`</span><span class="sxs-lookup"><span data-stu-id="ebc5f-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="ebc5f-130">W ten sposób można to zrobić `Startup` w sposób ciągły i reagować na zdarzenia centrów z serwera.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-130">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="ebc5f-131">Podczas inicjowania program `ClockHubClient` tworzy wystąpienie `HubConnection` a i `IClock.ShowTime` przydzieli `ShowTime` metodę jako procedurę obsługi dla zdarzenia centrum.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="ebc5f-132">`IHostedService.StartAsync` W implementacji `HubConnection` jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="ebc5f-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="ebc5f-133">W trakcie `HubConnection` metody jest usuwany asynchronicznie. `IHostedService.StopAsync`</span><span class="sxs-lookup"><span data-stu-id="ebc5f-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="ebc5f-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ebc5f-134">Additional resources</span></span>

* [<span data-ttu-id="ebc5f-135">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="ebc5f-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="ebc5f-136">Centra</span><span class="sxs-lookup"><span data-stu-id="ebc5f-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="ebc5f-137">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ebc5f-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="ebc5f-138">Centra o jednoznacznie określonym typie</span><span class="sxs-lookup"><span data-stu-id="ebc5f-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
