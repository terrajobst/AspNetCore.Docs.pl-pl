---
title: SignalR HubContext
author: rachelappel
description: Dowiedz się, jak używać usługi ASP.NET Core SignalR HubContext wysyłania powiadomień do klientów z poza koncentratora.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubcontext
ms.openlocfilehash: 79b91a776a38a2e6810cc89ff0b8d15fe836ce66
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35726106"
---
# <a name="send-messages-from-outside-a-hub"></a><span data-ttu-id="3678c-103">Wysyłanie komunikatów z poza koncentrator</span><span class="sxs-lookup"><span data-stu-id="3678c-103">Send messages from outside a hub</span></span>

<span data-ttu-id="3678c-104">Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="3678c-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="3678c-105">Centrum SignalR to core pozyskiwania do wysyłania wiadomości do klientów dołączonych do serwera SignalR.</span><span class="sxs-lookup"><span data-stu-id="3678c-105">The SignalR hub is the core abstraction for sending messages to clients connected to the SignalR server.</span></span> <span data-ttu-id="3678c-106">Istnieje również możliwość wysyłania wiadomości w innych miejscach w aplikacji za pomocą `IHubContext` usługi.</span><span class="sxs-lookup"><span data-stu-id="3678c-106">It's also possible to send messages from other places in your app using the `IHubContext` service.</span></span> <span data-ttu-id="3678c-107">W tym artykule opisano sposób uzyskiwania dostępu SignalR do `IHubContext` do wysyłania powiadomień do klientów z poza koncentratora.</span><span class="sxs-lookup"><span data-stu-id="3678c-107">This article explains how to access a SignalR `IHubContext` to send notifications to clients from outside a hub.</span></span>

<span data-ttu-id="3678c-108">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="3678c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="get-an-instance-of-ihubcontext"></a><span data-ttu-id="3678c-109">Pobranie wystąpienia `IHubContext`</span><span class="sxs-lookup"><span data-stu-id="3678c-109">Get an instance of `IHubContext`</span></span>

<span data-ttu-id="3678c-110">W ASP.NET Core SignalR, można uzyskać dostępu do wystąpienia `IHubContext` za pomocą iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="3678c-110">In ASP.NET Core SignalR, you can access an instance of `IHubContext` via dependency injection.</span></span> <span data-ttu-id="3678c-111">Można wstawić wystąpienia `IHubContext` do kontrolera, oprogramowanie pośredniczące lub innych usług Podpisane.</span><span class="sxs-lookup"><span data-stu-id="3678c-111">You can inject an instance of `IHubContext` into a controller, middleware, or other DI service.</span></span> <span data-ttu-id="3678c-112">Użyj wystąpienia do wysyłania wiadomości do klientów.</span><span class="sxs-lookup"><span data-stu-id="3678c-112">Use the instance to send messages to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="3678c-113">Ta różni się od biblioteki SignalR platformy ASP.NET, w której używane GlobalHost w celu zapewnienia dostępu do `IHubContext`.</span><span class="sxs-lookup"><span data-stu-id="3678c-113">This differs from ASP.NET SignalR which used GlobalHost to provide access to the `IHubContext`.</span></span> <span data-ttu-id="3678c-114">Platformy ASP.NET Core ma framework iniekcji zależności, która eliminuje potrzebę tym globalnych pojedynczym wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="3678c-114">ASP.NET Core has a dependency injection framework that removes the need for this global singleton.</span></span>

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a><span data-ttu-id="3678c-115">Wstaw wystąpienia `IHubContext` w kontrolerze</span><span class="sxs-lookup"><span data-stu-id="3678c-115">Inject an instance of `IHubContext` in a controller</span></span>

<span data-ttu-id="3678c-116">Można wstawić wystąpienia `IHubContext` do kontrolera, dodając ją z konstruktora:</span><span class="sxs-lookup"><span data-stu-id="3678c-116">You can inject an instance of `IHubContext` into a controller by adding it to your constructor:</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

<span data-ttu-id="3678c-117">Teraz, dając im dostęp do wystąpienia `IHubContext`, można wywoływać metod koncentratora tak, jakby był w Centrum w samej siebie.</span><span class="sxs-lookup"><span data-stu-id="3678c-117">Now, with access to an instance of `IHubContext`, you can call hub methods as if you were in the hub itself.</span></span>

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a><span data-ttu-id="3678c-118">Pobranie wystąpienia `IHubContext` w oprogramowaniu pośredniczącym</span><span class="sxs-lookup"><span data-stu-id="3678c-118">Get an instance of `IHubContext` in middleware</span></span>

<span data-ttu-id="3678c-119">Dostęp `IHubContext` w potoku oprogramowania pośredniczącego w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3678c-119">Access the `IHubContext` within the middleware pipeline like so:</span></span>

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
> <span data-ttu-id="3678c-120">Kiedy metod koncentratora są wywoływane z poza `Hub` klasa, nie istnieje żadne wywołującego skojarzone z wywołania.</span><span class="sxs-lookup"><span data-stu-id="3678c-120">When hub methods are called from outside of the `Hub` class, there's no caller associated with the invocation.</span></span> <span data-ttu-id="3678c-121">Dlatego nie jest Brak dostępu do `ConnectionId`, `Caller`, i `Others` właściwości.</span><span class="sxs-lookup"><span data-stu-id="3678c-121">Therefore, there's no access to the `ConnectionId`, `Caller`, and `Others` properties.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3678c-122">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="3678c-122">Related resources</span></span>

* [<span data-ttu-id="3678c-123">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="3678c-123">Get started</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="3678c-124">Centra</span><span class="sxs-lookup"><span data-stu-id="3678c-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3678c-125">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="3678c-125">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
