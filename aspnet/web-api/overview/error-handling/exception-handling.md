---
uid: web-api/overview/error-handling/exception-handling
title: Obsługa wyjątków w Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807664"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="8811b-102">Obsługa wyjątków w Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8811b-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="8811b-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8811b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8811b-104">W tym artykule opisano, błędów i obsługa wyjątków w Web API platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8811b-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="8811b-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="8811b-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="8811b-106">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="8811b-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="8811b-107">Rejestrowanie filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="8811b-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="8811b-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="8811b-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="8811b-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="8811b-109">HttpResponseException</span></span>

<span data-ttu-id="8811b-110">Co się stanie, jeśli kontroler Web API zgłosi nieprzechwycony wyjątek?</span><span class="sxs-lookup"><span data-stu-id="8811b-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="8811b-111">Domyślnie większość wyjątków są tłumaczone na odpowiedź HTTP z kodem stanu 500, wewnętrzny błąd serwera.</span><span class="sxs-lookup"><span data-stu-id="8811b-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="8811b-112">**HttpResponseException** typu jest przypadkiem szczególnym.</span><span class="sxs-lookup"><span data-stu-id="8811b-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="8811b-113">Ten wyjątek zwraca do kod stanu HTTP, który określisz w Konstruktorze wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8811b-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="8811b-114">Na przykład poniższa metoda zwraca 404, nie można odnaleźć, jeśli *identyfikator* parametr jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="8811b-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="8811b-115">Większą kontrolę nad odpowiedzi, można także skonstruować komunikat całej odpowiedzi i dołącz go za pomocą **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="8811b-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="8811b-116">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="8811b-116">Exception Filters</span></span>

<span data-ttu-id="8811b-117">Można dostosować, jak internetowy interfejs API obsługuje wyjątki, pisząc *filtra wyjątku*.</span><span class="sxs-lookup"><span data-stu-id="8811b-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="8811b-118">Filtra wyjątku jest wykonywany, gdy metoda kontrolera zgłasza wyjątek nieobsługiwany wyjątek, który jest *nie* **HttpResponseException** wyjątku.</span><span class="sxs-lookup"><span data-stu-id="8811b-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="8811b-119">**HttpResponseException** typ jest to szczególny przypadek, ponieważ jest on zaprojektowany specjalnie w celu zwrócenia odpowiedź HTTP.</span><span class="sxs-lookup"><span data-stu-id="8811b-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="8811b-120">Filtry wyjątków zaimplementować **System.Web.Http.Filters.IExceptionFilter** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="8811b-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="8811b-121">Najprostszym sposobem pisanie filtra wyjątku jest pochodną **System.Web.Http.Filters.ExceptionFilterAttribute** klasy, a także Przesłoń **OnException** metody.</span><span class="sxs-lookup"><span data-stu-id="8811b-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="8811b-122">Filtry wyjątków w interfejsie API sieci Web platformy ASP.NET są podobne do tych we wzorcu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8811b-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="8811b-123">Jednak są deklarowane w oddzielnych przestrzeni nazw, a funkcja oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="8811b-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="8811b-124">W szczególności **atrybutu HandleErrorAttribute** klasa używana w MVC nie obsługuje wyjątki wyrzucane przez kontrolerów internetowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="8811b-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="8811b-125">Oto filtr, który konwertuje **NotImplementedException** wyjątki w przypadku stanu HTTP kodu 501, nie zaimplementowano:</span><span class="sxs-lookup"><span data-stu-id="8811b-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="8811b-126">**Odpowiedzi** właściwość **HttpActionExecutedContext** obiekt zawiera komunikat odpowiedzi HTTP, który zostanie wysłany do klienta.</span><span class="sxs-lookup"><span data-stu-id="8811b-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="8811b-127">Rejestrowanie filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="8811b-127">Registering Exception Filters</span></span>

<span data-ttu-id="8811b-128">Istnieje kilka sposobów, aby zarejestrować filtra wyjątku interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="8811b-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="8811b-129">Za akcję</span><span class="sxs-lookup"><span data-stu-id="8811b-129">By action</span></span>
- <span data-ttu-id="8811b-130">Kontroler</span><span class="sxs-lookup"><span data-stu-id="8811b-130">By controller</span></span>
- <span data-ttu-id="8811b-131">Globalnie</span><span class="sxs-lookup"><span data-stu-id="8811b-131">Globally</span></span>

