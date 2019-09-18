---
title: Formatowanie danych odpowiedzi w ASP.NET Core Web API
author: ardalis
description: Dowiedz się, jak sformatować dane odpowiedzi w ASP.NET Core Web API.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 8bee4efdae5341ddab5bd3aec278ecfef37f0c08
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082348"
---
<!-- DO NOT EDIT BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12077 MERGES -->
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="8c203-103">Formatowanie danych odpowiedzi w ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="8c203-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="8c203-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8c203-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8c203-105">ASP.NET Core MVC ma wbudowaną obsługę formatowania danych odpowiedzi przy użyciu stałych formatów lub w odpowiedzi na specyfikacje klienta.</span><span class="sxs-lookup"><span data-stu-id="8c203-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="8c203-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c203-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="8c203-107">Wyniki akcji specyficzne dla formatu</span><span class="sxs-lookup"><span data-stu-id="8c203-107">Format-Specific Action Results</span></span>

<span data-ttu-id="8c203-108">Niektóre typy wyników akcji są specyficzne dla określonego formatu, takiego jak `JsonResult` i. `ContentResult`</span><span class="sxs-lookup"><span data-stu-id="8c203-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="8c203-109">Akcje mogą zwracać określone wyniki, które są zawsze sformatowane w określony sposób.</span><span class="sxs-lookup"><span data-stu-id="8c203-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="8c203-110">Na przykład zwrócenie danych w `JsonResult` formacie JSON będzie zwracało, niezależnie od preferencji klienta.</span><span class="sxs-lookup"><span data-stu-id="8c203-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="8c203-111">Analogicznie, zwrócenie `ContentResult` zwraca dane ciągu w formacie zwykłego tekstu (jak po prostu zwraca ciąg).</span><span class="sxs-lookup"><span data-stu-id="8c203-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="8c203-112">Akcja nie jest wymagana do zwrócenia żadnego określonego typu; MVC obsługuje dowolną wartość zwracaną przez obiekt.</span><span class="sxs-lookup"><span data-stu-id="8c203-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="8c203-113">Jeśli akcja zwróci `IActionResult` implementację, a kontroler dziedziczy po `Controller`, deweloperzy mają wiele metod pomocniczych odpowiadających wielu wyborom.</span><span class="sxs-lookup"><span data-stu-id="8c203-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="8c203-114">Wyniki akcji, które zwracają obiekty, które nie `IActionResult` są typami, będą serializowane przy `IOutputFormatter` użyciu odpowiedniej implementacji.</span><span class="sxs-lookup"><span data-stu-id="8c203-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="8c203-115">Aby zwrócić dane w określonym formacie z kontrolera, który dziedziczy z `Controller` klasy podstawowej, użyj wbudowanej metody `Json` pomocnika do zwracania kodu JSON i `Content` dla zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="8c203-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="8c203-116">Metoda działania powinna zwracać określony typ wyniku (na przykład `JsonResult`) lub. `IActionResult`</span><span class="sxs-lookup"><span data-stu-id="8c203-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="8c203-117">Zwracanie danych w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="8c203-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="8c203-118">Przykładowa odpowiedź z tej akcji:</span><span class="sxs-lookup"><span data-stu-id="8c203-118">Sample response from this action:</span></span>

![Karta sieciowa Narzędzia deweloperskie w przeglądarce Microsoft Edge pokazująca typ zawartości odpowiedzi to Application/JSON](formatting/_static/json-response.png)

<span data-ttu-id="8c203-120">Należy pamiętać, że typ zawartości odpowiedzi jest `application/json`pokazywany zarówno na liście żądań sieciowych, jak i w sekcji nagłówki odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8c203-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="8c203-121">Należy również zwrócić uwagę na listę opcji przedstawionych w przeglądarce (w tym przypadku Microsoft Edge) w nagłówku Accept w sekcji nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="8c203-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="8c203-122">Bieżąca technika powoduje ignorowanie tego nagłówka. przestrzeganie tych zagadnień jest omówione poniżej.</span><span class="sxs-lookup"><span data-stu-id="8c203-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="8c203-123">Aby zwrócić dane w formacie zwykłego tekstu `ContentResult` , użyj `Content` i pomocnika:</span><span class="sxs-lookup"><span data-stu-id="8c203-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="8c203-124">Odpowiedź z tej akcji:</span><span class="sxs-lookup"><span data-stu-id="8c203-124">A response from this action:</span></span>

