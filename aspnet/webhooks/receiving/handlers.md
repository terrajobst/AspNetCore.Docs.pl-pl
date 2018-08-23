---
uid: webhooks/receiving/handlers
title: Programy obsługi elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Sposób obsługi żądań w elementów Webhook programu ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754338"
---
# <a name="aspnet-webhooks-handlers"></a>Programy obsługi elementów Webhook programu ASP.NET

Gdy elementy Webhook żądania została zweryfikowana przez odbiorcę elementu WebHook, jest gotowe do przetworzenia przez kod użytkownika. Jest to miejsce *obsługi* pochodzą. Programy obsługi pochodzić od [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfejs, ale zazwyczaj używa [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) klasy zamiast wywodzić się bezpośrednio z interfejsu.

Żądanie elementu WebHook, mogą być przetwarzane przez jeden lub więcej programów obsługi. Programy obsługi są wywoływane w kolejności, w oparciu o ich odpowiednich *kolejności* właściwość przesyłane z najniższej do najwyższej gdzie kolejność jest liczbą całkowitą proste (sugerowany należeć do zakresu od 1 do 100):

![Procedura obsługi elementu WebHook kolejność właściwości diagramu](_static/Handlers.png)

Opcjonalnie możesz ustawić programu obsługi *odpowiedzi* właściwość WebHookHandlerContext, co będzie prowadzić przetwarzania do zatrzymania i odpowiedź do wysłania jako odpowiedzi HTTP do elementu WebHook. W przypadku powyższych obsługi języka C nie będzie wywoływana, ponieważ ma ona wyższego rzędu niż B i B ustawia odpowiedź.

Ustawienie odpowiedzi jest zwykle tylko istotne dla elementów Webhook, gdzie odpowiedzi mogą przenosić informacji do źródłowego interfejsu API. Jest to na przykład w przypadku elementów Webhook Slack, gdy odpowiedź opublikowaniu powrót do kanału, skąd pochodzą elementu WebHook. Programy obsługi można ustawić właściwości odbiorcy, jeśli chcą odbierać elementy Webhook z tego określonego odbiornika. Jeśli nie ustawiają odbiorcy, gdy są wywoływane dla wszystkich z nich.

Jeden innym typowym zastosowaniem odpowiedź jest użycie *410 Gone* odpowiedzi, aby wskazać, czy element WebHook nie jest już aktywne i powinny być przesyłane więcej żądań.

Domyślnie program obsługi zostanie wywołana przez wszystkich odbiorców elementu WebHook. Jednak jeśli *odbiorcy* właściwość jest ustawiona na nazwę programu obsługi, a następnie ten program obsługi subskrybent otrzyma tylko żądania elementu WebHook z tego odbiornika.

## <a name="processing-a-webhook"></a>Przetwarzanie elementu WebHook

Gdy program obsługi jest wywoływana, pobiera [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) zawierający informacje o żądanie elementu WebHook. Dane, zwykle treści żądania HTTP, są dostępne z *danych* właściwości.

Typ danych jest zazwyczaj dane formularza JSON lub HTML, ale można rzutować do bardziej określonego typu, w razie potrzeby. Na przykład niestandardowych elementów Webhook, generowane przez elementy Webhook ASP.NET mogą być rzutowane na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) w następujący sposób:

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

  ## <a name="queued-processing"></a>Przetwarzanie umieszczonych w kolejce

Większość nadawców elementu WebHook wyśle element WebHook, jeśli odpowiedź nie są generowane w ciągu kilku sekund. Oznacza to, że programu obsługi, należy wykonać przetwarzanie w tym przedziale czasu, w kolejności, nie na jego ponownie wywołana.

Jeśli przetwarzanie zajmuje więcej czasu lub lepszego obsługiwane osobno, a następnie [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) może służyć do przesyłania żądania elementu WebHook do kolejki, na przykład [usługi Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Zarys [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) wdrożenia można znaleźć tu:

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