<span data-ttu-id="8811b-132">Aby zastosować filtr do określonej akcji, Dodaj filtr jako atrybut do akcji:</span><span class="sxs-lookup"><span data-stu-id="8811b-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="8811b-133">Aby zastosować filtr do wszystkich akcji w kontrolerze, Dodaj filtr jako atrybut do klasy kontrolera:</span><span class="sxs-lookup"><span data-stu-id="8811b-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="8811b-134">Aby zastosować filtr globalnie do wszystkich kontrolerów składnika Web API, należy dodać wystąpienia filtru w celu **GlobalConfiguration.Configuration.Filters** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8811b-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="8811b-135">Filtry wyjątku w tej kolekcji są stosowane do dowolnej akcji kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8811b-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="8811b-136">Jeśli używasz szablonu projektu "Aplikacja sieci Web 4 MVC ASP.NET" do tworzenia projektu, umieść kod konfiguracji interfejsu API sieci Web wewnątrz `WebApiConfig` klasy, która znajduje się w aplikacji\_folder początkowy:</span><span class="sxs-lookup"><span data-stu-id="8811b-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="8811b-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="8811b-137">HttpError</span></span>

<span data-ttu-id="8811b-138">**HttpError** obiekt zapewnia spójny sposób, aby zwrócić informacje o błędzie w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8811b-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="8811b-139">Poniższy przykład pokazuje, jak zwrócić kod stanu HTTP 404 (nie znaleziono) za pomocą **HttpError** w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8811b-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="8811b-140">**CreateErrorResponse** metodę rozszerzającą zdefiniowano **System.Net.Http.HttpRequestMessageExtensions** klasy.</span><span class="sxs-lookup"><span data-stu-id="8811b-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="8811b-141">Wewnętrznie **CreateErrorResponse** tworzy **HttpError** wystąpienia, a następnie tworzy **obiektu HttpResponseMessage** zawierający **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="8811b-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="8811b-142">W tym przykładzie Jeśli metoda zakończy się pomyślnie, zwracany jest produkt w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="8811b-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="8811b-143">Ale jeśli żądana produktu nie zostanie znaleziony, odpowiedź HTTP zawiera **HttpError** w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="8811b-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="8811b-144">Odpowiedź może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="8811b-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="8811b-145">Należy zauważyć, że **HttpError** został wydany do formatu JSON, w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8811b-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="8811b-146">Jedną z zalet za pomocą **HttpError** jest, że przechodzi on przez takie same [negocjacji zawartości](../formats-and-model-binding/content-negotiation.md) i serializacji przetwarzania jak wszystkie inne z silnie typizowanym modelem.</span><span class="sxs-lookup"><span data-stu-id="8811b-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="8811b-147">HttpError i sprawdzanie poprawności modelu</span><span class="sxs-lookup"><span data-stu-id="8811b-147">HttpError and Model Validation</span></span>

<span data-ttu-id="8811b-148">Do weryfikacji modelu, można przekazać stan modelu do **CreateErrorResponse**, aby uwzględnić błędy sprawdzania poprawności w odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="8811b-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="8811b-149">W tym przykładzie może zwrócić następującą odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="8811b-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="8811b-150">Aby uzyskać więcej informacji o weryfikacji modelu, zobacz [weryfikacji modelu programu ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="8811b-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="8811b-151">Przy użyciu HttpResponseException HttpError</span><span class="sxs-lookup"><span data-stu-id="8811b-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="8811b-152">Poprzednie przykłady zwrócą **obiektu HttpResponseMessage** wiadomości z akcji kontrolera, ale można również użyć **HttpResponseException** do zwrócenia **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="8811b-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="8811b-153">Dzięki temu można zwrócić silnie typizowanym modelem w przypadku powodzenia normalne, podczas zwracania nadal **HttpError** Jeśli występuje błąd:</span><span class="sxs-lookup"><span data-stu-id="8811b-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
