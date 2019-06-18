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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="cbfd3-103">Klient modelu .NET SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cbfd3-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="cbfd3-104">Biblioteki klienta platformy ASP.NET Core SignalR .NET umożliwia komunikację z koncentratorami SignalR z aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="cbfd3-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cbfd3-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cbfd3-106">Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="cbfd3-107">Zainstaluj pakiet klienta SignalR platformy .NET</span><span class="sxs-lookup"><span data-stu-id="cbfd3-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="cbfd3-108">`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla klientów programu .NET połączyć się z koncentratorami SignalR.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="cbfd3-109">Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="cbfd3-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="cbfd3-110">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="cbfd3-110">Connect to a hub</span></span>

<span data-ttu-id="cbfd3-111">Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i wywołać `Build`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="cbfd3-112">Adres URL koncentratora, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="cbfd3-113">Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metody do `Build`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="cbfd3-114">Uruchom połączenie przy użyciu `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="cbfd3-115">Obsługa utracono połączenie</span><span class="sxs-lookup"><span data-stu-id="cbfd3-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="cbfd3-116">Automatyczne ponowne łączenie</span><span class="sxs-lookup"><span data-stu-id="cbfd3-116">Automatically reconnect</span></span>

<span data-ttu-id="cbfd3-117"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> Można skonfigurować, aby automatycznie ponownie połączyć się przy użyciu `WithAutomaticReconnect` metody <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="cbfd3-118">Nie będzie automatycznie ponownie się domyślnie.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="cbfd3-119">Bez żadnych parametrów `WithAutomaticReconnect()` konfiguruje klienta w celu odczekaj 0, 2, 10 i 30 sekund, odpowiednio, przed próbą każdą próbę ponownego nawiązania połączenia, trwa zatrzymywanie po czterech nieudanych próbach.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="cbfd3-120">Przed rozpoczęciem wszelkich prób ponownego połączenia `HubConnection` spowoduje przejście do `HubConnectionState.Reconnecting` stanu i szybko `Reconnecting` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="cbfd3-121">Zapewnia to możliwość ostrzegać użytkowników, że połączenie zostało utracone i wyłączający elementy interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="cbfd3-122">Nieinterakcyjne aplikacje można uruchomić usługi kolejkowania wiadomości lub usuwaniem komunikatów.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="cbfd3-123">Jeśli klient pomyślnie połączy się ponownie w ramach jego pierwsze cztery prób `HubConnection` przejdą do `Connected` stanu i szybko `Reconnected` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="cbfd3-124">Dzięki temu można informować użytkowników nawiązaniem połączenia kolejce i pobierać wszystkie wiadomości w kolejce.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="cbfd3-125">Ponieważ połączenia całkowicie nowych odwołuje się do serwera, nowy `ConnectionId` zostanie udzielona `Reconnected` procedury obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="cbfd3-126">`Reconnected` Obsługi zdarzeń `connectionId` parametr będzie równy null Jeśli `HubConnection` został skonfigurowany do [pominąć negocjacji](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="cbfd3-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="cbfd3-127">`WithAutomaticReconnect()` nie Konfiguruj `HubConnection` próbę uruchomienia początkowego błędów, więc błędy start muszą być obsługiwani ręcznie:</span><span class="sxs-lookup"><span data-stu-id="cbfd3-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="cbfd3-128">Jeśli klient nie pomyślnie ponownie połączyć w ramach jego pierwsze cztery prób `HubConnection` spowoduje przejście do `Disconnected` stanu i szybko <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="cbfd3-129">Zapewnia to możliwość próby nawiązania połączenia należy ręcznie uruchomić ponownie, lub poinformować użytkowników, że połączenie zostało trwale utracone.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="cbfd3-130">Aby można było skonfigurować niestandardowe liczbę prób ponownego połączenia przed rozłączeniem lub zmienić czas ponownego nawiązania połączenia `WithAutomaticReconnect` akceptuje tablicy liczb reprezentujący opóźnienie (w milisekundach) oczekiwania przed uruchomieniem każdą próbę ponownego połączenia.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="cbfd3-131">Poprzedni przykład konfiguruje `HubConnection` można uruchomić próby Ponowne podłączenia natychmiast, po połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="cbfd3-132">Dotyczy to również w konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="cbfd3-133">Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego nawiązania połączenia również rozpocznie się natychmiast zamiast czekać na 2 sekundy, tak jak miałoby to miejsce w konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="cbfd3-134">Jeśli druga próba ponownego połączenia nie powiedzie się, trzeci próba ponownego nawiązania połączenia zostanie uruchomiona w ciągu 10 sekund, które jest ponownie, takich jak konfiguracja domyślna.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="cbfd3-135">Niestandardowe zachowanie następnie diverges ponownie z zachowania domyślnego, zatrzymując po trzecie reconnect próby awarii.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="cbfd3-136">W domyślnej konfiguracji będzie można jeden bardziej ponownie próbę innego 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="cbfd3-137">Jeśli chcesz, aby jeszcze bardziej kontrolować czas i liczba automatyczne ponowne łączenie prób `WithAutomaticReconnect` akceptuje implementacji obiektu `IRetryPolicy` interfejs, który zawiera jedną metodę o nazwie `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="cbfd3-138">`NextRetryDelay` przyjmuje jeden argument o typie `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="cbfd3-139">`RetryContext` Ma trzy właściwości: `PreviousRetryCount`, `ElapsedTime` i `RetryReason` służą do `long`, `TimeSpan` i `Exception` odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="cbfd3-140">Przed pierwsza próba ponownego nawiązania połączenia zarówno `PreviousRetryCount` i `ElapsedTime` będzie mieć wartość zero, a `RetryReason` będzie wyjątek, który spowodował połączenie zostanie utracone.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="cbfd3-141">Po każdej próbie ponawiania nie powiodło się `PreviousRetryCount` jest zwiększany o jeden, `ElapsedTime` zostanie zaktualizowany, aby odzwierciedlić czas poświęcony na ponowne łączenie do tej pory i `RetryReason` będzie wyjątek, który spowodował ostatnia próba ponownego połączenia nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="cbfd3-142">`NextRetryDelay` musi zwracać albo element TimeSpan reprezentujący czas oczekiwania przed kolejnym próby ponownego nawiązania połączenia lub `null` Jeśli `HubConnection` ma zostać zatrzymana, ponowne łączenie.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="cbfd3-143">Alternatywnie, można napisać kod, który zostanie nawiązana ponownie ręcznie, jak pokazano w kliencie [ręcznie połączyć](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="cbfd3-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="cbfd3-144">Ręcznie połączyć</span><span class="sxs-lookup"><span data-stu-id="cbfd3-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="cbfd3-145">Przed 3.0 nie automatycznie ponownie klienta .NET dla elementu SignalR.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="cbfd3-146">Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="cbfd3-147">Użyj <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> zdarzenie, aby odpowiedzieć na utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="cbfd3-148">Na przykład możesz chcieć zautomatyzować ponowne nawiązanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="cbfd3-149">`Closed` Zdarzeń wymaga delegata, która zwraca `Task`, co umożliwia uruchomienie bez użycia kodu async `async void`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="cbfd3-150">Do zaspokojenia w podpisie delegata `Closed` programu obsługi zdarzeń, która działa synchronicznie, zwraca `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="cbfd3-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="cbfd3-151">Głównym powodem asynchroniczna pomoc techniczna jest więc będzie można ponownie rozpocząć połączenie.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="cbfd3-152">Uruchamianie połączenie jest operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="cbfd3-153">W `Closed` program obsługi, który uruchamia ponownie połączenie, należy wziąć pod uwagę oczekiwanie na niektórych losowego opóźnienia, aby zapobiec przeciążeniu serwera, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cbfd3-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="cbfd3-154">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="cbfd3-154">Call hub methods from client</span></span>

