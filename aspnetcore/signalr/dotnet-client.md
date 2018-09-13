---
title: Klient modelu .NET SignalR platformy ASP.NET Core
author: tdykstra
description: Informacje na temat klienta .NET SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ef84ede2ed45ddc3b64d4ce8f5bd0018a681faf6
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749324"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="89c84-103">Klient modelu .NET SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89c84-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="89c84-104">Biblioteki klienta platformy ASP.NET Core SignalR .NET umożliwia komunikację z koncentratorami SignalR z aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="89c84-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="89c84-105">Xamarin ma specjalne wymagania dotyczące wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89c84-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="89c84-106">Aby uzyskać więcej informacji, zobacz [klienta SignalR 2.1.1 w środowisku Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="89c84-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="89c84-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="89c84-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="89c84-108">Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="89c84-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="89c84-109">Zainstaluj pakiet klienta SignalR platformy .NET</span><span class="sxs-lookup"><span data-stu-id="89c84-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="89c84-110">`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla klientów programu .NET połączyć się z koncentratorami SignalR.</span><span class="sxs-lookup"><span data-stu-id="89c84-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="89c84-111">Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="89c84-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="89c84-112">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="89c84-112">Connect to a hub</span></span>

<span data-ttu-id="89c84-113">Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i wywołać `Build`.</span><span class="sxs-lookup"><span data-stu-id="89c84-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="89c84-114">Adres URL koncentratora, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="89c84-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="89c84-115">Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metody do `Build`.</span><span class="sxs-lookup"><span data-stu-id="89c84-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="89c84-116">Uruchom połączenie przy użyciu `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="89c84-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="89c84-117">Obsługa utracono połączenie</span><span class="sxs-lookup"><span data-stu-id="89c84-117">Handle lost connection</span></span>

<span data-ttu-id="89c84-118">Użyj <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> zdarzenie, aby odpowiedzieć na utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="89c84-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="89c84-119">Na przykład możesz chcieć zautomatyzować ponowne nawiązanie połączenia.</span><span class="sxs-lookup"><span data-stu-id="89c84-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="89c84-120">`Closed` Zdarzeń wymaga delegata, która zwraca `Task`, co umożliwia uruchomienie bez użycia kodu async `async void`.</span><span class="sxs-lookup"><span data-stu-id="89c84-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="89c84-121">Do zaspokojenia w podpisie delegata `Closed` programu obsługi zdarzeń, która działa synchronicznie, zwraca `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="89c84-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="89c84-122">Głównym powodem asynchroniczna pomoc techniczna jest więc będzie można ponownie rozpocząć połączenie.</span><span class="sxs-lookup"><span data-stu-id="89c84-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="89c84-123">Uruchamianie połączenie jest operacji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="89c84-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="89c84-124">W `Closed` program obsługi, który uruchamia ponownie połączenie, należy wziąć pod uwagę oczekiwanie na niektórych losowego opóźnienia, aby zapobiec przeciążeniu serwera, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="89c84-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="89c84-125">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="89c84-125">Call hub methods from client</span></span>

<span data-ttu-id="89c84-126">`InvokeAsync` wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="89c84-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="89c84-127">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="89c84-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="89c84-128">SignalR jest asynchroniczna, a więc `async` i `await` podczas nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="89c84-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="89c84-129">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="89c84-129">Call client methods from hub</span></span>

<span data-ttu-id="89c84-130">Definiowanie metody wywołania koncentratora, za pomocą `connection.On` po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="89c84-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="89c84-131">Powyższy kod w `connection.On` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="89c84-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="89c84-132">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="89c84-132">Error handling and logging</span></span>

<span data-ttu-id="89c84-133">Obsługa błędów przy użyciu instrukcji try-catch.</span><span class="sxs-lookup"><span data-stu-id="89c84-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="89c84-134">Sprawdzanie `Exception` obiektu, aby określić odpowiednie działanie do wykonania po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="89c84-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="89c84-135">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="89c84-135">Additional resources</span></span>

* [<span data-ttu-id="89c84-136">Centra</span><span class="sxs-lookup"><span data-stu-id="89c84-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="89c84-137">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="89c84-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="89c84-138">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="89c84-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
