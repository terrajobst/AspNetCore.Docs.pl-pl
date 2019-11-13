---
title: Korzystanie z centrów w ASP.NET Core SignalR
author: bradygaster
description: Dowiedz się, jak korzystać z centrów w ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/hubs
ms.openlocfilehash: f95766cab84bddff2c7c62f30bce1e6d1e43deab
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963800"
---
# <a name="use-hubs-in-opno-locsignalr-for-aspnet-core"></a><span data-ttu-id="d47da-103">Użyj centrów w SignalR dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d47da-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="d47da-104">Autor [Rachel Appel](https://twitter.com/rachelappel) i [Jan Griffin](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="d47da-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="d47da-105">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak pobrać)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="d47da-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-opno-locsignalr-hub"></a><span data-ttu-id="d47da-106">Co to jest centrum SignalR</span><span class="sxs-lookup"><span data-stu-id="d47da-106">What is a SignalR hub</span></span>

<span data-ttu-id="d47da-107">Interfejs API centrów SignalR umożliwia wywoływanie metod na podłączonych klientach z serwera.</span><span class="sxs-lookup"><span data-stu-id="d47da-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="d47da-108">W kodzie serwera należy zdefiniować metody, które są wywoływane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="d47da-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="d47da-109">W kodzie klienta należy zdefiniować metody, które są wywoływane z serwera programu.</span><span class="sxs-lookup"><span data-stu-id="d47da-109">In the client code, you define methods that are called from the server.</span></span> SignalR<span data-ttu-id="d47da-110"> zajmuje się wszystkimi wszystkimi scenami, które umożliwiają komunikację między klientem i serwerem a klientem w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="d47da-110"> takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-opno-locsignalr-hubs"></a><span data-ttu-id="d47da-111">Konfigurowanie centrów SignalR</span><span class="sxs-lookup"><span data-stu-id="d47da-111">Configure SignalR hubs</span></span>

<span data-ttu-id="d47da-112">Oprogramowanie pośredniczące SignalR wymaga pewnych usług, które są konfigurowane przez wywoływanie `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="d47da-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d47da-113">Podczas dodawania funkcji SignalR do aplikacji ASP.NET Core, skonfiguruj SignalR trasy, wywołując `endpoint.MapHub` w `Startup.Configure` wywołaniu metody `app.UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="d47da-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `endpoint.MapHub` in the `Startup.Configure` method's `app.UseEndpoints` callback.</span></span>

```csharp
app.UseRouting();
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/chathub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d47da-114">Podczas dodawania funkcji SignalR do aplikacji ASP.NET Core, skonfiguruj SignalR trasy, wywołując `app.UseSignalR` w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d47da-114">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

::: moniker-end

## <a name="create-and-use-hubs"></a><span data-ttu-id="d47da-115">Tworzenie i używanie centrów</span><span class="sxs-lookup"><span data-stu-id="d47da-115">Create and use hubs</span></span>

<span data-ttu-id="d47da-116">Utwórz centrum, deklarując klasę, która dziedziczy z `Hub`, i Dodaj do niej metody publiczne.</span><span class="sxs-lookup"><span data-stu-id="d47da-116">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="d47da-117">Klienci mogą wywoływać metody, które są zdefiniowane jako `public`.</span><span class="sxs-lookup"><span data-stu-id="d47da-117">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="d47da-118">Można określić typ zwracany i parametry, w tym typy złożone i tablice, tak jak w przypadku dowolnej C# metody.</span><span class="sxs-lookup"><span data-stu-id="d47da-118">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> SignalR<span data-ttu-id="d47da-119"> obsługuje serializacji i deserializacji złożonych obiektów i tablic w parametrach i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="d47da-119"> handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="d47da-120">Centra są przejściowe:</span><span class="sxs-lookup"><span data-stu-id="d47da-120">Hubs are transient:</span></span>
>
> * <span data-ttu-id="d47da-121">Nie przechowuj stanu we właściwości klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="d47da-121">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="d47da-122">Każde wywołanie metody centrum jest wykonywane na nowym wystąpieniu centrum.</span><span class="sxs-lookup"><span data-stu-id="d47da-122">Every hub method call is executed on a new hub instance.</span></span>
> * <span data-ttu-id="d47da-123">Użyj `await` podczas wywoływania metod asynchronicznych, które są zależne od utrzymywania aktywności przez centrum.</span><span class="sxs-lookup"><span data-stu-id="d47da-123">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="d47da-124">Na przykład metoda, taka jak `Clients.All.SendAsync(...)`, może się nie powieść, jeśli jest wywoływana bez `await` i zostanie zakończona Metoda Hub przed zakończeniem `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="d47da-124">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="d47da-125">Obiekt kontekstu</span><span class="sxs-lookup"><span data-stu-id="d47da-125">The Context object</span></span>

