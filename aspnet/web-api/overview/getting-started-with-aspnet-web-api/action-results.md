---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Akcja wyniki w interfejsie Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 74a837cb9f606d78fb516968343d498d3c37c4ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808930"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="8b0eb-102">Wyniki akcji w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="8b0eb-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="8b0eb-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8b0eb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8b0eb-104">W tym temacie opisano, jak ASP.NET Web API konwertuje wartość zwracaną akcji kontrolera, do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="8b0eb-105">Akcja kontrolera interfejsu API sieci Web może zwracać dowolne z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="8b0eb-106">void</span><span class="sxs-lookup"><span data-stu-id="8b0eb-106">void</span></span>
2. <span data-ttu-id="8b0eb-107">**Element HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="8b0eb-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="8b0eb-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="8b0eb-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="8b0eb-109">Innego typu</span><span class="sxs-lookup"><span data-stu-id="8b0eb-109">Some other type</span></span>

<span data-ttu-id="8b0eb-110">W zależności od tego, który z nich jest zwracany, interfejsu API sieci Web używa innego mechanizmu do tworzenia odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="8b0eb-111">Zwracany typ</span><span class="sxs-lookup"><span data-stu-id="8b0eb-111">Return type</span></span> | <span data-ttu-id="8b0eb-112">Jak interfejs API sieci Web tworzy odpowiedź</span><span class="sxs-lookup"><span data-stu-id="8b0eb-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="8b0eb-113">void</span><span class="sxs-lookup"><span data-stu-id="8b0eb-113">void</span></span> | <span data-ttu-id="8b0eb-114">Zwraca pusty 204 (Brak zawartości)</span><span class="sxs-lookup"><span data-stu-id="8b0eb-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="8b0eb-115">**Element HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="8b0eb-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="8b0eb-116">Konwertuj bezpośrednio do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="8b0eb-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="8b0eb-117">**IHttpActionResult**</span></span> | <span data-ttu-id="8b0eb-118">Wywołaj **ExecuteAsync** utworzyć **obiektu HttpResponseMessage**, następnie przekonwertować komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="8b0eb-119">Inny typ</span><span class="sxs-lookup"><span data-stu-id="8b0eb-119">Other type</span></span> | <span data-ttu-id="8b0eb-120">Zapisz wartość zwracaną serializacji do treści odpowiedzi; Zwraca 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="8b0eb-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="8b0eb-121">Pozostała część tego tematu opisano wszystkie opcje, które bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="8b0eb-122">void</span><span class="sxs-lookup"><span data-stu-id="8b0eb-122">void</span></span>

<span data-ttu-id="8b0eb-123">Jeśli typ zwracany jest `void`, interfejs API sieci Web po prostu zwraca pustą odpowiedź HTTP z kodem stanu 204 (Brak zawartości).</span><span class="sxs-lookup"><span data-stu-id="8b0eb-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="8b0eb-124">Przykład kontrolera:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="8b0eb-125">Odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="8b0eb-126">Element HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="8b0eb-126">HttpResponseMessage</span></span>

<span data-ttu-id="8b0eb-127">Jeśli działanie zwróci [obiektu HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), internetowy interfejs API konwertuje wartość zwracaną bezpośrednio na komunikat odpowiedzi HTTP przy użyciu właściwości **obiektu HttpResponseMessage** obiekt do wypełnienia odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="8b0eb-128">Ta opcja zapewnia wysoki poziom kontroli nad komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="8b0eb-129">Na przykład następującej akcji kontrolera ustawia nagłówek Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="8b0eb-130">Odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="8b0eb-131">W przypadku przekazania model domeny, aby **CreateResponse** metody, interfejs API sieci Web używa [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) do zapisania Zserializowany model na treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="8b0eb-132">Internetowy interfejs API używa nagłówek Accept w żądaniu, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="8b0eb-133">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="8b0eb-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="8b0eb-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="8b0eb-134">IHttpActionResult</span></span>

<span data-ttu-id="8b0eb-135">**IHttpActionResult** interfejsu została wprowadzona w sieci Web API 2.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="8b0eb-136">Zasadniczo definiuje **obiektu HttpResponseMessage** fabryki.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="8b0eb-137">Oto niektóre korzyści wynikające z używania **IHttpActionResult** interfejsu:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="8b0eb-138">Upraszcza [testy jednostkowe](../testing-and-debugging/unit-testing-controllers-in-web-api.md) kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="8b0eb-139">Przenosi wspólnej logiki do tworzenia odpowiedzi HTTP do osobnych klas.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="8b0eb-140">Sprawia, że celem akcji kontrolera, bardziej zrozumiały, ukrywając niskopoziomowych szczegółów konstruowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="8b0eb-141">**IHttpActionResult** zawiera jedną metodę **ExecuteAsync**, które asynchronicznie tworzy **obiektu HttpResponseMessage** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="8b0eb-142">Jeśli akcja kontrolera zwraca **IHttpActionResult**, wywołań interfejsu API sieci Web **ExecuteAsync** metodę w celu utworzenia **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="8b0eb-143">A następnie konwertuje **obiektu HttpResponseMessage** do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="8b0eb-144">Poniżej przedstawiono prosty przewidujące z **IHttpActionResult** tworząca odpowiedź jako zwykły tekst:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="8b0eb-145">Przykład akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="8b0eb-146">Odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="8b0eb-147">Częściej, użyjesz **IHttpActionResult** zdefiniowane w implementacji **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="8b0eb-148">**Klasy ApiController** klasa definiuje metody pomocnika, które zwracają wyniki tych wbudowanych akcji.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="8b0eb-149">W poniższym przykładzie, jeśli żądanie nie jest zgodny z istniejący identyfikator produktu, kontroler wywołuje [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) do tworzenia odpowiedzi 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="8b0eb-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="8b0eb-150">W przeciwnym razie kontroler wywołuje [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), która tworzy odpowiedź 200 (OK), zawiera produktu.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="8b0eb-151">Inne typy zwracane</span><span class="sxs-lookup"><span data-stu-id="8b0eb-151">Other Return Types</span></span>

<span data-ttu-id="8b0eb-152">Dla wszystkich innych typów zwrotu, korzysta z interfejsu API sieci Web [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) serializować wartość zwracaną.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="8b0eb-153">Interfejs API sieci Web zapisuje wartość zserializowana do treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="8b0eb-154">Kod stanu odpowiedzi to 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="8b0eb-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="8b0eb-155">Wadą tego podejścia jest, że bezpośrednio nie może zwrócić kod błędu, takie jak 404.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="8b0eb-156">Jednak może zgłosić **HttpResponseException** dla kodów błędów.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="8b0eb-157">Aby uzyskać więcej informacji, zobacz [Exception Handling in Web API platformy ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="8b0eb-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="8b0eb-158">Internetowy interfejs API używa nagłówek Accept w żądaniu, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="8b0eb-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="8b0eb-159">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="8b0eb-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="8b0eb-160">Przykładowe żądanie</span><span class="sxs-lookup"><span data-stu-id="8b0eb-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="8b0eb-161">Przykładowa odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="8b0eb-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
