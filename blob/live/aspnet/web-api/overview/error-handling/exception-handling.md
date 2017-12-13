---
uid: web-api/overview/error-handling/exception-handling
title: "Obsługa wyjątków w ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="e9489-102">Obsługa wyjątków w Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e9489-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e9489-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e9489-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e9489-104">W tym artykule opisano błąd i obsługa wyjątków w interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e9489-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="e9489-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="e9489-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="e9489-106">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="e9489-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="e9489-107">Rejestrowanie filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="e9489-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="e9489-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="e9489-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="e9489-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="e9489-109">HttpResponseException</span></span>

<span data-ttu-id="e9489-110">Co się stanie, jeśli kontroler Web API zgłasza nieprzechwycony wyjątek?</span><span class="sxs-lookup"><span data-stu-id="e9489-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="e9489-111">Domyślnie większość wyjątki są przekształcane na odpowiedzi HTTP z kodem stanu 500, wewnętrzny błąd serwera.</span><span class="sxs-lookup"><span data-stu-id="e9489-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="e9489-112">**HttpResponseException** szczególnych przypadkach jest typu.</span><span class="sxs-lookup"><span data-stu-id="e9489-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="e9489-113">Ten wyjątek zwraca do kod stanu HTTP, określonego w Konstruktorze wyjątku.</span><span class="sxs-lookup"><span data-stu-id="e9489-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="e9489-114">Na przykład następująca metoda zwraca 404, nie można odnaleźć, jeśli *identyfikator* parametr jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="e9489-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="e9489-115">Uzyskać większą kontrolę nad odpowiedzi, możesz również utworzyć komunikat całej odpowiedzi i dołącz ją z **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="e9489-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="e9489-116">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="e9489-116">Exception Filters</span></span>

<span data-ttu-id="e9489-117">Można dostosować sposób obsługi wyjątków interfejsu API sieci Web pisząc *filtru wyjątków*.</span><span class="sxs-lookup"><span data-stu-id="e9489-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="e9489-118">Filtra wyjątku jest wykonywany podczas kontrolera metodę nieobsługiwany wyjątek, który jest *nie* **HttpResponseException** wyjątku.</span><span class="sxs-lookup"><span data-stu-id="e9489-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="e9489-119">**HttpResponseException** typu jest szczególnych przypadkach, ponieważ został zaprojektowany specjalnie z myślą o zwracanie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e9489-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="e9489-120">Filtry wyjątków zaimplementować **System.Web.Http.Filters.IExceptionFilter** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="e9489-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="e9489-121">Najprostszym sposobem pisanie filtra wyjątku jest pochodzić z **System.Web.Http.Filters.ExceptionFilterAttribute** klasy i zastąpić **OnException** metody.</span><span class="sxs-lookup"><span data-stu-id="e9489-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="e9489-122">Filtry wyjątków w interfejsie API sieci Web platformy ASP.NET są podobne do tych na platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e9489-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="e9489-123">Jednak zostały zgłoszone w oddzielnych przestrzeni nazw i funkcja oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="e9489-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="e9489-124">W szczególności **atrybutu HandleErrorAttribute** klasa używana w MVC nie zapewnia obsługi wyjątków zgłaszanych przez kontrolery interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e9489-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="e9489-125">Oto filtr, który konwertuje **notimplementedexception —** 501 Nie zaimplementowano kodu wyjątków do stanu HTTP:</span><span class="sxs-lookup"><span data-stu-id="e9489-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="e9489-126">**Odpowiedzi** właściwość **HttpActionExecutedContext** obiekt zawiera komunikat odpowiedzi HTTP, które zostaną wysłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="e9489-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="e9489-127">Rejestrowanie filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="e9489-127">Registering Exception Filters</span></span>

<span data-ttu-id="e9489-128">Istnieje kilka sposobów, aby zarejestrować filtru wyjątków interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="e9489-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="e9489-129">Przez akcję</span><span class="sxs-lookup"><span data-stu-id="e9489-129">By action</span></span>
- <span data-ttu-id="e9489-130">Kontroler</span><span class="sxs-lookup"><span data-stu-id="e9489-130">By controller</span></span>
- <span data-ttu-id="e9489-131">Globalny</span><span class="sxs-lookup"><span data-stu-id="e9489-131">Globally</span></span>

