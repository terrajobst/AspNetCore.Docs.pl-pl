---
uid: webhooks/receiving/handlers
title: Programy obsługi elementów Webhook ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Sposób obsługi żądań w elementów Webhook ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 4cf5770a731ef77842eb29b0a66ee0aac5d85d85
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883674"
---
# <a name="aspnet-webhooks-handlers"></a>Programy obsługi elementów Webhook ASP.NET

Po żądań elementów Webhook został zweryfikowany przez odbiornik elementu WebHook, jest gotowe do przetworzenia przez kod użytkownika. Jest to, gdy *obsługi* się. Programy obsługi pochodzi od [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interfejsu, ale zazwyczaj używa [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) klasy zamiast bezpośrednio z interfejsu.

Żądanie WebHook mogą być przetwarzane przez jeden lub więcej programów obsługi. Programy obsługi są wywoływane w kolejności, w oparciu o ich odpowiednich *kolejności* właściwości przechodzenia z najniższą najwyższej kolejności w przypadku prostego liczba całkowita (sugerowane należeć do przedziału od 1 do 100):

![Diagram właściwości kolejności obsługi elementu WebHook](_static/Handlers.png)

Opcjonalnie możesz ustawić programu obsługi *odpowiedzi* właściwość WebHookHandlerContext, który prowadzi przetwarzania do zatrzymywania i odpowiedź do wysłania jako odpowiedzi HTTP elementu WebHook. W przypadku powyższych obsługi C nie pobrać wywołać, ponieważ ma ona wyższego rzędu niż B i B ustawia odpowiedź.

Ustawienie odpowiedzi jest zwykle tylko istotne dla elementów Webhook, gdzie odpowiedzi może przenosić informacji do źródłowego interfejsu API. Jest to na przykład w przypadku elementów Webhook zapas czasu, gdy odpowiedź jest opublikowane do kanału, skąd pochodzą elementu WebHook. Programy obsługi można ustawić właściwości odbiorcy, jeśli chce otrzymywać elementów Webhook tego konkretnego odbiorcy. Jeśli odbiornik nie ustawione są nazywane dla wszystkich z nich.

Jedno inne typowe użycie odpowiedzi jest użycie *410 Gone* odpowiedzi, aby wskazać, że elementu WebHook jest już aktywne i powinny być przekazywane nie dalszych żądań.

Domyślnie program obsługi zostanie wywołana przez wszystkie odbiorniki elementu WebHook. Jednak jeśli *odbiornika* właściwość jest ustawiona na nazwę programu obsługi, a następnie programu obsługi tylko będą otrzymywać żądania elementu WebHook z tego odbiornika.

## <a name="processing-a-webhook"></a>Przetwarzanie elementu WebHook

Podczas obsługi zostanie wywołany, pobiera [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) zawierający informacje o żądaniu elementu WebHook. Dane, zwykle treści żądania HTTP, są dostępne z *danych* właściwości.

Typ danych jest zwykle JSON lub HTML danych formularza, ale można rzutować na bardziej określonego typu, w razie potrzeby. Na przykład niestandardowych elementów Webhook generowane przez elementów Webhook ASP.NET mogą być rzutowane na typ [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) w następujący sposób:

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

  ## <a name="queued-processing"></a>Przetwarzanie w kolejce

Większość nadawców elementu WebHook wyśle elementu WebHook, jeśli odpowiedź nie jest generowana w kilku sekund. Oznacza to, że programu obsługi, należy wykonać przetwarzania w tym czasie w celu nie jej ponownie wywołany.

Przetwarzanie trwa dłużej, czy lepiej jest obsługiwane oddzielnie, a następnie [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) może służyć do przesyłania żądania elementu WebHook do kolejki, na przykład [kolejki magazynu Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Zarys [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementacji podano tutaj:

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
