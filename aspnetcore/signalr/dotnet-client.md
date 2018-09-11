---
title: Klient modelu .NET SignalR platformy ASP.NET Core
author: tdykstra
description: Informacje na temat klienta .NET SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373322"
---
# <a name="aspnet-core-signalr-net-client"></a>Klient modelu .NET SignalR platformy ASP.NET Core

Przez [Rachel Appel](http://twitter.com/rachelappel)

Biblioteki klienta platformy ASP.NET Core SignalR .NET umożliwia komunikację z koncentratorami SignalR z aplikacji .NET.

> [!NOTE]
> Xamarin ma specjalne wymagania dotyczące wersji programu Visual Studio. Aby uzyskać więcej informacji, zobacz [klienta SignalR 2.1.1 w środowisku Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.

## <a name="install-the-signalr-net-client-package"></a>Zainstaluj pakiet klienta SignalR platformy .NET

`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla klientów programu .NET połączyć się z koncentratorami SignalR. Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okna:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i wywołać `Build`. Adres URL koncentratora, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia. Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metody do `Build`. Uruchom połączenie przy użyciu `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora z klienta

`InvokeAsync` wywołania metody koncentratora. Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `InvokeAsync`. SignalR jest asynchroniczna, a więc `async` i `await` podczas nawiązywania połączeń.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Definiowanie metody wywołania koncentratora, za pomocą `connection.On` po kompilacji, ale przed rozpoczęciem połączenia.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Powyższy kod w `connection.On` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą `SendAsync` metody.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Rejestrowanie i obsługa błędów

Obsługa błędów przy użyciu instrukcji try-catch. Sprawdzanie `Exception` obiektu, aby określić odpowiednie działanie do wykonania po wystąpieniu błędu.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
