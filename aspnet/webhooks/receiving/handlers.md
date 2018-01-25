---
uid: webhooks/receiving/handlers
title: "Programy obsługi elementów Webhook ASP.NET | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Sposób obsługi żądań w elementów Webhook ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 12acae0883c12698a8f9c2150623ba792303e7ef
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="d76fe-103">Programy obsługi elementów Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d76fe-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="d76fe-104">Po żądań elementów Webhook został zweryfikowany przez odbiornik elementu WebHook, jest gotowe do przetworzenia przez kod użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d76fe-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="d76fe-105">Jest to, gdy *obsługi* się.</span><span class="sxs-lookup"><span data-stu-id="d76fe-105">This is where *handlers* come in.</span></span> <span data-ttu-id="d76fe-106">Programy obsługi pochodzi od [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfejsu, ale zazwyczaj używa [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) klasy zamiast bezpośrednio z interfejsu.</span><span class="sxs-lookup"><span data-stu-id="d76fe-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="d76fe-107">Żądanie WebHook mogą być przetwarzane przez jeden lub więcej programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="d76fe-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="d76fe-108">Programy obsługi są wywoływane w kolejności, w oparciu o ich odpowiednich *kolejności* właściwości przechodzenia z najniższą najwyższej kolejności w przypadku prostego liczba całkowita (sugerowane należeć do przedziału od 1 do 100):</span><span class="sxs-lookup"><span data-stu-id="d76fe-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagram właściwości kolejności obsługi elementu WebHook](_static/Handlers.png)

<span data-ttu-id="d76fe-110">Opcjonalnie możesz ustawić programu obsługi *odpowiedzi* właściwość WebHookHandlerContext, który prowadzi przetwarzania do zatrzymywania i odpowiedź do wysłania jako odpowiedzi HTTP elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="d76fe-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="d76fe-111">W przypadku powyższych obsługi C nie pobrać wywołać, ponieważ ma ona wyższego rzędu niż B i B ustawia odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="d76fe-111">In the case above, Handler C won’t get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="d76fe-112">Ustawienie odpowiedzi jest zwykle tylko istotne dla elementów Webhook, gdzie odpowiedzi może przenosić informacji do źródłowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d76fe-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="d76fe-113">Jest to na przykład w przypadku elementów Webhook zapas czasu, gdy odpowiedź jest opublikowane do kanału, skąd pochodzą elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="d76fe-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="d76fe-114">Programy obsługi można ustawić właściwości odbiorcy, jeśli chce otrzymywać elementów Webhook tego konkretnego odbiorcy.</span><span class="sxs-lookup"><span data-stu-id="d76fe-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="d76fe-115">Jeśli odbiornik nie ustawione są nazywane dla wszystkich z nich.</span><span class="sxs-lookup"><span data-stu-id="d76fe-115">If they don’t set the receiver they are called for all of them.</span></span>

<span data-ttu-id="d76fe-116">Jedno inne typowe użycie odpowiedzi jest użycie *410 Gone* odpowiedzi, aby wskazać, że elementu WebHook jest już aktywne i powinny być przekazywane nie dalszych żądań.</span><span class="sxs-lookup"><span data-stu-id="d76fe-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="d76fe-117">Domyślnie program obsługi zostanie wywołana przez wszystkie odbiorniki elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="d76fe-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="d76fe-118">Jednak jeśli *odbiornika* właściwość jest ustawiona na nazwę programu obsługi, a następnie programu obsługi tylko będą otrzymywać żądania elementu WebHook z tego odbiornika.</span><span class="sxs-lookup"><span data-stu-id="d76fe-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="d76fe-119">Przetwarzanie elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="d76fe-119">Processing a WebHook</span></span>

<span data-ttu-id="d76fe-120">Podczas obsługi zostanie wywołany, pobiera [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) zawierający informacje o żądaniu elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="d76fe-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="d76fe-121">Dane, zwykle treści żądania HTTP, są dostępne z *danych* właściwości.</span><span class="sxs-lookup"><span data-stu-id="d76fe-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="d76fe-122">Typ danych jest zwykle JSON lub HTML danych formularza, ale można rzutować na bardziej określonego typu, w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="d76fe-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="d76fe-123">Na przykład niestandardowych elementów Webhook generowane przez elementów Webhook ASP.NET mogą być rzutowane na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d76fe-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="d76fe-124">Przetwarzanie w kolejce</span><span class="sxs-lookup"><span data-stu-id="d76fe-124">Queued Processing</span></span>

<span data-ttu-id="d76fe-125">Większość nadawców elementu WebHook wyśle elementu WebHook, jeśli odpowiedź nie jest generowana w kilku sekund.</span><span class="sxs-lookup"><span data-stu-id="d76fe-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="d76fe-126">Oznacza to, że programu obsługi, należy wykonać przetwarzania w tym czasie w celu nie jej ponownie wywołany.</span><span class="sxs-lookup"><span data-stu-id="d76fe-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="d76fe-127">Przetwarzanie trwa dłużej, czy lepiej jest obsługiwane oddzielnie, a następnie [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) może służyć do przesyłania żądania elementu WebHook do kolejki, na przykład [kolejki magazynu Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="d76fe-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="d76fe-128">Zarys [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementacji podano tutaj:</span><span class="sxs-lookup"><span data-stu-id="d76fe-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
