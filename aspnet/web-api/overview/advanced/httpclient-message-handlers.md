---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Programy obsługi komunikatów HttpClient w interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831375"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="0ef64-102">Programy obsługi komunikatów HttpClient w interfejsie Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0ef64-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="0ef64-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0ef64-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0ef64-104">A *obsługi wiadomości* to klasa, która odbierze żądanie HTTP, a następnie zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="0ef64-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="0ef64-105">Zazwyczaj szereg programy obsługi komunikatów są połączone.</span><span class="sxs-lookup"><span data-stu-id="0ef64-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="0ef64-106">Pierwszego programu obsługi odebrania żądania HTTP, przetwarza je i zapewnia żądania do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="0ef64-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="0ef64-107">W pewnym momencie odpowiedzi jest tworzony i powraca łańcuch.</span><span class="sxs-lookup"><span data-stu-id="0ef64-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="0ef64-108">Ten wzorzec jest nazywany *delegowanie* programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="0ef64-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="0ef64-109">Po stronie klienta **HttpClient** klasa używa obsługi wiadomości do przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="0ef64-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="0ef64-110">Domyślny program obsługi jest **HttpClientHandler**, który wysyła żądanie za pośrednictwem sieci i pobiera odpowiedź z serwera.</span><span class="sxs-lookup"><span data-stu-id="0ef64-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="0ef64-111">Programy obsługi komunikatów niestandardowych można wstawić do potoku klienta:</span><span class="sxs-lookup"><span data-stu-id="0ef64-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="0ef64-112">Web API platformy ASP.NET używa także procedury obsługi komunikatów po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="0ef64-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="0ef64-113">Aby uzyskać więcej informacji, zobacz [programów obsługi komunikatów HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="0ef64-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="0ef64-114">Programy obsługi komunikatów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="0ef64-114">Custom Message Handlers</span></span>

<span data-ttu-id="0ef64-115">Aby napisać program obsługi komunikatów niestandardowych, pochodzi od **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="0ef64-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="0ef64-116">W tym miejscu znajduje się sygnatura metody:</span><span class="sxs-lookup"><span data-stu-id="0ef64-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="0ef64-117">Ta metoda przyjmuje **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="0ef64-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="0ef64-118">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0ef64-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="0ef64-119">Przetwarzanie komunikatu żądania.</span><span class="sxs-lookup"><span data-stu-id="0ef64-119">Process the request message.</span></span>
2. <span data-ttu-id="0ef64-120">Wywołaj `base.SendAsync` wysyłać żądania do wewnętrznego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="0ef64-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="0ef64-121">Wewnętrzny program obsługi zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0ef64-121">The inner handler returns a response message.</span></span> <span data-ttu-id="0ef64-122">(Ten krok jest asynchroniczne).</span><span class="sxs-lookup"><span data-stu-id="0ef64-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="0ef64-123">Przetworzenie odpowiedzi i zwraca go do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="0ef64-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="0ef64-124">Program obsługi komunikatów, który dodaje niestandardowego nagłówka do żądania wychodzącego można znaleźć w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0ef64-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="0ef64-125">Wywołanie `base.SendAsync` jest asynchroniczna.</span><span class="sxs-lookup"><span data-stu-id="0ef64-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="0ef64-126">Jeśli program obsługi ma wszelkie prace po tym wywołaniu, użyj **await** słowo kluczowe, aby wznowić wykonywanie po zakończeniu działania metody.</span><span class="sxs-lookup"><span data-stu-id="0ef64-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="0ef64-127">Program obsługi, który rejestruje kody błędów można znaleźć w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="0ef64-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="0ef64-128">Rejestrowanie, sama nie jest bardzo interesujące, ale w przykładzie pokazano, jak uzyskać w odpowiedzi wewnątrz programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="0ef64-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="0ef64-129">Dodawanie programów obsługi wiadomości do potoku klienta</span><span class="sxs-lookup"><span data-stu-id="0ef64-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="0ef64-130">Aby dodać niestandardowe programy obsługi do **HttpClient**, użyj **HttpClientFactory.Create** metody:</span><span class="sxs-lookup"><span data-stu-id="0ef64-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="0ef64-131">Programy obsługi komunikatów są wywoływane w kolejności, który jest przekazywany do **Utwórz** metody.</span><span class="sxs-lookup"><span data-stu-id="0ef64-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="0ef64-132">Ponieważ programy obsługi są zagnieżdżone, komunikat odpowiedzi jest wysyłane w drugą stronę.</span><span class="sxs-lookup"><span data-stu-id="0ef64-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="0ef64-133">Ostatnią procedurę obsługi jest pierwszy komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0ef64-133">That is, the last handler is the first to get the response message.</span></span>
