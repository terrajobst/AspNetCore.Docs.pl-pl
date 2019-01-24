---
title: Klient modelu .NET SignalR platformy ASP.NET Core
author: bradygaster
description: Informacje na temat klienta .NET SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 25b618f7a424b217c0fb55417754ea358280b95a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836309"
---
# <a name="aspnet-core-signalr-net-client"></a>Klient modelu .NET SignalR platformy ASP.NET Core

Biblioteki klienta platformy ASP.NET Core SignalR .NET umożliwia komunikację z koncentratorami SignalR z aplikacji .NET.

> [!NOTE]
> Xamarin ma specjalne wymagania dotyczące wersji programu Visual Studio. Aby uzyskać więcej informacji, zobacz [klienta SignalR 2.1.1 w środowisku Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.

## <a name="install-the-signalr-net-client-package"></a>Zainstaluj pakiet klienta SignalR platformy .NET

`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla klientów programu .NET połączyć się z koncentratorami SignalR. Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okna:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i wywołać `Build`. Adres URL koncentratora, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia. Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metody do `Build`. Uruchom połączenie przy użyciu `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Obsługa utracono połączenie

Użyj <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> zdarzenie, aby odpowiedzieć na utracono połączenie. Na przykład możesz chcieć zautomatyzować ponowne nawiązanie połączenia.

`Closed` Zdarzeń wymaga delegata, która zwraca `Task`, co umożliwia uruchomienie bez użycia kodu async `async void`. Do zaspokojenia w podpisie delegata `Closed` programu obsługi zdarzeń, która działa synchronicznie, zwraca `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Głównym powodem asynchroniczna pomoc techniczna jest więc będzie można ponownie rozpocząć połączenie. Uruchamianie połączenie jest operacji asynchronicznej.

W `Closed` program obsługi, który uruchamia ponownie połączenie, należy wziąć pod uwagę oczekiwanie na niektórych losowego opóźnienia, aby zapobiec przeciążeniu serwera, jak pokazano w poniższym przykładzie:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

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
