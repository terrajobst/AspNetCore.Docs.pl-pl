---
uid: web-api/overview/advanced/http-message-handlers
title: "Programy obsługi komunikatów HTTP w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="bb467-102">Programy obsługi komunikatów HTTP w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="bb467-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="bb467-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bb467-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bb467-104">A *obsługi wiadomości* jest klasa, która odbiera żądania HTTP i zwraca odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="bb467-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="bb467-105">Programy obsługi wiadomości pochodzi z klasy abstrakcyjnej **HttpMessageHandler** klasy.</span><span class="sxs-lookup"><span data-stu-id="bb467-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="bb467-106">Zazwyczaj szereg obsługi komunikatów są połączone.</span><span class="sxs-lookup"><span data-stu-id="bb467-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="bb467-107">Pierwsza metoda obsługi odbiera żądanie HTTP, nie przetwarza i przekazuje żądania do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="bb467-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="bb467-108">W pewnym momencie odpowiedzi jest tworzona i rośnie łańcucha.</span><span class="sxs-lookup"><span data-stu-id="bb467-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="bb467-109">Ten wzorzec jest nazywany *delegowanie* obsługi.</span><span class="sxs-lookup"><span data-stu-id="bb467-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="bb467-110">Programy obsługi wiadomości po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="bb467-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="bb467-111">Po stronie serwera potok składnika Web API używa niektóre programy obsługi komunikatów wbudowany:</span><span class="sxs-lookup"><span data-stu-id="bb467-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="bb467-112">**HttpServer** pobiera żądania od hosta.</span><span class="sxs-lookup"><span data-stu-id="bb467-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="bb467-113">**HttpRoutingDispatcher** wywołuje żądania na podstawie trasy.</span><span class="sxs-lookup"><span data-stu-id="bb467-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="bb467-114">**HttpControllerDispatcher** wysyła żądanie do kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bb467-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="bb467-115">Niestandardowe programy obsługi można dodać do potoku.</span><span class="sxs-lookup"><span data-stu-id="bb467-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="bb467-116">Programy obsługi komunikatów są dobrym dotyczy kompleksowymi, które działają na poziomie HTTP wiadomości (zamiast akcji kontrolera).</span><span class="sxs-lookup"><span data-stu-id="bb467-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="bb467-117">Może na przykład program obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="bb467-117">For example, a message handler might:</span></span>

