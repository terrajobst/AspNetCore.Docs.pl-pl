---
title: Klient modelu .NET SignalR platformy ASP.NET Core
author: tdykstra
description: Informacje na temat klienta .NET SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655255"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="2ab77-103">Klient modelu .NET SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ab77-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="2ab77-104">Przez [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="2ab77-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="2ab77-105">Klient programu ASP.NET Core SignalR .NET może być używany przez aplikacje platformy Xamarin, WPF, Windows Forms, konsola i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ab77-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="2ab77-106">Podobnie jak [klienta JavaScript](xref:signalr/javascript-client), klient modelu .NET umożliwia odbieranie i wysyłanie i odbieranie komunikatów w Centrum w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="2ab77-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="2ab77-107">Xamarin ma specjalne wymagania dotyczące wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2ab77-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="2ab77-108">Aby uzyskać więcej informacji, zobacz [klienta SignalR 2.1.1 w środowisku Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="2ab77-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="2ab77-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ab77-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2ab77-110">Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="2ab77-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="2ab77-111">Zainstaluj pakiet klienta SignalR platformy .NET</span><span class="sxs-lookup"><span data-stu-id="2ab77-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="2ab77-112">`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla klientów programu .NET połączyć się z koncentratorami SignalR.</span><span class="sxs-lookup"><span data-stu-id="2ab77-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="2ab77-113">Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="2ab77-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="2ab77-114">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="2ab77-114">Connect to a hub</span></span>

<span data-ttu-id="2ab77-115">Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i wywołać `Build`.</span><span class="sxs-lookup"><span data-stu-id="2ab77-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="2ab77-116">Adres URL koncentratora, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="2ab77-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="2ab77-117">Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metody do `Build`.</span><span class="sxs-lookup"><span data-stu-id="2ab77-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="2ab77-118">Uruchom połączenie przy użyciu `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="2ab77-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="2ab77-119">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="2ab77-119">Call hub methods from client</span></span>

<span data-ttu-id="2ab77-120">`InvokeAsync` wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="2ab77-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="2ab77-121">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="2ab77-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="2ab77-122">SignalR jest asynchroniczna, a więc `async` i `await` podczas nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="2ab77-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="2ab77-123">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="2ab77-123">Call client methods from hub</span></span>

<span data-ttu-id="2ab77-124">Definiowanie metody wywołania koncentratora, za pomocą `connection.On` po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="2ab77-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="2ab77-125">Powyższy kod w `connection.On` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="2ab77-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="2ab77-126">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="2ab77-126">Error handling and logging</span></span>

<span data-ttu-id="2ab77-127">Obsługa błędów przy użyciu instrukcji try-catch.</span><span class="sxs-lookup"><span data-stu-id="2ab77-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="2ab77-128">Sprawdzanie `Exception` obiektu, aby określić odpowiednie działanie do wykonania po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="2ab77-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="2ab77-129">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2ab77-129">Additional resources</span></span>

* [<span data-ttu-id="2ab77-130">Centra</span><span class="sxs-lookup"><span data-stu-id="2ab77-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="2ab77-131">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="2ab77-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="2ab77-132">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="2ab77-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
