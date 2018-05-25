---
title: Klient .NET SignalR platformy ASP.NET Core
author: rachelappel
description: Informacje o kliencie .NET SignalR platformy ASP.NET Core
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/18/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/dotnet-client
ms.openlocfilehash: 412d2362575789f1fb4792940df6d3dd24dbdd5a
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/23/2018
---
# <a name="aspnet-core-signalr-net-client"></a>Klient .NET SignalR platformy ASP.NET Core

Przez [Rachel Appel](http://twitter.com/rachelappel)

Klient ASP.NET Core SignalR .NET może służyć przez aplikacje platformy Xamarin, WPF formularzy systemu Windows, konsoli i .NET Core. Podobnie jak [klienta JavaScript](xref:signalr/javascript-client), klient .NET umożliwia odbieranie i wysyłanie i odbieranie wiadomości do koncentratora, w czasie rzeczywistym.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.

## <a name="setup-client"></a>Ustawienia klienta

`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla platformy .NET klientom na łączenie się z koncentratorami SignalR. Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okno:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i Wywołaj `Build`. Adres URL Centrum, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia. Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metod do `Build`. Uruchom połączenie z `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora od klienta

`InvokeAsync` wywołania metody koncentratora. Przekaż nazwę metody koncentratora i żadnych argumentów w metodzie koncentratora do `InvokeAsync`. SignalR jest asynchroniczne, należy więc `async` i `await` wywołania.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Definiowanie metod koncentratora wywołuje przy użyciu `connection.On` po budynku, ale przed rozpoczęciem połączenia.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Poprzedni kod w `connection.On` jest uruchamiany, gdy kod po stronie serwera wywołuje za pomocą `SendAsync` metody.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Obsługa błędów i rejestrowanie

Obsługa błędów w instrukcji try-catch. Sprawdź `Exception` obiektem, aby określić odpowiednich akcji do wykonania po wystąpieniu błędu.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)