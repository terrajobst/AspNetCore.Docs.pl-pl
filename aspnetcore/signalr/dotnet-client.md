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
ms.openlocfilehash: 39d9eccdb1e0457b177e75e6f94f3dd185b0093d
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146319"
---
# <a name="aspnet-core-opno-locsignalr-net-client"></a><span data-ttu-id="58478-103">ASP.NET Core SignalR klienta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="58478-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="58478-104">ASP.NET Core SignalR .NET Client Library umożliwia komunikowanie się z koncentratorami SignalR z poziomu aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="58478-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="58478-105">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="58478-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="58478-106">Przykładowy kod w tym artykule jest aplikacją WPF korzystającą z ASP.NET Core SignalR klienta platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="58478-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-opno-locsignalr-net-client-package"></a><span data-ttu-id="58478-107">Zainstaluj pakiet klienta programu SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="58478-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="58478-108">[Microsoft.AspNetCore.SignalR.](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)Do łączenia się z centrami SignalR jest wymagany pakiet klienta programu .NET.</span><span class="sxs-lookup"><span data-stu-id="58478-108">The [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package is required for .NET clients to connect to SignalR hubs.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="58478-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58478-109">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="58478-110">Aby zainstalować bibliotekę kliencką, uruchom następujące polecenie w oknie **konsola Menedżera pakietów** :</span><span class="sxs-lookup"><span data-stu-id="58478-110">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="58478-111">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="58478-111">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="58478-112">Aby zainstalować bibliotekę kliencką, uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="58478-112">To install the client library, run the following command in a command shell:</span></span>

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a><span data-ttu-id="58478-113">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="58478-113">Connect to a hub</span></span>

<span data-ttu-id="58478-114">Aby nawiązać połączenie, Utwórz `HubConnectionBuilder` i Wywołaj `Build`.</span><span class="sxs-lookup"><span data-stu-id="58478-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="58478-115">Podczas tworzenia połączenia można skonfigurować adres URL centrum, protokół, typ transportu, poziom dziennika, nagłówki i inne opcje.</span><span class="sxs-lookup"><span data-stu-id="58478-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="58478-116">Skonfiguruj wszystkie wymagane opcje, wstawiając dowolne `HubConnectionBuilder` metod do `Build`.</span><span class="sxs-lookup"><span data-stu-id="58478-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="58478-117">Rozpocznij połączenie z `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="58478-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="58478-118">Obsługa utraconych połączeń</span><span class="sxs-lookup"><span data-stu-id="58478-118">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="58478-119">Automatycznie Połącz ponownie</span><span class="sxs-lookup"><span data-stu-id="58478-119">Automatically reconnect</span></span>

