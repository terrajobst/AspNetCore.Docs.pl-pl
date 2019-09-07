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
# <a name="host-aspnet-core-signalr-in-background-services"></a>Hostowanie ASP.NET Core sygnalizacji w usługach w tle

Autor [Brady gastera](https://twitter.com/bradygaster)

Ten artykuł zawiera wskazówki dotyczące:

* Hosting centrów sygnałów przy użyciu procesu roboczego w tle hostowanego z ASP.NET Core.
* Wysyłanie komunikatów do podłączonych klientów z poziomu platformy .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Załącz sygnalizujący podczas uruchamiania

::: moniker range=">= aspnetcore-3.0"

Hostowanie ASP.NET Core centrów sygnałów w kontekście procesu roboczego w tle jest identyczne z hostem centrum w aplikacji sieci Web ASP.NET Core. W metodzie wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy iniekcja (ASP.NET Core zależność) do obsługi sygnałów. `Startup.ConfigureServices` W programie `Startup.Configure` Metodajestwywoływanawwywołaniuzwrotnym,abypołączyćpunktykońcowecentrumwpotokużądaniaASP.NETCore.`MapHub` `UseEndpoints`

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

Hostowanie ASP.NET Core centrów sygnałów w kontekście procesu roboczego w tle jest identyczne z hostem centrum w aplikacji sieci Web ASP.NET Core. W metodzie wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy iniekcja (ASP.NET Core zależność) do obsługi sygnałów. `Startup.ConfigureServices` W `Startup.Configure`programie `UseSignalR` Metoda jest wywoływana, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

W poprzednim przykładzie `ClockHub` Klasa `Hub<T>` implementuje klasę, aby utworzyć koncentrator o jednoznacznie określonym typie. Program został skonfigurowany `Startup` w klasie w celu reagowania na żądania w punkcie końcowym `/hubs/clock`. `ClockHub`

Aby uzyskać więcej informacji na temat silnych typach centrów, zobacz [Korzystanie z centrów w programie sygnalizującym dla ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Ta funkcja nie jest ograniczona do [klasy\<Hub t >](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Każda klasa, która dziedziczy z [koncentratora](xref:Microsoft.AspNetCore.SignalR.Hub), na przykład [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), również będzie działała.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Interfejs używany przez silnie typ `ClockHub` `IClock` jest interfejsem.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Wywoływanie centrum sygnałów z usługi w tle

Podczas uruchamiania `Worker` Klasa, a `BackgroundService`, jest przewodna przy użyciu `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Ponieważ sygnalizujący jest również koncentrator w trakcie `Startup` fazy, w którym każde centrum jest dołączone do pojedynczego punktu końcowego w potoku żądania HTTP ASP.NET Core, każde centrum jest reprezentowane `IHubContext<T>` przez serwer. Korzystając z ASP.NET Core di Features, inne klasy tworzone przez warstwę hostingu, takie jak `BackgroundService` klasy, klasy kontrolerów MVC lub modele stron Razor, mogą uzyskać odwołania do centrów po stronie serwera, akceptując `IHubContext<ClockHub, IClock>` wystąpienia podczas konstruowania.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Ponieważ metoda jest wywoływana iteracyjnie w usłudze w tle, bieżąca data i godzina serwera są wysyłane do podłączonych klientów `ClockHub`przy użyciu. `ExecuteAsync`

## <a name="react-to-signalr-events-with-background-services"></a>Reagowanie na zdarzenia sygnalizujące za pomocą usług w tle

Podobnie jak w przypadku aplikacji jednostronicowej korzystającej z klienta języka JavaScript dla programu sygnalizującego lub aplikacji klasycznej platformy <xref:signalr/dotnet-client>.NET, `BackgroundService` można `IHostedService` użyć, a lub wykonać implementację, aby nawiązać połączenie z centrami sygnałów i odpowiadać na zdarzenia.

Klasa implementuje `IClock` Interfejs i`IHostedService` interfejs. `ClockHubClient` W ten sposób można to zrobić `Startup` w sposób ciągły i reagować na zdarzenia centrów z serwera.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Podczas inicjowania program `ClockHubClient` tworzy wystąpienie `HubConnection` a i `IClock.ShowTime` przydzieli `ShowTime` metodę jako procedurę obsługi dla zdarzenia centrum.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

`IHostedService.StartAsync` W implementacji `HubConnection` jest uruchamiany asynchronicznie.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

W trakcie `HubConnection` metody jest usuwany asynchronicznie. `IHostedService.StopAsync`

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Centra o jednoznacznie określonym typie](xref:signalr/hubs#strongly-typed-hubs)
