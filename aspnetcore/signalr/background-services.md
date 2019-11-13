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
ms.openlocfilehash: 000732115153eeafed3948c2a07acf77ffc34218
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73964042"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a>ASP.NET Core hosta SignalR w usługach w tle

Autor [Brady gastera](https://twitter.com/bradygaster)

Ten artykuł zawiera wskazówki dotyczące:

* Hostowanie SignalR centrów przy użyciu procesu roboczego w tle hostowanego z ASP.NET Core.
* Wysyłanie komunikatów do podłączonych klientów z poziomu platformy .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="wire-up-opno-locsignalr-during-startup"></a>Załącz SignalR podczas uruchamiania

::: moniker range=">= aspnetcore-3.0"

Hostowanie ASP.NET Core centrów SignalR w kontekście procesu roboczego w tle jest takie samo, jak hostowanie centrum w aplikacji sieci Web ASP.NET Core. W metodzie `Startup.ConfigureServices` wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy wtrysku zależności (DI) ASP.NET Core do obsługi SignalR. W `Startup.Configure`Metoda `MapHub` jest wywoływana w wywołaniu wywołania zwrotnego `UseEndpoints`, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.

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

Hostowanie ASP.NET Core centrów SignalR w kontekście procesu roboczego w tle jest takie samo, jak hostowanie centrum w aplikacji sieci Web ASP.NET Core. W metodzie `Startup.ConfigureServices` wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy wtrysku zależności (DI) ASP.NET Core do obsługi SignalR. W `Startup.Configure`Metoda `UseSignalR` jest wywoływana, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

W poprzednim przykładzie Klasa `ClockHub` implementuje klasę `Hub<T>`, aby utworzyć koncentrator o jednoznacznie określonym typie. `ClockHub` został skonfigurowany w klasie `Startup`, aby odpowiadały na żądania w punkcie końcowym `/hubs/clock`.

Aby uzyskać więcej informacji na temat silnych typach centrów, zobacz [Korzystanie z centrów w SignalR ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Ta funkcja nie jest ograniczona do klasy [centrum >\<](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Każda klasa, która dziedziczy z [koncentratora](xref:Microsoft.AspNetCore.SignalR.Hub), na przykład [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), również będzie działała.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Interfejs używany przez silnie wpisaną `ClockHub` jest interfejsem `IClock`.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a>Wywoływanie centrum SignalR z poziomu usługi w tle

Podczas uruchamiania Klasa `Worker`, `BackgroundService`, jest przewodna przy użyciu `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Ponieważ SignalR jest również koncentratorem w fazie `Startup`, w którym każde centrum jest dołączone do pojedynczego punktu końcowego w potoku żądania HTTP ASP.NET Core, każde centrum jest reprezentowane przez `IHubContext<T>` na serwerze. Za pomocą ASP.NET Core DI Features, inne klasy tworzone przez warstwę hostingu, takie jak klasy `BackgroundService`, klasy kontrolerów MVC lub modele stron Razor, mogą uzyskać odwołania do centrów po stronie serwera, akceptując wystąpienia `IHubContext<ClockHub, IClock>` podczas konstruowania.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Ponieważ metoda `ExecuteAsync` jest wywoływana iteracyjnie w usłudze w tle, bieżąca data i godzina serwera są wysyłane do podłączonych klientów przy użyciu `ClockHub`.

## <a name="react-to-opno-locsignalr-events-with-background-services"></a>Reagowanie na zdarzenia SignalR z usługami w tle

Podobnie jak w przypadku aplikacji jednostronicowej korzystającej z klienta JavaScript dla SignalR lub aplikacji klasycznej platformy .NET można używać <xref:signalr/dotnet-client>, można także użyć implementacji `BackgroundService` lub `IHostedService`, aby nawiązać połączenie z SignalR centrami i odpowiedzieć na zdarzenia.

Klasa `ClockHubClient` implementuje interfejs `IClock` i interfejs `IHostedService`. W ten sposób można ją rozmieścić podczas `Startup`, aby działać w sposób ciągły i odpowiadać na zdarzenia centrów z serwera.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Podczas inicjowania `ClockHubClient` tworzy wystąpienie `HubConnection` i dzieli metodę `IClock.ShowTime` jako procedurę obsługi dla zdarzenia `ShowTime` centrum.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

W implementacji `IHostedService.StartAsync` `HubConnection` jest uruchamiany asynchronicznie.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Podczas `IHostedService.StopAsync` Metoda `HubConnection` jest usuwana asynchronicznie.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Centra o jednoznacznie określonym typie](xref:signalr/hubs#strongly-typed-hubs)
