---
title: Przy użyciu hubs w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać koncentratory w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: 17e3ee23967bc1097a3121b3e3e5b58cebe3887d
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538365"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="86fcf-103">Na użytek koncentratory w SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86fcf-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="86fcf-104">Przez [Rachel Appel](https://twitter.com/rachelappel) i [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="86fcf-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="86fcf-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="86fcf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="86fcf-106">Co to jest Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="86fcf-106">What is a SignalR hub</span></span>

<span data-ttu-id="86fcf-107">Interfejsu API centrów SignalR umożliwia wywoływanie metod na komputerach klienckich połączonych z serwera.</span><span class="sxs-lookup"><span data-stu-id="86fcf-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="86fcf-108">W kodzie serwera, można zdefiniować metody, które są wywoływane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="86fcf-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="86fcf-109">Kod klienta służy do definiowania metod, które są wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="86fcf-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="86fcf-110">SignalR zajmuje się wszystkiego w tle, które sprawia, że w czasie rzeczywistym komunikacji klient serwer i klient serwera jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="86fcf-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="86fcf-111">Konfigurowanie centrów SignalR</span><span class="sxs-lookup"><span data-stu-id="86fcf-111">Configure SignalR hubs</span></span>

<span data-ttu-id="86fcf-112">Oprogramowanie pośredniczące SignalR wymaga pewnych usług, które zostały skonfigurowane przez wywołanie metody `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="86fcf-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="86fcf-113">Podczas dodawania funkcji SignalR do aplikacji ASP.NET Core, należy skonfigurować trasy SignalR, wywołując `app.UseSignalR` w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="86fcf-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="86fcf-114">Tworzenie i używanie koncentratory</span><span class="sxs-lookup"><span data-stu-id="86fcf-114">Create and use hubs</span></span>

<span data-ttu-id="86fcf-115">Utwórz koncentrator od zadeklarowania klasy, która dziedziczy `Hub`i Dodaj metody publiczne do niego.</span><span class="sxs-lookup"><span data-stu-id="86fcf-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="86fcf-116">Klienci mogą wywoływać metody, które są zdefiniowane jako `public`.</span><span class="sxs-lookup"><span data-stu-id="86fcf-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="86fcf-117">Można określić zwracany typ i parametry, w tym typy złożone i tablice, tak jak w dowolnej metody języka C#.</span><span class="sxs-lookup"><span data-stu-id="86fcf-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="86fcf-118">SignalR obsługi serializacji i deserializacji obiektu złożonego obiekty i tablice parametrów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="86fcf-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="86fcf-119">Obiekt kontekstu</span><span class="sxs-lookup"><span data-stu-id="86fcf-119">The Context object</span></span>

<span data-ttu-id="86fcf-120">`Hub` Klasa ma `Context` właściwość, która zawiera następujące właściwości z informacjami o połączeniu:</span><span class="sxs-lookup"><span data-stu-id="86fcf-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="86fcf-121">Właściwość</span><span class="sxs-lookup"><span data-stu-id="86fcf-121">Property</span></span> | <span data-ttu-id="86fcf-122">Opis</span><span class="sxs-lookup"><span data-stu-id="86fcf-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="86fcf-123">Pobiera unikatowy identyfikator połączenia, przypisany przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="86fcf-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="86fcf-124">Istnieje jeden identyfikator połączenia dla każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="86fcf-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="86fcf-125">Pobiera [identyfikator użytkownika](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="86fcf-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="86fcf-126">Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonych z tym połączeniem jako identyfikator użytkownika.</span><span class="sxs-lookup"><span data-stu-id="86fcf-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="86fcf-127">Pobiera `ClaimsPrincipal` skojarzone z bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="86fcf-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="86fcf-128">Pobiera kolekcję kluczy/wartości, który może służyć do udostępniania danych w zakresie tego połączenia.</span><span class="sxs-lookup"><span data-stu-id="86fcf-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="86fcf-129">Dane mogą być przechowywane w tej kolekcji, a jej będzie zachowywane dla połączenia między różnych metod koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="86fcf-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="86fcf-130">Pobiera kolekcję funkcji dostępnych w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="86fcf-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="86fcf-131">Na razie ta kolekcja nie jest potrzebny w większości przypadków, więc nie jest on jeszcze opisane szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="86fcf-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="86fcf-132">Pobiera `CancellationToken` , otrzyma powiadomienie, gdy połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="86fcf-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="86fcf-133">`Hub.Context` zawiera również następujące metody:</span><span class="sxs-lookup"><span data-stu-id="86fcf-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="86fcf-134">Metoda</span><span class="sxs-lookup"><span data-stu-id="86fcf-134">Method</span></span> | <span data-ttu-id="86fcf-135">Opis</span><span class="sxs-lookup"><span data-stu-id="86fcf-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="86fcf-136">Zwraca `HttpContext` dla połączenia lub `null` Jeśli połączenie nie jest skojarzony z żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="86fcf-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="86fcf-137">W przypadku połączeń HTTP można użyć tej metody, aby uzyskać informacje, takie jak nagłówki HTTP i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="86fcf-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="86fcf-138">Przerywa połączenie.</span><span class="sxs-lookup"><span data-stu-id="86fcf-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="86fcf-139">Obiekt klientów</span><span class="sxs-lookup"><span data-stu-id="86fcf-139">The Clients object</span></span>

<span data-ttu-id="86fcf-140">`Hub` Klasa ma `Clients` właściwość, która zawiera następujące właściwości dla komunikacji między serwerem a klientem:</span><span class="sxs-lookup"><span data-stu-id="86fcf-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="86fcf-141">Właściwość</span><span class="sxs-lookup"><span data-stu-id="86fcf-141">Property</span></span> | <span data-ttu-id="86fcf-142">Opis</span><span class="sxs-lookup"><span data-stu-id="86fcf-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="86fcf-143">Wywołuje metodę dla wszystkich podłączonych klientów</span><span class="sxs-lookup"><span data-stu-id="86fcf-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="86fcf-144">Wywołuje metodę dla klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="86fcf-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="86fcf-145">Wywołuje metodę dla wszyscy połączeni klienci oprócz klienta, który wywołał metodę</span><span class="sxs-lookup"><span data-stu-id="86fcf-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="86fcf-146">`Hub.Clients` zawiera również następujące metody:</span><span class="sxs-lookup"><span data-stu-id="86fcf-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="86fcf-147">Metoda</span><span class="sxs-lookup"><span data-stu-id="86fcf-147">Method</span></span> | <span data-ttu-id="86fcf-148">Opis</span><span class="sxs-lookup"><span data-stu-id="86fcf-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="86fcf-149">Wywołuje metodę dla wszystkich podłączonych klientów z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="86fcf-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="86fcf-150">Wywołuje metodę dla określonego klienta połączone</span><span class="sxs-lookup"><span data-stu-id="86fcf-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="86fcf-151">Wywołuje metodę dla określonych klientów połączonych</span><span class="sxs-lookup"><span data-stu-id="86fcf-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="86fcf-152">Wywołuje metodę do wszystkich połączeń w określonej grupie</span><span class="sxs-lookup"><span data-stu-id="86fcf-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="86fcf-153">Wywołuje metodę do wszystkich połączeń w określonej grupie, z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="86fcf-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="86fcf-154">Wywołuje metodę do wielu grup połączeń</span><span class="sxs-lookup"><span data-stu-id="86fcf-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="86fcf-155">Wywołuje metodę do grupy połączeń z wykluczeniem klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="86fcf-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="86fcf-156">Wywołuje metodę, aby wszystkie połączenia skojarzone z określonym użytkownikiem</span><span class="sxs-lookup"><span data-stu-id="86fcf-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="86fcf-157">Wywołuje metodę, aby wszystkie połączenia skojarzone z określonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="86fcf-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="86fcf-158">Każda właściwość lub metoda w poprzednich tabelach zwraca obiekt z `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="86fcf-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="86fcf-159">`SendAsync` Metoda umożliwia podanie nazwy i parametry metody klienta do wywołania.</span><span class="sxs-lookup"><span data-stu-id="86fcf-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="86fcf-160">Wysyłanie komunikatów do klientów</span><span class="sxs-lookup"><span data-stu-id="86fcf-160">Send messages to clients</span></span>

