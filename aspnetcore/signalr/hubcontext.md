---
title: SignalR HubContext
author: tdykstra
description: Dowiedz się, jak używać usługi ASP.NET Core SignalR HubContext wysyłania powiadomień do klientów z poza koncentratora.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 6b955c2064d7d6a045594e56326e2f7df282675f
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095310"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="1e5d1-103">Wysyłanie komunikatów z poza Centrum</span><span class="sxs-lookup"><span data-stu-id="1e5d1-103">Send messages from outside a hub</span></span>

<span data-ttu-id="1e5d1-104">Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="1e5d1-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="1e5d1-105">Centrum SignalR to Abstrakcja core do wysyłania wiadomości do klientów dołączonych do serwera biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="1e5d1-106">Istnieje również możliwość wysyłać komunikaty z innych miejsc w aplikacji przy użyciu `IHubContext` usługi.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="1e5d1-107">W tym artykule wyjaśniono, jak uzyskać dostęp biblioteki SignalR `IHubContext` do wysyłania powiadomień do klientów z poza koncentratora.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="1e5d1-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="1e5d1-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="1e5d1-109">Pobierz wystąpienia `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="1e5d1-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="1e5d1-110">W biblioteki SignalR platformy ASP.NET Core, uzyskujesz dostęp do wystąpienia `IHubContext` za pomocą iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="1e5d1-111">Może wprowadzać wystąpienie `IHubContext` do kontrolera, oprogramowanie pośredniczące lub inna usługa DI.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="1e5d1-112">Aby wysyłać komunikaty do klientów, należy użyć wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="1e5d1-113">To różni się od biblioteki SignalR platformy ASP.NET, w której używane GlobalHost w celu zapewnienia dostępu do `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="1e5d1-114">Platforma ASP.NET Core ma strukturę iniekcji zależności, która eliminuje potrzebę tym globalnego pojedynczym wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="1e5d1-115">Wstrzykiwanie wystąpienie `IHubContext` w kontrolerze</span><span class="sxs-lookup"><span data-stu-id="1e5d1-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="1e5d1-116">Może wprowadzać wystąpienie `IHubContext` do kontrolera, dodając go do konstruktora:</span><span class="sxs-lookup"><span data-stu-id="1e5d1-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="1e5d1-117">Teraz dzięki dostępowi do wystąpienia `IHubContext`, można wywoływać metod koncentratora, tak, jakby były w Centrum sam.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="1e5d1-118">Pobierz wystąpienia `IHubContext` w oprogramowaniu pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="1e5d1-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="1e5d1-119">Dostęp do `IHubContext` w ramach potoku oprogramowania pośredniczącego w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="1e5d1-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> <span data-ttu-id="1e5d1-120">Kiedy metodach koncentratora są wywoływane z poza `Hub` klasy, nie ma żadnych wywołujący skojarzone z wywołania.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="1e5d1-121">Dlatego nie ma dostępu do `ConnectionId`, `Caller`, i `Others` właściwości.</span><span class="sxs-lookup"><span data-stu-id="1e5d1-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="1e5d1-122">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="1e5d1-122">Related resources</span></span>

* [<span data-ttu-id="1e5d1-123">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="1e5d1-123">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="1e5d1-124">Centra</span><span class="sxs-lookup"><span data-stu-id="1e5d1-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="1e5d1-125">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="1e5d1-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
