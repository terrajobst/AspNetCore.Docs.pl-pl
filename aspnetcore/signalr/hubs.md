---
title: Użyj koncentratory w ASP.NET Core SignalR
author: rachelappel
description: Dowiedz się, jak używać koncentratory w ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 5c477dd64c4cf8b7d6da1f121a290b00f3864f45
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="802f8-103">Użyj koncentratory w SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="802f8-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="802f8-104">Przez [Rachel Appel](https://twitter.com/rachelappel) i [Griffin Kevina](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="802f8-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="802f8-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="802f8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="802f8-106">Co to jest Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="802f8-106">What is a SignalR hub</span></span>

<span data-ttu-id="802f8-107">API koncentratorów SignalR można wywoływać metod w połączonych klientów z serwera.</span><span class="sxs-lookup"><span data-stu-id="802f8-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="802f8-108">W kodzie serwera należy zdefiniować metody, które są wywoływane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="802f8-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="802f8-109">W kodzie klienta można zdefiniować metody, które są wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="802f8-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="802f8-110">SignalR odpowiada on za wszystko w tle, które umożliwia komunikacji klient serwer i klient serwera w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="802f8-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="802f8-111">Konfigurowanie koncentratorów SignalR</span><span class="sxs-lookup"><span data-stu-id="802f8-111">Configure SignalR hubs</span></span>

<span data-ttu-id="802f8-112">Oprogramowanie pośredniczące SignalR wymaga niektórych usług, które są konfigurowane przez wywołanie metody `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="802f8-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="802f8-113">Podczas dodawania funkcji SignalR dla aplikacji platformy ASP.NET Core, Konfiguracja trasy SignalR przez wywołanie metody `app.UseSignalR` w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="802f8-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="802f8-114">Tworzenie i używanie koncentratory</span><span class="sxs-lookup"><span data-stu-id="802f8-114">Create and use hubs</span></span>

<span data-ttu-id="802f8-115">Utwórz koncentratora od zadeklarowania klasy, która dziedziczy `Hub`i Dodaj do niej metody publiczne.</span><span class="sxs-lookup"><span data-stu-id="802f8-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="802f8-116">Klientów można wywołać metody, które są zdefiniowane jako `public`.</span><span class="sxs-lookup"><span data-stu-id="802f8-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="802f8-117">Można określić zwracany typ i parametry, łącznie z typami złożonymi i tablic, tak jak w dowolnej metody C#.</span><span class="sxs-lookup"><span data-stu-id="802f8-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="802f8-118">SignalR obsługi serializacji i deserializacji złożone obiekty i tablice parametrów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="802f8-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="802f8-119">Obiekt klientów</span><span class="sxs-lookup"><span data-stu-id="802f8-119">The Clients object</span></span>

<span data-ttu-id="802f8-120">Każde wystąpienie `Hub` klasa ma właściwości o nazwie `Clients` zawiera następujące elementy członkowskie do komunikacji między serwerem a klientem:</span><span class="sxs-lookup"><span data-stu-id="802f8-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="802f8-121">Właściwość</span><span class="sxs-lookup"><span data-stu-id="802f8-121">Property</span></span> | <span data-ttu-id="802f8-122">Opis</span><span class="sxs-lookup"><span data-stu-id="802f8-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="802f8-123">Wywołuje metodę dla wszystkich połączonych klientów</span><span class="sxs-lookup"><span data-stu-id="802f8-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="802f8-124">Wywołuje metodę dla klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="802f8-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="802f8-125">Wywołuje metodę dla wszyscy połączeni klienci oprócz klienta, który wywołał metodę</span><span class="sxs-lookup"><span data-stu-id="802f8-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="802f8-126">Ponadto `Hub.Clients` zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="802f8-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="802f8-127">Metoda</span><span class="sxs-lookup"><span data-stu-id="802f8-127">Method</span></span> | <span data-ttu-id="802f8-128">Opis</span><span class="sxs-lookup"><span data-stu-id="802f8-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="802f8-129">Wywołuje metodę dla wszystkich połączonych klientów z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="802f8-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="802f8-130">Wywołuje metodę dla określonego połączony klient</span><span class="sxs-lookup"><span data-stu-id="802f8-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="802f8-131">Wywołuje metodę dla określonego połączonych klientów</span><span class="sxs-lookup"><span data-stu-id="802f8-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="802f8-132">Wysyła komunikat do wszystkich połączeń w określonej grupie</span><span class="sxs-lookup"><span data-stu-id="802f8-132">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="802f8-133">Wysyła komunikat do wszystkich połączeń w określonej grupie, z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="802f8-133">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="802f8-134">Wysyła komunikat do wielu grup połączeń</span><span class="sxs-lookup"><span data-stu-id="802f8-134">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="802f8-135">Wysyła komunikat do grupy połączeń z wykluczeniem klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="802f8-135">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="802f8-136">Wysyła komunikat do wszystkich połączeń skojarzone z określonym użytkownikiem</span><span class="sxs-lookup"><span data-stu-id="802f8-136">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="802f8-137">Wysyła komunikat do wszystkich połączeń skojarzone z określonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="802f8-137">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="802f8-138">Każda właściwość lub metoda w poprzednich tabelach zwraca obiekt z `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="802f8-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="802f8-139">`SendAsync` Metoda umożliwia podanie nazwy i parametry metody klienta do wywołania.</span><span class="sxs-lookup"><span data-stu-id="802f8-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="802f8-140">Wysyłanie komunikatów do klientów</span><span class="sxs-lookup"><span data-stu-id="802f8-140">Send messages to clients</span></span>

