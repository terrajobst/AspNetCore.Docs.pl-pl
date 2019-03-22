---
title: Przy użyciu hubs w biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się, jak używać koncentratory w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: f037c1f6fd7ef773b8e7b2fd4fdf6e28222c441a
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327264"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="c2f4d-103">Na użytek koncentratory w SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2f4d-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="c2f4d-104">Przez [Rachel Appel](https://twitter.com/rachelappel) i [Kevin Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="c2f4d-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="c2f4d-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c2f4d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="c2f4d-106">Co to jest Centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="c2f4d-106">What is a SignalR hub</span></span>

<span data-ttu-id="c2f4d-107">Interfejsu API centrów SignalR umożliwia wywoływanie metod na komputerach klienckich połączonych z serwera.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="c2f4d-108">W kodzie serwera, można zdefiniować metody, które są wywoływane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="c2f4d-109">Kod klienta służy do definiowania metod, które są wywoływane z serwera.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="c2f4d-110">SignalR zajmuje się wszystkiego w tle, które sprawia, że w czasie rzeczywistym komunikacji klient serwer i klient serwera jest możliwe.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="c2f4d-111">Konfigurowanie centrów SignalR</span><span class="sxs-lookup"><span data-stu-id="c2f4d-111">Configure SignalR hubs</span></span>

<span data-ttu-id="c2f4d-112">Oprogramowanie pośredniczące SignalR wymaga pewnych usług, które zostały skonfigurowane przez wywołanie metody `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="c2f4d-113">Podczas dodawania funkcji SignalR do aplikacji ASP.NET Core, należy skonfigurować trasy SignalR, wywołując `app.UseSignalR` w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="c2f4d-114">Tworzenie i używanie koncentratory</span><span class="sxs-lookup"><span data-stu-id="c2f4d-114">Create and use hubs</span></span>

<span data-ttu-id="c2f4d-115">Utwórz koncentrator od zadeklarowania klasy, która dziedziczy `Hub`i Dodaj metody publiczne do niego.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="c2f4d-116">Klienci mogą wywoływać metody, które są zdefiniowane jako `public`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="c2f4d-117">Można określić zwracany typ i parametry, w tym typy złożone i tablice, tak jak w dowolnej metody języka C#.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="c2f4d-118">SignalR obsługi serializacji i deserializacji obiektu złożonego obiekty i tablice parametrów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="c2f4d-119">Centra są przejściowe:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-119">Hubs are transient:</span></span>
>
> * <span data-ttu-id="c2f4d-120">Nie należy przechowywać stanu właściwością klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="c2f4d-121">Każde wywołanie metody koncentratora jest wykonywane na nowe wystąpienie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="c2f4d-122">Użyj `await` podczas wywoływania metod asynchronicznych, które są zależne od centrum pozostaje aktywne.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="c2f4d-123">Na przykład metoda takich jak `Clients.All.SendAsync(...)` może zakończyć się niepowodzeniem, jeśli jest wywoływana bez `await` i metody koncentratora zakończy się przed `SendAsync` zakończy się.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="c2f4d-124">Obiekt kontekstu</span><span class="sxs-lookup"><span data-stu-id="c2f4d-124">The Context object</span></span>

<span data-ttu-id="c2f4d-125">`Hub` Klasa ma `Context` właściwość, która zawiera następujące właściwości z informacjami o połączeniu:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="c2f4d-126">Właściwość</span><span class="sxs-lookup"><span data-stu-id="c2f4d-126">Property</span></span> | <span data-ttu-id="c2f4d-127">Opis</span><span class="sxs-lookup"><span data-stu-id="c2f4d-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="c2f4d-128">Pobiera unikatowy identyfikator połączenia, przypisany przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="c2f4d-129">Istnieje jeden identyfikator połączenia dla każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="c2f4d-130">Pobiera [identyfikator użytkownika](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="c2f4d-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="c2f4d-131">Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonych z tym połączeniem jako identyfikator użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="c2f4d-132">Pobiera `ClaimsPrincipal` skojarzone z bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="c2f4d-133">Pobiera kolekcję kluczy/wartości, który może służyć do udostępniania danych w zakresie tego połączenia.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="c2f4d-134">Dane mogą być przechowywane w tej kolekcji, a jej będzie zachowywane dla połączenia między różnych metod koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="c2f4d-135">Pobiera kolekcję funkcji dostępnych w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="c2f4d-136">Na razie ta kolekcja nie jest potrzebny w większości przypadków, więc nie jest on jeszcze opisane szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="c2f4d-137">Pobiera `CancellationToken` , otrzyma powiadomienie, gdy połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="c2f4d-138">`Hub.Context` zawiera również następujące metody:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="c2f4d-139">Metoda</span><span class="sxs-lookup"><span data-stu-id="c2f4d-139">Method</span></span> | <span data-ttu-id="c2f4d-140">Opis</span><span class="sxs-lookup"><span data-stu-id="c2f4d-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="c2f4d-141">Zwraca `HttpContext` dla połączenia lub `null` Jeśli połączenie nie jest skojarzony z żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="c2f4d-142">W przypadku połączeń HTTP można użyć tej metody, aby uzyskać informacje, takie jak nagłówki HTTP i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="c2f4d-143">Przerywa połączenie.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="c2f4d-144">Obiekt klientów</span><span class="sxs-lookup"><span data-stu-id="c2f4d-144">The Clients object</span></span>

<span data-ttu-id="c2f4d-145">`Hub` Klasa ma `Clients` właściwość, która zawiera następujące właściwości dla komunikacji między serwerem a klientem:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="c2f4d-146">Właściwość</span><span class="sxs-lookup"><span data-stu-id="c2f4d-146">Property</span></span> | <span data-ttu-id="c2f4d-147">Opis</span><span class="sxs-lookup"><span data-stu-id="c2f4d-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="c2f4d-148">Wywołuje metodę dla wszystkich podłączonych klientów</span><span class="sxs-lookup"><span data-stu-id="c2f4d-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="c2f4d-149">Wywołuje metodę dla klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="c2f4d-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="c2f4d-150">Wywołuje metodę dla wszyscy połączeni klienci oprócz klienta, który wywołał metodę</span><span class="sxs-lookup"><span data-stu-id="c2f4d-150">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="c2f4d-151">`Hub.Clients` zawiera również następujące metody:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="c2f4d-152">Metoda</span><span class="sxs-lookup"><span data-stu-id="c2f4d-152">Method</span></span> | <span data-ttu-id="c2f4d-153">Opis</span><span class="sxs-lookup"><span data-stu-id="c2f4d-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="c2f4d-154">Wywołuje metodę dla wszystkich podłączonych klientów z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="c2f4d-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="c2f4d-155">Wywołuje metodę dla określonego klienta połączone</span><span class="sxs-lookup"><span data-stu-id="c2f4d-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="c2f4d-156">Wywołuje metodę dla określonych klientów połączonych</span><span class="sxs-lookup"><span data-stu-id="c2f4d-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="c2f4d-157">Wywołuje metodę dla wszystkich połączeń w określonej grupie</span><span class="sxs-lookup"><span data-stu-id="c2f4d-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="c2f4d-158">Wywołuje metodę dla wszystkich połączeń w określonej grupie, z wyjątkiem wskazanych połączeń</span><span class="sxs-lookup"><span data-stu-id="c2f4d-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="c2f4d-159">Wywołuje metodę dla wielu grup połączeń</span><span class="sxs-lookup"><span data-stu-id="c2f4d-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="c2f4d-160">Wywołuje metodę dla grupy połączeń z wykluczeniem klienta, który wywołał metodę koncentratora</span><span class="sxs-lookup"><span data-stu-id="c2f4d-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="c2f4d-161">Wywołuje metodę dla wszystkich połączeń, skojarzone z określonym użytkownikiem</span><span class="sxs-lookup"><span data-stu-id="c2f4d-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="c2f4d-162">Wywołuje metodę dla wszystkich połączeń skojarzonych z określonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="c2f4d-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="c2f4d-163">Każda właściwość lub metoda w poprzednich tabelach zwraca obiekt z `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="c2f4d-164">`SendAsync` Metoda umożliwia podanie nazwy i parametry metody klienta do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="c2f4d-165">Wysyłanie komunikatów do klientów</span><span class="sxs-lookup"><span data-stu-id="c2f4d-165">Send messages to clients</span></span>