<span data-ttu-id="d47da-126">Klasa `Hub` ma właściwość `Context`, która zawiera następujące właściwości z informacjami o połączeniu:</span><span class="sxs-lookup"><span data-stu-id="d47da-126">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="d47da-127">Właściwość</span><span class="sxs-lookup"><span data-stu-id="d47da-127">Property</span></span> | <span data-ttu-id="d47da-128">Opis</span><span class="sxs-lookup"><span data-stu-id="d47da-128">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="d47da-129">Pobiera unikatowy identyfikator połączenia przypisany przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="d47da-129">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="d47da-130">Istnieje jeden identyfikator połączenia dla każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="d47da-130">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="d47da-131">Pobiera [Identyfikator użytkownika](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="d47da-131">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="d47da-132">Domyślnie SignalR używa `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonego z połączeniem jako identyfikatorem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d47da-132">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="d47da-133">Pobiera `ClaimsPrincipal` skojarzoną z bieżącym użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="d47da-133">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="d47da-134">Pobiera kolekcję klucz/wartość, której można użyć do udostępniania danych w zakresie tego połączenia.</span><span class="sxs-lookup"><span data-stu-id="d47da-134">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="d47da-135">Dane można przechowywać w tej kolekcji i będzie ona trwała dla połączenia między różnymi wywołaniami metody centrum.</span><span class="sxs-lookup"><span data-stu-id="d47da-135">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="d47da-136">Pobiera kolekcję funkcji dostępnych w ramach połączenia.</span><span class="sxs-lookup"><span data-stu-id="d47da-136">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="d47da-137">Na razie ta kolekcja nie jest wymagana w większości scenariuszy, więc nie jest jeszcze udokumentowana.</span><span class="sxs-lookup"><span data-stu-id="d47da-137">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="d47da-138">Pobiera `CancellationToken`, który powiadamia o przerwaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="d47da-138">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="d47da-139">`Hub.Context` również zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="d47da-139">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="d47da-140">Metoda</span><span class="sxs-lookup"><span data-stu-id="d47da-140">Method</span></span> | <span data-ttu-id="d47da-141">Opis</span><span class="sxs-lookup"><span data-stu-id="d47da-141">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="d47da-142">Zwraca `HttpContext` połączenia lub `null`, jeśli połączenie nie jest skojarzone z żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="d47da-142">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="d47da-143">W przypadku połączeń HTTP można użyć tej metody, aby uzyskać informacje takie jak nagłówki HTTP i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="d47da-143">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="d47da-144">Przerywa połączenie.</span><span class="sxs-lookup"><span data-stu-id="d47da-144">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="d47da-145">Obiekt clients</span><span class="sxs-lookup"><span data-stu-id="d47da-145">The Clients object</span></span>

<span data-ttu-id="d47da-146">Klasa `Hub` ma właściwość `Clients`, która zawiera następujące właściwości komunikacji między serwerem i klientem:</span><span class="sxs-lookup"><span data-stu-id="d47da-146">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="d47da-147">Właściwość</span><span class="sxs-lookup"><span data-stu-id="d47da-147">Property</span></span> | <span data-ttu-id="d47da-148">Opis</span><span class="sxs-lookup"><span data-stu-id="d47da-148">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="d47da-149">Wywołuje metodę na wszystkich połączonych klientach</span><span class="sxs-lookup"><span data-stu-id="d47da-149">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="d47da-150">Wywołuje metodę na kliencie, który wywołał metodę Hub</span><span class="sxs-lookup"><span data-stu-id="d47da-150">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="d47da-151">Wywołuje metodę na wszystkich połączonych klientach z wyjątkiem klienta, który wywołał metodę</span><span class="sxs-lookup"><span data-stu-id="d47da-151">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="d47da-152">`Hub.Clients` również zawiera następujące metody:</span><span class="sxs-lookup"><span data-stu-id="d47da-152">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="d47da-153">Metoda</span><span class="sxs-lookup"><span data-stu-id="d47da-153">Method</span></span> | <span data-ttu-id="d47da-154">Opis</span><span class="sxs-lookup"><span data-stu-id="d47da-154">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="d47da-155">Wywołuje metodę na wszystkich połączonych klientach z wyjątkiem określonych połączeń</span><span class="sxs-lookup"><span data-stu-id="d47da-155">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="d47da-156">Wywołuje metodę na określonym podłączonym kliencie</span><span class="sxs-lookup"><span data-stu-id="d47da-156">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="d47da-157">Wywołuje metodę na określonych połączonych klientach</span><span class="sxs-lookup"><span data-stu-id="d47da-157">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="d47da-158">Wywołuje metodę dla wszystkich połączeń w określonej grupie</span><span class="sxs-lookup"><span data-stu-id="d47da-158">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="d47da-159">Wywołuje metodę dla wszystkich połączeń w określonej grupie, z wyjątkiem określonych połączeń</span><span class="sxs-lookup"><span data-stu-id="d47da-159">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="d47da-160">Wywołuje metodę dla wielu grup połączeń</span><span class="sxs-lookup"><span data-stu-id="d47da-160">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="d47da-161">Wywołuje metodę w grupie połączeń z wyłączeniem klienta, który wywołał metodę Hub</span><span class="sxs-lookup"><span data-stu-id="d47da-161">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="d47da-162">Wywołuje metodę dla wszystkich połączeń skojarzonych z określonym użytkownikiem</span><span class="sxs-lookup"><span data-stu-id="d47da-162">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="d47da-163">Wywołuje metodę dla wszystkich połączeń skojarzonych z określonymi użytkownikami</span><span class="sxs-lookup"><span data-stu-id="d47da-163">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="d47da-164">Każda właściwość lub metoda w powyższych tabelach zwraca obiekt z metodą `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="d47da-164">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="d47da-165">Metoda `SendAsync` umożliwia podanie nazwy i parametrów metody klienta do wywołania.</span><span class="sxs-lookup"><span data-stu-id="d47da-165">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="d47da-166">Wysyłanie komunikatów do klientów</span><span class="sxs-lookup"><span data-stu-id="d47da-166">Send messages to clients</span></span>

<span data-ttu-id="d47da-167">Aby wykonać wywołania do określonych klientów, użyj właściwości obiektu `Clients`.</span><span class="sxs-lookup"><span data-stu-id="d47da-167">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="d47da-168">W poniższym przykładzie istnieją trzy metody centralne:</span><span class="sxs-lookup"><span data-stu-id="d47da-168">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="d47da-169">`SendMessage` wysyła komunikat do wszystkich połączonych klientów przy użyciu `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="d47da-169">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="d47da-170">`SendMessageToCaller` wysyła komunikat z powrotem do obiektu wywołującego przy użyciu `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="d47da-170">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="d47da-171">`SendMessageToGroups` wysyła komunikat do wszystkich klientów w grupie `SignalR Users`.</span><span class="sxs-lookup"><span data-stu-id="d47da-171">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="d47da-172">Centra o jednoznacznie określonym typie</span><span class="sxs-lookup"><span data-stu-id="d47da-172">Strongly typed hubs</span></span>

<span data-ttu-id="d47da-173">Wadą używania `SendAsync` jest to, że opiera się on na ciągu Magic, aby określić metodę klienta, która ma zostać wywołana.</span><span class="sxs-lookup"><span data-stu-id="d47da-173">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="d47da-174">Spowoduje to pozostawienie kodu otwartego na błędy środowiska uruchomieniowego, jeśli nazwa metody jest błędnie wpisana lub nie została podana w kliencie.</span><span class="sxs-lookup"><span data-stu-id="d47da-174">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="d47da-175">Alternatywą dla korzystania z `SendAsync` jest silnie wpisywanie `Hub` z <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span><span class="sxs-lookup"><span data-stu-id="d47da-175">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub%601>.</span></span> <span data-ttu-id="d47da-176">W poniższym przykładzie metody klienta `ChatHub` zostały wyodrębnione do interfejsu o nazwie `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="d47da-176">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="d47da-177">Ten interfejs może służyć do refaktoryzacji powyższego przykładu `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="d47da-177">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="d47da-178">Użycie `Hub<IChatClient>` umożliwia sprawdzenie w czasie kompilacji metod klienta.</span><span class="sxs-lookup"><span data-stu-id="d47da-178">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="d47da-179">Zapobiega to problemom spowodowanym użyciem ciągów Magic, ponieważ `Hub<T>` może zapewnić dostęp tylko do metod zdefiniowanych w interfejsie.</span><span class="sxs-lookup"><span data-stu-id="d47da-179">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="d47da-180">Użycie `Hub<T>` o jednoznacznie określonym typie uniemożliwia korzystanie z `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="d47da-180">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="d47da-181">Wszelkie metody zdefiniowane w interfejsie mogą być nadal zdefiniowane jako asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="d47da-181">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="d47da-182">W rzeczywistości każda z tych metod powinna zwracać `Task`.</span><span class="sxs-lookup"><span data-stu-id="d47da-182">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="d47da-183">Ponieważ jest to interfejs, nie używaj słowa kluczowego `async`.</span><span class="sxs-lookup"><span data-stu-id="d47da-183">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="d47da-184">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d47da-184">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="d47da-185">Sufiks `Async` nie jest usuwany z nazwy metody.</span><span class="sxs-lookup"><span data-stu-id="d47da-185">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="d47da-186">Chyba że metoda klienta nie jest zdefiniowana z `.on('MyMethodAsync')`, nie należy używać `MyMethodAsync` jako nazwy.</span><span class="sxs-lookup"><span data-stu-id="d47da-186">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="d47da-187">Zmień nazwę metody centrum</span><span class="sxs-lookup"><span data-stu-id="d47da-187">Change the name of a hub method</span></span>

<span data-ttu-id="d47da-188">Domyślnie nazwa metody centrum serwera jest nazwą metody .NET.</span><span class="sxs-lookup"><span data-stu-id="d47da-188">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="d47da-189">Można jednak użyć atrybutu [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) , aby zmienić to ustawienie domyślne, i ręcznie określić nazwę dla metody.</span><span class="sxs-lookup"><span data-stu-id="d47da-189">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="d47da-190">Klient powinien używać tej nazwy zamiast nazwy metody .NET podczas wywoływania metody.</span><span class="sxs-lookup"><span data-stu-id="d47da-190">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="d47da-191">Obsługa zdarzeń dla połączenia</span><span class="sxs-lookup"><span data-stu-id="d47da-191">Handle events for a connection</span></span>

<span data-ttu-id="d47da-192">Interfejs API centrów SignalR udostępnia metody wirtualne `OnConnectedAsync` i `OnDisconnectedAsync` do zarządzania połączeniami i ich śledzenia.</span><span class="sxs-lookup"><span data-stu-id="d47da-192">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="d47da-193">Zastąp metodę wirtualną `OnConnectedAsync`, aby wykonać akcje, gdy klient łączy się z centrum, na przykład dodając go do grupy.</span><span class="sxs-lookup"><span data-stu-id="d47da-193">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="d47da-194">Zastąp metodę wirtualną `OnDisconnectedAsync`, aby wykonywać akcje po rozłączeniu klienta.</span><span class="sxs-lookup"><span data-stu-id="d47da-194">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="d47da-195">Jeśli klient odłączy się celowo (przez wywołanie `connection.stop()`, na przykład), parametr `exception` zostanie `null`.</span><span class="sxs-lookup"><span data-stu-id="d47da-195">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="d47da-196">Jeśli jednak klient zostanie rozłączony z powodu błędu (takiego jak awaria sieci), parametr `exception` będzie zawierał wyjątek opisujący błąd.</span><span class="sxs-lookup"><span data-stu-id="d47da-196">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="d47da-197">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="d47da-197">Handle errors</span></span>

<span data-ttu-id="d47da-198">Wyjątki zgłoszone w metodach centrum są wysyłane do klienta, który wywołał metodę.</span><span class="sxs-lookup"><span data-stu-id="d47da-198">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="d47da-199">Na kliencie JavaScript metoda `invoke` zwraca [obietnicę języka JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="d47da-199">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="d47da-200">Gdy klient otrzymuje błąd z obsługą dołączoną do obietnicy przy użyciu `catch`, zostanie wywołany i przeszedł jako obiekt `Error` JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d47da-200">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="d47da-201">Jeśli centrum zgłosi wyjątek, połączenia nie są zamknięte.</span><span class="sxs-lookup"><span data-stu-id="d47da-201">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="d47da-202">Domyślnie SignalR zwraca ogólny komunikat o błędzie do klienta.</span><span class="sxs-lookup"><span data-stu-id="d47da-202">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="d47da-203">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d47da-203">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="d47da-204">Nieoczekiwane wyjątki często zawierają informacje poufne, takie jak nazwa serwera bazy danych w wyjątku wyzwalanym w przypadku niepowodzenia połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="d47da-204">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> SignalR<span data-ttu-id="d47da-205"> domyślnie nie uwidacznia tych szczegółowych komunikatów o błędach jako miary zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="d47da-205"> doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="d47da-206">Aby uzyskać więcej informacji o tym, dlaczego szczegóły wyjątku są pomijane, zobacz artykuł dotyczący [zagadnień dotyczących zabezpieczeń](xref:signalr/security#exceptions) .</span><span class="sxs-lookup"><span data-stu-id="d47da-206">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="d47da-207">Jeśli *masz wyjątkowe warunki, które chcesz* propagować do klienta, możesz użyć klasy `HubException`.</span><span class="sxs-lookup"><span data-stu-id="d47da-207">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="d47da-208">Jeśli wygenerujesz `HubException` z metody centrum **, SignalR** wyśle całą wiadomość do klienta, niezmodyfikowaną.</span><span class="sxs-lookup"><span data-stu-id="d47da-208">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR<span data-ttu-id="d47da-209"> tylko wysyła Właściwość `Message` wyjątku do klienta.</span><span class="sxs-lookup"><span data-stu-id="d47da-209"> only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="d47da-210">Ślad stosu i inne właściwości tego wyjątku nie są dostępne dla klienta.</span><span class="sxs-lookup"><span data-stu-id="d47da-210">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="d47da-211">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="d47da-211">Related resources</span></span>

* <span data-ttu-id="d47da-212">[Wprowadzenie do ASP.NET Core SignalR](xref:signalr/introduction)</span><span class="sxs-lookup"><span data-stu-id="d47da-212">[Intro to ASP.NET Core SignalR](xref:signalr/introduction)</span></span>
* [<span data-ttu-id="d47da-213">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="d47da-213">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="d47da-214">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="d47da-214">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