<span data-ttu-id="86fcf-161">Aby wykonywać wywołania określonych klientów, należy użyć właściwości `Clients` obiektu.</span><span class="sxs-lookup"><span data-stu-id="86fcf-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="86fcf-162">W poniższym przykładzie `SendMessageToCaller` metoda pokazuje wysyłania komunikatu do połączenia, który wywołał metodę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="86fcf-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="86fcf-163">`SendMessageToGroups` Metoda wysyła komunikat do grup, przechowywane w `List` o nazwie `groups`.</span><span class="sxs-lookup"><span data-stu-id="86fcf-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="86fcf-164">Koncentratory silnie typizowane</span><span class="sxs-lookup"><span data-stu-id="86fcf-164">Strongly typed hubs</span></span>

<span data-ttu-id="86fcf-165">Wadą korzystania z `SendAsync` jest, że opiera się na ciąg magiczny, aby określić metodę klienta do wywołania.</span><span class="sxs-lookup"><span data-stu-id="86fcf-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="86fcf-166">Spowoduje to pozostawienie Otwórz kod błędów czasu wykonywania, jeśli nazwa metody została błędnie napisana lub brak od klienta.</span><span class="sxs-lookup"><span data-stu-id="86fcf-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="86fcf-167">Alternatywa dla użycia `SendAsync` polega na wpisaniu silnie `Hub` z <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="86fcf-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="86fcf-168">W poniższym przykładzie `ChatHub` metody klienta zostały wyodrębnione na zewnątrz do interfejs o nazwie `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="86fcf-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="86fcf-169">Ten interfejs może służyć do refaktoryzacji poprzednim `ChatHub` przykład.</span><span class="sxs-lookup"><span data-stu-id="86fcf-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="86fcf-170">Za pomocą `Hub<IChatClient>` umożliwia w czasie kompilacji sprawdzania metody klienta.</span><span class="sxs-lookup"><span data-stu-id="86fcf-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="86fcf-171">Zapobiega to problem spowodował za pomocą magic ciągów, ponieważ `Hub<T>` tylko można zapewnić dostęp do metody zdefiniowane w interfejsie.</span><span class="sxs-lookup"><span data-stu-id="86fcf-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="86fcf-172">Za pomocą silnie typizowanej `Hub<T>` wyłącza możliwość używania `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="86fcf-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="86fcf-173">Obsługa zdarzeń dla połączenia</span><span class="sxs-lookup"><span data-stu-id="86fcf-173">Handle events for a connection</span></span>

