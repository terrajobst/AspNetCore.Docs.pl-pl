---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "Akcja powoduje składnika Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 68b82661b97434795e1c306b168033dfcde529bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="26b80-102">Wyniki akcji w składniku Web API 2</span><span class="sxs-lookup"><span data-stu-id="26b80-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="26b80-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="26b80-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="26b80-104">W tym temacie opisano, jak składnika ASP.NET Web API konwertuje wartość zwracaną akcji kontrolera do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="26b80-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="26b80-105">Akcja kontrolera interfejsu API sieci Web może zwracać jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="26b80-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="26b80-106">void</span><span class="sxs-lookup"><span data-stu-id="26b80-106">void</span></span>
2. <span data-ttu-id="26b80-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="26b80-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="26b80-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="26b80-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="26b80-109">Innego typu</span><span class="sxs-lookup"><span data-stu-id="26b80-109">Some other type</span></span>

<span data-ttu-id="26b80-110">W zależności od tego, który z nich jest zwracany, interfejsu API sieci Web używa inny mechanizm do tworzenia odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="26b80-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="26b80-111">Zwracany typ</span><span class="sxs-lookup"><span data-stu-id="26b80-111">Return type</span></span> | <span data-ttu-id="26b80-112">Jak interfejsu API sieci Web tworzy odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="26b80-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="26b80-113">void</span><span class="sxs-lookup"><span data-stu-id="26b80-113">void</span></span> | <span data-ttu-id="26b80-114">Zwraca pusty 204 (bez zawartości)</span><span class="sxs-lookup"><span data-stu-id="26b80-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="26b80-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="26b80-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="26b80-116">Konwertuj bezpośrednio do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="26b80-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="26b80-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="26b80-117">**IHttpActionResult**</span></span> | <span data-ttu-id="26b80-118">Wywołanie **ExecuteAsync** utworzyć **HttpResponseMessage**, następnie Konwertuj na komunikat odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="26b80-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="26b80-119">Innego typu</span><span class="sxs-lookup"><span data-stu-id="26b80-119">Other type</span></span> | <span data-ttu-id="26b80-120">Zapisywanie serializacji wartości zwracanej w treści odpowiedzi; Zwróć 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="26b80-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="26b80-121">Pozostała część tego tematu opisano poszczególne opcje więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="26b80-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="26b80-122">void</span><span class="sxs-lookup"><span data-stu-id="26b80-122">void</span></span>

<span data-ttu-id="26b80-123">Jeśli typem zwracanym jest `void`, interfejsu API sieci Web po prostu zwraca pustą odpowiedź HTTP z kodem stanu 204 (bez zawartości).</span><span class="sxs-lookup"><span data-stu-id="26b80-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="26b80-124">Przykład controller:</span><span class="sxs-lookup"><span data-stu-id="26b80-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="26b80-125">Odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="26b80-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="26b80-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="26b80-126">HttpResponseMessage</span></span>

<span data-ttu-id="26b80-127">Jeśli akcja zwraca [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx), interfejsu API sieci Web konwertuje wartości zwracanej bezpośrednio na komunikat odpowiedzi HTTP przy użyciu właściwości **HttpResponseMessage** obiektu, aby wypełnić odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="26b80-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="26b80-128">Ta opcja umożliwia szczegółową kontrolę nad komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="26b80-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="26b80-129">Na przykład następująca Akcja kontrolera ustawia nagłówek Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="26b80-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="26b80-130">Odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="26b80-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="26b80-131">W przypadku przekazania modelu domeny do **CreateResponse** metoda, używa interfejsu API sieci Web [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) do zapisania modelem serializacji w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="26b80-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="26b80-132">Interfejsu API sieci Web używa nagłówek Accept w żądaniu, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="26b80-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="26b80-133">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="26b80-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="26b80-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="26b80-134">IHttpActionResult</span></span>

