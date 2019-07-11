---
title: Formatowanie danych odpowiedzi w interfejsie API sieci Web platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak sformatować dane odpowiedzi w interfejsie API sieci Web platformy ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: b050011aa38743353fb2a7d133abcdca0b8c6d33
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814816"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="d72cc-103">Formatowanie danych odpowiedzi w interfejsie API sieci Web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d72cc-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="d72cc-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d72cc-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d72cc-105">Platforma ASP.NET Core MVC ma wbudowaną funkcję formatowanie danych odpowiedzi, przy użyciu formatów stałej lub w odpowiedzi do klienta specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="d72cc-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="d72cc-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d72cc-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="d72cc-107">Wyniki specyficzne dla formatu akcji</span><span class="sxs-lookup"><span data-stu-id="d72cc-107">Format-Specific Action Results</span></span>

<span data-ttu-id="d72cc-108">Niektóre typy wyników akcji są specyficzne dla danego formatu, takie jak `JsonResult` i `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="d72cc-109">Akcje może zwrócić określone wyniki, które są zawsze formatowane w szczególności sposób.</span><span class="sxs-lookup"><span data-stu-id="d72cc-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="d72cc-110">Na przykład, zwracając `JsonResult` zwróci dane w formacie JSON, niezależnie od preferencji klienta.</span><span class="sxs-lookup"><span data-stu-id="d72cc-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="d72cc-111">Podobnie, zwracając `ContentResult` zwróci dane ciągu w formacie tekstu zwykłego (tak jak będzie po prostu zwracanie ciągu).</span><span class="sxs-lookup"><span data-stu-id="d72cc-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="d72cc-112">Akcja nie jest wymagane do zwrócenia określonego typu; MVC obsługuje dowolną wartość zwracaną obiektu.</span><span class="sxs-lookup"><span data-stu-id="d72cc-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="d72cc-113">Jeśli działanie zwróci `IActionResult` implementacji i kontroler dziedziczy `Controller`, deweloperzy mają wiele metod pomocniczych odpowiadający wiele opcji.</span><span class="sxs-lookup"><span data-stu-id="d72cc-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="d72cc-114">Wyniki z akcji, które zwracają obiekty, które nie są `IActionResult` typy będzie serializowana za pomocą odpowiedniego `IOutputFormatter` implementacji.</span><span class="sxs-lookup"><span data-stu-id="d72cc-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="d72cc-115">Aby zwrócić dane w określonym formacie z kontrolerem, która dziedziczy `Controller` klasy bazowej, należy użyć metody wbudowanej Pomocy `Json` do zwracają kod JSON i `Content` zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="d72cc-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="d72cc-116">Metoda akcji powinna zwrócić typ określony wynik (na przykład `JsonResult`) lub `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="d72cc-117">Zwracanie danych w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="d72cc-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="d72cc-118">Przykładowa odpowiedź z tej akcji:</span><span class="sxs-lookup"><span data-stu-id="d72cc-118">Sample response from this action:</span></span>

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge wyświetlanie typ zawartości odpowiedzi to application/json](formatting/_static/json-response.png)

<span data-ttu-id="d72cc-120">Należy zauważyć, że typ zawartości odpowiedzi `application/json`, jak pokazano na liście żądań sieciowych i w sekcji nagłówków odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="d72cc-121">Należy również zauważyć listę opcji dostępnych w przeglądarce (w tym przypadku Microsoft Edge) w nagłówku Accept, w sekcji nagłówkami żądań.</span><span class="sxs-lookup"><span data-stu-id="d72cc-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="d72cc-122">Bieżącą metodę ignoruje tego pliku nagłówkowego; obeying go omówiono poniżej.</span><span class="sxs-lookup"><span data-stu-id="d72cc-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="d72cc-123">Aby zwrócić dane w formacie zwykłego tekstu, należy użyć `ContentResult` i `Content` pomocy:</span><span class="sxs-lookup"><span data-stu-id="d72cc-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="d72cc-124">Odpowiedź z tej akcji:</span><span class="sxs-lookup"><span data-stu-id="d72cc-124">A response from this action:</span></span>

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge wyświetlanie typ zawartości odpowiedzi to zwykły tekst](formatting/_static/text-response.png)

