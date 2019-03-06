---
title: Obsługa błędów w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345508"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="d2710-103">Obsługa błędów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2710-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="d2710-104">Przez [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d2710-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d2710-105">W tym artykule opisano typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d2710-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="d2710-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d2710-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="d2710-107">Stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="d2710-107">Developer Exception Page</span></span>

<span data-ttu-id="d2710-108">Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*.</span><span class="sxs-lookup"><span data-stu-id="d2710-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="d2710-109">Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d2710-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="d2710-110">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="d2710-110">Add a line to the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

<span data-ttu-id="d2710-111">Umieść wywołanie <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> przed dowolnego oprogramowania pośredniczącego, którym chcesz przechwytywać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="d2710-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="d2710-112">Włącz na stronie wyjątków dla deweloperów **tylko wtedy, gdy aplikacja jest uruchomiona w środowisku programistycznym**.</span><span class="sxs-lookup"><span data-stu-id="d2710-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="d2710-113">Nie chcesz publicznie udostępnić szczegółowe informacje o wyjątku, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="d2710-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="d2710-114">Aby uzyskać więcej informacji na temat konfigurowania środowiska, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d2710-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d2710-115">Aby wyświetlić stronę wyjątek dla deweloperów, uruchom przykładową aplikację w środowisku równa `Development` i Dodaj `?throw=true` do podstawowego adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2710-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="d2710-116">Strona zawiera następujące informacje dotyczące wyjątku i żądanie:</span><span class="sxs-lookup"><span data-stu-id="d2710-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="d2710-117">Ślad stosu</span><span class="sxs-lookup"><span data-stu-id="d2710-117">Stack trace</span></span>
* <span data-ttu-id="d2710-118">Parametry ciągu zapytania (jeśli istnieje)</span><span class="sxs-lookup"><span data-stu-id="d2710-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="d2710-119">Pliki cookie (jeśli istnieje)</span><span class="sxs-lookup"><span data-stu-id="d2710-119">Cookies (if any)</span></span>
* <span data-ttu-id="d2710-120">Nagłówki</span><span class="sxs-lookup"><span data-stu-id="d2710-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="d2710-121">Konfigurowanie niestandardowych wyjątków, Obsługa strony</span><span class="sxs-lookup"><span data-stu-id="d2710-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="d2710-122">Gdy aplikacja nie jest uruchomiona w środowisku programistycznym, wywołaj <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> metodę rozszerzenia, aby dodać obsługę wyjątków w oprogramowaniu pośredniczącym.</span><span class="sxs-lookup"><span data-stu-id="d2710-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="d2710-123">Oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="d2710-123">The middleware:</span></span>

* <span data-ttu-id="d2710-124">Przechwytuje wyjątki.</span><span class="sxs-lookup"><span data-stu-id="d2710-124">Catches exceptions.</span></span>
* <span data-ttu-id="d2710-125">Rejestruje wyjątki.</span><span class="sxs-lookup"><span data-stu-id="d2710-125">Logs exceptions.</span></span>
* <span data-ttu-id="d2710-126">Ponownie wykonuje żądanie w potoku usługi alternatywnej strony lub kontroler wskazane.</span><span class="sxs-lookup"><span data-stu-id="d2710-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="d2710-127">Żądanie nie jest wykonany ponownie, jeśli odpowiedź została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d2710-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="d2710-128">W poniższym przykładzie z przykładowej aplikacji <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> dodaje oprogramowanie pośredniczące obsługi wyjątków w środowiskach innych niż.</span><span class="sxs-lookup"><span data-stu-id="d2710-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="d2710-129">Określa metody rozszerzenia, stronę błędu lub kontrolera w `/Error` punktu końcowego dla żądań wykonany ponownie po wyjątki są przechwytywane i rejestrowane:</span><span class="sxs-lookup"><span data-stu-id="d2710-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

