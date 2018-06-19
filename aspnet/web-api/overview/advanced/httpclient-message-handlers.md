---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Programy obsługi wiadomości HttpClient w składniku ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566342"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Programy obsługi wiadomości HttpClient w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

A *obsługi wiadomości* jest klasa, która odbiera żądania HTTP i zwraca odpowiedź HTTP.

Zazwyczaj szereg obsługi komunikatów są połączone. Pierwsza metoda obsługi odbiera żądanie HTTP, nie przetwarza i przekazuje żądania do następnej procedury obsługi. W pewnym momencie odpowiedzi jest tworzona i rośnie łańcucha. Ten wzorzec jest nazywany *delegowanie* obsługi.

![](httpclient-message-handlers/_static/image1.png)

Po stronie klienta **HttpClient** klasy używa obsługi wiadomości do przetwarzania żądań. Domyślny program obsługi jest **HttpClientHandler**, który wysyła żądanie za pośrednictwem sieci i pobiera odpowiedź z serwera. Programy obsługi wiadomości niestandardowych można wstawiać do potoku klienta:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> Interfejs API sieci Web platformy ASP.NET używa również programy obsługi wiadomości po stronie serwera. Aby uzyskać więcej informacji, zobacz [obsługi komunikatów HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Programy obsługi wiadomości niestandardowych

Aby napisać program obsługi komunikatów niestandardowych, pochodzi z **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody. Podpis metody jest następujący:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Metoda korzysta z **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**. Typowa implementacja wykonuje następujące czynności:

1. Proces komunikat żądania.
2. Wywołanie `base.SendAsync` można wysłać żądania do wewnętrznego programu obsługi.
3. Wewnętrzny program obsługi zwraca komunikat odpowiedzi. (Ten krok jest asynchroniczne).
4. Przetwarzanie odpowiedzi i przywrócić go do obiektu wywołującego.

W poniższym przykładzie przedstawiono obsługi wiadomości, umożliwiający dodawanie niestandardowego nagłówka do żądania wychodzącego:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Wywołanie `base.SendAsync` jest asynchroniczne. Jeśli program obsługi działa żadnych po to wywołanie, użyj **await** — słowo kluczowe można wznowić wykonywania po zakończeniu metody. W poniższym przykładzie przedstawiono program obsługi, który rejestruje kody błędów. Rejestrowanie się nie jest bardzo interesujące, ale w przykładzie pokazano, jak uzyskać w odpowiedzi wewnątrz obsługi.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Dodawanie programów obsługi wiadomości do potoku klienta

Aby dodać niestandardowe programy obsługi, aby **HttpClient**, użyj **HttpClientFactory.Create** metody:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Programy obsługi komunikatów są wywoływane w kolejności ich do przekazania **Utwórz** metody. Ponieważ programy obsługi są zagnieżdżone, komunikat odpowiedzi porusza się w innym kierunku. Obsługa ostatniej jest pierwszą osobą, która jest wyświetlany komunikat odpowiedzi.
