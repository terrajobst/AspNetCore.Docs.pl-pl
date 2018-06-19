---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Jednostka testowania kontrolerów w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym temacie opisano niektóre określone techniki testowania kontrolerów w sieci Web API 2 jednostek. Przed przeczytaniem tego tematu, należy przeczytać samouczek jednostki...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039547"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="66ac1-104">Jednostka testowania kontrolerów w składniku ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="66ac1-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="66ac1-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="66ac1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="66ac1-106">W tym temacie opisano niektóre określone techniki testowania kontrolerów w sieci Web API 2 jednostek.</span><span class="sxs-lookup"><span data-stu-id="66ac1-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="66ac1-107">Przed przeczytaniem tego tematu, należy przeczytać samouczek [jednostek testowania ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), który wskazuje, jak dodać projektu testu jednostkowego do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="66ac1-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="66ac1-108">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="66ac1-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="66ac1-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="66ac1-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="66ac1-110">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="66ac1-110">Web API 2</span></span>
> - <span data-ttu-id="66ac1-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="66ac1-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="66ac1-112">Po użyciu Moq, ale sam pomysł ma zastosowanie do dowolnej mocking architektury.</span><span class="sxs-lookup"><span data-stu-id="66ac1-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="66ac1-113">Moq 4.5.30 (i nowszych) obsługuje Visual Studio 2017 r, Roslyn i .NET 4.5 i nowszymi wersjami.</span><span class="sxs-lookup"><span data-stu-id="66ac1-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="66ac1-114">Wspólnego wzorca w testach jednostkowych jest &quot;Rozmieść act-assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="66ac1-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="66ac1-115">Rozmieść: Konfigurowanie wymagań wstępnych dla testów do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="66ac1-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="66ac1-116">Działanie: Wykonywania testu.</span><span class="sxs-lookup"><span data-stu-id="66ac1-116">Act: Perform the test.</span></span>
- <span data-ttu-id="66ac1-117">Assert: Sprawdź, czy testu zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="66ac1-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="66ac1-118">W kroku Rozmieść będą często używane makiety lub stub obiektów.</span><span class="sxs-lookup"><span data-stu-id="66ac1-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="66ac1-119">Który minimalizuje liczbę zależności, więc badanie koncentruje się na rzecz testowania.</span><span class="sxs-lookup"><span data-stu-id="66ac1-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="66ac1-120">Poniżej przedstawiono niektóre czynności, które powinny testu jednostkowego w kontrolerach interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="66ac1-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="66ac1-121">Akcja zwraca poprawny typ odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66ac1-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="66ac1-122">Nieprawidłowe parametry zwraca odpowiedź Napraw błąd.</span><span class="sxs-lookup"><span data-stu-id="66ac1-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="66ac1-123">Akcja wywołuje metodę poprawne na warstwie repozytorium lub usługi.</span><span class="sxs-lookup"><span data-stu-id="66ac1-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="66ac1-124">Jeśli odpowiedź zawiera model domeny, sprawdź typ modelu.</span><span class="sxs-lookup"><span data-stu-id="66ac1-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="66ac1-125">Oto niektóre ogólne czynności, aby przetestować, ale szczegółowe informacje są zależne od implementacji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="66ac1-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="66ac1-126">W szczególności ułatwia istotną różnicą czy zwracać akcji kontrolera **HttpResponseMessage** lub **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="66ac1-127">Aby uzyskać więcej informacji na temat tych typów, zobacz [wyniki akcji w sieci Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="66ac1-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="66ac1-128">Testowanie akcji, które zwracają HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="66ac1-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="66ac1-129">Oto przykład kontrolera którego powrotu akcje **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="66ac1-130">Powiadomienie kontrolera używa iniekcji zależności można wstrzyknąć `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="66ac1-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="66ac1-131">Dzięki temu kontroler więcej testować, ponieważ może wstrzyknąć zasymulować repozytorium.</span><span class="sxs-lookup"><span data-stu-id="66ac1-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="66ac1-132">Następujące testu jednostkowego sprawdza, czy `Get` zapisy metody `Product` do treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66ac1-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="66ac1-133">Przyjęto założenie, że `repository` jest makiety `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="66ac1-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="66ac1-134">Ważne jest, aby ustawić **żądania** i **konfiguracji** na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="66ac1-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="66ac1-135">W przeciwnym razie test zakończy się niepowodzeniem z **argumentnullexception —** lub **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="66ac1-136">Testowanie generowania łącza</span><span class="sxs-lookup"><span data-stu-id="66ac1-136">Testing Link Generation</span></span>

<span data-ttu-id="66ac1-137">`Post` Wywołania metody **UrlHelper.Link** do utworzenia łącza w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66ac1-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="66ac1-138">Wymaga nieco więcej ustawień w testu jednostkowego:</span><span class="sxs-lookup"><span data-stu-id="66ac1-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="66ac1-139">**UrlHelper** klasa musi żądanie adresu URL i dane trasy, więc nie można ustawić wartości dla nich testu.</span><span class="sxs-lookup"><span data-stu-id="66ac1-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="66ac1-140">Innym rozwiązaniem jest makiety lub stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="66ac1-141">Takie podejście, należy zastąpić wartość domyślną [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) na wersję makiety lub stub zwracające wartość stałą.</span><span class="sxs-lookup"><span data-stu-id="66ac1-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="66ac1-142">Umożliwia ponowne zapisywanie adresów testu za pomocą [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="66ac1-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="66ac1-143">Zainstaluj `Moq` pakietu NuGet w projekcie testowym.</span><span class="sxs-lookup"><span data-stu-id="66ac1-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="66ac1-144">W tej wersji, nie trzeba skonfigurować dowolne dane trasy, ponieważ makiety **UrlHelper** zwraca ciąg stałej.</span><span class="sxs-lookup"><span data-stu-id="66ac1-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="66ac1-145">Testowanie akcji, które zwracają IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="66ac1-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="66ac1-146">W sieci Web API 2, mogą zwracać akcji kontrolera **IHttpActionResult**, który jest odpowiednikiem **ActionResult** na platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="66ac1-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="66ac1-147">**IHttpActionResult** interfejs definiuje polecenie wzorzec do tworzenia odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="66ac1-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="66ac1-148">Zamiast tworzyć bezpośrednio odpowiedzi, zwraca kontrolera **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="66ac1-149">Później, wywołuje potok **IHttpActionResult** do tworzenia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66ac1-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="66ac1-150">Takie podejście ułatwia pisanie testów jednostkowych, ponieważ można pominąć wiele ustawień, który jest wymagany dla **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="66ac1-151">Oto przykład controller którego powrotu akcje **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="66ac1-152">W tym przykładzie przedstawiono niektóre typowe wzorce przy użyciu **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="66ac1-153">Zobaczmy, jak do jednostki je przetestować.</span><span class="sxs-lookup"><span data-stu-id="66ac1-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="66ac1-154">Akcja zwraca 200 (OK) z treści odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="66ac1-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="66ac1-155">`Get` Wywołania metody `Ok(product)` Jeśli zostanie znaleziony produkt.</span><span class="sxs-lookup"><span data-stu-id="66ac1-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="66ac1-156">Upewnij się, jest zwracany typ w testu jednostkowego **OkNegotiatedContentResult** i zwrócony produkt ma prawo identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="66ac1-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="66ac1-157">Należy zauważyć, że testu jednostkowego nie jest wykonywana wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="66ac1-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="66ac1-158">Można założyć, że poprawnie tworzy wynik akcji odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="66ac1-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="66ac1-159">(Dlatego strukturę interfejsu API sieci Web ma własną testów jednostkowych!)</span><span class="sxs-lookup"><span data-stu-id="66ac1-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="66ac1-160">Akcja zwraca 404 (nie znaleziono)</span><span class="sxs-lookup"><span data-stu-id="66ac1-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="66ac1-161">`Get` Wywołania metody `NotFound()` Jeśli produktu nie zostanie znaleziony.</span><span class="sxs-lookup"><span data-stu-id="66ac1-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="66ac1-162">Dla tego przypadku testu jednostkowego sprawdza tylko jeśli typ zwracany jest **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="66ac1-163">Akcja zwraca 200 (OK) z treści odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="66ac1-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="66ac1-164">`Delete` Wywołania metody `Ok()` aby zwrócić pustą odpowiedź 200 protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="66ac1-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="66ac1-165">Tak jak w poprzednim przykładzie testu jednostkowego sprawdza zwracany typ w tym przypadku **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="66ac1-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="66ac1-166">Akcja zwraca 201 (utworzono) z nagłówkiem lokalizacji</span><span class="sxs-lookup"><span data-stu-id="66ac1-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="66ac1-167">`Post` Wywołania metody `CreatedAtRoute` do zwrócenia odpowiedź 201 protokołu HTTP z identyfikatora URI w nagłówku lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="66ac1-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="66ac1-168">Sprawdź, czy akcja ustawia poprawne wartości routingu w testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="66ac1-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="66ac1-169">Akcja zwraca innego 2xx z treści odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="66ac1-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="66ac1-170">`Put` Wywołania metody `Content` do zwracania odpowiedzi HTTP 202 (zaakceptowane) z treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="66ac1-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="66ac1-171">Ten przypadek jest podobny do zwracania 200 (OK), ale testu jednostkowego należy także sprawdzić kod stanu.</span><span class="sxs-lookup"><span data-stu-id="66ac1-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="66ac1-172">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="66ac1-172">Additional Resources</span></span>

- [<span data-ttu-id="66ac1-173">Mocking programu Entity Framework podczas testowania składnika ASP.NET Web API 2 jednostek</span><span class="sxs-lookup"><span data-stu-id="66ac1-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="66ac1-174">[Pisanie testów dla usługi interfejsu API sieci Web platformy ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (wpis w blogu przez Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="66ac1-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="66ac1-175">Debugowanie interfejsu API sieci Web ASP.NET za pomocą Route Debugger</span><span class="sxs-lookup"><span data-stu-id="66ac1-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