<span data-ttu-id="d2710-130">Szablon aplikacji stron Razor zawiera stronę błędu (*.cshtml*) i <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> klasy (`ErrorModel`) w folderze strony.</span><span class="sxs-lookup"><span data-stu-id="d2710-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="d2710-131">W aplikacji MVC następującą metodę procedury obsługi błędów jest zawarte w szablonie aplikacji MVC i pojawia się na kontrolerze głównej:</span><span class="sxs-lookup"><span data-stu-id="d2710-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="d2710-132">Nie dekoracji metody akcji programu obsługi błędów z atrybutami metody HTTP, takich jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="d2710-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="d2710-133">Jawne zleceń uniemożliwić metody osiągając niektórych żądań.</span><span class="sxs-lookup"><span data-stu-id="d2710-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="d2710-134">Zezwalaj na anonimowy dostęp do metody, aby były nieuwierzytelnionym użytkownikom możliwość odbierania widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="d2710-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="d2710-135">Konfigurowanie stanu strony kodowe</span><span class="sxs-lookup"><span data-stu-id="d2710-135">Configure status code pages</span></span>

<span data-ttu-id="d2710-136">Domyślnie aplikacji ASP.NET Core nie zapewnia stan stronę kodową dla kodów stanu HTTP, takich jak *404 — Nie można odnaleźć*.</span><span class="sxs-lookup"><span data-stu-id="d2710-136">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="d2710-137">Aplikacja zwraca kod stanu i pusta treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2710-137">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="d2710-138">Aby zapewnić stan stron kodowych, użyj oprogramowanie pośredniczące strony kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="d2710-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="d2710-139">Oprogramowanie pośredniczące jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d2710-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d2710-140">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="d2710-140">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="d2710-141">Wywołaj <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> metoda przed żądaniem obsługi oprogramowania pośredniczącego (na przykład oprogramowanie pośredniczące plików statycznych i oprogramowanie pośredniczące MVC).</span><span class="sxs-lookup"><span data-stu-id="d2710-141">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="d2710-142">Domyślnie, oprogramowanie pośredniczące strony kod stanu dodaje tekstowy programy obsługi dla typowych kodów stanu, takie jak *404 — Nie można odnaleźć*:</span><span class="sxs-lookup"><span data-stu-id="d2710-142">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="d2710-143">Oprogramowanie pośredniczące obsługuje kilka metod rozszerzenia, które pozwalają dostosować jego zachowanie.</span><span class="sxs-lookup"><span data-stu-id="d2710-143">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="d2710-144">Przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> pobiera Wyrażenie lambda, które służy do przetwarzania logiki niestandardowej obsługi błędów i ręcznie napisać odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="d2710-144">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="d2710-145">Przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przyjmuje zawartości typu i formatu ciągu, który służy do dostosowywania zawartości tekstu typu i odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="d2710-145">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="d2710-146">Przekierowanie, a następnie wykonaj ponownie metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="d2710-146">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="d2710-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="d2710-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="d2710-148">Wysyła *302 — znaleziono* kod stanu do klienta.</span><span class="sxs-lookup"><span data-stu-id="d2710-148">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="d2710-149">Przekierowuje klienta do lokalizacji, udostępnionego w szablonie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="d2710-149">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="d2710-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> często używane podczas aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d2710-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="d2710-151">Należy przekierowuje klienta do innego punktu końcowego, zazwyczaj w przypadkach, gdzie przetwarza błąd przez inną aplikację.</span><span class="sxs-lookup"><span data-stu-id="d2710-151">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="d2710-152">W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla przekierowanego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d2710-152">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="d2710-153">Nie należy zachować i zwróć oryginalny kod stanu odpowiedzi przekierowania początkowej.</span><span class="sxs-lookup"><span data-stu-id="d2710-153">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="d2710-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="d2710-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="d2710-155">Zwraca oryginalny kod stanu do klienta.</span><span class="sxs-lookup"><span data-stu-id="d2710-155">Returns the original status code to the client.</span></span>
* <span data-ttu-id="d2710-156">Generuje treści odpowiedzi przy ponownym wykonaniem Potok żądań przy użyciu ścieżki alternatywnej.</span><span class="sxs-lookup"><span data-stu-id="d2710-156">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="d2710-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> jest często używana aplikacja powinna:</span><span class="sxs-lookup"><span data-stu-id="d2710-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="d2710-158">Proces żądania bez przekierowania folderu do innego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d2710-158">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="d2710-159">W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla pierwotnie żądanego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d2710-159">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="d2710-160">Zachowanie i powrócić do oryginalnego kodu stanu z odpowiedzią.</span><span class="sxs-lookup"><span data-stu-id="d2710-160">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="d2710-161">Szablony mogą obejmować symbol zastępczy (`{0}`) dla kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="d2710-161">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="d2710-162">Szablon musi rozpoczynać się od ukośnika (`/`).</span><span class="sxs-lookup"><span data-stu-id="d2710-162">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="d2710-163">Korzystając z symbolem zastępczym, upewnij się, że punkt końcowy (strona lub kontroler) może przetwarzać segmentu ścieżki.</span><span class="sxs-lookup"><span data-stu-id="d2710-163">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="d2710-164">Na przykład strona Razor dla błędów należy zaakceptować wartość segmentu ścieżki opcjonalnie za pomocą `@page` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="d2710-164">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="d2710-165">Strony kodowe stanu można wyłączyć dla określonych żądań w metodzie obsługi stron Razor lub kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="d2710-165">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="d2710-166">Aby wyłączyć stron kodowych stanu, próba pobrania <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> w żądaniu [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) kolekcji i wyłączyć tę funkcję, jeśli jest dostępna:</span><span class="sxs-lookup"><span data-stu-id="d2710-166">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="d2710-167">Aby użyć <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przeciążenia, że wskazuje punkt końcowy w ramach aplikacji, Utwórz widoku MVC lub strona Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="d2710-167">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="d2710-168">Na przykład szablon aplikacji stron Razor tworzy następującą stronę i klasy modelu strony:</span><span class="sxs-lookup"><span data-stu-id="d2710-168">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="d2710-169">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d2710-169">*Error.cshtml*:</span></span>

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