<span data-ttu-id="d72cc-126">Należy pamiętać, w tym przypadku `Content-Type` zwracany jest `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="d72cc-127">Możesz również uzyskać to takie samo zachowanie za pomocą tylko parametry typu odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="d72cc-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="d72cc-128">Nietrywialnymi akcji z wieloma zwraca typy lub opcje (na przykład różne kody stanu HTTP na podstawie wyniku operacji wykonywanych) tak, `IActionResult` jako typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="d72cc-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="d72cc-129">Negocjowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="d72cc-129">Content Negotiation</span></span>

<span data-ttu-id="d72cc-130">Negocjowanie zawartości (*conneg* w skrócie) występuje, gdy klient określa [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="d72cc-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="d72cc-131">Domyślny format używany przez program ASP.NET Core MVC jest JSON.</span><span class="sxs-lookup"><span data-stu-id="d72cc-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="d72cc-132">Negocjowanie zawartości jest implementowany przez `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="d72cc-133">Również ma wbudowaną kod stanu określonej akcji wyniki zwrócone z metody pomocnicze (wszystkie opartych na `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="d72cc-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="d72cc-134">Może również zwracać typu modelu (klasy została zdefiniowana jako typ transferu danych) i struktura będzie automatycznie zawijany w `ObjectResult` dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="d72cc-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="d72cc-135">Używa następującej metody akcji `Ok` i `NotFound` metody pomocnicze:</span><span class="sxs-lookup"><span data-stu-id="d72cc-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="d72cc-136">Odpowiedź w formacie JSON zostaną zwrócone, chyba że zażądano inny format, a serwer może zwrócić żądany format.</span><span class="sxs-lookup"><span data-stu-id="d72cc-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="d72cc-137">Można użyć narzędzia, takiego jak [Fiddler](https://www.telerik.com/fiddler) utworzyć żądanie zawiera nagłówek Accept, i określ inny format.</span><span class="sxs-lookup"><span data-stu-id="d72cc-137">You can use a tool like [Fiddler](https://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="d72cc-138">W tym przypadku, jeśli serwer ma *program formatujący* , może utworzyć odpowiedź w formacie żądanego, zostanie zwrócony wynik w formacie preferowanego klienta.</span><span class="sxs-lookup"><span data-stu-id="d72cc-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Konsola programu fiddler pokazująca, że utworzone ręcznie pobrać żądania o wartości nagłówka Accept application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="d72cc-140">Na powyższym zrzucie ekranu Fiddler Composer został użyty do wygenerowania żądania, określając `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="d72cc-141">Domyślnie program ASP.NET Core MVC obsługuje tylko JSON, nawet jeśli inny format jest określony, zwracana jest nadal — w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="d72cc-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="d72cc-142">Pokazano, jak dodać dodatkowe programy formatujące w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d72cc-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="d72cc-143">Akcji kontrolera może zwrócić POCOs (zwykłe stare obiekty CLR), w którym to przypadku ASP.NET Core MVC automatycznie tworzy `ObjectResult` dla Ciebie, która otacza obiekt.</span><span class="sxs-lookup"><span data-stu-id="d72cc-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="d72cc-144">Klient otrzyma sformatowane Zserializowany obiekt (JSON format jest ustawieniem domyślnym; można skonfigurować, XML lub innych formatów).</span><span class="sxs-lookup"><span data-stu-id="d72cc-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="d72cc-145">Jeśli jest obiekt zwrócony `null`, a następnie zwróci ramach `204 No Content` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="d72cc-146">Zwraca typ obiektu:</span><span class="sxs-lookup"><span data-stu-id="d72cc-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="d72cc-147">W tym przykładzie żądania dla aliasu autora otrzyma odpowiedź 200 OK danymi autora.</span><span class="sxs-lookup"><span data-stu-id="d72cc-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="d72cc-148">Żądanie dotyczące nieprawidłowy alias otrzyma 204, Brak zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="d72cc-149">Poniżej przedstawiono zrzuty ekranu pokazujące odpowiedzi w formatach XML i JSON.</span><span class="sxs-lookup"><span data-stu-id="d72cc-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="d72cc-150">Proces negocjowania zawartości</span><span class="sxs-lookup"><span data-stu-id="d72cc-150">Content Negotiation Process</span></span>

<span data-ttu-id="d72cc-151">Zawartość *negocjacji* tylko ma miejsce, jeśli `Accept` nagłówek pojawia się w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="d72cc-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="d72cc-152">Jeśli żądanie zawiera nagłówek accept, struktura zostanie wyliczyć typów nośników w nagłówku accept w kolejności preferencji i spróbuje znaleźć element formatujący, który może utworzyć odpowiedź w jednym z formatów określony w nagłówku accept.</span><span class="sxs-lookup"><span data-stu-id="d72cc-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="d72cc-153">W przypadku, gdy element formatujący nie zostanie znaleziony, która może spełnić żądanie klienta, struktura spróbuje znaleźć pierwszy element formatujący, który może utworzyć odpowiedź (chyba że deweloper została skonfigurowana opcja w `MvcOptions` do zwrócenia 406 nie do przyjęcia zamiast).</span><span class="sxs-lookup"><span data-stu-id="d72cc-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="d72cc-154">Jeśli żądanie określa XML, ale nie skonfigurowano program formatujący kod XML, JSON program formatujący będą używane.</span><span class="sxs-lookup"><span data-stu-id="d72cc-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="d72cc-155">Ogólnie rzecz biorąc Jeśli element formatujący nie jest skonfigurowany, które oferują żądany format, a następnie pierwszy element formatujący, który można sformatować obiekt jest używany.</span><span class="sxs-lookup"><span data-stu-id="d72cc-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="d72cc-156">Jeśli podano bez nagłówka, pierwszy element formatujący, który może obsłużyć obiekt, który ma zostać zwrócona będzie służyć do serializacji w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="d72cc-157">W takim przypadku nie ma żadnych miejsce negocjacji — serwer jest określenie jakiego formatu, zostanie użyty.</span><span class="sxs-lookup"><span data-stu-id="d72cc-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="d72cc-158">Jeśli zawiera nagłówek Accept `*/*`, nagłówka zostanie zignorowany, chyba że `RespectBrowserAcceptHeader` jest ustawiona na wartość true na `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="d72cc-159">Przeglądarki i negocjacji zawartości</span><span class="sxs-lookup"><span data-stu-id="d72cc-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="d72cc-160">W odróżnieniu od typowych interfejsów API klienci, przeglądarki sieci web mają tendencję do dostarczania `Accept` nagłówków zawierających szerokiej gamy formatów, łącznie z symbolami wieloznacznymi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="d72cc-161">Domyślnie, gdy struktura wykryje, czy żądanie pochodzi z przeglądarki, jego zignoruje `Accept` nagłówka i zamiast tego zwracany zawartości w aplikacji skonfigurowany przez domyślny format (JSON, chyba że skonfigurowano inaczej).</span><span class="sxs-lookup"><span data-stu-id="d72cc-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="d72cc-162">Zapewnia bardziej spójne środowisko, korzystając z różnych przeglądarek korzystanie z interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="d72cc-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="d72cc-163">Jeśli wolisz nagłówków accept przeglądarce honor aplikacji można skonfigurować to w ramach konfiguracji MVC, ustawiając `RespectBrowserAcceptHeader` do `true` w `ConfigureServices` method in Class metoda *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d72cc-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="d72cc-164">Konfigurowanie elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="d72cc-164">Configuring Formatters</span></span>

<span data-ttu-id="d72cc-165">Jeśli aplikacja wymaga do obsługi dodatkowych formatach poza domyślnym formacie JSON, można dodać pakietów NuGet i skonfigurować MVC do ich obsługi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="d72cc-166">Istnieją osobne elementy formatujące dane wejściowe i wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="d72cc-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="d72cc-167">Elementy formatujące wejściowe są używane przez [powiązań modelu](xref:mvc/models/model-binding); elementy formatujące dane wyjściowe są używane do formatowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="d72cc-168">Można również skonfigurować [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="d72cc-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="d72cc-169">Skonfiguruj na podstawie System.Text.Json elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="d72cc-169">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="d72cc-170">Funkcje dla `System.Text.Json`— oparte na elementy formatujące można skonfigurować za pomocą `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-170">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.SerializerOptions.WriterSettings.Indented = true;
});
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="d72cc-171">Dodanie obsługi formatu JSON na podstawie Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="d72cc-171">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="d72cc-172">Przed platformy ASP.NET Core 3.0 to MVC ustawiana domyślnie przy użyciu elementów formatujących JSON implementowane przy użyciu `Newtonsoft.Json` pakietu.</span><span class="sxs-lookup"><span data-stu-id="d72cc-172">Prior to ASP.NET Core 3.0, MVC defaulted to using JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="d72cc-173">W programie ASP.NET Core 3.0 lub nowszej, elementy formatujące JSON domyślne są oparte na `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-173">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="d72cc-174">Obsługa `Newtonsoft.Json`— funkcje i elementy formatujące zależności jest dostępna po zainstalowaniu [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet pakietu i skonfigurowanie go w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-174">Support for `Newtonsoft.Json`-based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddMvc()
    .AddNewtonsoftJson();
```

