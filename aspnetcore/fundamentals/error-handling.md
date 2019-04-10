---
title: Obsługa błędów w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/07/2019
uid: fundamentals/error-handling
ms.openlocfilehash: cbb9462a3c6010e074dc391aa128ac2cbb901456
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468753"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="e5276-103">Obsługa błędów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5276-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="e5276-104">Przez [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e5276-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e5276-105">W tym artykule opisano typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e5276-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="e5276-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="e5276-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="e5276-107">([Sposobu pobierania](xref:index#how-to-download-a-sample).) Artykuł zawiera instrukcje dotyczące sposobu ustawiania dyrektywy preprocesora (`#if`, `#endif`, `#define`) Przykładowa aplikacja do obsługi różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="e5276-107">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="e5276-108">Stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="e5276-108">Developer Exception Page</span></span>

<span data-ttu-id="e5276-109">*Stronie wyjątków deweloperów* Wyświetla szczegółowe informacje o wyjątkach żądania.</span><span class="sxs-lookup"><span data-stu-id="e5276-109">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="e5276-110">Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e5276-110">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="e5276-111">Dodaj kod, aby `Startup.Configure` metodę, aby włączyć na stronie, gdy aplikacja jest uruchomiona w trakcie opracowywania [środowiska](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="e5276-111">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="e5276-112">Umieść wywołanie <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> przed dowolnego oprogramowania pośredniczącego, które chcesz przechwytywać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="e5276-112">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="e5276-113">Włącz na stronie wyjątków dla deweloperów **tylko wtedy, gdy aplikacja jest uruchomiona w środowisku programistycznym**.</span><span class="sxs-lookup"><span data-stu-id="e5276-113">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="e5276-114">Nie chcesz publicznie udostępnić szczegółowe informacje o wyjątku, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="e5276-114">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="e5276-115">Aby uzyskać więcej informacji na temat konfigurowania środowiska, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e5276-115">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="e5276-116">Strona zawiera następujące informacje dotyczące wyjątku i żądanie:</span><span class="sxs-lookup"><span data-stu-id="e5276-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="e5276-117">Ślad stosu</span><span class="sxs-lookup"><span data-stu-id="e5276-117">Stack trace</span></span>
* <span data-ttu-id="e5276-118">Parametry ciągu zapytania (jeśli istnieje)</span><span class="sxs-lookup"><span data-stu-id="e5276-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="e5276-119">Pliki cookie (jeśli istnieje)</span><span class="sxs-lookup"><span data-stu-id="e5276-119">Cookies (if any)</span></span>
* <span data-ttu-id="e5276-120">Nagłówki</span><span class="sxs-lookup"><span data-stu-id="e5276-120">Headers</span></span>

<span data-ttu-id="e5276-121">Aby wyświetlić stronę wyjątek dla deweloperów w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj `DevEnvironment` preprocesora dyrektywy i wybierz pozycję **wywołanie wyjątku** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="e5276-121">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="e5276-122">Strona obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="e5276-122">Exception handler page</span></span>

<span data-ttu-id="e5276-123">Aby skonfigurować niestandardową obsługę błędów na stronie w środowisku produkcyjnym, należy użyć oprogramowania pośredniczącego obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="e5276-123">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="e5276-124">Oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="e5276-124">The middleware:</span></span>

* <span data-ttu-id="e5276-125">Przechwytuje i dzienników wyjątków.</span><span class="sxs-lookup"><span data-stu-id="e5276-125">Catches and logs exceptions.</span></span>
* <span data-ttu-id="e5276-126">Ponownie wykonuje żądanie w potoku usługi alternatywnej strony lub kontroler wskazane.</span><span class="sxs-lookup"><span data-stu-id="e5276-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="e5276-127">Żądanie nie jest wykonany ponownie, jeśli odpowiedź została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="e5276-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="e5276-128">W poniższym przykładzie <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> dodaje oprogramowanie pośredniczące obsługi wyjątków w środowiskach innych niż:</span><span class="sxs-lookup"><span data-stu-id="e5276-128">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="e5276-129">Szablon aplikacji stron Razor zawiera stronę błędu (*.cshtml*) i <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> klasy (`ErrorModel`) w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="e5276-129">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="e5276-130">Dla aplikacji MVC szablon projektu obejmuje metodę akcji błędu i widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="e5276-130">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="e5276-131">Poniżej przedstawiono metody akcji:</span><span class="sxs-lookup"><span data-stu-id="e5276-131">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="e5276-132">Nie dekoracji metody akcji programu obsługi błędów z atrybutami metody HTTP, takich jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="e5276-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="e5276-133">Jawne zleceń uniemożliwić metody osiągając niektórych żądań.</span><span class="sxs-lookup"><span data-stu-id="e5276-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="e5276-134">Zezwalaj na anonimowy dostęp do metody, aby były nieuwierzytelnionym użytkownikom możliwość odbierania widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="e5276-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="e5276-135">Dostęp do wyjątku</span><span class="sxs-lookup"><span data-stu-id="e5276-135">Access the exception</span></span>

<span data-ttu-id="e5276-136">Użyj <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> dostęp do wyjątku i Oryginalna ścieżka żądania w kontrolerze obsługi błędów lub na stronie:</span><span class="sxs-lookup"><span data-stu-id="e5276-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="e5276-137">Czy **nie** udostępniają informacje o błędzie poufnych klientom.</span><span class="sxs-lookup"><span data-stu-id="e5276-137">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="e5276-138">Obsługuje błędy stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="e5276-138">Serving errors is a security risk.</span></span>

<span data-ttu-id="e5276-139">Aby wyświetlić stronę obsługi wyjątków w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj `ProdEnvironment` i `ErrorHandlerPage` dyrektywy preprocesora, a następnie wybierz **wywołanie wyjątku** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="e5276-139">To see the exception handling page in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="e5276-140">Lambda obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="e5276-140">Exception handler lambda</span></span>

<span data-ttu-id="e5276-141">Alternatywa [strony programu obsługi wyjątków niestandardowych](#exception-handler-page) ma na celu dostarczenie wyrażenia lambda <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="e5276-141">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="e5276-142">Używanie wyrażenia lambda zezwala na dostęp do błędu przed zwróceniem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e5276-142">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="e5276-143">Poniżej przedstawiono przykład użycia wyrażenia lambda do obsługi wyjątku:</span><span class="sxs-lookup"><span data-stu-id="e5276-143">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> <span data-ttu-id="e5276-144">Czy **nie** obsługiwać błąd poufnych informacji z <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> lub <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> do klientów.</span><span class="sxs-lookup"><span data-stu-id="e5276-144">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="e5276-145">Obsługuje błędy stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="e5276-145">Serving errors is a security risk.</span></span>

<span data-ttu-id="e5276-146">Aby wyświetlić wynik wyrażenia lambda obsługi wyjątków w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj `ProdEnvironment` i `ErrorHandlerLambda` dyrektywy preprocesora, a następnie wybierz **wywołanie wyjątku** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="e5276-146">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="e5276-147">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="e5276-147">UseStatusCodePages</span></span>

<span data-ttu-id="e5276-148">Domyślnie aplikacji ASP.NET Core nie zapewnia stan stronę kodową dla kodów stanu HTTP, takich jak *404 — Nie można odnaleźć*.</span><span class="sxs-lookup"><span data-stu-id="e5276-148">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="e5276-149">Aplikacja zwraca kod stanu i pusta treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e5276-149">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="e5276-150">Aby zapewnić stan stron kodowych, użyj oprogramowanie pośredniczące strony kodowe stanu.</span><span class="sxs-lookup"><span data-stu-id="e5276-150">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="e5276-151">Oprogramowanie pośredniczące jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e5276-151">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="e5276-152">Aby włączyć domyślne tekstowy programy obsługi dla typowe kody stanu błędu, należy wywołać <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> w `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="e5276-152">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="e5276-153">Wywołaj `UseStatusCodePages` przed żądaniem obsługi oprogramowania pośredniczącego (na przykład oprogramowanie pośredniczące plików statycznych i oprogramowanie pośredniczące MVC).</span><span class="sxs-lookup"><span data-stu-id="e5276-153">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="e5276-154">Oto przykład tekstu wyświetlanego przez programy obsługi domyślne:</span><span class="sxs-lookup"><span data-stu-id="e5276-154">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="e5276-155">Aby znajduje się w różnych formatach kod stanu strony w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj jednej z dyrektywy preprocesora, które zaczynają się od `StatusCodePages`i wybierz **wyzwalacza a 404** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="e5276-155">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="e5276-156">UseStatusCodePages za pomocą ciągu formatu</span><span class="sxs-lookup"><span data-stu-id="e5276-156">UseStatusCodePages with format string</span></span>

<span data-ttu-id="e5276-157">Aby dostosować typ zawartości odpowiedzi i tekst, użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przyjmującej zawartości typu i formatu ciągu:</span><span class="sxs-lookup"><span data-stu-id="e5276-157">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="e5276-158">UseStatusCodePages przy użyciu lambda</span><span class="sxs-lookup"><span data-stu-id="e5276-158">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="e5276-159">Aby określić niestandardową obsługę błędów i zapisywanie odpowiedzi kodu, użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przyjmującej wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="e5276-159">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a><span data-ttu-id="e5276-160">UseStatusCodePagesWithRedirect</span><span class="sxs-lookup"><span data-stu-id="e5276-160">UseStatusCodePagesWithRedirect</span></span>

<span data-ttu-id="e5276-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> — Metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="e5276-161">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="e5276-162">Wysyła *302 — znaleziono* kod stanu do klienta.</span><span class="sxs-lookup"><span data-stu-id="e5276-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="e5276-163">Przekierowuje klienta do lokalizacji, udostępnionego w szablonie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e5276-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="e5276-164">Szablon adresu URL może zawierać `{0}` symbol zastępczy dla kodu stanu, jak pokazano w przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e5276-164">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="e5276-165">Jeśli szablon adresu URL zaczyna się tyldą (~), za pomocą aplikacji zastępuje tylda `PathBase`.</span><span class="sxs-lookup"><span data-stu-id="e5276-165">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="e5276-166">Po wskazaniu punktu końcowego w aplikacji, Utwórz widoku składnika MVC lub strona Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="e5276-166">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="e5276-167">Na przykład stron Razor, zobacz [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="e5276-167">For a Razor Pages example, see [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="e5276-168">Ta metoda jest często używana, gdy aplikacja:</span><span class="sxs-lookup"><span data-stu-id="e5276-168">This method is commonly used when the app:</span></span>

* <span data-ttu-id="e5276-169">Należy przekierowuje klienta do innego punktu końcowego, zazwyczaj w przypadkach, gdzie przetwarza błąd przez inną aplikację.</span><span class="sxs-lookup"><span data-stu-id="e5276-169">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="e5276-170">W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla przekierowanego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="e5276-170">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="e5276-171">Nie należy zachować i zwróć oryginalny kod stanu odpowiedzi przekierowania początkowej.</span><span class="sxs-lookup"><span data-stu-id="e5276-171">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="e5276-172">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="e5276-172">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="e5276-173"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> — Metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="e5276-173">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="e5276-174">Zwraca oryginalny kod stanu do klienta.</span><span class="sxs-lookup"><span data-stu-id="e5276-174">Returns the original status code to the client.</span></span>
* <span data-ttu-id="e5276-175">Generuje treści odpowiedzi przy ponownym wykonaniem Potok żądań przy użyciu ścieżki alternatywnej.</span><span class="sxs-lookup"><span data-stu-id="e5276-175">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="e5276-176">Po wskazaniu punktu końcowego w aplikacji, Utwórz widoku składnika MVC lub strona Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="e5276-176">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="e5276-177">Na przykład stron Razor, zobacz [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) w [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="e5276-177">For a Razor Pages example, see [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="e5276-178">Ta metoda jest często używana aplikacja powinna:</span><span class="sxs-lookup"><span data-stu-id="e5276-178">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="e5276-179">Proces żądania bez przekierowania folderu do innego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="e5276-179">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="e5276-180">W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla pierwotnie żądanego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="e5276-180">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="e5276-181">Zachowanie i powrócić do oryginalnego kodu stanu z odpowiedzią.</span><span class="sxs-lookup"><span data-stu-id="e5276-181">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="e5276-182">Adres URL i zapytania szablony ciągów może zawierać symbolu zastępczego (`{0}`) dla kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="e5276-182">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="e5276-183">Szablon adresu URL musi rozpoczynać się od ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="e5276-183">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="e5276-184">Korzystając z symbolem zastępczym w ścieżce, upewnij się, że punkt końcowy (strona lub kontroler) może przetwarzać segmentu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="e5276-184">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="e5276-185">Na przykład strona Razor dla błędów należy zaakceptować wartość segmentu ścieżki opcjonalnie za pomocą `@page` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="e5276-185">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="e5276-186">Punkt końcowy, który przetwarza błędu można uzyskać oryginalny adres URL, który wygenerował błąd, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="e5276-186">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="e5276-187">Wyłączanie kod stanu</span><span class="sxs-lookup"><span data-stu-id="e5276-187">Disable status code pages</span></span>

<span data-ttu-id="e5276-188">Strony kodowe stanu można wyłączyć dla określonych żądań w metodzie obsługi stron Razor lub kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="e5276-188">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="e5276-189">Aby wyłączyć stron kodowych stanu, użyj <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span><span class="sxs-lookup"><span data-stu-id="e5276-189">To disable status code pages, use the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="e5276-190">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="e5276-190">Exception-handling code</span></span>

<span data-ttu-id="e5276-191">Kod obsługi stron wyjątków może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="e5276-191">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="e5276-192">Często jest dobrym rozwiązaniem dla stron błędów produkcyjnych, które ma zawierać zawartość statyczną wyłącznie.</span><span class="sxs-lookup"><span data-stu-id="e5276-192">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="e5276-193">Nagłówki odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="e5276-193">Response headers</span></span>

<span data-ttu-id="e5276-194">Gdy nagłówki odpowiedzi są wysyłane:</span><span class="sxs-lookup"><span data-stu-id="e5276-194">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="e5276-195">Aplikacja nie można zmienić kod stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e5276-195">The app can't change the response's status code.</span></span>
* <span data-ttu-id="e5276-196">Nie można uruchomić wszystkie strony wyjątków lub programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="e5276-196">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="e5276-197">Odpowiedzi muszą być wypełnione albo połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="e5276-197">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="e5276-198">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="e5276-198">Server exception handling</span></span>

<span data-ttu-id="e5276-199">Oprócz logiki aplikacji, obsługi wyjątków [implementację serwera HTTP](xref:fundamentals/servers/index) może obsługiwać niektóre wyjątki.</span><span class="sxs-lookup"><span data-stu-id="e5276-199">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="e5276-200">Jeśli serwer wyłapuje wyjątek, przed wysłaniem nagłówki odpowiedzi, serwer wysyła *500 — Wewnętrzny błąd serwera* odpowiedzi bez treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e5276-200">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="e5276-201">Jeśli serwer wyłapuje wyjątek po wysłaniu nagłówki odpowiedzi, serwer zamyka połączenie.</span><span class="sxs-lookup"><span data-stu-id="e5276-201">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="e5276-202">Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="e5276-202">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="e5276-203">Każdy wyjątek, który występuje, gdy ten serwer obsługuje żądania odbywa się przez wyjątek serwera obsługi.</span><span class="sxs-lookup"><span data-stu-id="e5276-203">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="e5276-204">Strony błędów niestandardowych aplikacji, obsługi oprogramowania pośredniczącego i filtry wyjątków nie wpływają na to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="e5276-204">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="e5276-205">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="e5276-205">Startup exception handling</span></span>

<span data-ttu-id="e5276-206">Tylko warstwa hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5276-206">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="e5276-207">Hosta można skonfigurować w celu [przechwytywania błędów uruchamiania](xref:fundamentals/host/web-host#capture-startup-errors) i [przechwytywać szczegółowe informacje o błędach](xref:fundamentals/host/web-host#detailed-errors).</span><span class="sxs-lookup"><span data-stu-id="e5276-207">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="e5276-208">Warstwa obsługi można wyświetlić stronę błędu dla błędów uruchamiania przechwycone tylko wtedy, gdy wystąpi błąd, po adresem/port hosta powiązania.</span><span class="sxs-lookup"><span data-stu-id="e5276-208">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="e5276-209">W przypadku niepowodzenia powiązania:</span><span class="sxs-lookup"><span data-stu-id="e5276-209">If binding fails:</span></span>

* <span data-ttu-id="e5276-210">Warstwa hostingu rejestruje wyjątek krytyczny.</span><span class="sxs-lookup"><span data-stu-id="e5276-210">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="e5276-211">Proces dotnet kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="e5276-211">The dotnet process crashes.</span></span>
* <span data-ttu-id="e5276-212">Strona błędu, nie jest wyświetlane, gdy serwer HTTP jest [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e5276-212">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="e5276-213">Podczas uruchamiania na [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - niepowodzenia procesu* jest zwracany przez [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) Jeśli nie można uruchomić procesu .</span><span class="sxs-lookup"><span data-stu-id="e5276-213">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="e5276-214">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="e5276-214">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="e5276-215">Aby uzyskać informacje na temat rozwiązywania problemów z uruchamianiem przy użyciu usługi Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="e5276-215">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="e5276-216">Strona błędu bazy danych</span><span class="sxs-lookup"><span data-stu-id="e5276-216">Database error page</span></span>

<span data-ttu-id="e5276-217">[Stronę błędu bazy danych](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) oprogramowania pośredniczącego przechwytuje wyjątki związane z bazy danych można rozwiązać za pomocą migracje platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e5276-217">The [Database Error Page](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="e5276-218">Gdy występują te wyjątki, jest generowany odpowiedzi HTML szczegółowe informacje o możliwych działań w celu rozwiązania problemu.</span><span class="sxs-lookup"><span data-stu-id="e5276-218">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="e5276-219">Ta strona powinna być włączona tylko w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="e5276-219">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="e5276-220">Włącz stronie przez dodanie kodu do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e5276-220">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

## <a name="exception-filters"></a><span data-ttu-id="e5276-221">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="e5276-221">Exception filters</span></span>

<span data-ttu-id="e5276-222">W aplikacji MVC filtry wyjątków można skonfigurować globalnie lub na zasadzie na kontroler lub akcję.</span><span class="sxs-lookup"><span data-stu-id="e5276-222">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="e5276-223">W aplikacjach stron Razor można je skonfigurować globalnie lub na modelu strony.</span><span class="sxs-lookup"><span data-stu-id="e5276-223">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="e5276-224">Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr.</span><span class="sxs-lookup"><span data-stu-id="e5276-224">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="e5276-225">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="e5276-225">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="e5276-226">Filtry wyjątków są przydatne zalewania wyjątków występujących w ramach akcji MVC, ale nie jest tak elastyczna jak oprogramowanie pośredniczące obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="e5276-226">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="e5276-227">Zalecamy używanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="e5276-227">We recommend using the middleware.</span></span> <span data-ttu-id="e5276-228">Filtry, tylko gdy potrzebujesz należy używać do wykonywania obsługi błędów, inaczej w zależności od Akcja kontrolera MVC, który jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="e5276-228">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="e5276-229">Błędy stanu modelu</span><span class="sxs-lookup"><span data-stu-id="e5276-229">Model state errors</span></span>

<span data-ttu-id="e5276-230">Aby dowiedzieć się, jak obsługiwać błędy stanu modelu, zobacz [wiązanie modelu](xref:mvc/models/model-binding) i [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="e5276-230">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5276-231">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e5276-231">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