<span data-ttu-id="c2f4d-166">Aby wykonywać wywołania określonych klientów, należy użyć właściwości `Clients` obiektu.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="c2f4d-167">W poniższym przykładzie istnieją trzy metody Centrum:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="c2f4d-168">`SendMessage` wysyła wiadomość do wszystkich połączonych klientów przy użyciu `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="c2f4d-169">`SendMessageToCaller` wysyła komunikat do obiektu wywołującego, za pomocą `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="c2f4d-170">`SendMessageToGroups` wysyła wiadomość do wszystkich klientów w `SignalR Users` grupy.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="c2f4d-171">Koncentratory silnie typizowane</span><span class="sxs-lookup"><span data-stu-id="c2f4d-171">Strongly typed hubs</span></span>

<span data-ttu-id="c2f4d-172">Wadą korzystania z `SendAsync` jest, że opiera się na ciąg magiczny, aby określić metodę klienta do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="c2f4d-173">Spowoduje to pozostawienie Otwórz kod błędów czasu wykonywania, jeśli nazwa metody została błędnie napisana lub brak od klienta.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="c2f4d-174">Alternatywa dla użycia `SendAsync` polega na wpisaniu silnie `Hub` z <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span></span> <span data-ttu-id="c2f4d-175">W poniższym przykładzie `ChatHub` metody klienta zostały wyodrębnione na zewnątrz do interfejs o nazwie `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="c2f4d-176">Ten interfejs może służyć do refaktoryzacji poprzednim `ChatHub` przykład.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="c2f4d-177">Za pomocą `Hub<IChatClient>` umożliwia w czasie kompilacji sprawdzania metody klienta.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="c2f4d-178">Zapobiega to problem spowodował za pomocą magic ciągów, ponieważ `Hub<T>` tylko można zapewnić dostęp do metody zdefiniowane w interfejsie.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="c2f4d-179">Za pomocą silnie typizowanej `Hub<T>` wyłącza możliwość używania `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="c2f4d-180">Wszelkie metody zdefiniowane w interfejsie nadal można zdefiniować jako asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-180">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="c2f4d-181">W rzeczywistości, każda z tych metod zwraca `Task`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-181">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="c2f4d-182">Ponieważ jest to interfejs, nie używaj `async` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-182">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="c2f4d-183">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-183">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="c2f4d-184">`Async` Sufiks nie jest usunięte z nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-184">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="c2f4d-185">Chyba, że metoda klienta jest zdefiniowana z `.on('MyMethodAsync')`, nie używaj `MyMethodAsync` jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-185">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="c2f4d-186">Zmień nazwę metody koncentratora</span><span class="sxs-lookup"><span data-stu-id="c2f4d-186">Change the name of a hub method</span></span>