<span data-ttu-id="86fcf-174">Interfejs API centrów SignalR udostępnia `OnConnectedAsync` i `OnDisconnectedAsync` metod wirtualnych, do zarządzania i śledzenia połączeń.</span><span class="sxs-lookup"><span data-stu-id="86fcf-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="86fcf-175">Zastąp `OnConnectedAsync` metody wirtualnej do wykonania akcji, gdy klient nawiąże połączenie z koncentratorem, takie jak dodanie go do grupy.</span><span class="sxs-lookup"><span data-stu-id="86fcf-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="86fcf-176">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="86fcf-176">Handle errors</span></span>

<span data-ttu-id="86fcf-177">Wyjątki zgłaszane w Twoich metodach koncentratora są wysyłane do klienta, który wywołał metodę.</span><span class="sxs-lookup"><span data-stu-id="86fcf-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="86fcf-178">Na kliencie JavaScript `invoke` metoda zwraca [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="86fcf-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="86fcf-179">Gdy klient odbierze błąd związany z programu obsługi dołączone do przy użyciu promise `catch`, ma ona wywoływana i przekazywane jako plik JavaScript `Error` obiektu.</span><span class="sxs-lookup"><span data-stu-id="86fcf-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="86fcf-180">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="86fcf-180">Related resources</span></span>

* [<span data-ttu-id="86fcf-181">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86fcf-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="86fcf-182">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="86fcf-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="86fcf-183">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="86fcf-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
