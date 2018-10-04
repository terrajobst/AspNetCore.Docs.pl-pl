---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Kontrolery testów jednostkowych we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym temacie opisano niektóre określone techniki kontrolery testów jednostkowych WE Web API 2. Przed odczytaniem w tym temacie, możesz chcieć przeczytaj samouczek jednostki...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794827"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="bc86f-104">Kontrolery testów jednostkowych we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bc86f-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bc86f-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bc86f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bc86f-106">W tym temacie opisano niektóre określone techniki kontrolery testów jednostkowych WE Web API 2.</span><span class="sxs-lookup"><span data-stu-id="bc86f-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="bc86f-107">Przed odczytaniem w tym temacie, możesz chcieć przeczytaj samouczek [jednostki testowania wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), który pokazuje, jak dodać projekt testu jednostkowego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="bc86f-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bc86f-108">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="bc86f-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="bc86f-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="bc86f-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="bc86f-110">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="bc86f-110">Web API 2</span></span>
> - <span data-ttu-id="bc86f-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="bc86f-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="bc86f-112">Po użyciu Moq, ale ten sam pomysł stosuje się do dowolnej architektury pozorowania.</span><span class="sxs-lookup"><span data-stu-id="bc86f-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="bc86f-113">Moq 4.5.30 (i nowszych) obsługuje program Visual Studio 2017, Roslyn i .NET 4.5 i nowsze wersje.</span><span class="sxs-lookup"><span data-stu-id="bc86f-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="bc86f-114">Typowym w testach jednostkowych &quot;Rozmieść act asercja&quot;:</span><span class="sxs-lookup"><span data-stu-id="bc86f-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="bc86f-115">Rozmieść: Konfigurowanie wymagań wstępnych dla testu do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="bc86f-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="bc86f-116">Ustawa: Wykonywania testu.</span><span class="sxs-lookup"><span data-stu-id="bc86f-116">Act: Perform the test.</span></span>
- <span data-ttu-id="bc86f-117">Asercja: Sprawdź, czy test powiodło się.</span><span class="sxs-lookup"><span data-stu-id="bc86f-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="bc86f-118">W kroku Rozmieść będą często używane pozorny lub namiastki obiektów.</span><span class="sxs-lookup"><span data-stu-id="bc86f-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="bc86f-119">Liczba zależności, które minimalizuje tak testu koncentruje się na testowanie jedno.</span><span class="sxs-lookup"><span data-stu-id="bc86f-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="bc86f-120">Oto kilka rzeczy, które powinno testu jednostkowego w kontrolerach interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="bc86f-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="bc86f-121">Akcja zwraca poprawny typ odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bc86f-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="bc86f-122">Nieprawidłowe parametry zwracania odpowiedzi usunąć błąd.</span><span class="sxs-lookup"><span data-stu-id="bc86f-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="bc86f-123">Akcja wywołuje metodę poprawne na warstwie repozytorium lub usługi.</span><span class="sxs-lookup"><span data-stu-id="bc86f-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="bc86f-124">Jeśli odpowiedź zawiera model domeny, należy sprawdzić typ modelu.</span><span class="sxs-lookup"><span data-stu-id="bc86f-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="bc86f-125">Poniżej przedstawiono niektóre ogólne czynności, aby przetestować, ale szczegółowe informacje są zależne od implementacji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bc86f-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="bc86f-126">W szczególności, to sprawia, że różnica czy zwrócić akcji kontrolera **obiektu HttpResponseMessage** lub **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="bc86f-127">Aby uzyskać więcej informacji na temat tych typów, zobacz [wyniki akcji w sieci Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="bc86f-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="bc86f-128">Testowanie akcji, które zwracają element HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="bc86f-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="bc86f-129">Oto przykład kontrolera którego zwracany akcje **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="bc86f-130">Zwróć uwagę, kontroler używa wstrzykiwanie zależności do dodania `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="bc86f-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="bc86f-131">Dzięki temu kontroler bardziej sprawdzalnego działa zgodnie, ponieważ może wprowadzać makiety repozytorium.</span><span class="sxs-lookup"><span data-stu-id="bc86f-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="bc86f-132">Następujący test jednostkowy sprawdza, czy `Get` zapisów metoda `Product` na treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bc86f-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="bc86f-133">Przyjęto założenie, że `repository` jest pozorny `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="bc86f-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="bc86f-134">Jest ważne, aby ustawić **żądania** i **konfiguracji** na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="bc86f-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="bc86f-135">W przeciwnym razie niepowodzenie testu **ArgumentNullException** lub **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="bc86f-136">Testowanie Generowanie konsolidacji</span><span class="sxs-lookup"><span data-stu-id="bc86f-136">Testing Link Generation</span></span>

