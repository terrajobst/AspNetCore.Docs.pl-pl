---
title: ASP.NET Core SignalR klienta platformy .NET
author: bradygaster
description: Informacje na temat ASP.NET Core SignalR klienta .NET
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/14/2020
no-loc:
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: a9583c9d6df52ff81a402df03e663ccc3847e51f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660042"
---
# <a name="aspnet-core-signalr-net-client"></a>Klient platformy .NET ASP.NET Core sygnalizujący

Biblioteka kliencka ASP.NET Coreowego sygnalizującego platformę .NET umożliwia komunikowanie się z centrami sygnałów z aplikacji .NET.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

Przykładowy kod w tym artykule jest aplikacją WPF korzystającą z klienta programu ASP.NET Core sygnalizującego.

## <a name="install-the-signalr-net-client-package"></a>Instalowanie pakietu klienckiego programu sygnalizującego

Pakiet [Microsoft. AspNetCore. signaler. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) jest wymagany do nawiązania połączenia z centrami sygnałów przez klientów platformy .NET.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Aby zainstalować bibliotekę kliencką, uruchom następujące polecenie w oknie **konsola Menedżera pakietów** :

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Aby zainstalować bibliotekę kliencką, uruchom następujące polecenie w powłoce poleceń:

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Aby nawiązać połączenie, Utwórz `HubConnectionBuilder` i Wywołaj `Build`. Podczas tworzenia połączenia można skonfigurować adres URL centrum, protokół, typ transportu, poziom dziennika, nagłówki i inne opcje. Skonfiguruj wszystkie wymagane opcje, wstawiając dowolne `HubConnectionBuilder` metod do `Build`. Rozpocznij połączenie z `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Obsługa utraconych połączeń

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Automatycznie Połącz ponownie

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> można skonfigurować do automatycznego ponownego nawiązywania połączenia przy użyciu metody `WithAutomaticReconnect` na <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. Domyślnie nie będzie automatycznie ponownie łączyć się.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Bez żadnych parametrów `WithAutomaticReconnect()` konfiguruje klienta tak, aby czekał 0, 2, 10 i 30 sekund przed podjęciem próby ponownego nawiązania połączenia, zatrzymując po czterech nieudanych próbach.

Przed rozpoczęciem dowolnych prób ponownego połączenia `HubConnection` przechodzi do stanu `HubConnectionState.Reconnecting` i wyzwala zdarzenie `Reconnecting`.  Dzięki temu można ostrzec użytkowników, że połączenie zostało utracone i wyłączyć elementy interfejsu użytkownika. Aplikacje nieinteraktywne mogą uruchamiać kolejkowanie lub porzucanie komunikatów.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Jeśli klient pomyślnie ponownie nawiąże połączenie w ramach pierwszych czterech prób, `HubConnection` przejdzie z powrotem do stanu `Connected` i spowoduje wyzwolenie zdarzenia `Reconnected`. Zapewnia to możliwość informowania użytkowników o tym, że połączenie zostało ponownie nawiązane i usuwa wszystkie wiadomości w kolejce.

Ponieważ połączenie jest całkowicie nowe dla serwera, do programów obsługi zdarzeń `Reconnected` zostanie udostępniona nowa `ConnectionId`.

> [!WARNING]
> Parametr `connectionId` programu obsługi zdarzeń `Reconnected` będzie miał wartość null, jeśli `HubConnection` został skonfigurowany do [pomijania negocjacji](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` nie skonfiguruje `HubConnection` w celu ponowienia nieudanych uruchomień początkowych, dlatego należy ręcznie obsługiwać błędy uruchamiania:

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

Jeśli klient nie będzie mógł ponownie nawiązać połączenia w ramach pierwszych czterech prób, `HubConnection` przejdzie do stanu `Disconnected` i uruchomi zdarzenie <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>. Zapewnia to możliwość próby ponownego uruchomienia połączenia ręcznie lub poinformowanie użytkowników, że połączenie zostało trwale utracone.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Aby skonfigurować niestandardową liczbę prób ponownego połączenia przed odłączeniem lub zmianą czasu ponownego połączenia, `WithAutomaticReconnect` akceptuje tablicę liczb reprezentujących opóźnienie (w milisekundach) przed rozpoczęciem każdej próby ponownego nawiązania połączenia.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

Powyższy przykład konfiguruje `HubConnection`, aby rozpocząć próbę ponownego połączenia natychmiast po utracie połączenia. Dotyczy to również konfiguracji domyślnej.

Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego połączenia zostanie również uruchomiona natychmiast, a nie w ciągu 2 sekund, tak jak w przypadku konfiguracji domyślnej.

Jeśli druga próba ponownego połączenia nie powiedzie się, trzecia próba ponownego nawiązania połączenia rozpocznie się w ciągu 10 sekund, co jest ponownie podobne do konfiguracji domyślnej.

