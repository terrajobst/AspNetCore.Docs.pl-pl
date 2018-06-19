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
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="5f001-102">Programy obsługi wiadomości HttpClient w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="5f001-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="5f001-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5f001-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5f001-104">A *obsługi wiadomości* jest klasa, która odbiera żądania HTTP i zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f001-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="5f001-105">Zazwyczaj szereg obsługi komunikatów są połączone.</span><span class="sxs-lookup"><span data-stu-id="5f001-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="5f001-106">Pierwsza metoda obsługi odbiera żądanie HTTP, nie przetwarza i przekazuje żądania do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="5f001-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="5f001-107">W pewnym momencie odpowiedzi jest tworzona i rośnie łańcucha.</span><span class="sxs-lookup"><span data-stu-id="5f001-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="5f001-108">Ten wzorzec jest nazywany *delegowanie* obsługi.</span><span class="sxs-lookup"><span data-stu-id="5f001-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="5f001-109">Po stronie klienta **HttpClient** klasy używa obsługi wiadomości do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="5f001-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="5f001-110">Domyślny program obsługi jest **HttpClientHandler**, który wysyła żądanie za pośrednictwem sieci i pobiera odpowiedź z serwera.</span><span class="sxs-lookup"><span data-stu-id="5f001-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="5f001-111">Programy obsługi wiadomości niestandardowych można wstawiać do potoku klienta:</span><span class="sxs-lookup"><span data-stu-id="5f001-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="5f001-112">Interfejs API sieci Web platformy ASP.NET używa również programy obsługi wiadomości po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="5f001-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="5f001-113">Aby uzyskać więcej informacji, zobacz [obsługi komunikatów HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="5f001-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="5f001-114">Programy obsługi wiadomości niestandardowych</span><span class="sxs-lookup"><span data-stu-id="5f001-114">Custom Message Handlers</span></span>

<span data-ttu-id="5f001-115">Aby napisać program obsługi komunikatów niestandardowych, pochodzi z **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="5f001-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="5f001-116">Podpis metody jest następujący:</span><span class="sxs-lookup"><span data-stu-id="5f001-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="5f001-117">Metoda korzysta z **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="5f001-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="5f001-118">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5f001-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="5f001-119">Proces komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="5f001-119">Process the request message.</span></span>
2. <span data-ttu-id="5f001-120">Wywołanie `base.SendAsync` można wysłać żądania do wewnętrznego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="5f001-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="5f001-121">Wewnętrzny program obsługi zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5f001-121">The inner handler returns a response message.</span></span> <span data-ttu-id="5f001-122">(Ten krok jest asynchroniczne).</span><span class="sxs-lookup"><span data-stu-id="5f001-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="5f001-123">Przetwarzanie odpowiedzi i przywrócić go do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="5f001-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="5f001-124">W poniższym przykładzie przedstawiono obsługi wiadomości, umożliwiający dodawanie niestandardowego nagłówka do żądania wychodzącego:</span><span class="sxs-lookup"><span data-stu-id="5f001-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="5f001-125">Wywołanie `base.SendAsync` jest asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="5f001-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="5f001-126">Jeśli program obsługi działa żadnych po to wywołanie, użyj **await** — słowo kluczowe można wznowić wykonywania po zakończeniu metody.</span><span class="sxs-lookup"><span data-stu-id="5f001-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="5f001-127">W poniższym przykładzie przedstawiono program obsługi, który rejestruje kody błędów.</span><span class="sxs-lookup"><span data-stu-id="5f001-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="5f001-128">Rejestrowanie się nie jest bardzo interesujące, ale w przykładzie pokazano, jak uzyskać w odpowiedzi wewnątrz obsługi.</span><span class="sxs-lookup"><span data-stu-id="5f001-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="5f001-129">Dodawanie programów obsługi wiadomości do potoku klienta</span><span class="sxs-lookup"><span data-stu-id="5f001-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="5f001-130">Aby dodać niestandardowe programy obsługi, aby **HttpClient**, użyj **HttpClientFactory.Create** metody:</span><span class="sxs-lookup"><span data-stu-id="5f001-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="5f001-131">Programy obsługi komunikatów są wywoływane w kolejności ich do przekazania **Utwórz** metody.</span><span class="sxs-lookup"><span data-stu-id="5f001-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="5f001-132">Ponieważ programy obsługi są zagnieżdżone, komunikat odpowiedzi porusza się w innym kierunku.</span><span class="sxs-lookup"><span data-stu-id="5f001-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="5f001-133">Obsługa ostatniej jest pierwszą osobą, która jest wyświetlany komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5f001-133">That is, the last handler is the first to get the response message.</span></span>