![Karta sieciowa Narzędzia deweloperskie w przeglądarce Microsoft Edge pokazująca typ zawartości odpowiedzi to Text/Plains](formatting/_static/text-response.png)

<span data-ttu-id="8c203-126">Zwróć uwagę, że w `Content-Type` tym przypadku `text/plain`zwracana jest wartość.</span><span class="sxs-lookup"><span data-stu-id="8c203-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="8c203-127">Możesz również osiągnąć takie samo zachowanie, używając tylko typu odpowiedzi ciągu:</span><span class="sxs-lookup"><span data-stu-id="8c203-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="8c203-128">Dla nieuproszczonych akcji z wieloma zwracanymi typami lub opcjami (na przykład innych kodów stanu HTTP w oparciu o wynik wykonanych operacji) Preferuj `IActionResult` jako zwracany typ.</span><span class="sxs-lookup"><span data-stu-id="8c203-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="8c203-129">Negocjowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="8c203-129">Content Negotiation</span></span>

<span data-ttu-id="8c203-130">Negocjowanie zawartości (*conneg* for Short) występuje, gdy klient określi [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="8c203-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="8c203-131">Domyślny format używany przez ASP.NET Core MVC to JSON.</span><span class="sxs-lookup"><span data-stu-id="8c203-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="8c203-132">Negocjowanie zawartości jest implementowane przez `ObjectResult`program.</span><span class="sxs-lookup"><span data-stu-id="8c203-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="8c203-133">Jest ona również wbudowana w wyniki akcji specyficzne dla kodu stanu zwracane z metod pomocnika (wszystkie są oparte na `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="8c203-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="8c203-134">Możesz również zwrócić Typ modelu (klasy zdefiniowanej jako typ transferu danych), a platforma automatycznie zawija ją w `ObjectResult` wybranym przez siebie sposób.</span><span class="sxs-lookup"><span data-stu-id="8c203-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="8c203-135">W poniższej metodzie działania są `Ok` stosowane `NotFound` metody pomocnika i:</span><span class="sxs-lookup"><span data-stu-id="8c203-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="8c203-136">Odpowiedź w formacie JSON zostanie zwrócona, chyba że zażądano innego formatu i serwer może zwrócić żądany format.</span><span class="sxs-lookup"><span data-stu-id="8c203-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="8c203-137">Za pomocą narzędzia, takiego jak [programu Fiddler](https://www.telerik.com/fiddler) , można utworzyć żądanie zawierające nagłówek Accept i określić inny format.</span><span class="sxs-lookup"><span data-stu-id="8c203-137">You can use a tool like [Fiddler](https://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="8c203-138">W takim przypadku, jeśli serwer zawiera program *formatujący* , który może wygenerować odpowiedź w żądanym formacie, wynik zostanie zwrócony w formacie preferowanym przez klienta.</span><span class="sxs-lookup"><span data-stu-id="8c203-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Konsola programu Fiddler pokazująca ręcznie utworzone żądanie GET z wartością nagłówka Accept dla aplikacji/XML](formatting/_static/fiddler-composer.png)

