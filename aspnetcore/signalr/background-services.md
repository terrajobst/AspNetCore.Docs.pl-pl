---
title: ASP.NET Core hosta SignalR w usługach w tle
author: bradygaster
description: Dowiedz się, jak wysyłać komunikaty do SignalR klientów z klas programu .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 86319cc93febab18c29e2fb6366cef0d025943ba
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658145"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a><span data-ttu-id="84976-103">ASP.NET Core hosta SignalR w usługach w tle</span><span class="sxs-lookup"><span data-stu-id="84976-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="84976-104">Autor [Brady gastera](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="84976-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="84976-105">Ten artykuł zawiera wskazówki dotyczące:</span><span class="sxs-lookup"><span data-stu-id="84976-105">This article provides guidance for:</span></span>

* <span data-ttu-id="84976-106">Hostowanie SignalR centrów przy użyciu procesu roboczego w tle hostowanego z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84976-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="84976-107">Wysyłanie komunikatów do podłączonych klientów z poziomu platformy .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="84976-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="84976-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="84976-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="enable-opno-locsignalr-in-startup"></a><span data-ttu-id="84976-109">Włącz SignalR podczas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="84976-109">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="84976-110">Hostowanie ASP.NET Core centrów SignalR w kontekście procesu roboczego w tle jest takie samo, jak hostowanie centrum w aplikacji sieci Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84976-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="84976-111">W metodzie `Startup.ConfigureServices` wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy wtrysku zależności (DI) ASP.NET Core do obsługi SignalR.</span><span class="sxs-lookup"><span data-stu-id="84976-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="84976-112">W `Startup.Configure`Metoda `MapHub` jest wywoływana w wywołaniu zwrotnym `UseEndpoints`, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84976-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

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

<span data-ttu-id="84976-113">Hostowanie ASP.NET Core centrów SignalR w kontekście procesu roboczego w tle jest takie samo, jak hostowanie centrum w aplikacji sieci Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84976-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="84976-114">W metodzie `Startup.ConfigureServices` wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy wtrysku zależności (DI) ASP.NET Core do obsługi SignalR.</span><span class="sxs-lookup"><span data-stu-id="84976-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="84976-115">W `Startup.Configure`Metoda `UseSignalR` jest wywoływana, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84976-115">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="84976-116">W poprzednim przykładzie Klasa `ClockHub` implementuje klasę `Hub<T>`, aby utworzyć koncentrator o jednoznacznie określonym typie.</span><span class="sxs-lookup"><span data-stu-id="84976-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="84976-117">`ClockHub` został skonfigurowany w klasie `Startup`, aby odpowiadały na żądania w punkcie końcowym `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="84976-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="84976-118">Aby uzyskać więcej informacji na temat silnych typach centrów, zobacz [Korzystanie z centrów w SignalR ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="84976-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="84976-119">Ta funkcja nie jest ograniczona do klasy [centrum >\<](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="84976-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="84976-120">Każda klasa, która dziedziczy z [koncentratora](xref:Microsoft.AspNetCore.SignalR.Hub), na przykład [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), również będzie działała.</span><span class="sxs-lookup"><span data-stu-id="84976-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="84976-121">Interfejs używany przez silnie wpisaną `ClockHub` jest interfejsem `IClock`.</span><span class="sxs-lookup"><span data-stu-id="84976-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a><span data-ttu-id="84976-122">Wywoływanie centrum SignalR z poziomu usługi w tle</span><span class="sxs-lookup"><span data-stu-id="84976-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="84976-123">Podczas uruchamiania, Klasa `Worker`, `BackgroundService`, jest włączona przy użyciu `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="84976-123">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="84976-124">Ponieważ SignalR jest również włączona w fazie `Startup`, w którym każde centrum jest dołączone do pojedynczego punktu końcowego w potoku żądania HTTP ASP.NET Core, każde centrum jest reprezentowane przez `IHubContext<T>` na serwerze.</span><span class="sxs-lookup"><span data-stu-id="84976-124">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="84976-125">Za pomocą ASP.NET Core DI Features, inne klasy tworzone przez warstwę hostingu, takie jak klasy `BackgroundService`, klasy kontrolerów MVC lub modele stron Razor, mogą uzyskać odwołania do centrów po stronie serwera, akceptując wystąpienia `IHubContext<ClockHub, IClock>` podczas konstruowania.</span><span class="sxs-lookup"><span data-stu-id="84976-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="84976-126">Ponieważ metoda `ExecuteAsync` jest wywoływana iteracyjnie w usłudze w tle, bieżąca data i godzina serwera są wysyłane do podłączonych klientów przy użyciu `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="84976-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-opno-locsignalr-events-with-background-services"></a><span data-ttu-id="84976-127">Reagowanie na zdarzenia SignalR z usługami w tle</span><span class="sxs-lookup"><span data-stu-id="84976-127">React to SignalR events with background services</span></span>

<span data-ttu-id="84976-128">Podobnie jak w przypadku aplikacji jednostronicowej korzystającej z klienta JavaScript dla SignalR lub aplikacji klasycznej platformy .NET można używać <xref:signalr/dotnet-client>, można także użyć implementacji `BackgroundService` lub `IHostedService`, aby nawiązać połączenie z SignalR centrami i odpowiedzieć na zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="84976-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="84976-129">Klasa `ClockHubClient` implementuje interfejs `IClock` i interfejs `IHostedService`.</span><span class="sxs-lookup"><span data-stu-id="84976-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="84976-130">W ten sposób można ją włączyć podczas `Startup`, aby działać w sposób ciągły i odpowiadać na zdarzenia centrów z serwera.</span><span class="sxs-lookup"><span data-stu-id="84976-130">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="84976-131">Podczas inicjowania `ClockHubClient` tworzy wystąpienie `HubConnection` i włącza metodę `IClock.ShowTime` jako procedurę obsługi dla zdarzenia `ShowTime` centrum.</span><span class="sxs-lookup"><span data-stu-id="84976-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="84976-132">W implementacji `IHostedService.StartAsync` `HubConnection` jest uruchamiany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="84976-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="84976-133">Podczas `IHostedService.StopAsync` Metoda `HubConnection` jest usuwana asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="84976-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="84976-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="84976-134">Additional resources</span></span>

* [<span data-ttu-id="84976-135">Rozpoczęcie pracy</span><span class="sxs-lookup"><span data-stu-id="84976-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="84976-136">Centra</span><span class="sxs-lookup"><span data-stu-id="84976-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="84976-137">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="84976-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="84976-138">Centra o jednoznacznie określonym typie</span><span class="sxs-lookup"><span data-stu-id="84976-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