<span data-ttu-id="58478-120"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> można skonfigurować do automatycznego ponownego nawiązywania połączenia przy użyciu metody `WithAutomaticReconnect` na <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="58478-120">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="58478-121">Domyślnie nie będzie automatycznie ponownie łączyć się.</span><span class="sxs-lookup"><span data-stu-id="58478-121">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="58478-122">Bez żadnych parametrów `WithAutomaticReconnect()` konfiguruje klienta tak, aby czekał 0, 2, 10 i 30 sekund przed podjęciem próby ponownego nawiązania połączenia, zatrzymując po czterech nieudanych próbach.</span><span class="sxs-lookup"><span data-stu-id="58478-122">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="58478-123">Przed rozpoczęciem dowolnych prób ponownego połączenia `HubConnection` przechodzi do stanu `HubConnectionState.Reconnecting` i wyzwala zdarzenie `Reconnecting`.</span><span class="sxs-lookup"><span data-stu-id="58478-123">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="58478-124">Dzięki temu można ostrzec użytkowników, że połączenie zostało utracone i wyłączyć elementy interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="58478-124">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="58478-125">Aplikacje nieinteraktywne mogą uruchamiać kolejkowanie lub porzucanie komunikatów.</span><span class="sxs-lookup"><span data-stu-id="58478-125">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="58478-126">Jeśli klient pomyślnie ponownie nawiąże połączenie w ramach pierwszych czterech prób, `HubConnection` przejdzie z powrotem do stanu `Connected` i spowoduje wyzwolenie zdarzenia `Reconnected`.</span><span class="sxs-lookup"><span data-stu-id="58478-126">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="58478-127">Zapewnia to możliwość informowania użytkowników o tym, że połączenie zostało ponownie nawiązane i usuwa wszystkie wiadomości w kolejce.</span><span class="sxs-lookup"><span data-stu-id="58478-127">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="58478-128">Ponieważ połączenie jest całkowicie nowe dla serwera, do programów obsługi zdarzeń `Reconnected` zostanie udostępniona nowa `ConnectionId`.</span><span class="sxs-lookup"><span data-stu-id="58478-128">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="58478-129">Parametr `connectionId` programu obsługi zdarzeń `Reconnected` będzie miał wartość null, jeśli `HubConnection` został skonfigurowany do [pomijania negocjacji](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="58478-129">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="58478-130">`WithAutomaticReconnect()` nie skonfiguruje `HubConnection` w celu ponowienia nieudanych uruchomień początkowych, dlatego należy ręcznie obsługiwać błędy uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="58478-130">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="58478-131">Jeśli klient nie będzie mógł ponownie nawiązać połączenia w ramach pierwszych czterech prób, `HubConnection` przejdzie do stanu `Disconnected` i uruchomi zdarzenie <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>.</span><span class="sxs-lookup"><span data-stu-id="58478-131">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="58478-132">Zapewnia to możliwość próby ponownego uruchomienia połączenia ręcznie lub poinformowanie użytkowników, że połączenie zostało trwale utracone.</span><span class="sxs-lookup"><span data-stu-id="58478-132">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="58478-133">Aby skonfigurować niestandardową liczbę prób ponownego połączenia przed odłączeniem lub zmianą czasu ponownego połączenia, `WithAutomaticReconnect` akceptuje tablicę liczb reprezentujących opóźnienie (w milisekundach) przed rozpoczęciem każdej próby ponownego nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="58478-133">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="58478-134">Powyższy przykład konfiguruje `HubConnection`, aby rozpocząć próbę ponownego połączenia natychmiast po utracie połączenia.</span><span class="sxs-lookup"><span data-stu-id="58478-134">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="58478-135">Dotyczy to również konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="58478-135">This is also true for the default configuration.</span></span>

<span data-ttu-id="58478-136">Jeśli pierwsza próba ponownego połączenia nie powiedzie się, druga próba ponownego połączenia zostanie również uruchomiona natychmiast, a nie w ciągu 2 sekund, tak jak w przypadku konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="58478-136">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="58478-137">Jeśli druga próba ponownego połączenia nie powiedzie się, trzecia próba ponownego nawiązania połączenia rozpocznie się w ciągu 10 sekund, co jest ponownie podobne do konfiguracji domyślnej.</span><span class="sxs-lookup"><span data-stu-id="58478-137">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="58478-138">Zachowanie niestandardowe jest następnie ponownie niezależne od zachowania domyślnego przez zatrzymanie po trzecim nieudanej próbie połączenia.</span><span class="sxs-lookup"><span data-stu-id="58478-138">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="58478-139">W konfiguracji domyślnej będzie jeszcze jedna kolejna próba ponownego połączenia w ciągu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="58478-139">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="58478-140">Jeśli potrzebujesz jeszcze większą kontrolę nad chronometrażem i liczbą prób automatycznego ponownego połączenia, `WithAutomaticReconnect` akceptuje obiekt implementujący interfejs `IRetryPolicy`, który ma pojedynczą metodę o nazwie `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="58478-140">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="58478-141">`NextRetryDelay` przyjmuje jeden argument z typem `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="58478-141">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="58478-142">`RetryContext` ma trzy właściwości: `PreviousRetryCount`, `ElapsedTime` i `RetryReason`, które są `long`, `TimeSpan` i `Exception` odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="58478-142">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason`, which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="58478-143">Przed pierwszą próbą ponownego połączenia oba `PreviousRetryCount` i `ElapsedTime` będą miały wartość zero, a `RetryReason` będzie wyjątek, który spowodował utratę połączenia.</span><span class="sxs-lookup"><span data-stu-id="58478-143">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="58478-144">Po każdym nieudanej próbie ponowieniu próby `PreviousRetryCount` będzie zwiększane o jeden, `ElapsedTime` zostanie zaktualizowany w celu odzwierciedlenia ilości czasu poświęconego na odłączenie do tej pory, a `RetryReason` będzie wyjątek, który spowodował, że Ostatnia próba ponownego połączenia nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="58478-144">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="58478-145">`NextRetryDelay` musi zwrócić wartość przedziału reprezentującą czas oczekiwania przed kolejną próbą ponownego połączenia lub `null`, jeśli `HubConnection` powinna zatrzymać Ponowne nawiązywanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="58478-145">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="58478-146">Alternatywnie można napisać kod, który ponownie podłącze klienta ręcznie, jak pokazano w [ręcznym ponownym nawiązaniu połączenia](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="58478-146">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="58478-147">Ręczne ponowne łączenie</span><span class="sxs-lookup"><span data-stu-id="58478-147">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="58478-148">Przed 3,0m klient platformy .NET dla SignalR nie będzie automatycznie ponownie łączyć się.</span><span class="sxs-lookup"><span data-stu-id="58478-148">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="58478-149">Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta.</span><span class="sxs-lookup"><span data-stu-id="58478-149">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="58478-150">Użyj zdarzenia <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>, aby odpowiedzieć na utracone połączenie.</span><span class="sxs-lookup"><span data-stu-id="58478-150">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="58478-151">Na przykład możesz chcieć zautomatyzować ponowne łączenie.</span><span class="sxs-lookup"><span data-stu-id="58478-151">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="58478-152">Zdarzenie `Closed` wymaga delegata zwracającego `Task`, co umożliwia uruchamianie kodu asynchronicznego bez użycia `async void`.</span><span class="sxs-lookup"><span data-stu-id="58478-152">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="58478-153">Aby spełnić podpis delegata w programie obsługi zdarzeń `Closed`, który jest uruchamiany synchronicznie, zwróć `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="58478-153">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="58478-154">Główną przyczyną obsługi asynchronicznej jest to, że można ponownie uruchomić połączenie.</span><span class="sxs-lookup"><span data-stu-id="58478-154">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="58478-155">Rozpoczęcie połączenia jest akcją asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="58478-155">Starting a connection is an async action.</span></span>

<span data-ttu-id="58478-156">W programie obsługi `Closed`, który ponownie uruchamia połączenie, rozważ oczekiwanie na losowe opóźnienie, aby zapobiec przeciążeniu serwera, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="58478-156">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="58478-157">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="58478-157">Call hub methods from client</span></span>

<span data-ttu-id="58478-158">`InvokeAsync` wywołań metod w centrum.</span><span class="sxs-lookup"><span data-stu-id="58478-158">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="58478-159">Przekaż nazwę metody Hub i wszystkie argumenty zdefiniowane w metodzie Hub, aby `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="58478-159">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> SignalR<span data-ttu-id="58478-160"> jest asynchroniczny, dlatego podczas wykonywania wywołań używaj `async` i `await`.</span><span class="sxs-lookup"><span data-stu-id="58478-160"> is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="58478-161">Metoda `InvokeAsync` zwraca `Task`, która kończy się, gdy metoda serwera zwróci wartość.</span><span class="sxs-lookup"><span data-stu-id="58478-161">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="58478-162">Wartość zwracana, jeśli istnieje, jest podawana jako wynik `Task`.</span><span class="sxs-lookup"><span data-stu-id="58478-162">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="58478-163">Wszystkie wyjątki zgłoszone przez metodę na serwerze generują `Task`z błędami.</span><span class="sxs-lookup"><span data-stu-id="58478-163">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="58478-164">Użyj składni `await`, aby poczekać na zakończenie metody serwera i `try...catch` składni, aby obsłużyć błędy.</span><span class="sxs-lookup"><span data-stu-id="58478-164">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="58478-165">Metoda `SendAsync` zwraca `Task`, która kończy się, gdy komunikat został wysłany do serwera.</span><span class="sxs-lookup"><span data-stu-id="58478-165">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="58478-166">Nie podano wartości zwracanej od momentu, gdy ta `Task` nie czeka na zakończenie metody serwera.</span><span class="sxs-lookup"><span data-stu-id="58478-166">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="58478-167">Wszystkie wyjątki zgłoszone na kliencie podczas wysyłania komunikatu generują `Task`błędów.</span><span class="sxs-lookup"><span data-stu-id="58478-167">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="58478-168">Użyj składni `await` i `try...catch`, aby obsłużyć błędy wysyłania.</span><span class="sxs-lookup"><span data-stu-id="58478-168">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="58478-169">Jeśli używasz usługi Azure SignalR w *trybie Bezserwerowym*, nie można wywoływać metod centralnych z poziomu klienta.</span><span class="sxs-lookup"><span data-stu-id="58478-169">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="58478-170">Aby uzyskać więcej informacji, zobacz [dokumentację usługiSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="58478-170">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="58478-171">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="58478-171">Call client methods from hub</span></span>

<span data-ttu-id="58478-172">Zdefiniuj metody wywołania przez centrum przy użyciu `connection.On` po skompilowaniu, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="58478-172">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="58478-173">Poprzedni kod w `connection.On` jest uruchamiany, gdy kod po stronie serwera wywoła go przy użyciu metody `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="58478-173">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="58478-174">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="58478-174">Error handling and logging</span></span>

<span data-ttu-id="58478-175">Obsługa błędów przy użyciu instrukcji try-catch.</span><span class="sxs-lookup"><span data-stu-id="58478-175">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="58478-176">Sprawdź obiekt `Exception`, aby określić poprawną akcję, która ma zostać podjęta po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="58478-176">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="58478-177">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="58478-177">Additional resources</span></span>

* [<span data-ttu-id="58478-178">Centra</span><span class="sxs-lookup"><span data-stu-id="58478-178">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="58478-179">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="58478-179">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="58478-180">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="58478-180">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* <span data-ttu-id="58478-181">[Dokumentacja bezserwerowa usługi SignalR platformy Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="58478-181">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