<span data-ttu-id="cbfd3-155">`InvokeAsync` wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="cbfd3-156">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="cbfd3-157">SignalR jest asynchroniczna, a więc `async` i `await` podczas nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="cbfd3-158">`InvokeAsync` Metoda zwraca `Task` co kończy się po powrocie z metody serwera.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="cbfd3-159">Wartość zwracana, jeśli znajduje się w wyniku `Task`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="cbfd3-160">Wyjątki zgłaszane przez metodę na serwerze generuje uszkodzoną `Task`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="cbfd3-161">Użyj `await` składni oczekiwania na ukończenie metody serwera i `try...catch` składni, aby obsługiwać błędy.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="cbfd3-162">`SendAsync` Metoda zwraca `Task` co kończy, gdy wiadomość została wysłana do serwera.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="cbfd3-163">Nie zwraca wartości znajduje się od to `Task` nie czeka na zakończenie metody serwera.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="cbfd3-164">Wyjątki zgłaszane na kliencie podczas wysyłania komunikatu utworzenia uszkodzoną `Task`.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="cbfd3-165">Użyj `await` i `try...catch` składnię, aby obsługiwać błędy wysyłania.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="cbfd3-166">Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="cbfd3-167">Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="cbfd3-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="cbfd3-168">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="cbfd3-168">Call client methods from hub</span></span>

<span data-ttu-id="cbfd3-169">Definiowanie metody wywołania koncentratora, za pomocą `connection.On` po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="cbfd3-170">Powyższy kod w `connection.On` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="cbfd3-171">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="cbfd3-171">Error handling and logging</span></span>

<span data-ttu-id="cbfd3-172">Obsługa błędów przy użyciu instrukcji try-catch.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="cbfd3-173">Sprawdzanie `Exception` obiektu, aby określić odpowiednie działanie do wykonania po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="cbfd3-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="cbfd3-174">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cbfd3-174">Additional resources</span></span>

* [<span data-ttu-id="cbfd3-175">Centra</span><span class="sxs-lookup"><span data-stu-id="cbfd3-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="cbfd3-176">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="cbfd3-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="cbfd3-177">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="cbfd3-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="cbfd3-178">Dokumentacja usługi Azure SignalR Service bez użycia serwera</span><span class="sxs-lookup"><span data-stu-id="cbfd3-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
