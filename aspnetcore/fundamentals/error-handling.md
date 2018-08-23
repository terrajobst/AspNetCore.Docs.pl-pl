---
title: Obsługa błędów w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: ff04ebeb6a682ec924afe896fd6716010a63f7cd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753641"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="c5fdd-103">Obsługa błędów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5fdd-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="c5fdd-104">Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="c5fdd-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="c5fdd-105">W tym artykule opisano typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="c5fdd-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c5fdd-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="c5fdd-107">Na stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="c5fdd-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c5fdd-108">Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="c5fdd-109">Strona udostępnione przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c5fdd-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c5fdd-110">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c5fdd-111">Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="c5fdd-112">Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="c5fdd-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="c5fdd-113">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c5fdd-114">Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="c5fdd-115">Strona jest udostępniana przez dodanie odwołania do pakietu dla [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakietu w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="c5fdd-116">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="c5fdd-117">Umieść wywołanie [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) przed dowolnego oprogramowania pośredniczącego, którym chcesz przechwytywać wyjątków, takie jak `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="c5fdd-118">Włącz na stronie wyjątków dla deweloperów **tylko wtedy, gdy aplikacja jest uruchomiona w środowisku programistycznym**.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="c5fdd-119">Nie chcesz publicznie udostępnić szczegółowe informacje o wyjątku, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="c5fdd-120">[Dowiedz się więcej na temat konfigurowania środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c5fdd-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="c5fdd-121">Aby wyświetlić stronę wyjątek dla deweloperów, uruchom przykładową aplikację w środowisku równa `Development`i Dodaj `?throw=true` do podstawowego adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="c5fdd-122">Strona zawiera kilka kart zawierających informacje o wyjątku i żądanie.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="c5fdd-123">Na pierwszej karcie znajdują się ślad stosu:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-123">The first tab includes a stack trace:</span></span>

