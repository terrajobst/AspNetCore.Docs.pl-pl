---
title: Obsługa błędów w ASP.NET Core
author: ardalis
description: Wykryj sposób obsługi błędów w aplikacji platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273712"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="f1547-103">Obsługa błędów w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1547-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="f1547-104">Przez [Steve Smith](https://ardalis.com/) i [Dykstra niestandardowy](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="f1547-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="f1547-105">W tym artykule omówiono typowe appoaches do obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1547-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="f1547-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f1547-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="f1547-107">Strona wyjątek dewelopera</span><span class="sxs-lookup"><span data-stu-id="f1547-107">The developer exception page</span></span>

<span data-ttu-id="f1547-108">Aby skonfigurować aplikację do wyświetlenia strony, który zawiera szczegółowe informacje dotyczące wyjątków, zainstaluj `Microsoft.AspNetCore.Diagnostics` NuGet pakiet, a następnie dodaj wiersz do [skonfigurować metodę w klasie uruchamiania](xref:fundamentals/startup):</span><span class="sxs-lookup"><span data-stu-id="f1547-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="f1547-109">Umieść `UseDeveloperExceptionPage` przed wszystkich programów pośredniczących chcesz przechwytywać wyjątki, takich jak `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="f1547-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="f1547-110">Włącz stronę wyjątek developer **tylko, gdy aplikacja jest uruchomiona w środowisku programistycznym**.</span><span class="sxs-lookup"><span data-stu-id="f1547-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="f1547-111">Nie chcesz udostępniać informacje szczegółowe wyjątek publicznie, po uruchomieniu aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f1547-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="f1547-112">[Dowiedz się więcej o konfigurowaniu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f1547-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="f1547-113">Aby wyświetlić stronę wyjątek developer, uruchom przykładową aplikację ze środowiskiem ustawioną `Development`i Dodaj `?throw=true` do podstawowego adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1547-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="f1547-114">Strona zawiera kilka kart, informacje o wyjątku i żądania.</span><span class="sxs-lookup"><span data-stu-id="f1547-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="f1547-115">Karta pierwszy zawiera ślad stosu.</span><span class="sxs-lookup"><span data-stu-id="f1547-115">The first tab includes a stack trace.</span></span> 

