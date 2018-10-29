---
title: SignalR HubContext
author: tdykstra
description: Dowiedz się, jak używać usługi ASP.NET Core SignalR HubContext wysyłania powiadomień do klientów z poza koncentratora.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 8be888e1f7b16d65ebbaa24b618e84fca029d80b
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207956"
---
# <a name="send-messages-from-outside-a-hub"></a>Wysyłanie komunikatów z poza Centrum

Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)

Centrum SignalR to Abstrakcja core do wysyłania wiadomości do klientów dołączonych do serwera biblioteki SignalR. Istnieje również możliwość wysyłać komunikaty z innych miejsc w aplikacji przy użyciu `IHubContext` usługi. W tym artykule wyjaśniono, jak uzyskać dostęp biblioteki SignalR `IHubContext` do wysyłania powiadomień do klientów z poza koncentratora.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Pobierz wystąpienia IHubContext

W biblioteki SignalR platformy ASP.NET Core, uzyskujesz dostęp do wystąpienia `IHubContext` za pomocą iniekcji zależności. Może wprowadzać wystąpienie `IHubContext` do kontrolera, oprogramowanie pośredniczące lub inna usługa DI. Aby wysyłać komunikaty do klientów, należy użyć wystąpienia.

> [!NOTE]
> To różni się od ASP.NET 4.x SignalR, w której używane GlobalHost w celu zapewnienia dostępu do `IHubContext`. Platforma ASP.NET Core ma strukturę iniekcji zależności, która eliminuje potrzebę tym globalnego pojedynczym wystąpieniu.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Wstrzykiwanie wystąpienia IHubContext w kontrolerze

Może wprowadzać wystąpienie `IHubContext` do kontrolera, dodając go do konstruktora:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Teraz dzięki dostępowi do wystąpienia `IHubContext`, można wywoływać metod koncentratora, tak, jakby były w Centrum sam.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Pobierz wystąpienia IHubContext w oprogramowaniu pośredniczącym

Dostęp do `IHubContext` w ramach potoku oprogramowania pośredniczącego w następujący sposób:

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> Kiedy metodach koncentratora są wywoływane z poza `Hub` klasy, nie ma żadnych wywołujący skojarzone z wywołania. Dlatego nie ma dostępu do `ConnectionId`, `Caller`, i `Others` właściwości.

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