<span data-ttu-id="bc86f-137">`Post` Wywołania metody **UrlHelper.Link** do tworzenia łączy w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bc86f-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="bc86f-138">Ta migracja wymaga nieco więcej instalacji w test jednostkowy:</span><span class="sxs-lookup"><span data-stu-id="bc86f-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="bc86f-139">**UrlHelper** klasy musi żądanie adresu URL i dane trasy, więc nie można ustawić wartości dla tych testów.</span><span class="sxs-lookup"><span data-stu-id="bc86f-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="bc86f-140">Innym rozwiązaniem jest pozorny ani klas zastępczych **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="bc86f-141">W przypadku tej metody, zastąp wartość domyślną [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) pozorny ani klas zastępczych wersji, która zwraca wartość stałą.</span><span class="sxs-lookup"><span data-stu-id="bc86f-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="bc86f-142">Teraz Zapisz ponownie test za pomocą [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="bc86f-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="bc86f-143">Zainstaluj `Moq` pakietu NuGet w projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="bc86f-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="bc86f-144">W tej wersji, nie trzeba skonfigurować wszelkie dane trasy, ponieważ pozorny **UrlHelper** zwraca ciąg stałej.</span><span class="sxs-lookup"><span data-stu-id="bc86f-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="bc86f-145">Testowanie akcji, które zwracają IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="bc86f-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="bc86f-146">W sieci Web API 2, można powrócić do akcji kontrolera **IHttpActionResult**, który jest odpowiednikiem **ActionResult** we wzorcu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bc86f-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="bc86f-147">**IHttpActionResult** interfejs definiuje wzorzec polecenia do tworzenia odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc86f-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="bc86f-148">Zamiast tworzyć odpowiedź bezpośrednio, ten kontroler zwraca **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="bc86f-149">Później, potok wywołuje **IHttpActionResult** do tworzenia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bc86f-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="bc86f-150">Takie podejście ułatwia pisanie testów jednostkowych, ponieważ możesz pominąć wiele ustawień, które są potrzebne do **obiektu HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="bc86f-151">Oto przykład controller którego zwracany akcje **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="bc86f-152">W tym przykładzie przedstawiono niektóre typowe wzorce przy użyciu **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="bc86f-153">Zobaczmy, jak do jednostki przetestować je.</span><span class="sxs-lookup"><span data-stu-id="bc86f-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="bc86f-154">Akcja zwraca 200 (OK) z treści odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="bc86f-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="bc86f-155">`Get` Wywołania metody `Ok(product)` Jeśli zostanie znaleziony produktu.</span><span class="sxs-lookup"><span data-stu-id="bc86f-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="bc86f-156">Upewnij się, jest zwracany typ w test jednostkowy **OkNegotiatedContentResult** i zwrócony produkt ma prawo identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="bc86f-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="bc86f-157">Należy zauważyć, że testów jednostkowych nie jest wykonywany wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="bc86f-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="bc86f-158">Można założyć, że poprawnie tworzy wynik akcji odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc86f-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="bc86f-159">(Dlatego strukturę interfejsu API sieci Web ma swoje własne testy jednostkowe!)</span><span class="sxs-lookup"><span data-stu-id="bc86f-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="bc86f-160">Akcja zwraca 404 (nie znaleziono)</span><span class="sxs-lookup"><span data-stu-id="bc86f-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="bc86f-161">`Get` Wywołania metody `NotFound()` Jeśli produktu nie zostanie znaleziony.</span><span class="sxs-lookup"><span data-stu-id="bc86f-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="bc86f-162">Dla tego przypadku testów jednostkowych sprawdza tylko, jeśli typ zwracany jest **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="bc86f-163">Akcja zwraca 200 (OK) bez treści odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="bc86f-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="bc86f-164">`Delete` Wywołania metody `Ok()` zwrócić pustą odpowiedź HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="bc86f-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="bc86f-165">Podobnie jak w poprzednim przykładzie test jednostkowy sprawdza, czy zwracany typ, w tym przypadku **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="bc86f-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="bc86f-166">Akcja zwraca 201 (utworzono) przy użyciu nagłówka lokalizacji</span><span class="sxs-lookup"><span data-stu-id="bc86f-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="bc86f-167">`Post` Wywołania metody `CreatedAtRoute` zwrócić odpowiedź 201 protokołu HTTP z identyfikatorem URI w nagłówku Location.</span><span class="sxs-lookup"><span data-stu-id="bc86f-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="bc86f-168">Testy jednostkowe Sprawdź, czy akcja ustawia poprawne wartości routingu.</span><span class="sxs-lookup"><span data-stu-id="bc86f-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="bc86f-169">Akcja zwraca inną 2xx z treści odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="bc86f-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="bc86f-170">`Put` Wywołania metody `Content` zwrócić odpowiedź HTTP 202 (zaakceptowano) z treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bc86f-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="bc86f-171">Ten przypadek jest podobny do zwracania 200 (OK), ale test jednostkowy należy także sprawdzić kod stanu.</span><span class="sxs-lookup"><span data-stu-id="bc86f-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="bc86f-172">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bc86f-172">Additional Resources</span></span>

- [<span data-ttu-id="bc86f-173">Pozorowanie programu Entity Framework podczas testowania ASP.NET Web API 2 jednostek</span><span class="sxs-lookup"><span data-stu-id="bc86f-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="bc86f-174">[Pisanie testów dla usługi interfejsu API sieci Web platformy ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (wpis na blogu autorstwa Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="bc86f-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="bc86f-175">Debugowanie ASP.NET Web API za pomocą debugera trasy</span><span class="sxs-lookup"><span data-stu-id="bc86f-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
