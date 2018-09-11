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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="03d7b-103">Klient modelu .NET SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03d7b-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="03d7b-104">Przez [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="03d7b-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="03d7b-105">Biblioteki klienta platformy ASP.NET Core SignalR .NET umożliwia komunikację z koncentratorami SignalR z aplikacji .NET.</span><span class="sxs-lookup"><span data-stu-id="03d7b-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="03d7b-106">Xamarin ma specjalne wymagania dotyczące wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03d7b-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="03d7b-107">Aby uzyskać więcej informacji, zobacz [klienta SignalR 2.1.1 w środowisku Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="03d7b-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="03d7b-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03d7b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="03d7b-109">Przykładowy kod w tym artykule jest aplikacja WPF, które używa klienta platformy ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="03d7b-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="03d7b-110">Zainstaluj pakiet klienta SignalR platformy .NET</span><span class="sxs-lookup"><span data-stu-id="03d7b-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="03d7b-111">`Microsoft.AspNetCore.SignalR.Client` Pakiet jest wymagany dla klientów programu .NET połączyć się z koncentratorami SignalR.</span><span class="sxs-lookup"><span data-stu-id="03d7b-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="03d7b-112">Aby zainstalować bibliotekę klienta, uruchom następujące polecenie **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="03d7b-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="03d7b-113">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="03d7b-113">Connect to a hub</span></span>

<span data-ttu-id="03d7b-114">Aby nawiązać połączenie, należy utworzyć `HubConnectionBuilder` i wywołać `Build`.</span><span class="sxs-lookup"><span data-stu-id="03d7b-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="03d7b-115">Adres URL koncentratora, protokół, typem transportu, poziom dziennika, nagłówki i inne opcje można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="03d7b-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="03d7b-116">Skonfiguruj wymagane opcje, wstawiając `HubConnectionBuilder` metody do `Build`.</span><span class="sxs-lookup"><span data-stu-id="03d7b-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="03d7b-117">Uruchom połączenie przy użyciu `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="03d7b-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="03d7b-118">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="03d7b-118">Call hub methods from client</span></span>

<span data-ttu-id="03d7b-119">`InvokeAsync` wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="03d7b-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="03d7b-120">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="03d7b-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="03d7b-121">SignalR jest asynchroniczna, a więc `async` i `await` podczas nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="03d7b-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="03d7b-122">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="03d7b-122">Call client methods from hub</span></span>

<span data-ttu-id="03d7b-123">Definiowanie metody wywołania koncentratora, za pomocą `connection.On` po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="03d7b-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="03d7b-124">Powyższy kod w `connection.On` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="03d7b-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="03d7b-125">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="03d7b-125">Error handling and logging</span></span>

<span data-ttu-id="03d7b-126">Obsługa błędów przy użyciu instrukcji try-catch.</span><span class="sxs-lookup"><span data-stu-id="03d7b-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="03d7b-127">Sprawdzanie `Exception` obiektu, aby określić odpowiednie działanie do wykonania po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="03d7b-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="03d7b-128">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="03d7b-128">Additional resources</span></span>

* [<span data-ttu-id="03d7b-129">Centra</span><span class="sxs-lookup"><span data-stu-id="03d7b-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="03d7b-130">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="03d7b-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="03d7b-131">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="03d7b-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