<span data-ttu-id="d2710-170">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2710-170">*Error.cshtml.cs*:</span></span>

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="d2710-171">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d2710-171">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a><span data-ttu-id="d2710-172">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="d2710-172">Exception-handling code</span></span>

<span data-ttu-id="d2710-173">Kod obsługi stron wyjątków może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="d2710-173">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="d2710-174">Często jest dobrym rozwiązaniem dla stron błędów produkcyjnych, które ma zawierać zawartość statyczną wyłącznie.</span><span class="sxs-lookup"><span data-stu-id="d2710-174">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="d2710-175">Ponadto należy pamiętać, że wysłany nagłówki odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="d2710-175">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="d2710-176">Aplikacja nie można zmienić kod stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2710-176">The app can't change the response's status code.</span></span>
* <span data-ttu-id="d2710-177">Nie można uruchomić wszystkie strony wyjątków lub programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="d2710-177">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="d2710-178">Odpowiedzi muszą być wypełnione albo połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="d2710-178">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="d2710-179">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="d2710-179">Server exception handling</span></span>

<span data-ttu-id="d2710-180">Oprócz logiki aplikacji, obsługi wyjątków [implementacji serwera](xref:fundamentals/servers/index) może obsługiwać niektóre wyjątki.</span><span class="sxs-lookup"><span data-stu-id="d2710-180">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="d2710-181">Jeśli serwer wyłapuje wyjątek, przed wysłaniem nagłówki odpowiedzi, serwer wysyła *500 — Wewnętrzny błąd serwera* odpowiedzi bez treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d2710-181">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="d2710-182">Jeśli serwer wyłapuje wyjątek po wysłaniu nagłówki odpowiedzi, serwer zamyka połączenie.</span><span class="sxs-lookup"><span data-stu-id="d2710-182">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="d2710-183">Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="d2710-183">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="d2710-184">Każdy wyjątek, który występuje odbywa się przez wyjątek serwera obsługi.</span><span class="sxs-lookup"><span data-stu-id="d2710-184">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="d2710-185">Oprogramowanie pośredniczące obsługi wyjątków lub filtry nie wpływają na to zachowanie lub dowolne skonfigurowane strony błędów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="d2710-185">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="d2710-186">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="d2710-186">Startup exception handling</span></span>