![Ślad stosu](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="c5fdd-125">Następna karta przedstawia zapytanie parametry ciągu ewentualne:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-125">The next tab shows the query string parameters, if any:</span></span>

![Parametry ciągu zapytania](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="c5fdd-127">Jeśli żądanie ma pliki cookie, są wyświetlane na **plików cookie** kartę. Nagłówki są widoczne na karcie ostatnich:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Nagłówki](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="c5fdd-129">Konfigurowanie niestandardowych wyjątków, Obsługa strony</span><span class="sxs-lookup"><span data-stu-id="c5fdd-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="c5fdd-130">Strona obsługi wyjątków do użycia, gdy aplikacja nie jest uruchomiona konfiguracji `Development` środowiska:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="c5fdd-131">W aplikacji stron Razor [dotnet nowe](/dotnet/core/tools/dotnet-new) stron Razor szablon zawiera stronę błędu i `ErrorModel` stronie klasy modelu w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="c5fdd-132">W aplikacji MVC nie dekoracji metody akcji programu obsługi błędów z atrybutami metody HTTP, takich jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="c5fdd-133">Jawne zleceń uniemożliwić metody osiągając niektórych żądań.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="c5fdd-134">Zezwalaj na anonimowy dostęp do metody, aby były nieuwierzytelnionym użytkownikom możliwość odbierania widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="c5fdd-135">Na przykład, następujące metody obsługi błędów są dostarczane przez [dotnet nowe](/dotnet/core/tools/dotnet-new) szablon MVC i pojawia się na kontrolerze głównej:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="c5fdd-136">Konfigurowanie stanu strony kodowe</span><span class="sxs-lookup"><span data-stu-id="c5fdd-136">Configuring status code pages</span></span>

<span data-ttu-id="c5fdd-137">Domyślnie aplikacji nie zapewnia stronę kodową sformatowanego stanu dla kodów stanu HTTP, takich jak *404 Nie znaleziono*.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="c5fdd-138">Aby zapewnić stan stron kodowych, należy skonfigurować oprogramowanie pośredniczące strony kod stanu przez dodanie wiersza do `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="c5fdd-139">Domyślnie oprogramowanie pośredniczące strony kod stanu dodaje tekstowy programy obsługi dla typowych kodów stanu, takie jak 404:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![strona 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="c5fdd-141">Oprogramowanie pośredniczące obsługuje kilka metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="c5fdd-142">Jedna metoda przyjmuje wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="c5fdd-143">Inna metoda przyjmuje zawartości typu i formatu ciągu:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="c5fdd-144">Istnieją również przekierować, a następnie wykonaj ponownie metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-144">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="c5fdd-145">Metoda przekierowania wysyła *302 Found* kod stanu dla klienta i przekierowuje klienta do szablonu adresu URL podanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-145">The redirect method sends a *302 Found* status code to the client and redirects the client to the provided location URL template.</span></span> <span data-ttu-id="c5fdd-146">Szablon może obejmować `{0}` symbol zastępczy dla kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-146">The template may include a `{0}` placeholder for the status code.</span></span> <span data-ttu-id="c5fdd-147">Adresy URL, począwszy od `~` ma ścieżki podstawowej, dołączony.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-147">URLs starting with `~` have the base path prepended.</span></span> <span data-ttu-id="c5fdd-148">Adres URL, który nie zaczyna się od `~` jest używany jako jest.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-148">A URL that doesn't start with `~` is used as is.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="c5fdd-149">Wykonaj ponownie metoda zwraca oryginalny kod stanu do klienta i określa, czy treść odpowiedzi powinny być generowane ponownie, wykonując Potok żądań przy użyciu ścieżki alternatywnej.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-149">The re-execute method returns the original status code to the client and specifies that the response body should be generated by re-executing the request pipeline using an alternate path.</span></span> <span data-ttu-id="c5fdd-150">Ta ścieżka może zawierać `{0}` symbol zastępczy kod stanu:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-150">This path may contain a `{0}` placeholder for the status code:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="c5fdd-151">Strony kodowe stanu można wyłączyć dla określonych żądań w metodzie obsługi stron Razor lub kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-151">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="c5fdd-152">Aby wyłączyć stron kodowych stanu, próba pobrania [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) w żądaniu [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) kolekcji i wyłączyć tę funkcję, jeśli jest dostępna:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-152">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="c5fdd-153">Jeśli przy użyciu `UseStatusCodePages*` przeciążenia, że wskazuje punkt końcowy w ramach aplikacji, Utwórz widoku MVC lub strona Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-153">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="c5fdd-154">Na przykład [dotnet nowe](/dotnet/core/tools/dotnet-new) szablonu dla aplikacji stron Razor tworzy następującą stronę i klasy modelu strony:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-154">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="c5fdd-155">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-155">*Error.cshtml*:</span></span>

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

<span data-ttu-id="c5fdd-156">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c5fdd-156">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="c5fdd-157">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="c5fdd-157">Exception-handling code</span></span>

<span data-ttu-id="c5fdd-158">Kod obsługi stron wyjątków może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-158">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="c5fdd-159">Często jest dobrym rozwiązaniem dla stron błędów produkcyjnych, które ma zawierać zawartość statyczną wyłącznie.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-159">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="c5fdd-160">Ponadto należy pamiętać, że nie można zmienić kod stanu odpowiedzi po wysłaniu nagłówki odpowiedzi, ponieważ wszystkie strony wyjątków lub obsługi uruchomić.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-160">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="c5fdd-161">Odpowiedzi muszą być wypełnione albo połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-161">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="c5fdd-162">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="c5fdd-162">Server exception handling</span></span>

<span data-ttu-id="c5fdd-163">Oprócz logiki aplikacji, obsługi wyjątków [serwera](xref:fundamentals/servers/index) hostującego twoją aplikację wykonuje niektóre obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-163">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="c5fdd-164">Jeśli serwer wyłapuje wyjątek, zanim nagłówki są wysyłane, serwer wysyła *500 Wewnętrzny błąd serwera* odpowiedzi z bez treści.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-164">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="c5fdd-165">Jeśli serwer wyłapuje wyjątek po wysłaniu nagłówków, serwer zamyka połączenie.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-165">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="c5fdd-166">Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-166">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="c5fdd-167">Każdy wyjątek, który występuje odbywa się przez wyjątek serwera obsługi.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-167">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="c5fdd-168">Oprogramowanie pośredniczące obsługi wyjątków lub filtry nie wpływają na to zachowanie lub dowolne skonfigurowane strony błędów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-168">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="c5fdd-169">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c5fdd-169">Startup exception handling</span></span>

<span data-ttu-id="c5fdd-170">Tylko warstwa hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-170">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="c5fdd-171">Za pomocą [hosta sieci Web](xref:fundamentals/host/web-host), możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](xref:fundamentals/host/web-host#detailed-errors) z `captureStartupErrors` i `detailedErrors` kluczy.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-171">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="c5fdd-172">Jeśli wystąpi błąd, po adresem/port hosta powiązania hostingu można wyświetlić tylko stronę błędu dla błędów uruchamiania przechwycone.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-172">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="c5fdd-173">Jeśli wszystkie powiązania nie powiedzie się z jakiegokolwiek powodu, hostingu warstwy rejestruje wyjątek krytyczny dotnet awarii procesów, i stronę błędu, nie jest wyświetlane, gdy aplikacja jest uruchomiona na [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-173">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="c5fdd-174">Podczas uruchamiania na [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 niepowodzenia procesu* jest zwracany przez [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) , jeśli proces nie może być pracę.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-174">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="c5fdd-175">Aby uzyskać informacje na temat rozwiązywania problemów problemy z uruchamianiem w przypadku hostowania za pomocą programu IIS, zobacz <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-175">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="c5fdd-176">Aby uzyskać informacje na temat rozwiązywania problemów z uruchamianiem przy użyciu usługi Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-176">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="c5fdd-177">Obsługa błędów programu ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c5fdd-177">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="c5fdd-178">[MVC](xref:mvc/overview) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-178">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="c5fdd-179">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="c5fdd-179">Exception filters</span></span>

<span data-ttu-id="c5fdd-180">Filtry wyjątków można skonfigurować globalnie lub na zasadzie na kontroler lub akcję w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-180">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="c5fdd-181">Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-181">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="c5fdd-182">Te filtry nie są wywoływane w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-182">These filters aren't called otherwise.</span></span> <span data-ttu-id="c5fdd-183">Aby dowiedzieć się więcej, zobacz [filtry](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="c5fdd-183">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="c5fdd-184">Filtry wyjątków są dobre zalewania wyjątków występujących w ramach akcji MVC, ale nie jest tak elastyczna jak błąd obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-184">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="c5fdd-185">Zazwyczaj preferują użycie oprogramowania pośredniczącego i używania filtrów, tylko gdy potrzebujesz do obsługi błędów *inaczej* oparte na akcję MVC, która jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-185">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="c5fdd-186">Obsługa błędy stanu modelu</span><span class="sxs-lookup"><span data-stu-id="c5fdd-186">Handling model state errors</span></span>

<span data-ttu-id="c5fdd-187">[Walidacja modelu](xref:mvc/models/validation) występuje przed wywołaniem akcji każdego kontrolera i odpowiada metoda akcji sprawdzić `ModelState.IsValid` i odpowiednio reagują.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-187">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="c5fdd-188">Niektóre aplikacje chce wykonać standardowej Konwencji za zajmowanie się błędy sprawdzania poprawności modelu, w którym to przypadku [filtru](xref:mvc/controllers/filters) może być w odpowiednim miejscu, aby zaimplementować takie zasady.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-188">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="c5fdd-189">Należy sprawdzić, jak Twoje działania zachowują się ze Stanami nieprawidłowy model.</span><span class="sxs-lookup"><span data-stu-id="c5fdd-189">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="c5fdd-190">Dowiedz się więcej w [logikę kontrolera testu](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="c5fdd-190">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c5fdd-191">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c5fdd-191">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
