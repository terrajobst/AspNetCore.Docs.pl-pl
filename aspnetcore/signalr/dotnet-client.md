---
title: Klient modelu .NET SignalR platformy ASP.NET Core
author: bradygaster
description: Informacje na temat klienta .NET SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153121"
---
# <a name="aspnet-core-signalr-net-client"></a>Klient modelu .NET SignalR platformy ASP.NET Core

Biblioteki klienta platformy ASP.NET Core SignalR .NET umożliwia komunikację z koncentratorami SignalR z aplikacji .NET.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

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

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Automatyczne ponowne łączenie

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> Można skonfigurować, aby automatycznie ponownie połączyć się przy użyciu `WithAutomaticReconnect` metody <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. Nie będzie automatycznie ponownie się domyślnie.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Bez żadnych parametrów `WithAutomaticReconnect()` konfiguruje klienta w celu odczekaj 0, 2, 10 i 30 sekund, odpowiednio, przed próbą każdą próbę ponownego nawiązania połączenia, trwa zatrzymywanie po czterech nieudanych próbach.

Przed rozpoczęciem wszelkich prób ponownego połączenia `HubConnection` spowoduje przejście do `HubConnectionState.Reconnecting` stanu i szybko `Reconnecting` zdarzeń.  Zapewnia to możliwość ostrzegać użytkowników, że połączenie zostało utracone i wyłączający elementy interfejsu użytkownika. Nieinterakcyjne aplikacje można uruchomić usługi kolejkowania wiadomości lub usuwaniem komunikatów.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Jeśli klient pomyślnie połączy się ponownie w ramach jego pierwsze cztery prób `HubConnection` przejdą do `Connected` stanu i szybko `Reconnected` zdarzeń. Dzięki temu można informować użytkowników nawiązaniem połączenia kolejce i pobierać wszystkie wiadomości w kolejce.

Ponieważ połączenia całkowicie nowych odwołuje się do serwera, nowy `ConnectionId` zostanie udzielona `Reconnected` procedury obsługi zdarzeń.

> [!WARNING]
> `Reconnected` Obsługi zdarzeń `connectionId` parametr będzie równy null Jeśli `HubConnection` został skonfigurowany do [pominąć negocjacji](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` nie Konfiguruj `HubConnection` próbę uruchomienia początkowego błędów, więc błędy start muszą być obsługiwani ręcznie:

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

Jeśli klient nie pomyślnie ponownie połączyć w ramach jego pierwsze cztery prób `HubConnection` spowoduje przejście do `Disconnected` stanu i szybko <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> zdarzeń. Zapewnia to możliwość próby nawiązania połączenia należy ręcznie uruchomić ponownie, lub poinformować użytkowników, że połączenie zostało trwale utracone.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Aby można było skonfigurować niestandardowe liczbę prób ponownego połączenia przed rozłączeniem lub zmienić czas ponownego nawiązania połączenia `WithAutomaticReconnect` akceptuje tablicy liczb reprezentujący opóźnienie (w milisekundach) oczekiwania przed uruchomieniem każdą próbę ponownego połączenia.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

Poprzedni przykład konfiguruje `HubConnection` można uruchomić próby Ponowne podłączenia natychmiast, po połączenie zostanie przerwane. Dotyczy to również w konfiguracji domyślnej.

Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego nawiązania połączenia również rozpocznie się natychmiast zamiast czekać na 2 sekundy, tak jak miałoby to miejsce w konfiguracji domyślnej.

Jeśli druga próba ponownego połączenia nie powiedzie się, trzeci próba ponownego nawiązania połączenia zostanie uruchomiona w ciągu 10 sekund, które jest ponownie, takich jak konfiguracja domyślna.

Niestandardowe zachowanie następnie diverges ponownie z zachowania domyślnego, zatrzymując po trzecie reconnect próby awarii. W domyślnej konfiguracji będzie można jeden bardziej ponownie próbę innego 30 sekund.

Jeśli chcesz, aby jeszcze bardziej kontrolować czas i liczba automatyczne ponowne łączenie prób `WithAutomaticReconnect` akceptuje implementacji obiektu `IRetryPolicy` interfejs, który zawiera jedną metodę o nazwie `NextRetryDelay`.

`NextRetryDelay` przyjmuje jeden argument o typie `RetryContext`. `RetryContext` Ma trzy właściwości: `PreviousRetryCount`, `ElapsedTime` i `RetryReason` służą do `long`, `TimeSpan` i `Exception` odpowiednio. Przed pierwsza próba ponownego nawiązania połączenia zarówno `PreviousRetryCount` i `ElapsedTime` będzie mieć wartość zero, a `RetryReason` będzie wyjątek, który spowodował połączenie zostanie utracone. Po każdej próbie ponawiania nie powiodło się `PreviousRetryCount` jest zwiększany o jeden, `ElapsedTime` zostanie zaktualizowany, aby odzwierciedlić czas poświęcony na ponowne łączenie do tej pory i `RetryReason` będzie wyjątek, który spowodował ostatnia próba ponownego połączenia nie powiedzie się.

`NextRetryDelay` musi zwracać albo element TimeSpan reprezentujący czas oczekiwania przed kolejnym próby ponownego nawiązania połączenia lub `null` Jeśli `HubConnection` ma zostać zatrzymana, ponowne łączenie.

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
            return TimeSpan.FromSeconds(_random.Next() * 10);
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

Alternatywnie, można napisać kod, który zostanie nawiązana ponownie ręcznie, jak pokazano w kliencie [ręcznie połączyć](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Ręcznie połączyć

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Przed 3.0 nie automatycznie ponownie klienta .NET dla elementu SignalR. Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.

::: moniker-end

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

`InvokeAsync` Metoda zwraca `Task` co kończy się po powrocie z metody serwera. Wartość zwracana, jeśli znajduje się w wyniku `Task`. Wyjątki zgłaszane przez metodę na serwerze generuje uszkodzoną `Task`. Użyj `await` składni oczekiwania na ukończenie metody serwera i `try...catch` składni, aby obsługiwać błędy.

`SendAsync` Metoda zwraca `Task` co kończy, gdy wiadomość została wysłana do serwera. Nie zwraca wartości znajduje się od to `Task` nie czeka na zakończenie metody serwera. Wyjątki zgłaszane na kliencie podczas wysyłania komunikatu utworzenia uszkodzoną `Task`. Użyj `await` i `try...catch` składnię, aby obsługiwać błędy wysyłania.

> [!NOTE]
> Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta. Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

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
* [Dokumentacja usługi Azure SignalR Service bez użycia serwera](/azure/azure-signalr/signalr-concept-serverless-development-config)
