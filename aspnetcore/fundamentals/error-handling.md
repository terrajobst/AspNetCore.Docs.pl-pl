---
title: Obsługa błędów w ASP.NET Core
author: rick-anderson
description: Odkryj, jak obsługiwać błędy w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: 28b463bccfb8aff4d10b95aa9a984455b4f4b976
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658817"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="15c33-103">Obsługa błędów w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15c33-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="15c33-104">Autorzy [Dykstra](https://github.com/tdykstra/) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="15c33-104">By [Tom Dykstra](https://github.com/tdykstra/) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="15c33-105">W tym artykule opisano typowe podejścia do obsługi błędów w aplikacjach sieci Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15c33-105">This article covers common approaches to handling errors in ASP.NET Core web apps.</span></span> <span data-ttu-id="15c33-106">Zobacz <xref:web-api/handle-errors> interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="15c33-106">See <xref:web-api/handle-errors> for web APIs.</span></span>

<span data-ttu-id="15c33-107">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="15c33-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="15c33-108">([Jak pobrać](xref:index#how-to-download-a-sample)) Artykuł zawiera instrukcje dotyczące sposobu ustawiania dyrektyw preprocesora (`#if`, `#endif`, `#define`) w przykładowej aplikacji, aby umożliwić korzystanie z różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="15c33-108">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="15c33-109">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="15c33-109">Developer Exception Page</span></span>

<span data-ttu-id="15c33-110">Na *stronie wyjątku dla deweloperów* są wyświetlane szczegółowe informacje o wyjątkach żądania.</span><span class="sxs-lookup"><span data-stu-id="15c33-110">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="15c33-111">Strona jest udostępniana przez pakiet [Microsoft. AspNetCore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) , który znajduje się w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="15c33-111">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="15c33-112">Dodaj kod do metody `Startup.Configure`, aby włączyć stronę, gdy aplikacja jest uruchomiona w [środowisku](xref:fundamentals/environments)deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="15c33-112">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="15c33-113">Należy umieścić wywołanie <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> przed dowolnym oprogramowanie pośredniczące, które ma przechwytywać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="15c33-113">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="15c33-114">Stronę wyjątku dla deweloperów należy włączyć tylko wtedy, **gdy aplikacja jest uruchomiona w środowisku deweloperskim**.</span><span class="sxs-lookup"><span data-stu-id="15c33-114">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="15c33-115">Nie chcesz udostępniać szczegółowych informacji o wyjątku publicznie, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="15c33-115">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="15c33-116">Aby uzyskać więcej informacji na temat konfigurowania środowisk, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="15c33-116">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="15c33-117">Na stronie znajdują się następujące informacje o wyjątku i żądaniu:</span><span class="sxs-lookup"><span data-stu-id="15c33-117">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="15c33-118">Ślad stosu</span><span class="sxs-lookup"><span data-stu-id="15c33-118">Stack trace</span></span>
* <span data-ttu-id="15c33-119">Parametry ciągu zapytania (jeśli istnieją)</span><span class="sxs-lookup"><span data-stu-id="15c33-119">Query string parameters (if any)</span></span>
* <span data-ttu-id="15c33-120">Pliki cookie (jeśli istnieją)</span><span class="sxs-lookup"><span data-stu-id="15c33-120">Cookies (if any)</span></span>
* <span data-ttu-id="15c33-121">Nagłówki</span><span class="sxs-lookup"><span data-stu-id="15c33-121">Headers</span></span>