<span data-ttu-id="8c203-140">Na powyższym zrzucie ekranu układacz programu Fiddler został użyty do wygenerowania żądania, określając `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="8c203-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="8c203-141">Domyślnie ASP.NET Core MVC obsługuje tylko kod JSON, więc nawet jeśli określono inny format, zwracany wynik jest nadal sformatowany w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="8c203-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="8c203-142">Zobaczysz, jak dodać dodatkowe elementy formatujące w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="8c203-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="8c203-143">Akcje kontrolera mogą zwracać POCOs (zwykłe stare obiekty CLR), w tym przypadku ASP.NET Core MVC automatycznie tworzy `ObjectResult` dla Ciebie obiekt.</span><span class="sxs-lookup"><span data-stu-id="8c203-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="8c203-144">Klient pobierze sformatowany obiekt serializowany (format JSON jest domyślny; można skonfigurować plik XML lub inne formaty).</span><span class="sxs-lookup"><span data-stu-id="8c203-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="8c203-145">Jeśli zwracany obiekt to `null`, struktura `204 No Content` zwróci odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="8c203-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="8c203-146">Zwracanie typu obiektu:</span><span class="sxs-lookup"><span data-stu-id="8c203-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="8c203-147">W przykładzie żądanie dotyczące prawidłowego aliasu autora otrzyma odpowiedź 200 OK od danych autora.</span><span class="sxs-lookup"><span data-stu-id="8c203-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="8c203-148">Żądanie dotyczące nieprawidłowego aliasu otrzyma odpowiedź 204 braku zawartości.</span><span class="sxs-lookup"><span data-stu-id="8c203-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="8c203-149">Poniżej przedstawiono zrzuty ekranu przedstawiające odpowiedź w formatach XML i JSON.</span><span class="sxs-lookup"><span data-stu-id="8c203-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="8c203-150">Proces negocjacji zawartości</span><span class="sxs-lookup"><span data-stu-id="8c203-150">Content Negotiation Process</span></span>

<span data-ttu-id="8c203-151">*Negocjowanie* zawartości odbywa się tylko wtedy `Accept` , gdy w żądaniu zostanie wyświetlony nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8c203-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="8c203-152">Gdy żądanie zawiera nagłówek Accept, struktura będzie wyliczać typy nośników w nagłówku Accept w kolejności preferencji i spróbuje znaleźć program formatujący, który może wygenerować odpowiedź w jednym z formatów określonych przez nagłówek Accept.</span><span class="sxs-lookup"><span data-stu-id="8c203-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="8c203-153">W przypadku, gdy nie znaleziono programu formatującego, który może spełnić żądanie klienta, platforma podejmie próbę znalezienia pierwszego elementu formatującego, który może wygenerować odpowiedź (chyba że deweloper skonfigurował opcję w `MvcOptions` celu zwrócenia 406).</span><span class="sxs-lookup"><span data-stu-id="8c203-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="8c203-154">Jeśli żądanie określa XML, ale nie skonfigurowano programu formatującego XML, zostanie użyty program formatujący JSON.</span><span class="sxs-lookup"><span data-stu-id="8c203-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="8c203-155">Ogólnie rzecz biorąc, jeśli nie skonfigurowano programu formatującego, który może zapewnić żądany format, zostanie użyty pierwszy program formatujący, który może formatować obiekt.</span><span class="sxs-lookup"><span data-stu-id="8c203-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="8c203-156">Jeśli nagłówek nie zostanie określony, pierwszy program formatujący, który może obsłużyć zwracany obiekt, zostanie użyty do serializacji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8c203-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="8c203-157">W takim przypadku nie ma żadnej negocjacji — serwer określa format, który będzie używany.</span><span class="sxs-lookup"><span data-stu-id="8c203-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="8c203-158">Jeśli nagłówek Accept zawiera `*/*`, nagłówek będzie ignorowany, chyba że `RespectBrowserAcceptHeader` `MvcOptions`jest ustawiona na wartość true.</span><span class="sxs-lookup"><span data-stu-id="8c203-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="8c203-159">Przeglądarki i negocjacje zawartości</span><span class="sxs-lookup"><span data-stu-id="8c203-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="8c203-160">W przeciwieństwie do typowych klientów interfejsu API, przeglądarki sieci `Accept` Web zastępują do dostarczania nagłówków zawierających szeroką gamę formatów, w tym symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="8c203-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="8c203-161">Domyślnie, gdy struktura wykryje, że żądanie pochodzi z przeglądarki, zignoruje `Accept` nagłówek i zamiast tego zwróci zawartość w skonfigurowanym formacie domyślnym (JSON, chyba że zostanie skonfigurowany inaczej).</span><span class="sxs-lookup"><span data-stu-id="8c203-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="8c203-162">Zapewnia to bardziej spójny interfejs, który umożliwia korzystanie z interfejsów API w różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="8c203-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="8c203-163">Jeśli wolisz, aby `RespectBrowserAcceptHeader` `true` aplikacja honorować nagłówki akceptujące w przeglądarce, możesz ją skonfigurować jako część konfiguracji MVC, ustawiając wartość w `ConfigureServices` metodzie w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8c203-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="8c203-164">Konfigurowanie elementów formatujących</span><span class="sxs-lookup"><span data-stu-id="8c203-164">Configuring Formatters</span></span>

<span data-ttu-id="8c203-165">Jeśli aplikacja wymaga obsługi dodatkowych formatów poza wartością domyślną JSON, można dodać pakiety NuGet i skonfigurować MVC do ich obsługi.</span><span class="sxs-lookup"><span data-stu-id="8c203-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="8c203-166">Istnieją osobne elementy formatujące dla danych wejściowych i wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="8c203-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="8c203-167">Wejściowe elementy formatującego są używane przez [powiązanie modelu](xref:mvc/models/model-binding); wyjściowe elementy formatujące są używane do formatowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8c203-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="8c203-168">Możesz również skonfigurować [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="8c203-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="8c203-169">Skonfiguruj elementy formatujące system. Text. JSON w oparciu o</span><span class="sxs-lookup"><span data-stu-id="8c203-169">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="8c203-170">Funkcje dla `System.Text.Json`elementów formatujących opartych na programie można skonfigurować `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`przy użyciu polecenia.</span><span class="sxs-lookup"><span data-stu-id="8c203-170">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="8c203-171">Opcje serializacji danych wyjściowych dla poszczególnych akcji można skonfigurować przy użyciu polecenia `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="8c203-171">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="8c203-172">Przykład:</span><span class="sxs-lookup"><span data-stu-id="8c203-172">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="8c203-173">Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="8c203-173">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="8c203-174">Przed ASP.NET Core 3,0, MVC domyślnie korzysta z elementów formatujących JSON wdrożonych przy użyciu `Newtonsoft.Json` pakietu.</span><span class="sxs-lookup"><span data-stu-id="8c203-174">Prior to ASP.NET Core 3.0, MVC defaulted to using JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="8c203-175">W ASP.NET Core 3,0 lub nowszych domyślne elementy formatujące JSON są oparte na `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="8c203-175">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="8c203-176">Obsługa programów formatującego i funkcji opartych na programie jest dostępna przez zainstalowanie pakietu NuGet [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) i skonfigurowanie go w `Startup.ConfigureServices`programie. `Newtonsoft.Json`</span><span class="sxs-lookup"><span data-stu-id="8c203-176">Support for `Newtonsoft.Json`-based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddControllers()
    .AddNewtonsoftJson();
```

<span data-ttu-id="8c203-177">Niektóre funkcje mogą nie współdziałać `System.Text.Json`z modułami formatującego opartymi na systemie i wymagają `Newtonsoft.Json`odwołania do elementów formatujących opartych na danych dla ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="8c203-177">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters for the ASP.NET Core 3.0 release.</span></span> <span data-ttu-id="8c203-178">Kontynuuj korzystanie z `Newtonsoft.Json`programu formatującego w oparciu o, jeśli Twoja aplikacja ASP.NET Core 3,0 lub nowsza:</span><span class="sxs-lookup"><span data-stu-id="8c203-178">Continue using the `Newtonsoft.Json`-based formatters if your ASP.NET Core 3.0 or later app:</span></span>

* <span data-ttu-id="8c203-179">Używa `Newtonsoft.Json` atrybutów (na `[JsonProperty]` przykład lub `[JsonIgnore]`), dostosowuje `Newtonsoft.Json` ustawienia serializacji lub opiera się na udostępnianych funkcjach.</span><span class="sxs-lookup"><span data-stu-id="8c203-179">Uses `Newtonsoft.Json` attributes (for example, `[JsonProperty]` or `[JsonIgnore]`), customizes the serialization settings, or relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="8c203-180">Konfiguruje `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="8c203-180">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="8c203-181">Przed ASP.NET Core 3,0, `JsonResult.SerializerSettings` akceptuje `JsonSerializerSettings` wystąpienie, które jest specyficzne dla `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="8c203-181">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="8c203-182">Generuje dokumentację [openapi](<xref:tutorials/web-api-help-pages-using-swagger>) .</span><span class="sxs-lookup"><span data-stu-id="8c203-182">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="8c203-183">Funkcje dla `Newtonsoft.Json`elementów formatujących opartych na programie można skonfigurować `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="8c203-183">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefautlContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="8c203-184">Opcje serializacji danych wyjściowych dla poszczególnych akcji można skonfigurować przy użyciu polecenia `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="8c203-184">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="8c203-185">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8c203-185">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

### <a name="add-xml-format-support"></a><span data-ttu-id="8c203-186">Dodawanie obsługi formatu XML</span><span class="sxs-lookup"><span data-stu-id="8c203-186">Add XML format support</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="8c203-187">Aby dodać obsługę formatowania XML w ASP.NET Core 2,2 lub starszej, zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. formatującegos. XML](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) .</span><span class="sxs-lookup"><span data-stu-id="8c203-187">To add XML formatting support in ASP.NET Core 2.2 or earlier, install the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

::: moniker-end

<span data-ttu-id="8c203-188">Elementy formatujące XML zaimplementowane `System.Xml.Serialization.XmlSerializer` przy użyciu można skonfigurować, <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> wywołując `Startup.ConfigureServices`w:</span><span class="sxs-lookup"><span data-stu-id="8c203-188">XML formatters implemented using `System.Xml.Serialization.XmlSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="8c203-189">Alternatywnie można skonfigurować elementy formatujące XML `System.Runtime.Serialization.DataContractSerializer` zaimplementowane przy użyciu, wywołując <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8c203-189">Alternatively, XML formatters implemented using `System.Runtime.Serialization.DataContractSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

<span data-ttu-id="8c203-190">Po dodaniu obsługi formatowania XML, metody kontrolera powinny zwrócić odpowiedni format na podstawie `Accept` nagłówka żądania, ponieważ przykład programu Fiddler ilustruje:</span><span class="sxs-lookup"><span data-stu-id="8c203-190">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Programu Fiddler: Karta RAW dla żądania zawiera wartość nagłówka Accept to Application/XML.](formatting/_static/xml-response.png)

<span data-ttu-id="8c203-193">Na karcie inspektorzy można zobaczyć, że nieprzetworzone żądanie Get zostało wykonane z `Accept: application/xml` zestawem nagłówków.</span><span class="sxs-lookup"><span data-stu-id="8c203-193">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="8c203-194">Okienko odpowiedzi zawiera `Content-Type: application/xml` nagłówek, `Author` a obiekt został Zserializowany do formatu XML.</span><span class="sxs-lookup"><span data-stu-id="8c203-194">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="8c203-195">Użyj karty układacz, aby zmodyfikować żądanie do określenia `application/json` `Accept` w nagłówku.</span><span class="sxs-lookup"><span data-stu-id="8c203-195">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="8c203-196">Wykonaj żądanie, a odpowiedź zostanie sformatowana w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="8c203-196">Execute the request, and the response will be formatted as JSON:</span></span>

![Programu Fiddler: Karta RAW dla żądania pokazuje, że wartość nagłówka Accept to Application/JSON.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="8c203-199">Na tym zrzucie ekranu można zobaczyć, czy żądanie ustawia nagłówek `Accept: application/json` , a odpowiedź określa takie samo, jak jego. `Content-Type`</span><span class="sxs-lookup"><span data-stu-id="8c203-199">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="8c203-200">`Author` Obiekt jest wyświetlany w treści odpowiedzi w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="8c203-200">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="8c203-201">Wymuszanie określonego formatu</span><span class="sxs-lookup"><span data-stu-id="8c203-201">Forcing a Particular Format</span></span>

<span data-ttu-id="8c203-202">Jeśli chcesz ograniczyć formaty odpowiedzi dla konkretnej akcji, możesz zastosować `[Produces]` filtr.</span><span class="sxs-lookup"><span data-stu-id="8c203-202">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="8c203-203">`[Produces]` Filtr określa formaty odpowiedzi dla określonej akcji (lub kontrolera).</span><span class="sxs-lookup"><span data-stu-id="8c203-203">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="8c203-204">Podobnie jak większość [filtrów](xref:mvc/controllers/filters), można to zastosować w ramach akcji, kontrolera lub zakresu globalnego.</span><span class="sxs-lookup"><span data-stu-id="8c203-204">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="8c203-205">Filtr wymusi, że `AuthorsController` wszystkie akcje w programie mają zwracać odpowiedzi w formacie JSON, nawet jeśli dla aplikacji skonfigurowano inne elementy formatujące, `Accept` a klient dostarczył nagłówek z żądaniem innego, dostępnego `[Produces]` Formatowanie.</span><span class="sxs-lookup"><span data-stu-id="8c203-205">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="8c203-206">Zobacz [filtry](xref:mvc/controllers/filters) , aby dowiedzieć się więcej, w tym, jak zastosować filtry globalnie.</span><span class="sxs-lookup"><span data-stu-id="8c203-206">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="8c203-207">Specjalne elementy formatujące Case</span><span class="sxs-lookup"><span data-stu-id="8c203-207">Special Case Formatters</span></span>

<span data-ttu-id="8c203-208">Niektóre specjalne przypadki są implementowane przy użyciu wbudowanych elementów formatujących.</span><span class="sxs-lookup"><span data-stu-id="8c203-208">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="8c203-209">Domyślnie `string` typy zwracane będą formatowane jako *tekst/zwykły* (*tekst/HTML* , jeśli żądano za `Accept` pośrednictwem nagłówka).</span><span class="sxs-lookup"><span data-stu-id="8c203-209">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="8c203-210">To zachowanie można usunąć, usuwając `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8c203-210">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="8c203-211">Elementy formatującego można usunąć w `Configure` metodzie w *Startup.cs* (pokazanej poniżej).</span><span class="sxs-lookup"><span data-stu-id="8c203-211">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="8c203-212">Akcje, które mają typ zwracany obiektu modelu zwróci 204 braku odpowiedzi zawartości podczas powrotu `null`.</span><span class="sxs-lookup"><span data-stu-id="8c203-212">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="8c203-213">To zachowanie można usunąć, usuwając `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8c203-213">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="8c203-214">Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8c203-214">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="8c203-215">`TextOutputFormatter`Bez zwracanychtypówzwraca406nieakceptowalny`string` , na przykład.</span><span class="sxs-lookup"><span data-stu-id="8c203-215">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="8c203-216">Należy pamiętać, że jeśli element formatujący XML istnieje, `string` format zwracanych typów `TextOutputFormatter` , jeśli zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="8c203-216">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="8c203-217">Bez obiektów `HttpNoContentOutputFormatter`o wartości null są formatowane przy użyciu skonfigurowanego programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="8c203-217">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="8c203-218">Na przykład, program formatujący JSON po prostu zwróci odpowiedź z treścią `null`, podczas gdy program formatujący XML zwróci pusty element XML z zestawem atrybutów. `xsi:nil="true"`</span><span class="sxs-lookup"><span data-stu-id="8c203-218">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="8c203-219">Mapowania adresów URL w formacie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="8c203-219">Response Format URL Mappings</span></span>

<span data-ttu-id="8c203-220">Klienci mogą zażądać określonego formatu jako części adresu URL, takiego jak ciąg zapytania lub część ścieżki lub przy użyciu rozszerzenia pliku specyficznego dla formatu, takiego jak. XML lub. JSON.</span><span class="sxs-lookup"><span data-stu-id="8c203-220">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="8c203-221">Mapowanie ze ścieżki żądania należy określić w marszrucie używanej przez interfejs API.</span><span class="sxs-lookup"><span data-stu-id="8c203-221">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="8c203-222">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8c203-222">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="8c203-223">Ta trasa zezwala na określenie żądanego formatu jako opcjonalnego rozszerzenia pliku.</span><span class="sxs-lookup"><span data-stu-id="8c203-223">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="8c203-224">Atrybut sprawdza obecność wartości format `RouteData` w i mapuje format odpowiedzi do odpowiedniego programu formatującego podczas tworzenia odpowiedzi. `[FormatFilter]`</span><span class="sxs-lookup"><span data-stu-id="8c203-224">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="8c203-225">Szlak</span><span class="sxs-lookup"><span data-stu-id="8c203-225">Route</span></span>            |             <span data-ttu-id="8c203-226">EQ</span><span class="sxs-lookup"><span data-stu-id="8c203-226">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="8c203-227">Domyślny program formatujący dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="8c203-227">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="8c203-228">Program formatujący JSON (jeśli jest skonfigurowany)</span><span class="sxs-lookup"><span data-stu-id="8c203-228">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="8c203-229">Program formatujący XML (jeśli jest skonfigurowany)</span><span class="sxs-lookup"><span data-stu-id="8c203-229">The XML formatter (if configured)</span></span>  |