<span data-ttu-id="d2710-187">Tylko warstwa hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d2710-187">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="d2710-188">Za pomocą [hosta sieci Web](xref:fundamentals/host/web-host), możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](xref:fundamentals/host/web-host#detailed-errors) z `captureStartupErrors` i `detailedErrors` kluczy.</span><span class="sxs-lookup"><span data-stu-id="d2710-188">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="d2710-189">Jeśli wystąpi błąd, po adresem/port hosta powiązania hostingu można wyświetlić tylko stronę błędu dla błędów uruchamiania przechwycone.</span><span class="sxs-lookup"><span data-stu-id="d2710-189">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="d2710-190">Jeśli wszystkie powiązania nie powiedzie się z jakiegokolwiek powodu:</span><span class="sxs-lookup"><span data-stu-id="d2710-190">If any binding fails for any reason:</span></span>

* <span data-ttu-id="d2710-191">Warstwa hostingu rejestruje wyjątek krytyczny.</span><span class="sxs-lookup"><span data-stu-id="d2710-191">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="d2710-192">Proces dotnet kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="d2710-192">The dotnet process crashes.</span></span>
* <span data-ttu-id="d2710-193">Strona błędu, nie jest wyświetlane, gdy aplikacja jest uruchomiona na [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="d2710-193">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="d2710-194">Podczas uruchamiania na [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - niepowodzenia procesu* jest zwracany przez [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) Jeśli nie można uruchomić procesu .</span><span class="sxs-lookup"><span data-stu-id="d2710-194">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="d2710-195">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d2710-195">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="d2710-196">Aby uzyskać informacje na temat rozwiązywania problemów z uruchamianiem przy użyciu usługi Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d2710-196">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="d2710-197">Obsługa błędów programu ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="d2710-197">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="d2710-198">[MVC](xref:mvc/overview) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="d2710-198">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="d2710-199">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="d2710-199">Exception filters</span></span>

<span data-ttu-id="d2710-200">Filtry wyjątków można skonfigurować globalnie lub na zasadzie na kontroler lub akcję w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="d2710-200">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="d2710-201">Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr.</span><span class="sxs-lookup"><span data-stu-id="d2710-201">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="d2710-202">Te filtry nie są wywoływane w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="d2710-202">These filters aren't called otherwise.</span></span> <span data-ttu-id="d2710-203">Aby dowiedzieć się więcej, zobacz <xref:mvc/controllers/filters>.</span><span class="sxs-lookup"><span data-stu-id="d2710-203">To learn more, see <xref:mvc/controllers/filters>.</span></span>

> [!TIP]
> <span data-ttu-id="d2710-204">Filtry wyjątków są przydatne zalewania wyjątków występujących w ramach akcji MVC, ale nie jest tak elastyczna jak błąd obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d2710-204">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="d2710-205">Firma Microsoft zaleca korzystanie z oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="d2710-205">We recommend the use of middleware.</span></span> <span data-ttu-id="d2710-206">Użyj filtrów, tylko gdy potrzebujesz, aby wykonać obsługi błędów *inaczej* oparte na akcję MVC, która jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="d2710-206">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="d2710-207">Obsługiwać błędy stanu modelu</span><span class="sxs-lookup"><span data-stu-id="d2710-207">Handle model state errors</span></span>

<span data-ttu-id="d2710-208">[Walidacja modelu](xref:mvc/models/validation) występuje przed wywołaniem akcji każdego kontrolera i odpowiada metoda akcji sprawdzić [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) i odpowiednio reagują.</span><span class="sxs-lookup"><span data-stu-id="d2710-208">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="d2710-209">Niektóre aplikacje chce wykonać standardowej konwencji radzenia sobie z [Walidacja modelu](xref:mvc/models/validation) błędów, w którym to przypadku [filtru](xref:mvc/controllers/filters) może być w odpowiednim miejscu, aby zaimplementować takie zasady.</span><span class="sxs-lookup"><span data-stu-id="d2710-209">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="d2710-210">Należy sprawdzić, jak Twoje działania zachowują się ze Stanami nieprawidłowy model.</span><span class="sxs-lookup"><span data-stu-id="d2710-210">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="d2710-211">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/testing>.</span><span class="sxs-lookup"><span data-stu-id="d2710-211">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d2710-212">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d2710-212">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
