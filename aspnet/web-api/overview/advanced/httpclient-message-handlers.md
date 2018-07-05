---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Programy obsługi komunikatów HttpClient w interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 1712f190c5a313c79b7c91b671214dd8972cb3c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402944"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Programy obsługi komunikatów HttpClient w interfejsie Web API platformy ASP.NET
====================
przez [Mike Wasson](https://github.com/MikeWasson)

A *obsługi wiadomości* to klasa, która odbierze żądanie HTTP, a następnie zwraca odpowiedź HTTP.

Zazwyczaj szereg programy obsługi komunikatów są połączone. Pierwszego programu obsługi odebrania żądania HTTP, przetwarza je i zapewnia żądania do następnej procedury obsługi. W pewnym momencie odpowiedzi jest tworzony i powraca łańcuch. Ten wzorzec jest nazywany *delegowanie* programu obsługi.

![](httpclient-message-handlers/_static/image1.png)

Po stronie klienta **HttpClient** klasa używa obsługi wiadomości do przetwarzania żądań. Domyślny program obsługi jest **HttpClientHandler**, który wysyła żądanie za pośrednictwem sieci i pobiera odpowiedź z serwera. Programy obsługi komunikatów niestandardowych można wstawić do potoku klienta:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> Web API platformy ASP.NET używa także procedury obsługi komunikatów po stronie serwera. Aby uzyskać więcej informacji, zobacz [programów obsługi komunikatów HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Programy obsługi komunikatów niestandardowych

Aby napisać program obsługi komunikatów niestandardowych, pochodzi od **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody. W tym miejscu znajduje się sygnatura metody:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Ta metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **obiektu HttpResponseMessage**. Typowa implementacja wykonuje następujące czynności:

1. Przetwarzanie komunikatu żądania.
2. Wywołaj `base.SendAsync` wysyłać żądania do wewnętrznego programu obsługi.
3. Wewnętrzny program obsługi zwraca komunikat odpowiedzi. (Ten krok jest asynchroniczne).
4. Przetworzenie odpowiedzi i zwraca go do obiektu wywołującego.

Program obsługi komunikatów, który dodaje niestandardowego nagłówka do żądania wychodzącego można znaleźć w poniższym przykładzie:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Wywołanie `base.SendAsync` jest asynchroniczna. Jeśli program obsługi ma wszelkie prace po tym wywołaniu, użyj **await** słowo kluczowe, aby wznowić wykonywanie po zakończeniu działania metody. Program obsługi, który rejestruje kody błędów można znaleźć w poniższym przykładzie. Rejestrowanie, sama nie jest bardzo interesujące, ale w przykładzie pokazano, jak uzyskać w odpowiedzi wewnątrz programu obsługi.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Dodawanie programów obsługi wiadomości do potoku klienta

Aby dodać niestandardowe programy obsługi do **HttpClient**, użyj **HttpClientFactory.Create** metody:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Programy obsługi komunikatów są wywoływane w kolejności, który jest przekazywany do **Utwórz** metody. Ponieważ programy obsługi są zagnieżdżone, komunikat odpowiedzi jest wysyłane w drugą stronę. Ostatnią procedurę obsługi jest pierwszy komunikat odpowiedzi.
