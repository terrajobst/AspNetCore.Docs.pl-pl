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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="b949d-103">Klient .NET SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b949d-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="b949d-104">Przez [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b949d-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="b949d-105">Klient ASP.NET Core SignalR .NET może służyć przez aplikacje platformy Xamarin, WPF formularzy systemu Windows, konsoli i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b949d-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="b949d-106">Podobnie jak [klienta JavaScript](xref:signalr/javascript-client), klient .NET umożliwia odbieranie i wysyłanie i odbieranie wiadomości do koncentratora, w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="b949d-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="b949d-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b949d-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b949d-108">Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="b949d-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="setup-client"></a><span data-ttu-id="b949d-109">Ustawienia klienta</span><span class="sxs-lookup"><span data-stu-id="b949d-109">Setup client</span></span>

<span data-ttu-id="b949d-110">`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla platformy .NET klientom na łączenie się z koncentratorami SignalR.</span><span class="sxs-lookup"><span data-stu-id="b949d-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="b949d-111">Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okno:</span><span class="sxs-lookup"><span data-stu-id="b949d-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="b949d-112">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="b949d-112">Connect to a hub</span></span>

<span data-ttu-id="b949d-113">Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i Wywołaj `Build`.</span><span class="sxs-lookup"><span data-stu-id="b949d-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="b949d-114">Adres URL Centrum, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="b949d-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="b949d-115">Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metod do `Build`.</span><span class="sxs-lookup"><span data-stu-id="b949d-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="b949d-116">Uruchom połączenie z `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="b949d-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="b949d-117">Wywoływanie metod koncentratora od klienta</span><span class="sxs-lookup"><span data-stu-id="b949d-117">Call hub methods from client</span></span>

<span data-ttu-id="b949d-118">`InvokeAsync` wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="b949d-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="b949d-119">Przekaż nazwę metody koncentratora i żadnych argumentów w metodzie koncentratora do `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="b949d-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="b949d-120">SignalR jest asynchroniczne, należy więc `async` i `await` wywołania.</span><span class="sxs-lookup"><span data-stu-id="b949d-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="b949d-121">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="b949d-121">Call client methods from hub</span></span>

<span data-ttu-id="b949d-122">Definiowanie metod koncentratora wywołuje przy użyciu `connection.On` po budynku, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="b949d-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="b949d-123">Poprzedni kod w `connection.On` jest uruchamiany, gdy kod po stronie serwera wywołuje za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="b949d-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="b949d-124">Obsługa błędów i rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="b949d-124">Error handling and logging</span></span>

<span data-ttu-id="b949d-125">Obsługa błędów w instrukcji try-catch.</span><span class="sxs-lookup"><span data-stu-id="b949d-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="b949d-126">Sprawdź `Exception` obiektem, aby określić odpowiednich akcji do wykonania po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="b949d-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="b949d-127">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b949d-127">Additional resources</span></span>

* [<span data-ttu-id="b949d-128">Centra</span><span class="sxs-lookup"><span data-stu-id="b949d-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b949d-129">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="b949d-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b949d-130">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="b949d-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)