<span data-ttu-id="c2f4d-187">Domyślnie nazwa metody koncentratora serwera jest nazwa metody .NET.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-187">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="c2f4d-188">Można jednak użyć [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) atrybutu, aby zmienić to ustawienie domyślne i ręcznie określić nazwę metody.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-188">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="c2f4d-189">Klienta należy użyć tej nazwy zamiast nazwy metody .NET, podczas wywoływania metody.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-189">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="c2f4d-190">Obsługa zdarzeń dla połączenia</span><span class="sxs-lookup"><span data-stu-id="c2f4d-190">Handle events for a connection</span></span>

<span data-ttu-id="c2f4d-191">Interfejs API centrów SignalR udostępnia `OnConnectedAsync` i `OnDisconnectedAsync` metod wirtualnych, do zarządzania i śledzenia połączeń.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-191">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="c2f4d-192">Zastąp `OnConnectedAsync` metody wirtualnej do wykonania akcji, gdy klient nawiąże połączenie z koncentratorem, takie jak dodanie go do grupy.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-192">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="c2f4d-193">Zastąp `OnDisconnectedAsync` metody wirtualnej do wykonania akcji w przypadku, gdy klient zakończy połączenie.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-193">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="c2f4d-194">Jeśli klient odłączy się celowo (przez wywołanie metody `connection.stop()`, na przykład), `exception` parametr będzie `null`.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-194">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="c2f4d-195">Jednakże, jeśli klient został odłączony z powodu błędu (np. awarii sieci), `exception` parametr będzie zawierać wyjątek zawierająca opis błędu.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-195">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="c2f4d-196">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="c2f4d-196">Handle errors</span></span>

<span data-ttu-id="c2f4d-197">Wyjątki zgłaszane w Twoich metodach koncentratora są wysyłane do klienta, który wywołał metodę.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-197">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="c2f4d-198">Na kliencie JavaScript `invoke` metoda zwraca [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="c2f4d-198">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="c2f4d-199">Gdy klient odbierze błąd związany z programu obsługi dołączone do przy użyciu promise `catch`, ma ona wywoływana i przekazywane jako plik JavaScript `Error` obiektu.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-199">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="c2f4d-200">Jeśli koncentrator zgłasza wyjątek, połączenia nie są zamknięte.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-200">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="c2f4d-201">Domyślnie SignalR zwraca ogólny komunikat o błędzie do klienta.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-201">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="c2f4d-202">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c2f4d-202">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="c2f4d-203">Nieoczekiwane wyjątki często zawierają poufne informacje, takie jak nazwa serwera bazy danych, wyjątek wyzwalane, gdy połączenie z bazą danych nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-203">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="c2f4d-204">SignalR nie ujawnia te szczegółowe komunikaty o błędach domyślnie ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-204">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="c2f4d-205">Zobacz [artykułu zagadnienia dotyczące zabezpieczeń](xref:signalr/security#exceptions) Aby uzyskać więcej informacji o tym, dlaczego szczegóły wyjątku są pomijane.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-205">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="c2f4d-206">Jeśli masz wyjątkowych warunków możesz *czy* zostanie rozpropagowany do klienta, można użyć `HubException` klasy.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-206">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="c2f4d-207">Jeśli możesz zgłosić `HubException` ze swojej metody koncentratora SignalR **będzie** Wyślij cały komunikat do klienta bez modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-207">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="c2f4d-208">Wysyła tylko SignalR `Message` właściwości wyjątku do klienta.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-208">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="c2f4d-209">Ślad stosu i inne właściwości w drodze wyjątku nie są dostępne dla klienta.</span><span class="sxs-lookup"><span data-stu-id="c2f4d-209">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="c2f4d-210">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="c2f4d-210">Related resources</span></span>

* [<span data-ttu-id="c2f4d-211">Wprowadzenie do SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2f4d-211">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="c2f4d-212">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="c2f4d-212">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c2f4d-213">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="c2f4d-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
