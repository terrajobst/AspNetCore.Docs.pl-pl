---
title: "Obsługa błędów w ASP.NET Core"
author: ardalis
description: "Wykryj sposób obsługi błędów w aplikacji platformy ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49507e90cd659be5da08df17e175297adad0fea1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="fd23d-103">Wprowadzenie do obsługi błędów w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd23d-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="fd23d-104">Przez [Steve Smith](https://ardalis.com/) i [Dykstra niestandardowy](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="fd23d-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="fd23d-105">W tym artykule omówiono typowe appoaches do obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd23d-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="fd23d-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd23d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="fd23d-107">Strona wyjątek dewelopera</span><span class="sxs-lookup"><span data-stu-id="fd23d-107">The developer exception page</span></span>

<span data-ttu-id="fd23d-108">Aby skonfigurować aplikację do wyświetlenia strony, który zawiera szczegółowe informacje dotyczące wyjątków, zainstaluj `Microsoft.AspNetCore.Diagnostics` NuGet pakiet, a następnie dodaj wiersz do [skonfigurować metodę w klasie uruchamiania](startup.md):</span><span class="sxs-lookup"><span data-stu-id="fd23d-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="fd23d-109">Umieść `UseDeveloperExceptionPage` przed wszystkich programów pośredniczących chcesz przechwytywać wyjątki, takich jak `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="fd23d-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="fd23d-110">Włącz stronę wyjątek developer **tylko, gdy aplikacja jest uruchomiona w środowisku programistycznym**.</span><span class="sxs-lookup"><span data-stu-id="fd23d-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="fd23d-111">Nie chcesz udostępniać informacje szczegółowe wyjątek publicznie, po uruchomieniu aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="fd23d-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="fd23d-112">[Dowiedz się więcej o konfigurowaniu środowisk](environments.md).</span><span class="sxs-lookup"><span data-stu-id="fd23d-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="fd23d-113">Aby wyświetlić stronę wyjątek developer, uruchom przykładową aplikację ze środowiskiem ustawioną `Development`i Dodaj `?throw=true` do podstawowego adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd23d-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="fd23d-114">Strona zawiera kilka kart, informacje o wyjątku i żądania.</span><span class="sxs-lookup"><span data-stu-id="fd23d-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="fd23d-115">Karta pierwszy zawiera ślad stosu.</span><span class="sxs-lookup"><span data-stu-id="fd23d-115">The first tab includes a stack trace.</span></span> 