Zachowanie niestandardowe jest następnie ponownie niezależne od zachowania domyślnego przez zatrzymanie po trzecim nieudanej próbie połączenia. W konfiguracji domyślnej będzie jeszcze jedna kolejna próba ponownego połączenia w ciągu 30 sekund.

Jeśli potrzebujesz jeszcze większą kontrolę nad chronometrażem i liczbą prób automatycznego ponownego połączenia, `WithAutomaticReconnect` akceptuje obiekt implementujący interfejs `IRetryPolicy`, który ma pojedynczą metodę o nazwie `NextRetryDelay`.

`NextRetryDelay` przyjmuje jeden argument z typem `RetryContext`. `RetryContext` ma trzy właściwości: `PreviousRetryCount`, `ElapsedTime` i `RetryReason`, które są `long`, `TimeSpan` i `Exception` odpowiednio. Przed pierwszą próbą ponownego połączenia oba `PreviousRetryCount` i `ElapsedTime` będą miały wartość zero, a `RetryReason` będzie wyjątek, który spowodował utratę połączenia. Po każdym nieudanej próbie ponowieniu próby `PreviousRetryCount` będzie zwiększane o jeden, `ElapsedTime` zostanie zaktualizowany w celu odzwierciedlenia ilości czasu poświęconego na odłączenie do tej pory, a `RetryReason` będzie wyjątek, który spowodował, że Ostatnia próba ponownego połączenia nie powiedzie się.

`NextRetryDelay` musi zwrócić wartość przedziału reprezentującą czas oczekiwania przed kolejną próbą ponownego połączenia lub `null`, jeśli `HubConnection` powinna zatrzymać Ponowne nawiązywanie połączenia.

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.NextDouble() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

Alternatywnie można napisać kod, który ponownie podłącze klienta ręcznie, jak pokazano w [ręcznym ponownym nawiązaniu połączenia](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Ręczne ponowne łączenie

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Przed 3,0m klient platformy .NET dla SignalR nie będzie automatycznie ponownie łączyć się. Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.

::: moniker-end

Użyj zdarzenia <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>, aby odpowiedzieć na utracone połączenie. Na przykład możesz chcieć zautomatyzować ponowne łączenie.

Zdarzenie `Closed` wymaga delegata zwracającego `Task`, co umożliwia uruchamianie kodu asynchronicznego bez użycia `async void`. Aby spełnić podpis delegata w programie obsługi zdarzeń `Closed`, który jest uruchamiany synchronicznie, zwróć `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Główną przyczyną obsługi asynchronicznej jest to, że można ponownie uruchomić połączenie. Rozpoczęcie połączenia jest akcją asynchroniczną.

W programie obsługi `Closed`, który ponownie uruchamia połączenie, rozważ oczekiwanie na losowe opóźnienie, aby zapobiec przeciążeniu serwera, jak pokazano w następującym przykładzie:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora z klienta

`InvokeAsync` wywołań metod w centrum. Przekaż nazwę metody Hub i wszystkie argumenty zdefiniowane w metodzie Hub, aby `InvokeAsync`. SignalR jest asynchroniczny, dlatego podczas wykonywania wywołań używaj `async` i `await`.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

Metoda `InvokeAsync` zwraca `Task`, która kończy się, gdy metoda serwera zwróci wartość. Wartość zwracana, jeśli istnieje, jest podawana jako wynik `Task`. Wszystkie wyjątki zgłoszone przez metodę na serwerze generują `Task`z błędami. Użyj składni `await`, aby poczekać na zakończenie metody serwera i `try...catch` składni, aby obsłużyć błędy.

Metoda `SendAsync` zwraca `Task`, która kończy się, gdy komunikat został wysłany do serwera. Nie podano wartości zwracanej od momentu, gdy ta `Task` nie czeka na zakończenie metody serwera. Wszystkie wyjątki zgłoszone na kliencie podczas wysyłania komunikatu generują `Task`błędów. Użyj składni `await` i `try...catch`, aby obsłużyć błędy wysyłania.

> [!NOTE]
> Jeśli używasz usługi Azure SignalR w *trybie Bezserwerowym*, nie można wywoływać metod centralnych z poziomu klienta. Aby uzyskać więcej informacji, zobacz [dokumentację usługiSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Zdefiniuj metody wywołania przez centrum przy użyciu `connection.On` po skompilowaniu, ale przed rozpoczęciem połączenia.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Poprzedni kod w `connection.On` jest uruchamiany, gdy kod po stronie serwera wywoła go przy użyciu metody `SendAsync`.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Rejestrowanie i obsługa błędów

Obsługa błędów przy użyciu instrukcji try-catch. Sprawdź obiekt `Exception`, aby określić poprawną akcję, która ma zostać podjęta po wystąpieniu błędu.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient środowiska JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Dokumentacja bezserwerowa usługi SignalR platformy Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