- <span data-ttu-id="bb467-118">Odczyt lub modyfikacja nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="bb467-118">Read or modify request headers.</span></span>
- <span data-ttu-id="bb467-119">Dodaj nagłówek odpowiedzi do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bb467-119">Add a response header to responses.</span></span>
- <span data-ttu-id="bb467-120">Sprawdzanie poprawności żądań przed dotarciem kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bb467-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="bb467-121">Ten diagram przedstawia dwa niestandardowe programy obsługi dodaje do potoku:</span><span class="sxs-lookup"><span data-stu-id="bb467-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="bb467-122">Po stronie klienta HttpClient używa programy obsługi wiadomości.</span><span class="sxs-lookup"><span data-stu-id="bb467-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="bb467-123">Aby uzyskać więcej informacji, zobacz [obsługi komunikatów HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="bb467-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="bb467-124">Programy obsługi wiadomości niestandardowych</span><span class="sxs-lookup"><span data-stu-id="bb467-124">Custom Message Handlers</span></span>

<span data-ttu-id="bb467-125">Aby napisać program obsługi komunikatów niestandardowych, pochodzi z **System.Net.Http.DelegatingHandler** i zastąpić **SendAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="bb467-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="bb467-126">Ta metoda ma następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="bb467-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="bb467-127">Metoda korzysta z **HttpRequestMessage** jako dane wejściowe i asynchronicznie zwraca **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="bb467-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="bb467-128">Typowa implementacja wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="bb467-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="bb467-129">Proces komunikat żądania.</span><span class="sxs-lookup"><span data-stu-id="bb467-129">Process the request message.</span></span>
2. <span data-ttu-id="bb467-130">Wywołanie `base.SendAsync` można wysłać żądania do wewnętrznego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="bb467-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="bb467-131">Wewnętrzny program obsługi zwraca komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bb467-131">The inner handler returns a response message.</span></span> <span data-ttu-id="bb467-132">(Ten krok jest asynchroniczne).</span><span class="sxs-lookup"><span data-stu-id="bb467-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="bb467-133">Przetwarzanie odpowiedzi i przywrócić go do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="bb467-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="bb467-134">Oto przykład prosta:</span><span class="sxs-lookup"><span data-stu-id="bb467-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="bb467-135">Wywołanie `base.SendAsync` jest asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="bb467-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="bb467-136">Jeśli program obsługi działa żadnych po to wywołanie, użyj **await** — słowo kluczowe, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="bb467-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="bb467-137">Delegujące obsługi można również pominąć wewnętrznym programem obsługi i bezpośrednio utworzyć odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="bb467-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="bb467-138">Jeśli delegowanie obsługi tworzy odpowiedź bez wywoływania elementu `base.SendAsync`, żądanie pomija pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="bb467-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="bb467-139">Może to być przydatne w przypadku obsługi, która weryfikuje żądanie (Tworzenie odpowiedzi błędu).</span><span class="sxs-lookup"><span data-stu-id="bb467-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="bb467-140">Dodawanie obsługi do potoku</span><span class="sxs-lookup"><span data-stu-id="bb467-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="bb467-141">Aby dodać obsługi wiadomości po stronie serwera, Dodaj program obsługi do **HttpConfiguration.MessageHandlers** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bb467-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="bb467-142">Jeśli szablon "Platformy ASP.NET MVC 4 aplikacji sieci Web" są używane do tworzenia projektu, możesz to zrobić to wewnątrz **WebApiConfig** klasy:</span><span class="sxs-lookup"><span data-stu-id="bb467-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="bb467-143">Programy obsługi komunikatów są nazywane w tej samej kolejności, w jakiej występują w **MessageHandlers** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bb467-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="bb467-144">Ponieważ są one zagnieżdżone, komunikat odpowiedzi porusza się w innym kierunku.</span><span class="sxs-lookup"><span data-stu-id="bb467-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="bb467-145">Obsługa ostatniej jest pierwszą osobą, która jest wyświetlany komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bb467-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="bb467-146">Zwróć uwagę, że nie trzeba ustawić obsługi wewnętrzny; strukturę interfejsu API sieci Web automatycznie łączy programy obsługi wiadomości.</span><span class="sxs-lookup"><span data-stu-id="bb467-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="bb467-147">Jeśli jesteś [hostingu samodzielnego](../older-versions/self-host-a-web-api.md), Utwórz wystąpienie **HttpSelfHostConfiguration** klasy i Dodaj do obsługi **MessageHandlers** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="bb467-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="bb467-148">Teraz Przyjrzyjmy się przykłady obsługi komunikatów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="bb467-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="bb467-149">Przykład: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="bb467-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="bb467-150">X-HTTP-Method-Override jest niestandardowy nagłówek HTTP.</span><span class="sxs-lookup"><span data-stu-id="bb467-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="bb467-151">Jest on przeznaczony dla klientów, którzy nie mogą wysyłać niektóre typy żądań HTTP, takie jak PUT i DELETE.</span><span class="sxs-lookup"><span data-stu-id="bb467-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="bb467-152">Zamiast tego klient wysyła żądanie POST i ustawia dla nagłówka X-HTTP-Method-Override żądanej metody.</span><span class="sxs-lookup"><span data-stu-id="bb467-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="bb467-153">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bb467-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="bb467-154">Oto obsługi wiadomości, który dodaje obsługę X-HTTP-Method-Override:</span><span class="sxs-lookup"><span data-stu-id="bb467-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="bb467-155">W **SendAsync** metody, program obsługi sprawdza, czy komunikat żądania jest wysłanie żądania POST i czy zawiera nagłówek X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="bb467-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="bb467-156">Jeśli tak, sprawdza poprawność wartości nagłówka, a następnie modyfikuje metodę żądania.</span><span class="sxs-lookup"><span data-stu-id="bb467-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="bb467-157">Na koniec wywołuje program obsługi `base.SendAsync` do przekazywania wiadomości do następnej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="bb467-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="bb467-158">Gdy żądanie dotrze **HttpControllerDispatcher** klasy **HttpControllerDispatcher** będzie przekierowywać żądania na podstawie metody żądania zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="bb467-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="bb467-159">Przykład: Dodawanie nagłówka odpowiedzi niestandardowych</span><span class="sxs-lookup"><span data-stu-id="bb467-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="bb467-160">Oto obsługi wiadomości, umożliwiający dodawanie niestandardowego nagłówka do każdej wiadomości odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="bb467-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="bb467-161">Po pierwsze, wywołuje program obsługi `base.SendAsync` do przekazania żądania do obsługi wiadomości wewnętrzny.</span><span class="sxs-lookup"><span data-stu-id="bb467-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="bb467-162">Wewnętrzny program obsługi zwraca komunikat odpowiedzi, ale pojawia się więc asynchronicznie za pomocą **zadań&lt;T&gt;**  obiektu.</span><span class="sxs-lookup"><span data-stu-id="bb467-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="bb467-163">Komunikat odpowiedzi nie jest dostępna do `base.SendAsync` kończy asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="bb467-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="bb467-164">W tym przykładzie użyto **await** — słowo kluczowe do wykonywania pracy asynchronicznie po `SendAsync` zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="bb467-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="bb467-165">Jeśli ma być przeznaczona dla programu .NET Framework 4.0, użyj **zadań**&lt;T&gt;**. ContinueWith** metody:</span><span class="sxs-lookup"><span data-stu-id="bb467-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="bb467-166">Przykład: Sprawdzanie klucz interfejsu API</span><span class="sxs-lookup"><span data-stu-id="bb467-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="bb467-167">Niektóre usługi sieci web wymaga klientów obejmują klucz interfejsu API w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="bb467-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="bb467-168">W poniższym przykładzie pokazano, jak sprawdzić obsługi wiadomości żądania prawidłowy klucz interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="bb467-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="bb467-169">Ten program obsługi jest szuka klucz interfejsu API w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="bb467-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="bb467-170">(W tym przykładzie przyjęto założenie, że klucz jest statyczny ciąg.</span><span class="sxs-lookup"><span data-stu-id="bb467-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="bb467-171">Rzeczywistego wykonania prawdopodobnie użyje bardziej złożonych reguł sprawdzania poprawności.) Jeśli ciąg zapytania zawiera klucz, program obsługi przekazuje żądanie do wewnętrznego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="bb467-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="bb467-172">Jeśli żądanie nie zawiera prawidłowego klucza, program obsługi tworzy komunikat odpowiedzi o stanie 403, zabroniony.</span><span class="sxs-lookup"><span data-stu-id="bb467-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="bb467-173">W takim przypadku nie wywołuje program obsługi `base.SendAsync`, więc wewnętrznym programem obsługi. nigdy nie odbiera żądanie, ani nie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bb467-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="bb467-174">W związku z tym kontrolerem można założyć, że wszystkie żądania przychodzące ma prawidłowy klucz interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="bb467-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="bb467-175">Jeśli klucz interfejsu API ma zastosowanie tylko do określonych akcji kontrolera, należy rozważyć użycie filtr akcji zamiast obsługi wiadomości.</span><span class="sxs-lookup"><span data-stu-id="bb467-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="bb467-176">Filtry akcji Uruchom po wykonaniu routingu identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="bb467-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="bb467-177">Programy obsługi wiadomości trasy</span><span class="sxs-lookup"><span data-stu-id="bb467-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="bb467-178">Programy obsługi zdarzeń w **HttpConfiguration.MessageHandlers** kolekcji są stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="bb467-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="bb467-179">Alternatywnie można dodać obsługi wiadomości do określonej trasy, podczas definiowania trasy:</span><span class="sxs-lookup"><span data-stu-id="bb467-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="bb467-180">W tym przykładzie, jeśli pasuje do identyfikatora URI żądania "Route2", żądanie jest wysyłane do `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="bb467-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="bb467-181">Na poniższym diagramie przedstawiono potoku dla tych na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="bb467-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="bb467-182">Zwróć uwagę, że `MessageHandler2` zastępuje domyślny **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="bb467-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="bb467-183">W tym przykładzie `MessageHandler2` tworzy odpowiedzi i żądania zgodnych "Route2" nigdy nie przechodź do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bb467-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="bb467-184">Dzięki temu można zastąpić cały mechanizm kontrolera interfejsu API sieci Web własne niestandardowe punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bb467-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="bb467-185">Alternatywnie można delegować program obsługi komunikatów dla trasy **HttpControllerDispatcher**, który następnie wysyła do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bb467-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="bb467-186">Poniższy kod przedstawia sposób konfigurowania tej trasy:</span><span class="sxs-lookup"><span data-stu-id="bb467-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