<span data-ttu-id="802f8-141">W celu wykonywania wywołań do określonych klientów, należy użyć właściwości `Clients` obiektu.</span><span class="sxs-lookup"><span data-stu-id="802f8-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="802f8-142">W poniższym przykładzie `SendMessageToCaller` przedstawiono metody wysyłania komunikatu do połączenia, który wywołał metodę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="802f8-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="802f8-143">`SendMessageToGroups` Metoda wysyła komunikat do grupy przechowywane w `List` o nazwie `groups`.</span><span class="sxs-lookup"><span data-stu-id="802f8-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="802f8-144">Obsługa zdarzeń dla połączenia</span><span class="sxs-lookup"><span data-stu-id="802f8-144">Handle events for a connection</span></span>

<span data-ttu-id="802f8-145">Interfejs API koncentratorów SignalR zawiera `OnConnectedAsync` i `OnDisconnectedAsync` metody wirtualne do zarządzania i śledzenia połączeń.</span><span class="sxs-lookup"><span data-stu-id="802f8-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="802f8-146">Zastąpienie `OnConnectedAsync` metody wirtualnej do wykonania akcji, gdy klient nawiąże połączenie z koncentratorem, takie jak dodanie go do grupy.</span><span class="sxs-lookup"><span data-stu-id="802f8-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="802f8-147">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="802f8-147">Handle errors</span></span>

<span data-ttu-id="802f8-148">Wyjątki zgłaszane w metodach koncentratora, z są wysyłane do klienta, który wywołał metodę.</span><span class="sxs-lookup"><span data-stu-id="802f8-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="802f8-149">Na komputerze klienckim JavaScript `invoke` metoda zwraca [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="802f8-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="802f8-150">Gdy klient odbierze błąd obsługi dołączony do promise przy użyciu `catch`, ma ona wywoływana i przekazywane jako JavaScript `Error` obiektu.</span><span class="sxs-lookup"><span data-stu-id="802f8-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="802f8-151">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="802f8-151">Related resources</span></span>

* [<span data-ttu-id="802f8-152">Wprowadzenie do usługi SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="802f8-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="802f8-153">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="802f8-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="802f8-154">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="802f8-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