<span data-ttu-id="26b80-135">**IHttpActionResult** interfejs został wprowadzony w sieci Web API 2.</span><span class="sxs-lookup"><span data-stu-id="26b80-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="26b80-136">Zasadniczo definiuje **HttpResponseMessage** fabryki.</span><span class="sxs-lookup"><span data-stu-id="26b80-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="26b80-137">Oto niektóre korzyści wynikające z używania **IHttpActionResult** interfejsu:</span><span class="sxs-lookup"><span data-stu-id="26b80-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="26b80-138">Upraszcza [testy jednostkowe](../testing-and-debugging/unit-testing-controllers-in-web-api.md) kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="26b80-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="26b80-139">Przenosi wspólnej logiki do tworzenia odpowiedzi HTTP do osobnych klas.</span><span class="sxs-lookup"><span data-stu-id="26b80-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="26b80-140">Sprawia, że celem akcji kontrolera jaśniejszy, ukrywając szczegółów niskiego poziomu konstruowanie odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="26b80-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="26b80-141">**IHttpActionResult** zawiera jedną metodę **ExecuteAsync**, które asynchronicznie tworzy **HttpResponseMessage** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="26b80-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="26b80-142">Jeśli akcja kontrolera zwraca **IHttpActionResult**, wywołania interfejsu API sieci Web **ExecuteAsync** metodę w celu utworzenia **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="26b80-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="26b80-143">Następnie konwertuje **HttpResponseMessage** do komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="26b80-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="26b80-144">Oto prosty przewidujące z **IHttpActionResult** tworzącą odpowiedź jako zwykły tekst:</span><span class="sxs-lookup"><span data-stu-id="26b80-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="26b80-145">Przykład akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="26b80-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="26b80-146">Odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="26b80-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="26b80-147">Częściej, którego użyjesz **IHttpActionResult** zdefiniowane w implementacji  **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)**  przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="26b80-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="26b80-148">**Klasy ApiController** klasa definiuje metody pomocnicze, które zwracają wyniki tych działań wbudowanych.</span><span class="sxs-lookup"><span data-stu-id="26b80-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="26b80-149">W poniższym przykładzie, jeśli żądanie nie pasuje do istniejącego Identyfikatora produktu, kontrolera wywołuje [ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) do tworzenia odpowiedzi 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="26b80-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="26b80-150">W przeciwnym razie wywołuje kontrolera [ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx), który utworzy odpowiedź 200 (OK) która zawiera produktu.</span><span class="sxs-lookup"><span data-stu-id="26b80-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="26b80-151">Inne typy zwracane</span><span class="sxs-lookup"><span data-stu-id="26b80-151">Other Return Types</span></span>

<span data-ttu-id="26b80-152">Dla wszystkich innych typów zwracanych, używa interfejsu API sieci Web [nośnika elementu formatującego](../formats-and-model-binding/media-formatters.md) do serializacji wartości zwracanej.</span><span class="sxs-lookup"><span data-stu-id="26b80-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="26b80-153">Interfejs API sieci Web zapisuje wartość zserializowana do treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="26b80-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="26b80-154">Kod stanu odpowiedzi jest 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="26b80-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="26b80-155">Wadą tego podejścia to, że bezpośrednio nie może zwrócić kodu błędu, takie jak 404.</span><span class="sxs-lookup"><span data-stu-id="26b80-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="26b80-156">Jednak może zgłosić **HttpResponseException** kodów błędów.</span><span class="sxs-lookup"><span data-stu-id="26b80-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="26b80-157">Aby uzyskać więcej informacji, zobacz [obsługi wyjątków w ASP.NET Web API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="26b80-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="26b80-158">Interfejsu API sieci Web używa nagłówek Accept w żądaniu, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="26b80-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="26b80-159">Aby uzyskać więcej informacji, zobacz [negocjacje zawartości](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="26b80-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="26b80-160">Przykładowe żądanie</span><span class="sxs-lookup"><span data-stu-id="26b80-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="26b80-161">Przykład odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="26b80-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
