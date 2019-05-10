---
title: Host platformy ASP.NET Core SignalR w usługi w tle
author: bradygaster
description: Dowiedz się, jak wysyłać komunikaty do klientów SignalR z klas .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: dcd62f0c7056a3f987291b6c8bb8b87f94160865
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087767"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Host platformy ASP.NET Core SignalR w usługi w tle

Przez [Brady'ego Gastera](https://twitter.com/bradygaster)

Ten artykuł zawiera wskazówki dotyczące:

* Hosting centrów SignalR przy użyciu procesu roboczego tła hostowanych za pomocą programu ASP.NET Core.
* Wysyłanie komunikatów do połączonych klientów z w ramach platformy .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Podłączanie SignalR podczas uruchamiania

Hosting centrów SignalR programu ASP.NET Core w ramach procesu roboczego tła jest taka sama jak hostowania Centrum w aplikacji sieci web platformy ASP.NET Core. W `Startup.ConfigureServices` metody, wywoływania `services.AddSignalR` dodaje wymagane usługi do warstwy platformy ASP.NET Core zależności iniekcji (DI) do obsługi SignalR. W `Startup.Configure`, `UseSignalR` metoda jest wywoływana, aby przewodowy się punkty końcowe: koncentratora, w potoku żądania programu ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

W powyższym przykładzie `ClockHub` klasy implementuje `Hub<T>` klasy, aby utworzyć silnie typizowaną koncentratora. `ClockHub` Został skonfigurowany w `Startup` klasy, aby odpowiadać na żądania w punkcie końcowym `/hubs/clock`.

Aby uzyskać więcej informacji na silnie typizowaną Hubs, zobacz [dla platformy ASP.NET Core przy użyciu hubs w SignalR](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Ta funkcja nie jest ograniczona do [Centrum\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) klasy. Każda klasa, która dziedziczy z [Centrum](xref:Microsoft.AspNetCore.SignalR.Hub), takich jak [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), również będą działać.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

Interfejs używany przez silnie typizowaną `ClockHub` jest `IClock` interfejsu.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Wywoływanie Centrum SignalR z usługę w tle

Podczas uruchamiania `Worker` klasy `BackgroundService`, przewodowej przy użyciu `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Ponieważ SignalR jest również przewodowej podczas `Startup` fazy, w którym każdy Centrum jest dołączony do poszczególnych punktu końcowego w Potok żądań HTTP platformy ASP.NET Core, każdego Centrum jest reprezentowany przez `IHubContext<T>` na serwerze. Za pomocą platformy ASP.NET Core DI funkcji, innych klas, tworzone przez warstwę obsługi, takie jak `BackgroundService` klasy, klasy kontrolera MVC lub modele strony Razor, można uzyskać odwołania do koncentratorów po stronie serwera, akceptując wystąpień `IHubContext<ClockHub, IClock>` podczas konstruowania.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Jako `ExecuteAsync` wywoływana jest metoda iteracyjne usługi w tle, bieżącą datę i godzinę serwera są wysyłane do połączonych klientów przy użyciu `ClockHub`.

## <a name="react-to-signalr-events-with-background-services"></a>Reagowanie na zdarzenia SignalR przy użyciu usługi w tle

Podobnie jak aplikacja jednostronicowa przy użyciu klienta języka JavaScript dla biblioteki SignalR lub aplikacji klasycznej platformy .NET można zrobić za pomocą modułu za pomocą polecenia <xref:signalr/dotnet-client>, `BackgroundService` lub `IHostedService` implementacji można również nawiązać połączenie z centrów SignalR i reagowania na zdarzenia.

`ClockHubClient` Klasa implementuje interfejsy `IClock` interfejsu i `IHostedService` interfejsu. W ten sposób mogą być połączone się podczas `Startup` wykonuj ciągle i reagowania na zdarzenia Centrum z serwera. 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Podczas inicjowania `ClockHubClient` tworzy wystąpienie `HubConnection` i tworzącej powiązania `IClock.ShowTime` metodę jako program obsługi Centrum `ShowTime` zdarzeń.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

W `IHostedService.StartAsync` implementacji `HubConnection` jest uruchamiany asynchronicznie.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Podczas `IHostedService.StopAsync` metody `HubConnection` jest usunięty asynchronicznie.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Koncentratory silnie typizowane](xref:signalr/hubs#strongly-typed-hubs)
