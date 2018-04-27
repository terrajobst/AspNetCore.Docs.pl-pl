---
title: Użyj koncentratory w ASP.NET Core SignalR
author: rachelappel
description: Dowiedz się, jak używać koncentratory w ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 7da0c4832b1aa6a844172bf751a46b280a02f37a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="99ef7-103">Użyj koncentratory w SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99ef7-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="99ef7-104">Przez [Rachel Appel](https://twitter.com/rachelappel) i [Griffin Kevina](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="99ef7-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="99ef7-105">Co to jest Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="99ef7-105">What is a SignalR hub</span></span>

<span data-ttu-id="99ef7-106">API koncentratorów SignalR można wywoływać metod w połączonych klientów z serwera.</span><span class="sxs-lookup"><span data-stu-id="99ef7-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="99ef7-107">W kodzie serwera należy zdefiniować metody, które są wywoływane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="99ef7-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="99ef7-108">W kodzie klienta można zdefiniować metody, które są wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="99ef7-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="99ef7-109">SignalR odpowiada on za wszystko w tle, które umożliwia komunikacji klient serwer i klient serwera w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="99ef7-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="99ef7-110">Konfigurowanie koncentratorów SignalR</span><span class="sxs-lookup"><span data-stu-id="99ef7-110">Configure SignalR hubs</span></span>

<span data-ttu-id="99ef7-111">Oprogramowanie pośredniczące SignalR wymaga niektórych usług, które są konfigurowane przez wywołanie metody `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="99ef7-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="99ef7-112">Podczas dodawania funkcji SignalR dla aplikacji platformy ASP.NET Core, Konfiguracja trasy SignalR przez wywołanie metody `app.UseSignalR` w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="99ef7-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="99ef7-113">Tworzenie i używanie koncentratory</span><span class="sxs-lookup"><span data-stu-id="99ef7-113">Create and use hubs</span></span>

<span data-ttu-id="99ef7-114">Utwórz koncentratora od zadeklarowania klasy, która dziedziczy `Hub`i Dodaj do niej metody publiczne.</span><span class="sxs-lookup"><span data-stu-id="99ef7-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="99ef7-115">Klientów można wywołać metody, które są zdefiniowane jako `public`.</span><span class="sxs-lookup"><span data-stu-id="99ef7-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="99ef7-116">Można określić zwracany typ i parametry, łącznie z typami złożonymi i tablic, tak jak w dowolnej metody C#.</span><span class="sxs-lookup"><span data-stu-id="99ef7-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="99ef7-117">SignalR obsługi serializacji i deserializacji złożone obiekty i tablice parametrów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="99ef7-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="99ef7-118">Obiekt klientów</span><span class="sxs-lookup"><span data-stu-id="99ef7-118">The Clients object</span></span>

<span data-ttu-id="99ef7-119">Każde wystąpienie `Hub` klasa ma właściwości o nazwie `Clients` zawiera następujące elementy członkowskie do komunikacji między serwerem a klientem:</span><span class="sxs-lookup"><span data-stu-id="99ef7-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="99ef7-120">Właściwość</span><span class="sxs-lookup"><span data-stu-id="99ef7-120">Property</span></span> | <span data-ttu-id="99ef7-121">Opis</span><span class="sxs-lookup"><span data-stu-id="99ef7-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="99ef7-122">Wywołuje metodę dla wszystkich połączonych klientów</span><span class="sxs-lookup"><span data-stu-id="99ef7-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="99ef7-123">Wywołuje metodę dla klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="99ef7-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="99ef7-124">Wywołuje metodę dla wszyscy połączeni klienci oprócz klienta, który wywołał metodę</span><span class="sxs-lookup"><span data-stu-id="99ef7-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="99ef7-125">Ponadto `Hub` klasa zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="99ef7-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="99ef7-126">Metoda</span><span class="sxs-lookup"><span data-stu-id="99ef7-126">Method</span></span> | <span data-ttu-id="99ef7-127">Opis</span><span class="sxs-lookup"><span data-stu-id="99ef7-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="99ef7-128">Wywołuje metodę dla wszystkich połączonych klientów z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="99ef7-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="99ef7-129">Wywołuje metodę dla określonego połączony klient</span><span class="sxs-lookup"><span data-stu-id="99ef7-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="99ef7-130">Wywołuje metodę dla określonego połączonych klientów</span><span class="sxs-lookup"><span data-stu-id="99ef7-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="99ef7-131">Wysyła komunikat do wszystkich połączeń w określonej grupie</span><span class="sxs-lookup"><span data-stu-id="99ef7-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="99ef7-132">Wysyła komunikat do wszystkich połączeń w określonej grupie, z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="99ef7-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="99ef7-133">Wysyła komunikat do wielu grup połączeń</span><span class="sxs-lookup"><span data-stu-id="99ef7-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="99ef7-134">Wysyła komunikat do grupy połączeń z wykluczeniem klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="99ef7-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="99ef7-135">Wysyła komunikat do wszystkich połączeń skojarzone z określonym użytkownikiem</span><span class="sxs-lookup"><span data-stu-id="99ef7-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="99ef7-136">Wysyła komunikat do wszystkich połączeń skojarzone z określonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="99ef7-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="99ef7-137">Każda właściwość lub metoda w poprzednich tabelach zwraca obiekt z `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="99ef7-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="99ef7-138">`SendAsync` Metoda umożliwia podanie nazwy i parametry metody klienta do wywołania.</span><span class="sxs-lookup"><span data-stu-id="99ef7-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="99ef7-139">Wysyłanie komunikatów do klientów</span><span class="sxs-lookup"><span data-stu-id="99ef7-139">Send messages to clients</span></span>

<span data-ttu-id="99ef7-140">W celu wykonywania wywołań do określonych klientów, należy użyć właściwości `Clients` obiektu.</span><span class="sxs-lookup"><span data-stu-id="99ef7-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="99ef7-141">W poniższym przykładzie poniżej `SendMessageToCaller` przedstawiono metody wysyłania komunikatu do połączenia, który wywołał metodę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="99ef7-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="99ef7-142">`SendMessageToGroups` Metoda wysyła komunikat do grupy przechowywane w `List` o nazwie `groups`.</span><span class="sxs-lookup"><span data-stu-id="99ef7-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="99ef7-143">Obsługa zdarzeń dla połączenia</span><span class="sxs-lookup"><span data-stu-id="99ef7-143">Handle events for a connection</span></span>

<span data-ttu-id="99ef7-144">Interfejs API koncentratorów SignalR zawiera `OnConnectedAsync` i `OnDisconnectedAsync` metody wirtualne do zarządzania i śledzenia połączeń.</span><span class="sxs-lookup"><span data-stu-id="99ef7-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="99ef7-145">Zastąpienie `OnConnectedAsync` metody wirtualnej do wykonania akcji, gdy klient nawiąże połączenie z koncentratorem, takie jak dodanie go do grupy.</span><span class="sxs-lookup"><span data-stu-id="99ef7-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="99ef7-146">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="99ef7-146">Handle errors</span></span>

<span data-ttu-id="99ef7-147">Wyjątki zgłaszane w metodach koncentratora, z są wysyłane do klienta, który wywołał metodę.</span><span class="sxs-lookup"><span data-stu-id="99ef7-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="99ef7-148">Na komputerze klienckim JavaScript `invoke` metoda zwraca [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="99ef7-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="99ef7-149">Gdy klient odbierze błąd obsługi dołączony do promise przy użyciu `catch`, ma ona wywoływana i przekazywane jako JavaScript `Error` obiektu.</span><span class="sxs-lookup"><span data-stu-id="99ef7-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="99ef7-150">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="99ef7-150">Related resources</span></span>

[<span data-ttu-id="99ef7-151">Wprowadzenie do usługi SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99ef7-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)