![Ślad stosu](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="f1547-117">Następna karta zawiera zapytanie parametrów ciągu ewentualne.</span><span class="sxs-lookup"><span data-stu-id="f1547-117">The next tab shows the query string parameters, if any.</span></span>

![Parametry ciągu zapytania](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="f1547-119">To żądanie nie ma żadnych plików cookie, ale jeśli jak, będą widoczne w **plików cookie** kartę. Nagłówki, które zostały przekazane na karcie ostatniego jest widoczny.</span><span class="sxs-lookup"><span data-stu-id="f1547-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Nagłówki](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="f1547-121">Konfigurowanie niestandardowych wyjątków, Obsługa strony</span><span class="sxs-lookup"><span data-stu-id="f1547-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="f1547-122">Konfigurowanie strony obsługi wyjątków do użycia, gdy aplikacja nie jest uruchomiona `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="f1547-122">Configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="f1547-123">W aplikacji stron Razor [dotnet nowe](/dotnet/core/tools/dotnet-new) stron Razor szablon zawiera stronę błędu i `ErrorModel` strony klasy modelu w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="f1547-123">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="f1547-124">W aplikacji MVC nie dekoracji metody akcji programu obsługi błędu z atrybutami metody HTTP, takie jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="f1547-124">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="f1547-125">Jawne zleceń zapobiec osiągnięciu metody niektórych żądań.</span><span class="sxs-lookup"><span data-stu-id="f1547-125">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="f1547-126">Zezwala na dostęp anonimowy do metody, aby mogły otrzymywać widoku błędów nieuwierzytelnionym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="f1547-126">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="f1547-127">Na przykład następujące metody obsługi błędów są dostarczane przez [dotnet nowe](/dotnet/core/tools/dotnet-new) szablonu MVC i pojawia się w kontrolerze głównej:</span><span class="sxs-lookup"><span data-stu-id="f1547-127">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="f1547-128">Konfigurowanie stanu strony kodowe</span><span class="sxs-lookup"><span data-stu-id="f1547-128">Configuring status code pages</span></span>

<span data-ttu-id="f1547-129">Domyślnie aplikacja nie zapewnia strona kodowa sformatowanego stan kodów stanu HTTP, takich jak *404 — Nie znaleziono*.</span><span class="sxs-lookup"><span data-stu-id="f1547-129">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="f1547-130">Aby zapewnić stan stron kodowych, należy skonfigurować oprogramowanie pośredniczące strony kod stanu przez dodanie wiersza do `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="f1547-130">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="f1547-131">Domyślnie oprogramowanie pośredniczące strony kod stanu dodaje prosty, tekstowy obsługi wspólnej kodów stanu, na przykład 404:</span><span class="sxs-lookup"><span data-stu-id="f1547-131">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![strona 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="f1547-133">Oprogramowanie pośredniczące obsługuje kilka metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f1547-133">The middleware supports several extension methods.</span></span> <span data-ttu-id="f1547-134">Jedna metoda przyjmuje wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="f1547-134">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="f1547-135">Inna metoda przyjmuje ciąg zawartości typu i formatu:</span><span class="sxs-lookup"><span data-stu-id="f1547-135">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="f1547-136">Istnieją również przekierować, a następnie wykonaj ponownie metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f1547-136">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="f1547-137">Metoda przekierowania wysyła kod stanu 302 do klienta:</span><span class="sxs-lookup"><span data-stu-id="f1547-137">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="f1547-138">Wykonaj ponownie metoda zwraca oryginalnego kodu stanu do klienta, ale również wykonuje program obsługi dla adresu URL przekierowania:</span><span class="sxs-lookup"><span data-stu-id="f1547-138">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="f1547-139">Strony kodowe stanu można wyłączyć dla określonych żądań w metoda obsługi stron Razor lub kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="f1547-139">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="f1547-140">Aby wyłączyć stron kodowych stanu, próba pobrania [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) w żądaniu [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) kolekcji i wyłączanie funkcji, jeśli jest dostępna:</span><span class="sxs-lookup"><span data-stu-id="f1547-140">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="f1547-141">Jeśli przy użyciu `UseStatusCodePages*` przeciążenia, że wskazuje punkt końcowy w aplikacji, Utwórz MVC widoku lub strony Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="f1547-141">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="f1547-142">Na przykład [dotnet nowe](/dotnet/core/tools/dotnet-new) szablonu aplikacji dla stron Razor tworzy następującą stronę i klasy modelu strony:</span><span class="sxs-lookup"><span data-stu-id="f1547-142">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="f1547-143">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f1547-143">*Error.cshtml*:</span></span>

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
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="f1547-144">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1547-144">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="f1547-145">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="f1547-145">Exception-handling code</span></span>

<span data-ttu-id="f1547-146">Kod w stronach obsługi wyjątków można zgłaszają wyjątki.</span><span class="sxs-lookup"><span data-stu-id="f1547-146">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="f1547-147">Często jest dobrym rozwiązaniem dla stron błędów produkcji ma zawierać wyłącznie statyczne.</span><span class="sxs-lookup"><span data-stu-id="f1547-147">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="f1547-148">Ponadto należy pamiętać, że po wysłaniu nagłówków odpowiedzi kod stanu odpowiedzi nie można zmienić ani żadnych stron wyjątek lub obsługi uruchomić.</span><span class="sxs-lookup"><span data-stu-id="f1547-148">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="f1547-149">Odpowiedzi muszą być wypełnione lub połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="f1547-149">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="f1547-150">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="f1547-150">Server exception handling</span></span>

<span data-ttu-id="f1547-151">Oprócz obsługi logikę w aplikacji, wyjątków [serwera](xref:fundamentals/servers/index) hosting aplikacji wykonuje niektóre obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="f1547-151">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="f1547-152">Jeśli serwer przechwytuje wyjątek przed wysłaniem nagłówki, serwer wysyła *500 Wewnętrzny błąd serwera* odpowiedzi nie jednostki.</span><span class="sxs-lookup"><span data-stu-id="f1547-152">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="f1547-153">Jeśli serwer przechwytuje wyjątek po wysłaniu nagłówków, serwer zamyka połączenie.</span><span class="sxs-lookup"><span data-stu-id="f1547-153">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="f1547-154">Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="f1547-154">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="f1547-155">Wszystkie wyjątki, która występuje jest obsługiwany przez wyjątek serwera obsługi.</span><span class="sxs-lookup"><span data-stu-id="f1547-155">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="f1547-156">Wszelkie skonfigurowane niestandardowe strony błędów lub oprogramowanie pośredniczące obsługi wyjątków lub filtrów nie mają wpływu na tego zachowania.</span><span class="sxs-lookup"><span data-stu-id="f1547-156">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="f1547-157">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="f1547-157">Startup exception handling</span></span>

<span data-ttu-id="f1547-158">Tylko warstwę hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1547-158">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="f1547-159">Przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host), możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](xref:fundamentals/host/web-host#detailed-errors) z `captureStartupErrors` i `detailedErrors` kluczy.</span><span class="sxs-lookup"><span data-stu-id="f1547-159">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="f1547-160">Jeśli błąd pojawia się po adres/port hosta powiązanie hosting można wyświetlić tylko stronę błędu dla błędu uruchomienia przechwycony.</span><span class="sxs-lookup"><span data-stu-id="f1547-160">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="f1547-161">Jeśli żadnego powiązania nie powiedzie się z jakiegokolwiek powodu, hostingu warstwy loguje wyjątek krytyczny dotnet awarie procesów, a żadna strona błędu jest wyświetlane, gdy aplikacja jest uruchomiona [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="f1547-161">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="f1547-162">Podczas uruchamiania [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 awarii procesu* zwróconego przez [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) Jeśli proces nie może być Rozpoczęto.</span><span class="sxs-lookup"><span data-stu-id="f1547-162">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="f1547-163">Wykonaj porady dotyczące rozwiązywania problemów w [Rozwiązywanie problemów z platformy ASP.NET Core w usługach IIS](xref:host-and-deploy/iis/troubleshoot) tematu.</span><span class="sxs-lookup"><span data-stu-id="f1547-163">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="f1547-164">Obsługa błędów platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f1547-164">ASP.NET MVC error handling</span></span>

<span data-ttu-id="f1547-165">[MVC](xref:mvc/overview) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="f1547-165">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="f1547-166">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="f1547-166">Exception Filters</span></span>

<span data-ttu-id="f1547-167">Filtry wyjątków można skonfigurować globalnie lub na podstawie-controller lub -action w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="f1547-167">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="f1547-168">Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr, a nie są nazywane inaczej.</span><span class="sxs-lookup"><span data-stu-id="f1547-168">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="f1547-169">Dowiedz się więcej na temat filtrów wyjątków na [filtry](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="f1547-169">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="f1547-170">Filtry wyjątków są dobrym zalewania wyjątków, które występują w ramach działań MVC, ale nie są one tak elastyczne jako błąd obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="f1547-170">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="f1547-171">Preferowane jest oprogramowanie pośredniczące w przypadku ogólnych i za pomocą filtrów, tylko gdy należy wykonywać obsługi błędów *inaczej* oparte na Akcja kontrolera MVC, który został wybrany.</span><span class="sxs-lookup"><span data-stu-id="f1547-171">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="f1547-172">Stan modelu obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="f1547-172">Handling Model State Errors</span></span>

<span data-ttu-id="f1547-173">[Sprawdzanie poprawności modelu](xref:mvc/models/validation) występuje przed wywołaniem akcji każdego kontrolera i odpowiada metoda akcji sprawdzić `ModelState.IsValid` i odpowiednio zareagować.</span><span class="sxs-lookup"><span data-stu-id="f1547-173">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="f1547-174">Niektóre aplikacje wybierze wykonać standardowej konwencji zajmujących się błędy sprawdzania poprawności modelu, w którym to przypadku [filtru](xref:mvc/controllers/filters) może być odpowiednie miejsce do wdrożenia tych zasad.</span><span class="sxs-lookup"><span data-stu-id="f1547-174">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="f1547-175">Należy przetestować zachowanie akcji stanów nieprawidłowy model.</span><span class="sxs-lookup"><span data-stu-id="f1547-175">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="f1547-176">Dowiedz się więcej w [logikę kontrolera testu](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="f1547-176">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>