<span data-ttu-id="d72cc-175">Niektóre funkcje mogą nie działać poprawnie z `System.Text.Json`— na podstawie elementy formatujące i wymagają odwołania do `Newtonsoft.Json`— na podstawie elementy formatujące dla wersji platformy ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d72cc-175">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters for the ASP.NET Core 3.0 release.</span></span> <span data-ttu-id="d72cc-176">Nadal korzystać z `Newtonsoft.Json`— na podstawie elementów formatujących, jeśli sieci Web platformy ASP.NET Core 3.0 lub nowszej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d72cc-176">Continue using the `Newtonsoft.Json`-based formatters if your ASP.NET Core 3.0 or later app:</span></span>

* <span data-ttu-id="d72cc-177">Używa `Newtonsoft.Json` atrybutów (na przykład `[JsonProperty]` lub `[JsonIgnore]`), dostosowuje ustawienia serializacji lub opiera się na funkcji `Newtonsoft.Json` udostępnia.</span><span class="sxs-lookup"><span data-stu-id="d72cc-177">Uses `Newtonsoft.Json` attributes (for example, `[JsonProperty]` or `[JsonIgnore]`), customizes the serialization settings, or relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="d72cc-178">Konfiguruje `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-178">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="d72cc-179">Przed platformy ASP.NET Core 3.0 to `JsonResult.SerializerSettings` akceptuje wystąpienie `JsonSerializerSettings` specyficznego `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-179">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="d72cc-180">Generuje [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="d72cc-180">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

