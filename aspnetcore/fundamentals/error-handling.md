---
title: Obsługa błędów w ASP.NET Core
author: rick-anderson
description: Odkryj, jak obsługiwać błędy w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a5bdbc3ce75f5897c9cd67fe18897281bf2fb57b
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037571"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="2d48a-103">Obsługa błędów w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2d48a-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="2d48a-104">Autorzy [Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2d48a-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2d48a-105">W tym artykule opisano typowe podejścia do obsługi błędów w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2d48a-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="2d48a-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="2d48a-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="2d48a-107">([Jak pobrać](xref:index#how-to-download-a-sample)) Artykuł zawiera instrukcje dotyczące sposobu ustawiania dyrektyw preprocesora (`#if`, `#endif`, `#define`) w przykładowej aplikacji, aby umożliwić korzystanie z różnych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="2d48a-107">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="2d48a-108">Strona wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="2d48a-108">Developer Exception Page</span></span>

<span data-ttu-id="2d48a-109">Na *stronie wyjątku dla deweloperów* są wyświetlane szczegółowe informacje o wyjątkach żądania.</span><span class="sxs-lookup"><span data-stu-id="2d48a-109">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="2d48a-110">Strona jest udostępniana przez pakiet [Microsoft. AspNetCore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) , który znajduje się w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2d48a-110">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2d48a-111">Dodaj kod do metody `Startup.Configure`, aby włączyć stronę, gdy aplikacja jest uruchomiona w [środowisku](xref:fundamentals/environments)deweloperskim:</span><span class="sxs-lookup"><span data-stu-id="2d48a-111">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="2d48a-112">Należy umieścić wywołanie <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> przed dowolnym oprogramowanie pośredniczące, które ma przechwytywać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="2d48a-112">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="2d48a-113">Stronę wyjątku dla deweloperów należy włączyć tylko wtedy, **gdy aplikacja jest uruchomiona w środowisku deweloperskim**.</span><span class="sxs-lookup"><span data-stu-id="2d48a-113">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="2d48a-114">Nie chcesz udostępniać szczegółowych informacji o wyjątku publicznie, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="2d48a-114">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="2d48a-115">Aby uzyskać więcej informacji na temat konfigurowania środowisk, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="2d48a-115">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="2d48a-116">Na stronie znajdują się następujące informacje o wyjątku i żądaniu:</span><span class="sxs-lookup"><span data-stu-id="2d48a-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="2d48a-117">Ślad stosu</span><span class="sxs-lookup"><span data-stu-id="2d48a-117">Stack trace</span></span>
* <span data-ttu-id="2d48a-118">Parametry ciągu zapytania (jeśli istnieją)</span><span class="sxs-lookup"><span data-stu-id="2d48a-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="2d48a-119">Pliki cookie (jeśli istnieją)</span><span class="sxs-lookup"><span data-stu-id="2d48a-119">Cookies (if any)</span></span>
* <span data-ttu-id="2d48a-120">Nagłówka</span><span class="sxs-lookup"><span data-stu-id="2d48a-120">Headers</span></span>

<span data-ttu-id="2d48a-121">Aby wyświetlić stronę wyjątku dla deweloperów w [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj dyrektywy preprocesora `DevEnvironment` i wybierz pozycję **Wyzwól wyjątek** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="2d48a-121">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="2d48a-122">Strona obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="2d48a-122">Exception handler page</span></span>

<span data-ttu-id="2d48a-123">Aby skonfigurować niestandardową stronę obsługi błędów dla środowiska produkcyjnego, należy użyć wyjątku obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2d48a-123">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="2d48a-124">Oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="2d48a-124">The middleware:</span></span>

* <span data-ttu-id="2d48a-125">Przechwytuje i rejestruje wyjątki.</span><span class="sxs-lookup"><span data-stu-id="2d48a-125">Catches and logs exceptions.</span></span>
* <span data-ttu-id="2d48a-126">Wykonuje ponowne żądanie w alternatywnym potoku dla wskazanej strony lub kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2d48a-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="2d48a-127">Żądanie nie jest wykonywane w przypadku rozpoczęcia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2d48a-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="2d48a-128">W poniższym przykładzie <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> dodaje wyjątek obsługujący oprogramowanie pośredniczące w środowiskach innych niż programistyczne:</span><span class="sxs-lookup"><span data-stu-id="2d48a-128">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="2d48a-129">Szablon aplikacji Razor Pages zawiera stronę błędów ( *. cshtml*) i <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> klasy (`ErrorModel`) w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="2d48a-129">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="2d48a-130">W przypadku aplikacji MVC szablon projektu zawiera metodę akcji błędu i widok błędu.</span><span class="sxs-lookup"><span data-stu-id="2d48a-130">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="2d48a-131">Oto Metoda działania:</span><span class="sxs-lookup"><span data-stu-id="2d48a-131">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="2d48a-132">Nie dekorować metody akcji procedury obsługi błędów z atrybutami metody HTTP, takimi jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="2d48a-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="2d48a-133">Jawne czasowniki uniemożliwiają niektórym żądaniem osiągnięcie metody.</span><span class="sxs-lookup"><span data-stu-id="2d48a-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="2d48a-134">Zezwalaj na anonimowy dostęp do metody, aby nieuwierzytelnionym użytkownikom mogli odebrać widok błędów.</span><span class="sxs-lookup"><span data-stu-id="2d48a-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="2d48a-135">Uzyskaj dostęp do wyjątku</span><span class="sxs-lookup"><span data-stu-id="2d48a-135">Access the exception</span></span>

<span data-ttu-id="2d48a-136">Użyj <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature>, aby uzyskać dostęp do wyjątku i oryginalnej ścieżki żądania w kontrolerze lub stronie procedury obsługi błędów:</span><span class="sxs-lookup"><span data-stu-id="2d48a-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="2d48a-137">**Nie** należy podawać poufnych informacji o błędach do klientów.</span><span class="sxs-lookup"><span data-stu-id="2d48a-137">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="2d48a-138">Obsługa błędów stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="2d48a-138">Serving errors is a security risk.</span></span>

<span data-ttu-id="2d48a-139">Aby wyświetlić stronę obsługa wyjątków w [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj dyrektyw preprocesora `ProdEnvironment` i `ErrorHandlerPage`, a następnie wybierz pozycję **Wyzwól wyjątek** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="2d48a-139">To see the exception handling page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="2d48a-140">Procedura obsługi wyjątków lambda</span><span class="sxs-lookup"><span data-stu-id="2d48a-140">Exception handler lambda</span></span>

<span data-ttu-id="2d48a-141">Alternatywą dla [niestandardowej strony obsługi wyjątków](#exception-handler-page) jest udostępnienie wyrażenia lambda do <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span><span class="sxs-lookup"><span data-stu-id="2d48a-141">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="2d48a-142">Użycie wyrażenia lambda umożliwia dostęp do błędu przed zwróceniem odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2d48a-142">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="2d48a-143">Oto przykład użycia wyrażenia lambda dla obsługi wyjątków:</span><span class="sxs-lookup"><span data-stu-id="2d48a-143">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> <span data-ttu-id="2d48a-144">**Nie** należy podawać poufnych informacji o błędach z <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> lub <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> do klientów.</span><span class="sxs-lookup"><span data-stu-id="2d48a-144">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="2d48a-145">Obsługa błędów stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="2d48a-145">Serving errors is a security risk.</span></span>

<span data-ttu-id="2d48a-146">Aby zobaczyć wynik obsługi wyjątku lambda w [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj dyrektyw preprocesora `ProdEnvironment` i `ErrorHandlerLambda`, a następnie wybierz pozycję **Wyzwól wyjątek** na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="2d48a-146">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="2d48a-147">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="2d48a-147">UseStatusCodePages</span></span>

<span data-ttu-id="2d48a-148">Domyślnie aplikacja ASP.NET Core nie udostępnia strony kodowej stanu dla kodów stanu HTTP, takich jak *404 — nie znaleziono*.</span><span class="sxs-lookup"><span data-stu-id="2d48a-148">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="2d48a-149">Aplikacja zwraca kod stanu i pustą treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2d48a-149">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="2d48a-150">Aby udostępnić strony kodu stanu, użyj oprogramowania pośredniczącego stron kodowych.</span><span class="sxs-lookup"><span data-stu-id="2d48a-150">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="2d48a-151">Oprogramowanie pośredniczące jest udostępniane przez pakiet [Microsoft. AspNetCore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) , który znajduje się w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2d48a-151">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2d48a-152">Aby włączyć obsługę tylko tekstu dla typowych kodów stanu błędu, wywołaj <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> w metodzie `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2d48a-152">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="2d48a-153">Wywołaj `UseStatusCodePages` przed zażądaniem obsługi oprogramowania pośredniczącego (na przykład statycznego oprogramowania pośredniczącego i oprogramowania MVC).</span><span class="sxs-lookup"><span data-stu-id="2d48a-153">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="2d48a-154">Oto przykład tekstu wyświetlanego przez domyślne programy obsługi:</span><span class="sxs-lookup"><span data-stu-id="2d48a-154">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="2d48a-155">Aby wyświetlić jeden z różnych formatów stron kodu stanu w [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), należy użyć jednej z dyrektyw preprocesora, które zaczynają się od `StatusCodePages` i na stronie głównej wybierz opcję **Wyzwól a 404** .</span><span class="sxs-lookup"><span data-stu-id="2d48a-155">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="2d48a-156">UseStatusCodePages z ciągiem formatu</span><span class="sxs-lookup"><span data-stu-id="2d48a-156">UseStatusCodePages with format string</span></span>

<span data-ttu-id="2d48a-157">Aby dostosować typ zawartości odpowiedzi i tekst, Użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>, które pobiera typ zawartości i ciąg formatu:</span><span class="sxs-lookup"><span data-stu-id="2d48a-157">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="2d48a-158">UseStatusCodePages z wyrażeniem lambda</span><span class="sxs-lookup"><span data-stu-id="2d48a-158">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="2d48a-159">Aby określić niestandardową obsługę błędów i kod do pisania, Użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>, które przyjmuje wyrażenie lambda:</span><span class="sxs-lookup"><span data-stu-id="2d48a-159">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a><span data-ttu-id="2d48a-160">UseStatusCodePagesWithRedirect</span><span class="sxs-lookup"><span data-stu-id="2d48a-160">UseStatusCodePagesWithRedirect</span></span>

<span data-ttu-id="2d48a-161">Metoda rozszerzenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="2d48a-161">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="2d48a-162">Wysyła do klienta kod stanu *znaleziony przez 302* .</span><span class="sxs-lookup"><span data-stu-id="2d48a-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="2d48a-163">Przekierowuje klienta do lokalizacji podanej w szablonie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2d48a-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="2d48a-164">Szablon adresu URL może zawierać symbol zastępczy `{0}` dla kodu stanu, jak pokazano w przykładzie.</span><span class="sxs-lookup"><span data-stu-id="2d48a-164">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="2d48a-165">Jeśli szablon adresu URL zaczyna się od tyldy (~), tylda jest zastępowana `PathBase` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d48a-165">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="2d48a-166">Jeśli wskażesz punkt końcowy w aplikacji, Utwórz widok MVC lub stronę Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="2d48a-166">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="2d48a-167">Aby uzyskać Razor Pages przykład, zobacz *Pages/StatusCode. cshtml* w [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="2d48a-167">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="2d48a-168">Ta metoda jest często używana w przypadku aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2d48a-168">This method is commonly used when the app:</span></span>

* <span data-ttu-id="2d48a-169">Powinien przekierować klienta do innego punktu końcowego, zazwyczaj w przypadkach, gdy inna aplikacja przetwarza błąd.</span><span class="sxs-lookup"><span data-stu-id="2d48a-169">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="2d48a-170">W przypadku usługi Web Apps pasek adresu przeglądarki klienta odzwierciedla przekierowany punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="2d48a-170">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="2d48a-171">Nie należy zachować oryginalnego kodu stanu i zwrócić go do początkowej odpowiedzi przekierowania.</span><span class="sxs-lookup"><span data-stu-id="2d48a-171">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="2d48a-172">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="2d48a-172">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="2d48a-173">Metoda rozszerzenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="2d48a-173">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="2d48a-174">Zwraca oryginalny kod stanu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="2d48a-174">Returns the original status code to the client.</span></span>
* <span data-ttu-id="2d48a-175">Generuje treść odpowiedzi przez ponowne wykonanie potoku żądania przy użyciu ścieżki alternatywnej.</span><span class="sxs-lookup"><span data-stu-id="2d48a-175">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="2d48a-176">Jeśli wskażesz punkt końcowy w aplikacji, Utwórz widok MVC lub stronę Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="2d48a-176">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="2d48a-177">Aby uzyskać Razor Pages przykład, zobacz *Pages/StatusCode. cshtml* w [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span><span class="sxs-lookup"><span data-stu-id="2d48a-177">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="2d48a-178">Ta metoda jest często używana, gdy aplikacja powinna:</span><span class="sxs-lookup"><span data-stu-id="2d48a-178">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="2d48a-179">Przetwórz żądanie bez przekierowania do innego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="2d48a-179">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="2d48a-180">W przypadku usługi Web Apps pasek adresu przeglądarki klienta odzwierciedla pierwotnie żądany punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="2d48a-180">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="2d48a-181">Zachowywanie i zwracanie oryginalnego kodu stanu z odpowiedzią.</span><span class="sxs-lookup"><span data-stu-id="2d48a-181">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="2d48a-182">Szablony adresu URL i ciągu zapytania mogą zawierać symbol zastępczy (`{0}`) dla kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="2d48a-182">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="2d48a-183">Szablon adresu URL musi rozpoczynać się od ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="2d48a-183">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="2d48a-184">W przypadku używania symbolu zastępczego w ścieżce upewnij się, że punkt końcowy (strona lub kontroler) może przetworzyć segment ścieżki.</span><span class="sxs-lookup"><span data-stu-id="2d48a-184">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="2d48a-185">Na przykład strona Razor dla błędów powinna akceptować opcjonalną wartość segmentu ścieżki z dyrektywą `@page`:</span><span class="sxs-lookup"><span data-stu-id="2d48a-185">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="2d48a-186">Punkt końcowy, który przetwarza błąd, może uzyskać oryginalny adres URL, który wygenerował błąd, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="2d48a-186">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="2d48a-187">Wyłącz strony kodu stanu</span><span class="sxs-lookup"><span data-stu-id="2d48a-187">Disable status code pages</span></span>

<span data-ttu-id="2d48a-188">Aby wyłączyć strony kodowe stanu dla kontrolera MVC lub metody akcji, Użyj atrybutu [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) .</span><span class="sxs-lookup"><span data-stu-id="2d48a-188">To disable status code pages for an MVC controller or action method, use the [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) attribute.</span></span>

<span data-ttu-id="2d48a-189">Aby wyłączyć strony kodowe stanu dla określonych żądań w metodzie obsługi Razor Pages lub w kontrolerze MVC, użyj <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span><span class="sxs-lookup"><span data-stu-id="2d48a-189">To disable status code pages for specific requests in a Razor Pages handler method or in an MVC controller, use <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="2d48a-190">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="2d48a-190">Exception-handling code</span></span>

<span data-ttu-id="2d48a-191">Kod na stronach obsługi wyjątków może generować wyjątki.</span><span class="sxs-lookup"><span data-stu-id="2d48a-191">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="2d48a-192">Często dobrym pomysłem jest, aby strony błędów produkcyjnych obejmowały czysto statyczną zawartość.</span><span class="sxs-lookup"><span data-stu-id="2d48a-192">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="2d48a-193">Nagłówki odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="2d48a-193">Response headers</span></span>

<span data-ttu-id="2d48a-194">Po wysłaniu nagłówków odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="2d48a-194">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="2d48a-195">Aplikacja nie może zmienić kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2d48a-195">The app can't change the response's status code.</span></span>
* <span data-ttu-id="2d48a-196">Nie można uruchomić żadnych stron ani programów wyjątków.</span><span class="sxs-lookup"><span data-stu-id="2d48a-196">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="2d48a-197">Odpowiedź musi zostać zakończona lub połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="2d48a-197">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="2d48a-198">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="2d48a-198">Server exception handling</span></span>

<span data-ttu-id="2d48a-199">Oprócz logiki obsługi wyjątków w aplikacji, [Implementacja serwera http](xref:fundamentals/servers/index) może obsłużyć wyjątki.</span><span class="sxs-lookup"><span data-stu-id="2d48a-199">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="2d48a-200">Jeśli serwer przechwytuje wyjątek przed wysłaniem nagłówków odpowiedzi, serwer wysyła do *500 — wewnętrzny błąd serwera* bez treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="2d48a-200">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="2d48a-201">Jeśli serwer przechwytuje wyjątek po wysłaniu nagłówków odpowiedzi, serwer zamknie połączenie.</span><span class="sxs-lookup"><span data-stu-id="2d48a-201">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="2d48a-202">Żądania, które nie są obsługiwane przez aplikację, są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="2d48a-202">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="2d48a-203">Każdy wyjątek, który występuje, gdy serwer obsługuje żądanie jest obsługiwany przez obsługę wyjątków serwera.</span><span class="sxs-lookup"><span data-stu-id="2d48a-203">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="2d48a-204">Niestandardowe strony błędów aplikacji, obsługa wyjątków i filtry nie wpływają na to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="2d48a-204">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="2d48a-205">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="2d48a-205">Startup exception handling</span></span>

<span data-ttu-id="2d48a-206">Tylko warstwa hostingu może obsługiwać wyjątki, które odbywają się podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d48a-206">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="2d48a-207">Host można skonfigurować do [przechwytywania błędów uruchamiania](xref:fundamentals/host/web-host#capture-startup-errors) i [przechwytywania szczegółowych błędów](xref:fundamentals/host/web-host#detailed-errors).</span><span class="sxs-lookup"><span data-stu-id="2d48a-207">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="2d48a-208">W warstwie hostingu może zostać wyświetlona strona błędu przechwyconego błędu uruchomienia tylko wtedy, gdy błąd występuje po powiązaniu adresu hosta/portu.</span><span class="sxs-lookup"><span data-stu-id="2d48a-208">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="2d48a-209">Jeśli powiązanie nie powiedzie się:</span><span class="sxs-lookup"><span data-stu-id="2d48a-209">If binding fails:</span></span>

* <span data-ttu-id="2d48a-210">Warstwa hostingu rejestruje wyjątek krytyczny.</span><span class="sxs-lookup"><span data-stu-id="2d48a-210">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="2d48a-211">Proces dotnet ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="2d48a-211">The dotnet process crashes.</span></span>
* <span data-ttu-id="2d48a-212">Nie zostanie wyświetlona strona błędu, gdy serwer HTTP jest [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="2d48a-212">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="2d48a-213">W przypadku uruchamiania [programu w usługach IIS](/iis) (lub Azure App Service) lub [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), [moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) zwrócił *błąd 502,5-proces* , jeśli nie można uruchomić procesu.</span><span class="sxs-lookup"><span data-stu-id="2d48a-213">When running on [IIS](/iis) (or Azure App Service) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="2d48a-214">Aby uzyskać więcej informacji, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="2d48a-214">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="2d48a-215">Strona błędu bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d48a-215">Database error page</span></span>

<span data-ttu-id="2d48a-216">Strona błędu bazy danych oprogramowanie pośredniczące przechwytuje wyjątki związane z bazą danych, które można rozwiązać za pomocą migracji Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2d48a-216">Database Error Page Middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="2d48a-217">Kiedy te wyjątki wystąpią, zostanie wygenerowana odpowiedź HTML z szczegółowymi działaniami umożliwiającymi rozwiązanie problemu.</span><span class="sxs-lookup"><span data-stu-id="2d48a-217">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="2d48a-218">Ta strona powinna być włączona tylko w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="2d48a-218">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="2d48a-219">Włącz stronę, dodając kod do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2d48a-219">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

<!-- FUTURE UPDATE: On the next topic overhaul/release update, add API crosslink to this section for xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage* when available via the API docs. -->

## <a name="exception-filters"></a><span data-ttu-id="2d48a-220">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="2d48a-220">Exception filters</span></span>

<span data-ttu-id="2d48a-221">W aplikacjach MVC filtry wyjątków można konfigurować globalnie lub na podstawie poszczególnych kontrolerów lub akcji.</span><span class="sxs-lookup"><span data-stu-id="2d48a-221">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="2d48a-222">W aplikacjach Razor Pages można je skonfigurować globalnie lub na model strony.</span><span class="sxs-lookup"><span data-stu-id="2d48a-222">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="2d48a-223">Te filtry obsługują wszystkie Nieobsłużone wyjątki, które występują podczas wykonywania akcji kontrolera lub innego filtru.</span><span class="sxs-lookup"><span data-stu-id="2d48a-223">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="2d48a-224">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/filters#exception-filters>.</span><span class="sxs-lookup"><span data-stu-id="2d48a-224">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="2d48a-225">Filtry wyjątków są przydatne w przypadku zalewek wyjątków, które występują w akcjach MVC, ale nie są tak elastyczne, jak wyjątek obsługujący oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="2d48a-225">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="2d48a-226">Zalecamy używanie oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="2d48a-226">We recommend using the middleware.</span></span> <span data-ttu-id="2d48a-227">Używaj filtrów tylko wtedy, gdy musisz wykonać obsługę błędów w różny sposób, w zależności od wybranej akcji MVC.</span><span class="sxs-lookup"><span data-stu-id="2d48a-227">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="2d48a-228">Błędy stanu modelu</span><span class="sxs-lookup"><span data-stu-id="2d48a-228">Model state errors</span></span>

<span data-ttu-id="2d48a-229">Aby uzyskać informacje o sposobie obsługi błędów stanu modelu, zobacz [powiązanie modelu](xref:mvc/models/model-binding) i [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="2d48a-229">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2d48a-230">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2d48a-230">Additional resources</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