![Ślad stosu](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="fd23d-117">Następna karta zawiera zapytanie parametrów ciągu ewentualne.</span><span class="sxs-lookup"><span data-stu-id="fd23d-117">The next tab shows the query string parameters, if any.</span></span>

![Parametry ciągu zapytania](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="fd23d-119">To żądanie nie ma żadnych plików cookie, ale jeśli jak, będą widoczne w **plików cookie** kartę. Nagłówki, które zostały przekazane na karcie ostatniego jest widoczny.</span><span class="sxs-lookup"><span data-stu-id="fd23d-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Nagłówki](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="fd23d-121">Konfigurowanie niestandardowych wyjątków, Obsługa strony</span><span class="sxs-lookup"><span data-stu-id="fd23d-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="fd23d-122">Jest dobrym rozwiązaniem, aby skonfigurować stronę obsługi wyjątków do użycia, gdy aplikacja nie jest uruchomiona `Development` środowiska.</span><span class="sxs-lookup"><span data-stu-id="fd23d-122">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="fd23d-123">W aplikacji MVC nie jawnie dekoracji metody akcji programu obsługi błędu z atrybutami metody HTTP, takie jak `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="fd23d-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="fd23d-124">Za pomocą jawnego zlecenia może uniemożliwić osiągnięcia metody niektórych żądań.</span><span class="sxs-lookup"><span data-stu-id="fd23d-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="fd23d-125">Konfigurowanie stanu strony kodowe</span><span class="sxs-lookup"><span data-stu-id="fd23d-125">Configuring status code pages</span></span>

<span data-ttu-id="fd23d-126">Domyślnie aplikacja nie zapewnia strona kodowa sformatowanego stan kodów stanu HTTP, takich jak 500 (wewnętrzny błąd serwera) lub 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="fd23d-126">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="fd23d-127">Można skonfigurować `StatusCodePagesMiddleware` przez dodanie wiersza do `Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="fd23d-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="fd23d-128">Domyślnie to oprogramowanie pośredniczące dodaje prosty, tekstowy obsługi wspólnej kodów stanu, na przykład 404:</span><span class="sxs-lookup"><span data-stu-id="fd23d-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![strona 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="fd23d-130">Oprogramowanie pośredniczące obsługuje kilka metod inne rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="fd23d-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="fd23d-131">Wyższy wyrażenia lambda, ma inny ciąg zawartości typu i formatu.</span><span class="sxs-lookup"><span data-stu-id="fd23d-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="fd23d-132">Brak metody rozszerzenia przekierowania.</span><span class="sxs-lookup"><span data-stu-id="fd23d-132">There are also redirect extension methods.</span></span> <span data-ttu-id="fd23d-133">Kod stanu 302 jedną wysyła do klienta, a jeden zwraca oryginalnego kodu stanu do klienta, ale również wykonuje program obsługi dla adresu URL przekierowania.</span><span class="sxs-lookup"><span data-stu-id="fd23d-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="fd23d-134">Jeśli trzeba wyłączyć stan strony kodowe dla niektórych żądań, możesz to zrobić:</span><span class="sxs-lookup"><span data-stu-id="fd23d-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="fd23d-135">Kod obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="fd23d-135">Exception-handling code</span></span>

<span data-ttu-id="fd23d-136">Kod w stronach obsługi wyjątków można zgłaszają wyjątki.</span><span class="sxs-lookup"><span data-stu-id="fd23d-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="fd23d-137">Często jest dobrym rozwiązaniem dla stron błędów produkcji ma zawierać wyłącznie statyczne.</span><span class="sxs-lookup"><span data-stu-id="fd23d-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="fd23d-138">Ponadto należy pamiętać, że po wysłaniu nagłówków odpowiedzi kod stanu odpowiedzi nie można zmienić ani żadnych stron wyjątek lub obsługi uruchomić.</span><span class="sxs-lookup"><span data-stu-id="fd23d-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="fd23d-139">Odpowiedzi muszą być wypełnione lub połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="fd23d-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="fd23d-140">Obsługa wyjątków serwera</span><span class="sxs-lookup"><span data-stu-id="fd23d-140">Server exception handling</span></span>

<span data-ttu-id="fd23d-141">Oprócz obsługi logikę w aplikacji, wyjątków [serwera](servers/index.md) hosting aplikacji wykonuje niektóre obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="fd23d-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="fd23d-142">Jeśli serwer przechwytuje wyjątek przed wysłaniem nagłówki, serwer wysyła 500 Wewnętrzny błąd serwera odpowiedzi z nie treści.</span><span class="sxs-lookup"><span data-stu-id="fd23d-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="fd23d-143">Jeśli serwer przechwytuje wyjątek po wysłaniu nagłówków, serwer zamyka połączenie.</span><span class="sxs-lookup"><span data-stu-id="fd23d-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="fd23d-144">Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer.</span><span class="sxs-lookup"><span data-stu-id="fd23d-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="fd23d-145">Wszystkie wyjątki, która występuje jest obsługiwany przez wyjątek serwera obsługi.</span><span class="sxs-lookup"><span data-stu-id="fd23d-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="fd23d-146">Wszelkie skonfigurowane niestandardowe strony błędów lub oprogramowanie pośredniczące obsługi wyjątków lub filtrów nie mają wpływu na tego zachowania.</span><span class="sxs-lookup"><span data-stu-id="fd23d-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="fd23d-147">Obsługa wyjątków uruchamiania</span><span class="sxs-lookup"><span data-stu-id="fd23d-147">Startup exception handling</span></span>

<span data-ttu-id="fd23d-148">Tylko warstwę hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fd23d-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="fd23d-149">Możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](hosting.md#detailed-errors) przy użyciu `captureStartupErrors` i `detailedErrors` klucza.</span><span class="sxs-lookup"><span data-stu-id="fd23d-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="fd23d-150">Jeśli błąd pojawia się po adres/port hosta powiązanie hosting można wyświetlić tylko stronę błędu dla błędu uruchomienia przechwycony.</span><span class="sxs-lookup"><span data-stu-id="fd23d-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="fd23d-151">Jeśli żadnego powiązania nie powiedzie się z jakiegokolwiek powodu, hostingu warstwy rejestruje wyjątek krytyczny dotnet procesu awarie (Crash), i jest wyświetlana żadna strona błędu.</span><span class="sxs-lookup"><span data-stu-id="fd23d-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="fd23d-152">Obsługa błędów platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fd23d-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="fd23d-153">[MVC](../mvc/index.md) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="fd23d-153">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="fd23d-154">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="fd23d-154">Exception Filters</span></span>

<span data-ttu-id="fd23d-155">Filtry wyjątków można skonfigurować globalnie lub na podstawie-controller lub -action w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="fd23d-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="fd23d-156">Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr, a nie są nazywane inaczej.</span><span class="sxs-lookup"><span data-stu-id="fd23d-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="fd23d-157">Dowiedz się więcej na temat filtrów wyjątków na [filtry](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="fd23d-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="fd23d-158">Filtry wyjątków są dobrym zalewania wyjątków, które występują w ramach działań MVC, ale nie są one tak elastyczne jako błąd obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="fd23d-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="fd23d-159">Preferowane jest oprogramowanie pośredniczące w przypadku ogólnych i za pomocą filtrów, tylko gdy należy wykonywać obsługi błędów *inaczej* oparte na Akcja kontrolera MVC, który został wybrany.</span><span class="sxs-lookup"><span data-stu-id="fd23d-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="fd23d-160">Stan modelu obsługi błędów</span><span class="sxs-lookup"><span data-stu-id="fd23d-160">Handling Model State Errors</span></span>

<span data-ttu-id="fd23d-161">[Sprawdzanie poprawności modelu](../mvc/models/validation.md) występuje przed każdego wywoływana Akcja kontrolera i odpowiada metoda akcji sprawdzić `ModelState.IsValid` i odpowiednio zareagować.</span><span class="sxs-lookup"><span data-stu-id="fd23d-161">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="fd23d-162">Niektóre aplikacje wybierze wykonać standardowej konwencji zajmujących się błędy sprawdzania poprawności modelu, w którym to przypadku [filtru](../mvc/controllers/filters.md) może być odpowiednie miejsce do wdrożenia tych zasad.</span><span class="sxs-lookup"><span data-stu-id="fd23d-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="fd23d-163">Należy przetestować zachowanie akcji stanów nieprawidłowy model.</span><span class="sxs-lookup"><span data-stu-id="fd23d-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="fd23d-164">Dowiedz się więcej w [testowania logiką kontrolera](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="fd23d-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