::: moniker-end

### <a name="add-xml-format-support"></a><span data-ttu-id="d72cc-181">Dodanie obsługi formatu XML</span><span class="sxs-lookup"><span data-stu-id="d72cc-181">Add XML format support</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d72cc-182">Aby dodać kod XML, formatowaniem obsługę platformy ASP.NET Core 2.2 lub wcześniej, należy zainstalować [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="d72cc-182">To add XML formatting support in ASP.NET Core 2.2 or earlier, install the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

::: moniker-end

<span data-ttu-id="d72cc-183">Programy formatujące XML implementowane przy użyciu `System.Xml.Serialization.XmlSerializer` mogą być konfigurowane przez wywołanie <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d72cc-183">XML formatters implemented using `System.Xml.Serialization.XmlSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="d72cc-184">Alternatywnie XML elementy formatujące implementowane przy użyciu `System.Runtime.Serialization.DataContractSerializer` mogą być konfigurowane przez wywołanie <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d72cc-184">Alternatively, XML formatters implemented using `System.Runtime.Serialization.DataContractSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

<span data-ttu-id="d72cc-185">Po dodaniu obsługi formatowania XML metody kontroler powinien zwrócić odpowiedni format na podstawie żądania `Accept` nagłówka, jak Fiddler, ten przykład pokazuje:</span><span class="sxs-lookup"><span data-stu-id="d72cc-185">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Konsola programu fiddler: Nieprzetworzone kartę dla żądania pokazuje wartość nagłówka Accept jest aplikacja/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="d72cc-188">Widoczne na karcie inspektorzy, która pierwotne Pobierz żądanie zostało wystosowane z `Accept: application/xml` zestaw nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d72cc-188">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="d72cc-189">Przedstawia okienko odpowiedzi `Content-Type: application/xml` nagłówka, a `Author` obiekt został serializowany do XML.</span><span class="sxs-lookup"><span data-stu-id="d72cc-189">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="d72cc-190">Użyj karty Composer (kompozytor), aby zmodyfikować żądanie, aby określić `application/json` w `Accept` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="d72cc-190">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="d72cc-191">Wykonanie żądania i odpowiedzi będą formatowane jako dane JSON:</span><span class="sxs-lookup"><span data-stu-id="d72cc-191">Execute the request, and the response will be formatted as JSON:</span></span>

![Konsola programu fiddler: Nieprzetworzone kartę dla żądania pokazuje wartość nagłówka Accept jest application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="d72cc-194">W tym zrzucie ekranu widać, żądanie ustawia nagłówek `Accept: application/json` i określa odpowiedź, taka sama jak jego `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-194">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="d72cc-195">`Author` Obiektu jest wyświetlany w treści odpowiedzi w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="d72cc-195">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="d72cc-196">Wymuszanie określonym formacie</span><span class="sxs-lookup"><span data-stu-id="d72cc-196">Forcing a Particular Format</span></span>