<span data-ttu-id="15c33-122">Aby wyświetlić stronę wyjątku dla deweloperów w [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj dyrektywy preprocesora `DevEnvironment` i wybierz pozycję **Wyzwól wyjątek** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="15c33-122">To see the Developer Exception Page in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="15c33-123">Strona obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="15c33-123">Exception handler page</span></span>

<span data-ttu-id="15c33-124">Aby skonfigurować niestandardową stronę obsługi błędów dla środowiska produkcyjnego, należy użyć wyjątku obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="15c33-124">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="15c33-125">Oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="15c33-125">The middleware:</span></span>

* <span data-ttu-id="15c33-126">Przechwytuje i rejestruje wyjątki.</span><span class="sxs-lookup"><span data-stu-id="15c33-126">Catches and logs exceptions.</span></span>
* <span data-ttu-id="15c33-127">Wykonuje ponowne żądanie w alternatywnym potoku dla wskazanej strony lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="15c33-127">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="15c33-128">Żądanie nie jest wykonywane w przypadku rozpoczęcia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="15c33-128">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="15c33-129">Poniższy przykład <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> dodaje wyjątek obsługujący oprogramowanie pośredniczące w środowiskach innych niż programistyczne:</span><span class="sxs-lookup"><span data-stu-id="15c33-129">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="15c33-130">Szablon aplikacji Razor Pages zawiera stronę błędów ( *. cshtml*) i klasę <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="15c33-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="15c33-131">W przypadku aplikacji MVC szablon projektu zawiera metodę akcji błędu i widok błędu.</span><span class="sxs-lookup"><span data-stu-id="15c33-131">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="15c33-132">Oto Metoda działania:</span><span class="sxs-lookup"><span data-stu-id="15c33-132">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="15c33-133">Nie zaznaczaj metody akcji procedury obsługi błędów z atrybutami metody HTTP, takimi jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="15c33-133">Don't mark the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="15c33-134">Jawne czasowniki uniemożliwiają niektórym żądaniem osiągnięcie metody.</span><span class="sxs-lookup"><span data-stu-id="15c33-134">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="15c33-135">Zezwalaj na anonimowy dostęp do metody, aby nieuwierzytelnionym użytkownikom mogli odebrać widok błędów.</span><span class="sxs-lookup"><span data-stu-id="15c33-135">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="15c33-136">Uzyskaj dostęp do wyjątku</span><span class="sxs-lookup"><span data-stu-id="15c33-136">Access the exception</span></span>

<span data-ttu-id="15c33-137">Użyj <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature>, aby uzyskać dostęp do wyjątku i oryginalnej ścieżki żądania w kontrolerze lub stronie procedury obsługi błędów:</span><span class="sxs-lookup"><span data-stu-id="15c33-137">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="15c33-138">**Nie** należy podawać poufnych informacji o błędach do klientów.</span><span class="sxs-lookup"><span data-stu-id="15c33-138">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="15c33-139">Obsługa błędów stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="15c33-139">Serving errors is a security risk.</span></span>

<span data-ttu-id="15c33-140">Aby wyświetlić stronę obsługa wyjątków w [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj dyrektyw preprocesora `ProdEnvironment` i `ErrorHandlerPage`, a następnie wybierz pozycję **Wyzwól wyjątek** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="15c33-140">To see the exception handling page in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="15c33-141">Procedura obsługi wyjątków lambda</span><span class="sxs-lookup"><span data-stu-id="15c33-141">Exception handler lambda</span></span>

<span data-ttu-id="15c33-142">Alternatywą dla [niestandardowej strony obsługi wyjątków](#exception-handler-page) jest podanie wyrażenia lambda do <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="15c33-142">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="15c33-143">Użycie wyrażenia lambda umożliwia dostęp do błędu przed zwróceniem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="15c33-143">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="15c33-144">Oto przykład użycia wyrażenia lambda dla obsługi wyjątków:</span><span class="sxs-lookup"><span data-stu-id="15c33-144">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

<span data-ttu-id="15c33-145">W poprzednim kodzie `await context.Response.WriteAsync(new string(' ', 512));` został dodany, aby przeglądarka Internet Explorer wyświetlała komunikat o błędzie zamiast komunikatu o błędzie programu IE.</span><span class="sxs-lookup"><span data-stu-id="15c33-145">In the preceding code, `await context.Response.WriteAsync(new string(' ', 512));` is added so the Internet Explorer browser displays the error message rather than an IE error message.</span></span> <span data-ttu-id="15c33-146">Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/16144)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="15c33-146">For more information, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/16144).</span></span>

> [!WARNING]
> <span data-ttu-id="15c33-147">**Nie** należy podawać poufnych informacji o błędach z <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> lub <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> do klientów.</span><span class="sxs-lookup"><span data-stu-id="15c33-147">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="15c33-148">Obsługa błędów stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="15c33-148">Serving errors is a security risk.</span></span>

<span data-ttu-id="15c33-149">Aby zobaczyć wynik obsługi wyjątku lambda w [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj dyrektyw preprocesora `ProdEnvironment` i `ErrorHandlerLambda`, a następnie wybierz pozycję **Wyzwól wyjątek** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="15c33-149">To see the result of the exception handling lambda in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="15c33-150">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="15c33-150">UseStatusCodePages</span></span>

<span data-ttu-id="15c33-151">Domyślnie aplikacja ASP.NET Core nie udostępnia strony kodowej stanu dla kodów stanu HTTP, takich jak *404 — nie znaleziono*.</span><span class="sxs-lookup"><span data-stu-id="15c33-151">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="15c33-152">Aplikacja zwraca kod stanu i pustą treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="15c33-152">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="15c33-153">Aby udostępnić strony kodu stanu, użyj oprogramowania pośredniczącego stron kodowych.</span><span class="sxs-lookup"><span data-stu-id="15c33-153">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="15c33-154">Oprogramowanie pośredniczące jest udostępniane przez pakiet [Microsoft. AspNetCore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) , który znajduje się w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="15c33-154">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="15c33-155">Aby włączyć obsługę tylko tekstu dla typowych kodów stanu błędu, wywołaj <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> w metodzie `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="15c33-155">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="15c33-156">Wywołaj `UseStatusCodePages` przed zażądaniem obsługi oprogramowania pośredniczącego (na przykład statycznych plików oprogramowania pośredniczącego i oprogramowania MVC).</span><span class="sxs-lookup"><span data-stu-id="15c33-156">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="15c33-157">Oto przykład tekstu wyświetlanego przez domyślne programy obsługi:</span><span class="sxs-lookup"><span data-stu-id="15c33-157">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="15c33-158">Aby wyświetlić jeden z różnych formatów stron kodu stanu w [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), należy użyć jednej z dyrektyw preprocesora, która rozpoczyna się od `StatusCodePages`, a na stronie głównej wybierz pozycję **Wyzwól a 404** .</span><span class="sxs-lookup"><span data-stu-id="15c33-158">To see one of the various status code page formats in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="15c33-159">UseStatusCodePages z ciągiem formatu</span><span class="sxs-lookup"><span data-stu-id="15c33-159">UseStatusCodePages with format string</span></span>

<span data-ttu-id="15c33-160">Aby dostosować typ zawartości odpowiedzi i tekst, Użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>, które pobiera typ zawartości i ciąg formatu:</span><span class="sxs-lookup"><span data-stu-id="15c33-160">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="15c33-161">UseStatusCodePages z wyrażeniem lambda</span><span class="sxs-lookup"><span data-stu-id="15c33-161">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="15c33-162">Aby określić niestandardową obsługę błędów i kod do pisania, Użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>, które przyjmuje wyrażenie lambda:</span><span class="sxs-lookup"><span data-stu-id="15c33-162">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirects"></a><span data-ttu-id="15c33-163">UseStatusCodePagesWithRedirects</span><span class="sxs-lookup"><span data-stu-id="15c33-163">UseStatusCodePagesWithRedirects</span></span>

<span data-ttu-id="15c33-164">Metoda rozszerzenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="15c33-164">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="15c33-165">Wysyła do klienta kod stanu *znaleziony przez 302* .</span><span class="sxs-lookup"><span data-stu-id="15c33-165">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="15c33-166">Przekierowuje klienta do lokalizacji podanej w szablonie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="15c33-166">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="15c33-167">Szablon adresu URL może zawierać `{0}` symbol zastępczy dla kodu stanu, jak pokazano w przykładzie.</span><span class="sxs-lookup"><span data-stu-id="15c33-167">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="15c33-168">Jeśli szablon adresu URL zaczyna się od tyldy (~), tylda jest zastępowana `PathBase`aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15c33-168">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="15c33-169">Jeśli wskażesz punkt końcowy w aplikacji, Utwórz widok MVC lub stronę Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="15c33-169">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="15c33-170">Aby uzyskać Razor Pages przykład, zobacz *Pages/StatusCode. cshtml* w [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="15c33-170">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="15c33-171">Ta metoda jest często używana w przypadku aplikacji:</span><span class="sxs-lookup"><span data-stu-id="15c33-171">This method is commonly used when the app:</span></span>

* <span data-ttu-id="15c33-172">Powinien przekierować klienta do innego punktu końcowego, zazwyczaj w przypadkach, gdy inna aplikacja przetwarza błąd.</span><span class="sxs-lookup"><span data-stu-id="15c33-172">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="15c33-173">W przypadku usługi Web Apps pasek adresu przeglądarki klienta odzwierciedla przekierowany punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="15c33-173">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="15c33-174">Nie należy zachować oryginalnego kodu stanu i zwrócić go do początkowej odpowiedzi przekierowania.</span><span class="sxs-lookup"><span data-stu-id="15c33-174">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="15c33-175">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="15c33-175">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="15c33-176">Metoda rozszerzenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="15c33-176">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="15c33-177">Zwraca oryginalny kod stanu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="15c33-177">Returns the original status code to the client.</span></span>
* <span data-ttu-id="15c33-178">Generuje treść odpowiedzi przez ponowne wykonanie potoku żądania przy użyciu ścieżki alternatywnej.</span><span class="sxs-lookup"><span data-stu-id="15c33-178">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="15c33-179">Jeśli wskażesz punkt końcowy w aplikacji, Utwórz widok MVC lub stronę Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="15c33-179">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="15c33-180">Aby uzyskać Razor Pages przykład, zobacz *Pages/StatusCode. cshtml* w [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="15c33-180">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="15c33-181">Ta metoda jest często używana, gdy aplikacja powinna:</span><span class="sxs-lookup"><span data-stu-id="15c33-181">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="15c33-182">Przetwórz żądanie bez przekierowania do innego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="15c33-182">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="15c33-183">W przypadku usługi Web Apps pasek adresu przeglądarki klienta odzwierciedla pierwotnie żądany punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="15c33-183">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="15c33-184">Zachowywanie i zwracanie oryginalnego kodu stanu z odpowiedzią.</span><span class="sxs-lookup"><span data-stu-id="15c33-184">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="15c33-185">Szablony adresu URL i ciągu zapytania mogą zawierać symbol zastępczy (`{0}`) dla kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="15c33-185">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="15c33-186">Szablon adresu URL musi rozpoczynać się od ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="15c33-186">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="15c33-187">W przypadku używania symbolu zastępczego w ścieżce upewnij się, że punkt końcowy (strona lub kontroler) może przetworzyć segment ścieżki.</span><span class="sxs-lookup"><span data-stu-id="15c33-187">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="15c33-188">Na przykład strona Razor dla błędów powinna akceptować opcjonalną wartość segmentu ścieżki przy użyciu dyrektywy `@page`:</span><span class="sxs-lookup"><span data-stu-id="15c33-188">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="15c33-189">Punkt końcowy, który przetwarza błąd, może uzyskać oryginalny adres URL, który wygenerował błąd, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="15c33-189">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="15c33-190">Wyłącz strony kodu stanu</span><span class="sxs-lookup"><span data-stu-id="15c33-190">Disable status code pages</span></span>

<span data-ttu-id="15c33-191">Aby wyłączyć strony kodowe stanu dla kontrolera MVC lub metody akcji, Użyj atrybutu [`[SkipStatusCodePages]`](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) .</span><span class="sxs-lookup"><span data-stu-id="15c33-191">To disable status code pages for an MVC controller or action method, use the [`[SkipStatusCodePages]`](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) attribute.</span></span>

<span data-ttu-id="15c33-192">Aby wyłączyć strony kodowe stanu dla określonych żądań w metodzie obsługi Razor Pages lub w kontrolerze MVC, użyj <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span><span class="sxs-lookup"><span data-stu-id="15c33-192">To disable status code pages for specific requests in a Razor Pages handler method or in an MVC controller, use <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="15c33-193">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="15c33-193">Exception-handling code</span></span>

<span data-ttu-id="15c33-194">Kod na stronach obsługi wyjątków może generować wyjątki.</span><span class="sxs-lookup"><span data-stu-id="15c33-194">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="15c33-195">Często dobrym pomysłem jest, aby strony błędów produkcyjnych obejmowały czysto statyczną zawartość.</span><span class="sxs-lookup"><span data-stu-id="15c33-195">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="15c33-196">Nagłówki odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="15c33-196">Response headers</span></span>

<span data-ttu-id="15c33-197">Po wysłaniu nagłówków odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="15c33-197">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="15c33-198">Aplikacja nie może zmienić kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="15c33-198">The app can't change the response's status code.</span></span>
* <span data-ttu-id="15c33-199">Nie można uruchomić żadnych stron ani programów wyjątków.</span><span class="sxs-lookup"><span data-stu-id="15c33-199">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="15c33-200">Odpowiedź musi zostać zakończona lub połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="15c33-200">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="15c33-201">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="15c33-201">Server exception handling</span></span>

<span data-ttu-id="15c33-202">Oprócz logiki obsługi wyjątków w aplikacji, [Implementacja serwera http](xref:fundamentals/servers/index) może obsłużyć wyjątki.</span><span class="sxs-lookup"><span data-stu-id="15c33-202">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="15c33-203">Jeśli serwer przechwytuje wyjątek przed wysłaniem nagłówków odpowiedzi, serwer wysyła do *500 — wewnętrzny błąd serwera* bez treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="15c33-203">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="15c33-204">Jeśli serwer przechwytuje wyjątek po wysłaniu nagłówków odpowiedzi, serwer zamknie połączenie.</span><span class="sxs-lookup"><span data-stu-id="15c33-204">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="15c33-205">Żądania, które nie są obsługiwane przez aplikację, są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="15c33-205">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="15c33-206">Każdy wyjątek, który występuje, gdy serwer obsługuje żądanie jest obsługiwany przez obsługę wyjątków serwera.</span><span class="sxs-lookup"><span data-stu-id="15c33-206">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="15c33-207">Niestandardowe strony błędów aplikacji, obsługa wyjątków i filtry nie wpływają na to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="15c33-207">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="15c33-208">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="15c33-208">Startup exception handling</span></span>

<span data-ttu-id="15c33-209">Tylko warstwa hostingu może obsługiwać wyjątki, które odbywają się podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15c33-209">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="15c33-210">Host można skonfigurować do [przechwytywania błędów uruchamiania](xref:fundamentals/host/web-host#capture-startup-errors) i [przechwytywania szczegółowych błędów](xref:fundamentals/host/web-host#detailed-errors).</span><span class="sxs-lookup"><span data-stu-id="15c33-210">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="15c33-211">W warstwie hostingu może zostać wyświetlona strona błędu przechwyconego błędu uruchomienia tylko wtedy, gdy błąd występuje po powiązaniu adresu hosta/portu.</span><span class="sxs-lookup"><span data-stu-id="15c33-211">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="15c33-212">Jeśli powiązanie nie powiedzie się:</span><span class="sxs-lookup"><span data-stu-id="15c33-212">If binding fails:</span></span>

* <span data-ttu-id="15c33-213">Warstwa hostingu rejestruje wyjątek krytyczny.</span><span class="sxs-lookup"><span data-stu-id="15c33-213">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="15c33-214">Proces dotnet ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="15c33-214">The dotnet process crashes.</span></span>
* <span data-ttu-id="15c33-215">Nie zostanie wyświetlona strona błędu, gdy serwer HTTP jest [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="15c33-215">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="15c33-216">W przypadku uruchamiania [programu w usługach IIS](/iis) (lub Azure App Service) lub [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), [moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) zwrócił *błąd 502,5-proces* , jeśli nie można uruchomić procesu.</span><span class="sxs-lookup"><span data-stu-id="15c33-216">When running on [IIS](/iis) (or Azure App Service) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="15c33-217">Aby uzyskać więcej informacji, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="15c33-217">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="15c33-218">Strona błędu bazy danych</span><span class="sxs-lookup"><span data-stu-id="15c33-218">Database error page</span></span>

<span data-ttu-id="15c33-219">Strona błędu bazy danych oprogramowanie pośredniczące przechwytuje wyjątki związane z bazą danych, które można rozwiązać za pomocą migracji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15c33-219">Database Error Page Middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="15c33-220">Kiedy te wyjątki wystąpią, zostanie wygenerowana odpowiedź HTML z szczegółowymi działaniami umożliwiającymi rozwiązanie problemu.</span><span class="sxs-lookup"><span data-stu-id="15c33-220">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="15c33-221">Ta strona powinna być włączona tylko w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="15c33-221">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="15c33-222">Włącz stronę, dodając kod do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="15c33-222">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

<!-- FUTURE UPDATE: On the next topic overhaul/release update, add API crosslink to this section for xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage* when available via the API docs. -->

## <a name="exception-filters"></a><span data-ttu-id="15c33-223">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="15c33-223">Exception filters</span></span>

<span data-ttu-id="15c33-224">W aplikacjach MVC filtry wyjątków można konfigurować globalnie lub na podstawie poszczególnych kontrolerów lub akcji.</span><span class="sxs-lookup"><span data-stu-id="15c33-224">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="15c33-225">W aplikacjach Razor Pages można je skonfigurować globalnie lub na model strony.</span><span class="sxs-lookup"><span data-stu-id="15c33-225">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="15c33-226">Te filtry obsługują wszystkie Nieobsłużone wyjątki, które występują podczas wykonywania akcji kontrolera lub innego filtru.</span><span class="sxs-lookup"><span data-stu-id="15c33-226">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="15c33-227">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="15c33-227">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="15c33-228">Filtry wyjątków są przydatne w przypadku zalewek wyjątków, które występują w akcjach MVC, ale nie są tak elastyczne, jak wyjątek obsługujący oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="15c33-228">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="15c33-229">Zalecamy używanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="15c33-229">We recommend using the middleware.</span></span> <span data-ttu-id="15c33-230">Używaj filtrów tylko wtedy, gdy musisz wykonać obsługę błędów w różny sposób, w zależności od wybranej akcji MVC.</span><span class="sxs-lookup"><span data-stu-id="15c33-230">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="15c33-231">Błędy stanu modelu</span><span class="sxs-lookup"><span data-stu-id="15c33-231">Model state errors</span></span>

<span data-ttu-id="15c33-232">Aby uzyskać informacje o sposobie obsługi błędów stanu modelu, zobacz [powiązanie modelu](xref:mvc/models/model-binding) i [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="15c33-232">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15c33-233">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="15c33-233">Additional resources</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
