---
title: Obsługa błędów w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: 126a782bfd32f9ecd0596045218371ef5ccc82f2
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894143"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="aece7-103">Obsługa błędów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aece7-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="aece7-104">Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="aece7-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="aece7-105">W tym artykule opisano typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aece7-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="aece7-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aece7-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="aece7-107">Na stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="aece7-107">The developer exception page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aece7-108">Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*.</span><span class="sxs-lookup"><span data-stu-id="aece7-108">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="aece7-109">Strona udostępnione przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="aece7-109">The page made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="aece7-110">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="aece7-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aece7-111">Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*.</span><span class="sxs-lookup"><span data-stu-id="aece7-111">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="aece7-112">Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="aece7-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="aece7-113">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="aece7-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aece7-114">Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*.</span><span class="sxs-lookup"><span data-stu-id="aece7-114">To configure an app to display a page that shows detailed information about exceptions, use the *developer exception page*.</span></span> <span data-ttu-id="aece7-115">Strona jest udostępniana przez dodanie odwołania do pakietu dla [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakietu w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="aece7-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="aece7-116">Dodaj wiersz w celu `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="aece7-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="aece7-117">Umieść wywołanie [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) przed dowolnego oprogramowania pośredniczącego, którym chcesz przechwytywać wyjątków, takie jak `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="aece7-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="aece7-118">Włącz na stronie wyjątków dla deweloperów **tylko wtedy, gdy aplikacja jest uruchomiona w środowisku programistycznym**.</span><span class="sxs-lookup"><span data-stu-id="aece7-118">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="aece7-119">Nie chcesz publicznie udostępnić szczegółowe informacje o wyjątku, gdy aplikacja jest uruchamiana w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="aece7-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="aece7-120">[Dowiedz się więcej na temat konfigurowania środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="aece7-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="aece7-121">Aby wyświetlić stronę wyjątek dla deweloperów, uruchom przykładową aplikację w środowisku równa `Development`i Dodaj `?throw=true` do podstawowego adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aece7-121">To see the developer exception page, run the sample app with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="aece7-122">Strona zawiera kilka kart zawierających informacje o wyjątku i żądanie.</span><span class="sxs-lookup"><span data-stu-id="aece7-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="aece7-123">Na pierwszej karcie znajdują się ślad stosu:</span><span class="sxs-lookup"><span data-stu-id="aece7-123">The first tab includes a stack trace:</span></span>

![Ślad stosu](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="aece7-125">Następna karta przedstawia zapytanie parametry ciągu ewentualne:</span><span class="sxs-lookup"><span data-stu-id="aece7-125">The next tab shows the query string parameters, if any:</span></span>

![Parametry ciągu zapytania](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="aece7-127">Jeśli żądanie ma pliki cookie, są wyświetlane na **plików cookie** kartę. Nagłówki są widoczne na karcie ostatnich:</span><span class="sxs-lookup"><span data-stu-id="aece7-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Nagłówki](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="aece7-129">Konfigurowanie niestandardowych wyjątków, Obsługa strony</span><span class="sxs-lookup"><span data-stu-id="aece7-129">Configuring a custom exception handling page</span></span>

<span data-ttu-id="aece7-130">Strona obsługi wyjątków do użycia, gdy aplikacja nie jest uruchomiona konfiguracji `Development` środowiska:</span><span class="sxs-lookup"><span data-stu-id="aece7-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="aece7-131">W aplikacji stron Razor [dotnet nowe](/dotnet/core/tools/dotnet-new) stron Razor szablon zawiera stronę błędu i `ErrorModel` stronie klasy modelu w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="aece7-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="aece7-132">W aplikacji MVC nie dekoracji metody akcji programu obsługi błędów z atrybutami metody HTTP, takich jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="aece7-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="aece7-133">Jawne zleceń uniemożliwić metody osiągając niektórych żądań.</span><span class="sxs-lookup"><span data-stu-id="aece7-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="aece7-134">Zezwalaj na anonimowy dostęp do metody, aby były nieuwierzytelnionym użytkownikom możliwość odbierania widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="aece7-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="aece7-135">Na przykład, następujące metody obsługi błędów są dostarczane przez [dotnet nowe](/dotnet/core/tools/dotnet-new) szablon MVC i pojawia się na kontrolerze głównej:</span><span class="sxs-lookup"><span data-stu-id="aece7-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="aece7-136">Konfigurowanie stanu strony kodowe</span><span class="sxs-lookup"><span data-stu-id="aece7-136">Configuring status code pages</span></span>

<span data-ttu-id="aece7-137">Domyślnie aplikacji nie zapewnia stronę kodową sformatowanego stanu dla kodów stanu HTTP, takich jak *404 Nie znaleziono*.</span><span class="sxs-lookup"><span data-stu-id="aece7-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="aece7-138">Aby zapewnić stan stron kodowych, należy skonfigurować oprogramowanie pośredniczące strony kod stanu przez dodanie wiersza do `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="aece7-138">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="aece7-139">Domyślnie oprogramowanie pośredniczące strony kod stanu dodaje tekstowy programy obsługi dla typowych kodów stanu, takie jak 404:</span><span class="sxs-lookup"><span data-stu-id="aece7-139">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![strona 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="aece7-141">Oprogramowanie pośredniczące obsługuje kilka metod rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="aece7-141">The middleware supports several extension methods.</span></span> <span data-ttu-id="aece7-142">Jedna metoda przyjmuje wyrażenia lambda:</span><span class="sxs-lookup"><span data-stu-id="aece7-142">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="aece7-143">Inna metoda przyjmuje zawartości typu i formatu ciągu:</span><span class="sxs-lookup"><span data-stu-id="aece7-143">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="aece7-144">Istnieje również przekierować, a następnie wykonaj ponownie metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="aece7-144">There's also redirect and re-execute extension methods.</span></span> <span data-ttu-id="aece7-145">Metoda przekierowania wysyła *302 Found* kod stanu do klienta:</span><span class="sxs-lookup"><span data-stu-id="aece7-145">The redirect method sends a *302 Found* status code to the client:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="aece7-146">Wykonaj ponownie metoda zwraca oryginalny kod stanu do klienta, ale wykonuje również obsługę dla adresu URL przekierowania:</span><span class="sxs-lookup"><span data-stu-id="aece7-146">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="aece7-147">Strony kodowe stanu można wyłączyć dla określonych żądań w metodzie obsługi stron Razor lub kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="aece7-147">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="aece7-148">Aby wyłączyć stron kodowych stanu, próba pobrania [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) w żądaniu [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) kolekcji i wyłączyć tę funkcję, jeśli jest dostępna:</span><span class="sxs-lookup"><span data-stu-id="aece7-148">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="aece7-149">Jeśli przy użyciu `UseStatusCodePages*` przeciążenia, że wskazuje punkt końcowy w ramach aplikacji, Utwórz widoku MVC lub strona Razor dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="aece7-149">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="aece7-150">Na przykład [dotnet nowe](/dotnet/core/tools/dotnet-new) szablonu dla aplikacji stron Razor tworzy następującą stronę i klasy modelu strony:</span><span class="sxs-lookup"><span data-stu-id="aece7-150">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="aece7-151">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aece7-151">*Error.cshtml*:</span></span>

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

<span data-ttu-id="aece7-152">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="aece7-152">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="aece7-153">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="aece7-153">Exception-handling code</span></span>

<span data-ttu-id="aece7-154">Kod obsługi stron wyjątków może zgłaszać wyjątki.</span><span class="sxs-lookup"><span data-stu-id="aece7-154">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="aece7-155">Często jest dobrym rozwiązaniem dla stron błędów produkcyjnych, które ma zawierać zawartość statyczną wyłącznie.</span><span class="sxs-lookup"><span data-stu-id="aece7-155">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="aece7-156">Ponadto należy pamiętać, że nie można zmienić kod stanu odpowiedzi po wysłaniu nagłówki odpowiedzi, ponieważ wszystkie strony wyjątków lub obsługi uruchomić.</span><span class="sxs-lookup"><span data-stu-id="aece7-156">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="aece7-157">Odpowiedzi muszą być wypełnione albo połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="aece7-157">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="aece7-158">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="aece7-158">Server exception handling</span></span>

<span data-ttu-id="aece7-159">Oprócz logiki aplikacji, obsługi wyjątków [serwera](xref:fundamentals/servers/index) hostującego twoją aplikację wykonuje niektóre obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="aece7-159">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="aece7-160">Jeśli serwer wyłapuje wyjątek, zanim nagłówki są wysyłane, serwer wysyła *500 Wewnętrzny błąd serwera* odpowiedzi z bez treści.</span><span class="sxs-lookup"><span data-stu-id="aece7-160">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="aece7-161">Jeśli serwer wyłapuje wyjątek po wysłaniu nagłówków, serwer zamyka połączenie.</span><span class="sxs-lookup"><span data-stu-id="aece7-161">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="aece7-162">Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="aece7-162">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="aece7-163">Każdy wyjątek, który występuje odbywa się przez wyjątek serwera obsługi.</span><span class="sxs-lookup"><span data-stu-id="aece7-163">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="aece7-164">Oprogramowanie pośredniczące obsługi wyjątków lub filtry nie wpływają na to zachowanie lub dowolne skonfigurowane strony błędów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="aece7-164">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="aece7-165">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="aece7-165">Startup exception handling</span></span>

<span data-ttu-id="aece7-166">Tylko warstwa hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aece7-166">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="aece7-167">Za pomocą [hosta sieci Web](xref:fundamentals/host/web-host), możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](xref:fundamentals/host/web-host#detailed-errors) z `captureStartupErrors` i `detailedErrors` kluczy.</span><span class="sxs-lookup"><span data-stu-id="aece7-167">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="aece7-168">Jeśli wystąpi błąd, po adresem/port hosta powiązania hostingu można wyświetlić tylko stronę błędu dla błędów uruchamiania przechwycone.</span><span class="sxs-lookup"><span data-stu-id="aece7-168">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="aece7-169">Jeśli wszystkie powiązania nie powiedzie się z jakiegokolwiek powodu, hostingu warstwy rejestruje wyjątek krytyczny dotnet awarii procesów, i stronę błędu, nie jest wyświetlane, gdy aplikacja jest uruchomiona na [Kestrel](xref:fundamentals/servers/kestrel) serwera.</span><span class="sxs-lookup"><span data-stu-id="aece7-169">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="aece7-170">Podczas uruchamiania na [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 niepowodzenia procesu* jest zwracany przez [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) , jeśli proces nie może być pracę.</span><span class="sxs-lookup"><span data-stu-id="aece7-170">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="aece7-171">Postępuj zgodnie z porady dotyczące rozwiązywania problemów w [Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS](xref:host-and-deploy/iis/troubleshoot) tematu.</span><span class="sxs-lookup"><span data-stu-id="aece7-171">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="aece7-172">Obsługa błędów platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="aece7-172">ASP.NET MVC error handling</span></span>

<span data-ttu-id="aece7-173">[MVC](xref:mvc/overview) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="aece7-173">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="aece7-174">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="aece7-174">Exception filters</span></span>

<span data-ttu-id="aece7-175">Filtry wyjątków można skonfigurować globalnie lub na zasadzie na kontroler lub akcję w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="aece7-175">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="aece7-176">Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr.</span><span class="sxs-lookup"><span data-stu-id="aece7-176">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="aece7-177">Te filtry nie są wywoływane w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="aece7-177">These filters aren't called otherwise.</span></span> <span data-ttu-id="aece7-178">Aby dowiedzieć się więcej, zobacz [filtry](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="aece7-178">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="aece7-179">Filtry wyjątków są dobre zalewania wyjątków występujących w ramach akcji MVC, ale nie jest tak elastyczna jak błąd obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="aece7-179">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="aece7-180">Zazwyczaj preferują użycie oprogramowania pośredniczącego i używania filtrów, tylko gdy potrzebujesz do obsługi błędów *inaczej* oparte na akcję MVC, która jest wybierany.</span><span class="sxs-lookup"><span data-stu-id="aece7-180">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="aece7-181">Obsługa błędy stanu modelu</span><span class="sxs-lookup"><span data-stu-id="aece7-181">Handling model state errors</span></span>

<span data-ttu-id="aece7-182">[Walidacja modelu](xref:mvc/models/validation) występuje przed wywołaniem akcji każdego kontrolera i odpowiada metoda akcji sprawdzić `ModelState.IsValid` i odpowiednio reagują.</span><span class="sxs-lookup"><span data-stu-id="aece7-182">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="aece7-183">Niektóre aplikacje chce wykonać standardowej Konwencji za zajmowanie się błędy sprawdzania poprawności modelu, w którym to przypadku [filtru](xref:mvc/controllers/filters) może być w odpowiednim miejscu, aby zaimplementować takie zasady.</span><span class="sxs-lookup"><span data-stu-id="aece7-183">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="aece7-184">Należy sprawdzić, jak Twoje działania zachowują się ze Stanami nieprawidłowy model.</span><span class="sxs-lookup"><span data-stu-id="aece7-184">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="aece7-185">Dowiedz się więcej w [logikę kontrolera testu](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="aece7-185">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aece7-186">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="aece7-186">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/azure-apps/troubleshoot>