<span data-ttu-id="d72cc-197">Jeśli chcesz ograniczyć formaty odpowiedzi dla określonej akcji można, można zastosować `[Produces]` filtru.</span><span class="sxs-lookup"><span data-stu-id="d72cc-197">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="d72cc-198">`[Produces]` Filtr Określa format odpowiedzi dla określonej akcji (lub kontroler).</span><span class="sxs-lookup"><span data-stu-id="d72cc-198">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="d72cc-199">Większość, takich jak [filtry](xref:mvc/controllers/filters), to można zastosować na działania, kontrolera lub zakresie globalnym.</span><span class="sxs-lookup"><span data-stu-id="d72cc-199">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="d72cc-200">`[Produces]` Filtru spowoduje to wymuszenie wszystkich akcji w ramach `AuthorsController` do zwrócenia odpowiedzi w formacie JSON, nawet jeśli inne programy formatujące zostały skonfigurowane na potrzeby aplikacji i klienta, pod warunkiem `Accept` nagłówka żądania różne, dostępne Format.</span><span class="sxs-lookup"><span data-stu-id="d72cc-200">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="d72cc-201">Zobacz [filtry](xref:mvc/controllers/filters) Aby uzyskać więcej informacji, w tym sposobu stosowania filtrów globalnie.</span><span class="sxs-lookup"><span data-stu-id="d72cc-201">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="d72cc-202">Specjalnych przypadków elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="d72cc-202">Special Case Formatters</span></span>

<span data-ttu-id="d72cc-203">Niektóre szczególne przypadki są implementowane za pomocą wbudowanych elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="d72cc-203">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="d72cc-204">Domyślnie `string` zwracane typy, które będą formatowane jako *text/plain* (*text/html* na żądanie za pośrednictwem `Accept` nagłówek).</span><span class="sxs-lookup"><span data-stu-id="d72cc-204">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="d72cc-205">To zachowanie można usunąć przez usunięcie `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-205">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="d72cc-206">Usuń elementy formatujące w `Configure` method in Class metoda *Startup.cs* (pokazana poniżej).</span><span class="sxs-lookup"><span data-stu-id="d72cc-206">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="d72cc-207">Akcje, które mają obiektu modelu zwracać typ zwróci 204 zawartości odpowiedzi podczas zwracania `null`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-207">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="d72cc-208">To zachowanie można usunąć przez usunięcie `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-208">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="d72cc-209">Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d72cc-209">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="d72cc-210">Bez `TextOutputFormatter`, `string` zwraca typy zwracają 406 nie do przyjęcia, na przykład.</span><span class="sxs-lookup"><span data-stu-id="d72cc-210">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="d72cc-211">Należy pamiętać, że jeśli element formatujący XML istnieje, jej sformatuje `string` typy zwracane, jeśli `TextOutputFormatter` zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="d72cc-211">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="d72cc-212">Bez `HttpNoContentOutputFormatter`, obiektów o wartości null są formatowane przy użyciu skonfigurowanego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="d72cc-212">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="d72cc-213">Na przykład program formatujący JSON po prostu zwróci odpowiedź z treści pewnego `null`, podczas gdy program formatujący kod XML zwróci puste XML element z atrybutem `xsi:nil="true"` zestawu.</span><span class="sxs-lookup"><span data-stu-id="d72cc-213">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="d72cc-214">Mapowania adresów URL Format odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d72cc-214">Response Format URL Mappings</span></span>

<span data-ttu-id="d72cc-215">Klienci mogą żądać określonym formacie jako część adresu URL, takich jak w ciągu zapytania lub część ścieżki lub przy użyciu rozszerzenia specyficzne dla formatu plików, takich jak XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="d72cc-215">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="d72cc-216">Powinny być określone mapowanie ścieżki żądania w trasy, która używa interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d72cc-216">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="d72cc-217">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d72cc-217">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="d72cc-218">Ta trasa pozwoliłoby żądany format można określić jako rozszerzenie pliku opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="d72cc-218">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="d72cc-219">`[FormatFilter]` Atrybut sprawdza istnienie wartości formatu `RouteData` i będzie zmapowana format odpowiedzi do odpowiedniego obiektu formatującego, po utworzeniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d72cc-219">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="d72cc-220">trasy</span><span class="sxs-lookup"><span data-stu-id="d72cc-220">Route</span></span>            |             <span data-ttu-id="d72cc-221">Element formatujący</span><span class="sxs-lookup"><span data-stu-id="d72cc-221">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="d72cc-222">Domyślny element formatujący dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="d72cc-222">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="d72cc-223">Element formatujący JSON (jeśli jest skonfigurowane)</span><span class="sxs-lookup"><span data-stu-id="d72cc-223">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="d72cc-224">Element formatujący XML (jeśli jest skonfigurowane)</span><span class="sxs-lookup"><span data-stu-id="d72cc-224">The XML formatter (if configured)</span></span>  |
