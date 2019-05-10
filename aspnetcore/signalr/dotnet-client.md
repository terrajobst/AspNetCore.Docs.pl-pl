---
title: Klient modelu .NET SignalR platformy ASP.NET Core
author: bradygaster
description: Informacje na temat klienta .NET SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: b59af0f9c84a008f778709970dba2273abdfcd4f
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087715"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="86345-103">Klient modelu .NET SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86345-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="86345-104">Biblioteki klienta platformy ASP.NET Core SignalR .NET umożliwia komunikację z koncentratorami SignalR z aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="86345-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="86345-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="86345-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="86345-106">Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="86345-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="86345-107">Zainstaluj pakiet klienta SignalR platformy .NET</span><span class="sxs-lookup"><span data-stu-id="86345-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="86345-108">`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla klientów programu .NET połączyć się z koncentratorami SignalR.</span><span class="sxs-lookup"><span data-stu-id="86345-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="86345-109">Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="86345-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="86345-110">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="86345-110">Connect to a hub</span></span>

<span data-ttu-id="86345-111">Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i wywołać `Build`.</span><span class="sxs-lookup"><span data-stu-id="86345-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="86345-112">Adres URL koncentratora, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="86345-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="86345-113">Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metody do `Build`.</span><span class="sxs-lookup"><span data-stu-id="86345-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="86345-114">Uruchom połączenie przy użyciu `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="86345-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="86345-115">Obsługa utracono połączenie</span><span class="sxs-lookup"><span data-stu-id="86345-115">Handle lost connection</span></span>

<span data-ttu-id="86345-116">Użyj <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> zdarzenie, aby odpowiedzieć na utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="86345-116">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="86345-117">Na przykład możesz chcieć zautomatyzować ponowne nawiązanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="86345-117">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="86345-118">`Closed` Zdarzeń wymaga delegata, która zwraca `Task`, co umożliwia uruchomienie bez użycia kodu async `async void`.</span><span class="sxs-lookup"><span data-stu-id="86345-118">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="86345-119">Do zaspokojenia w podpisie delegata `Closed` programu obsługi zdarzeń, która działa synchronicznie, zwraca `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="86345-119">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="86345-120">Głównym powodem asynchroniczna pomoc techniczna jest więc będzie można ponownie rozpocząć połączenie.</span><span class="sxs-lookup"><span data-stu-id="86345-120">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="86345-121">Uruchamianie połączenie jest operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="86345-121">Starting a connection is an async action.</span></span>

<span data-ttu-id="86345-122">W `Closed` program obsługi, który uruchamia ponownie połączenie, należy wziąć pod uwagę oczekiwanie na niektórych losowego opóźnienia, aby zapobiec przeciążeniu serwera, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="86345-122">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="86345-123">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="86345-123">Call hub methods from client</span></span>

<span data-ttu-id="86345-124">`InvokeAsync` wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="86345-124">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="86345-125">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="86345-125">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="86345-126">SignalR jest asynchroniczna, a więc `async` i `await` podczas nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="86345-126">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="86345-127">`InvokeAsync` Metoda zwraca `Task` co kończy się po powrocie z metody serwera.</span><span class="sxs-lookup"><span data-stu-id="86345-127">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="86345-128">Wartość zwracana, jeśli znajduje się w wyniku `Task`.</span><span class="sxs-lookup"><span data-stu-id="86345-128">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="86345-129">Wyjątki zgłaszane przez metodę na serwerze generuje uszkodzoną `Task`.</span><span class="sxs-lookup"><span data-stu-id="86345-129">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="86345-130">Użyj `await` składni oczekiwania na ukończenie metody serwera i `try...catch` składni, aby obsługiwać błędy.</span><span class="sxs-lookup"><span data-stu-id="86345-130">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="86345-131">`SendAsync` Metoda zwraca `Task` co kończy, gdy wiadomość została wysłana do serwera.</span><span class="sxs-lookup"><span data-stu-id="86345-131">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="86345-132">Nie zwraca wartości znajduje się od to `Task` nie czeka na zakończenie metody serwera.</span><span class="sxs-lookup"><span data-stu-id="86345-132">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="86345-133">Wyjątki zgłaszane na kliencie podczas wysyłania komunikatu utworzenia uszkodzoną `Task`.</span><span class="sxs-lookup"><span data-stu-id="86345-133">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="86345-134">Użyj `await` i `try...catch` składnię, aby obsługiwać błędy wysyłania.</span><span class="sxs-lookup"><span data-stu-id="86345-134">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="86345-135">Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta.</span><span class="sxs-lookup"><span data-stu-id="86345-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="86345-136">Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="86345-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="86345-137">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="86345-137">Call client methods from hub</span></span>

<span data-ttu-id="86345-138">Definiowanie metody wywołania koncentratora, za pomocą `connection.On` po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="86345-138">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="86345-139">Powyższy kod w `connection.On` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="86345-139">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="86345-140">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="86345-140">Error handling and logging</span></span>

<span data-ttu-id="86345-141">Obsługa błędów przy użyciu instrukcji try-catch.</span><span class="sxs-lookup"><span data-stu-id="86345-141">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="86345-142">Sprawdzanie `Exception` obiektu, aby określić odpowiednie działanie do wykonania po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="86345-142">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="86345-143">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="86345-143">Additional resources</span></span>

* [<span data-ttu-id="86345-144">Centra</span><span class="sxs-lookup"><span data-stu-id="86345-144">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="86345-145">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="86345-145">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="86345-146">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="86345-146">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="86345-147">Dokumentacja usługi Azure SignalR Service bez użycia serwera</span><span class="sxs-lookup"><span data-stu-id="86345-147">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