<span data-ttu-id="e9489-132">Aby zastosować filtr do określonej akcji, należy dodać filtr jako atrybut do akcji:</span><span class="sxs-lookup"><span data-stu-id="e9489-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="e9489-133">Aby zastosować filtr do wszystkich akcji w kontrolerze, Dodaj filtr jako atrybut do klasy kontrolera:</span><span class="sxs-lookup"><span data-stu-id="e9489-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="e9489-134">Aby zastosować filtr globalnie do wszystkich kontrolerów interfejsu API sieci Web, należy dodać wystąpienia filtr, aby **GlobalConfiguration.Configuration.Filters** kolekcji.</span><span class="sxs-lookup"><span data-stu-id="e9489-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="e9489-135">Filtry wyjątkiem w tej kolekcji mają zastosowanie do dowolnej akcji kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e9489-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="e9489-136">Użycie szablonu projektu "Platformy ASP.NET MVC 4 aplikacji sieci Web" Aby utworzyć projekt, umieść kod konfiguracji interfejsu API sieci Web wewnątrz `WebApiConfig` klasy, która znajduje się w aplikacji\_folder początkowy:</span><span class="sxs-lookup"><span data-stu-id="e9489-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="e9489-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="e9489-137">HttpError</span></span>

<span data-ttu-id="e9489-138">**HttpError** obiekt zapewnia spójny sposób, aby zwrócić informacje o błędzie w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e9489-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="e9489-139">Poniższy przykład przedstawia sposób zwrócenia kod stanu HTTP 404 (nie znaleziono) z **HttpError** w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e9489-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="e9489-140">**CreateErrorResponse** — metoda rozszerzenia jest zdefiniowany w **System.Net.Http.HttpRequestMessageExtensions** klasy.</span><span class="sxs-lookup"><span data-stu-id="e9489-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="e9489-141">Wewnętrznie **CreateErrorResponse** tworzy **HttpError** wystąpienia, a następnie tworzy **HttpResponseMessage** zawierający **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="e9489-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="e9489-142">W tym przykładzie Jeśli metoda zakończy się pomyślnie, zwraca produktu w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e9489-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="e9489-143">Ale jeśli nie odnaleziono żądanego produktu, odpowiedź HTTP zawiera **HttpError** w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="e9489-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="e9489-144">Odpowiedź może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="e9489-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="e9489-145">Zwróć uwagę, że **HttpError** została wykonana serializacja JSON, w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e9489-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="e9489-146">Jedną z zalet przy użyciu **HttpError** jest, że przechodzi ona przez taki sam [negocjowanie zawartości](../formats-and-model-binding/content-negotiation.md) i serializacji przetwarzania jak wszystkie inne silnie typizowanym modelem.</span><span class="sxs-lookup"><span data-stu-id="e9489-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="e9489-147">HttpError i weryfikacja modelu</span><span class="sxs-lookup"><span data-stu-id="e9489-147">HttpError and Model Validation</span></span>

<span data-ttu-id="e9489-148">Do weryfikacji modelu, można przekazać stan modelu do **CreateErrorResponse**, aby uwzględnić w odpowiedzi na błędy sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="e9489-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="e9489-149">W tym przykładzie może zwrócić następującą odpowiedź:</span><span class="sxs-lookup"><span data-stu-id="e9489-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="e9489-150">Aby uzyskać więcej informacji o weryfikacji modelu, zobacz [weryfikacji modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e9489-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="e9489-151">Korzystanie z HttpResponseException HttpError</span><span class="sxs-lookup"><span data-stu-id="e9489-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="e9489-152">Zwraca poprzednich przykładach **HttpResponseMessage** komunikat z akcji kontrolera, ale można również użyć **HttpResponseException** do zwrócenia **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="e9489-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="e9489-153">Dzięki temu można zwrócić silnie typizowanym modelem w przypadku powodzenia normalne, podczas zwracania nadal **HttpError** Jeśli występuje błąd:</span><span class="sxs-lookup"><span data-stu-id="e9489-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
