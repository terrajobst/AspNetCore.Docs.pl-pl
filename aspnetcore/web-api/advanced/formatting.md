---
title: Formatowanie danych odpowiedzi w interfejsu API platformy ASP.NET Core sieci Web
author: ardalis
description: Dowiedz się, jak formatowanie danych odpowiedzi w interfejsu API platformy ASP.NET Core sieci Web.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/formatting
ms.openlocfilehash: f25705bcd3a704de12ea24c3e196de1ba1afd492
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2018
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="01848-103">Formatowanie danych odpowiedzi w interfejsu API platformy ASP.NET Core sieci Web</span><span class="sxs-lookup"><span data-stu-id="01848-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="01848-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="01848-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="01848-105">ASP.NET Core MVC ma wbudowaną obsługę formatowania danych odpowiedzi, za pomocą stałych formatów lub w odpowiedzi do klienta specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="01848-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="01848-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="01848-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="01848-107">Wyniki akcji specyficzne dla formatu</span><span class="sxs-lookup"><span data-stu-id="01848-107">Format-Specific Action Results</span></span>

<span data-ttu-id="01848-108">Niektóre typy wyników akcji są specyficzne dla określonego formatu, takie jak `JsonResult` i `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="01848-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="01848-109">Akcje może zwrócić określone wyniki, które są zawsze formatowane w określonym czasie.</span><span class="sxs-lookup"><span data-stu-id="01848-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="01848-110">Na przykład zwracanie `JsonResult` zwróci dane w formacie JSON, niezależnie od preferencje dotyczące klienta.</span><span class="sxs-lookup"><span data-stu-id="01848-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="01848-111">Podobnie, zwracając `ContentResult` zwraca ciąg w formacie tekstu zwykłego danych (podobnie jak po prostu zwraca ciąg).</span><span class="sxs-lookup"><span data-stu-id="01848-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="01848-112">Akcja nie jest wymagane, aby zwrócić określonego typu; MVC obsługuje żadnej wartości zwracanych obiektów.</span><span class="sxs-lookup"><span data-stu-id="01848-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="01848-113">Jeśli działanie zwróci `IActionResult` implementacji i kontrolera dziedziczy `Controller`, deweloperzy mają wiele metody pomocnicze odpowiednie do wielu opcji.</span><span class="sxs-lookup"><span data-stu-id="01848-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="01848-114">Wyniki z akcji, które zwracają obiekty, które nie są `IActionResult` typy będzie można zserializować przy użyciu odpowiednich `IOutputFormatter` implementacji.</span><span class="sxs-lookup"><span data-stu-id="01848-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="01848-115">Aby zwrócić dane w określonym formacie z kontrolerem, która dziedziczy `Controller` klasy podstawowej, należy użyć metody pomocniczej wbudowanych `Json` do zwrócenia JSON i `Content` na zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="01848-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="01848-116">Twoje metoda akcji powinna zwrócić typ określony wynik (na przykład `JsonResult`) lub `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="01848-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="01848-117">Zwraca dane w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="01848-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="01848-118">Przykładowa odpowiedź z tej akcji:</span><span class="sxs-lookup"><span data-stu-id="01848-118">Sample response from this action:</span></span>

![Karta sieci narzędzi dla deweloperów w programie Microsoft Edge przedstawiający typ zawartości odpowiedzi to application/json](formatting/_static/json-response.png)

<span data-ttu-id="01848-120">Należy pamiętać, że typ zawartości odpowiedzi jest `application/json`, pokazano zarówno na liście żądania sieci, jak i w sekcji nagłówków odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="01848-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="01848-121">Należy również zauważyć listę opcji dostępnych w przeglądarce (w tym przypadku Microsoft Edge) w nagłówku Accept w sekcji nagłówki żądania.</span><span class="sxs-lookup"><span data-stu-id="01848-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="01848-122">Bieżąca metoda ignoruje ten nagłówek; Poniżej omówiono obeying go.</span><span class="sxs-lookup"><span data-stu-id="01848-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="01848-123">Aby zwrócić dane w formacie zwykłego tekstu, należy użyć `ContentResult` i `Content` pomocy:</span><span class="sxs-lookup"><span data-stu-id="01848-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="01848-124">Odpowiedź z tej akcji:</span><span class="sxs-lookup"><span data-stu-id="01848-124">A response from this action:</span></span>

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge przedstawiający typ zawartości odpowiedzi to zwykły tekst](formatting/_static/text-response.png)

<span data-ttu-id="01848-126">W takim przypadku należy zwrócić uwagę `Content-Type` zwracane jest `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="01848-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="01848-127">Można również uzyskać to samo przy użyciu tylko ciąg typu odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="01848-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="01848-128">Dla nietrywialnej akcji z wieloma zwracać typów lub opcji (na przykład różnych kodów stanu HTTP na podstawie wyniku operacji wykonywanych) tak, `IActionResult` jako typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="01848-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="01848-129">Negocjowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="01848-129">Content Negotiation</span></span>

<span data-ttu-id="01848-130">Negocjowanie zawartości (*conneg* skrócie) występuje, gdy klient określa [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="01848-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="01848-131">Domyślny format używany przez program ASP.NET Core MVC jest JSON.</span><span class="sxs-lookup"><span data-stu-id="01848-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="01848-132">Negocjacje zawartości jest implementowany przez `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="01848-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="01848-133">Również jest wbudowana kod stanu wyników określonych akcji zwrócony z metody pomocnicze (wszystkie oparte na `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="01848-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="01848-134">Można także wrócić typu modelu (klasa została zdefiniowana jako typ transferu danych) i ramach będzie automatycznie zawijany w `ObjectResult` dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="01848-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="01848-135">Używa następującej metody akcji `Ok` i `NotFound` metody pomocnicze:</span><span class="sxs-lookup"><span data-stu-id="01848-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="01848-136">Odpowiedź w formacie JSON zostaną zwrócone, chyba że zażądano innego formatu i serwer może zwrócić żądany format.</span><span class="sxs-lookup"><span data-stu-id="01848-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="01848-137">Można użyć narzędzia, takiego jak [Fiddler](http://www.telerik.com/fiddler) Aby utworzyć żądanie zawiera nagłówek Accept i określić innego formatu.</span><span class="sxs-lookup"><span data-stu-id="01848-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="01848-138">W takim przypadku, jeśli serwer ma *program formatujący* które powodują uzyskanie odpowiedzi w formacie żądanego, wynik zostanie zwrócony w formacie preferowanego klienta.</span><span class="sxs-lookup"><span data-stu-id="01848-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler Konsola pokazująca, że ręcznie utworzonych pobrać żądania o wartości nagłówka Accept application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="01848-140">Na zrzucie ekranu powyżej, Fiddler Composer został użyty do wygenerowania żądania, określając `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="01848-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="01848-141">Domyślnie, ASP.NET Core MVC obsługuje tylko JSON, dlatego nawet jeśli nie określono innego formatu, zwracana jest nadal formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="01848-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="01848-142">Jak dodać dodatkowe elementy formatujące w następnej sekcji zostanie wyświetlone.</span><span class="sxs-lookup"><span data-stu-id="01848-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="01848-143">Akcji kontrolera może zwrócić POCOs (zwykły stare obiekty CLR), w którym to przypadku platformy ASP.NET Core MVC automatycznie tworzy `ObjectResult` dla Ciebie, który opakowuje obiektu.</span><span class="sxs-lookup"><span data-stu-id="01848-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="01848-144">Klient pobierze sformatowany Zserializowany obiekt (JSON format jest domyślnie; można skonfigurować XML lub inne formaty).</span><span class="sxs-lookup"><span data-stu-id="01848-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="01848-145">Jeśli obiekt zwracane jest `null`, a następnie zwraca platformę `204 No Content` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="01848-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="01848-146">Zwraca typ obiektu:</span><span class="sxs-lookup"><span data-stu-id="01848-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="01848-147">W przykładzie żądania dla aliasu autora otrzyma odpowiedź 200 OK z danymi autora.</span><span class="sxs-lookup"><span data-stu-id="01848-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="01848-148">Żądanie nieprawidłowy alias otrzyma 204 odpowiedzi bez zawartości.</span><span class="sxs-lookup"><span data-stu-id="01848-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="01848-149">Poniżej przedstawiono zrzuty ekranu przedstawiający odpowiedzi w formacie XML i JSON.</span><span class="sxs-lookup"><span data-stu-id="01848-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="01848-150">Proces negocjacje zawartości</span><span class="sxs-lookup"><span data-stu-id="01848-150">Content Negotiation Process</span></span>

<span data-ttu-id="01848-151">Zawartości *negocjacji* tylko ma miejsce, jeśli `Accept` nagłówek pojawia się w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="01848-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="01848-152">Jeśli żądanie zawiera nagłówek accept, platformę będzie wyliczyć typów nośników w nagłówku accept w kolejności preferencji i wyszukiwany element formatujący, który może utworzyć odpowiedź w jednym z formatów określony przez nagłówek accept.</span><span class="sxs-lookup"><span data-stu-id="01848-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="01848-153">W przypadku, gdy element formatujący nie zostanie znaleziony, który można spełnić żądania klienta, platformę spróbuje znaleźć pierwszy element formatujący, który może utworzyć odpowiedź (chyba że deweloper została skonfigurowana opcja w `MvcOptions` do zwrócenia 406 nie do przyjęcia zamiast niego).</span><span class="sxs-lookup"><span data-stu-id="01848-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="01848-154">Jeśli żądanie określa XML, ale nie skonfigurowano element formatujący XML, JSON program formatujący będzie używany.</span><span class="sxs-lookup"><span data-stu-id="01848-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="01848-155">Ogólnie rzecz biorąc Jeśli element formatujący nie skonfigurowano które zapewniają żądany format, a następnie pierwszy element formatujący, który można sformatować obiekt jest używany.</span><span class="sxs-lookup"><span data-stu-id="01848-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="01848-156">Jeśli nie zostanie podany bez nagłówka, pierwszy element formatujący, który może obsługiwać obiektu, który ma zostać zwrócona będzie używany do serializacji w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="01848-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="01848-157">W takim przypadku nie ma żadnych negocjacji miejsce — serwer jest określenie format zostanie użyty.</span><span class="sxs-lookup"><span data-stu-id="01848-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="01848-158">Jeśli zawiera nagłówek Accept `*/*`, nagłówek będą ignorowane, chyba że `RespectBrowserAcceptHeader` ma ustawioną wartość true na `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="01848-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="01848-159">Przeglądarki i negocjacje zawartości</span><span class="sxs-lookup"><span data-stu-id="01848-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="01848-160">W odróżnieniu od typowych klientów interfejsu API, przeglądarki sieci web mają tendencję do dostarczania `Accept` nagłówków zawierających szerokiej gamy formatów, w tym symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="01848-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="01848-161">Domyślnie, gdy w ramach wykrywa, czy żądanie pochodzi z przeglądarki, jego zignoruje `Accept` nagłówka i zamiast tego zwracany zawartości w aplikacji skonfigurowana domyślny format (JSON, chyba że skonfigurowane w inny sposób).</span><span class="sxs-lookup"><span data-stu-id="01848-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="01848-162">Zapewnia bardziej spójne środowisko, korzystając z różnych przeglądarek użycie interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="01848-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="01848-163">Jeśli wolisz nagłówków accept przeglądarkę honoruj aplikacji, można skonfigurować to w ramach konfiguracji MVC przez ustawienie `RespectBrowserAcceptHeader` do `true` w `ConfigureServices` metody w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="01848-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="01848-164">Konfigurowanie programów formatujących</span><span class="sxs-lookup"><span data-stu-id="01848-164">Configuring Formatters</span></span>

<span data-ttu-id="01848-165">Jeśli aplikacja wymaga do obsługi dodatkowych formatach ponad wartość domyślną JSON, można dodać pakiety NuGet i skonfigurować MVC do ich obsługi.</span><span class="sxs-lookup"><span data-stu-id="01848-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="01848-166">Istnieją oddzielne elementy formatujące dla danych wejściowych i wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="01848-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="01848-167">Wejściowy elementy formatujące są używane przez [powiązanie modelu](xref:mvc/models/model-binding); elementy formatujące dane wyjściowe są używane do formatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="01848-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="01848-168">Można również skonfigurować [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="01848-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="01848-169">Dodawanie obsługi formatu XML</span><span class="sxs-lookup"><span data-stu-id="01848-169">Adding XML Format Support</span></span>

<span data-ttu-id="01848-170">Aby dodać obsługę formatowania XML, zainstaluj `Microsoft.AspNetCore.Mvc.Formatters.Xml` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="01848-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="01848-171">Dodaj XmlSerializerFormatters do konfiguracji MVC w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="01848-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="01848-172">Alternatywnie można dodać tylko element formatujący danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="01848-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="01848-173">Te dwie metody będzie serializować wyników za pomocą `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="01848-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="01848-174">Jeśli wolisz, możesz użyć `System.Runtime.Serialization.DataContractSerializer` przez dodanie jego skojarzony element formatujący:</span><span class="sxs-lookup"><span data-stu-id="01848-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="01848-175">Po dodaniu obsługę formatowania XML metody kontroler powinien zwrócić odpowiedni format na podstawie żądania `Accept` nagłówka, jak ta Fiddler przykładzie pokazano:</span><span class="sxs-lookup"><span data-stu-id="01848-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Konsola fiddler: Raw kartę dla żądania zawiera wartość nagłówka Accept jest aplikacja/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="01848-178">Można zobaczyć w karcie inspektorzy zgłaszającego żądanie Raw GET z `Accept: application/xml` zestaw nagłówka.</span><span class="sxs-lookup"><span data-stu-id="01848-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="01848-179">Pokazuje okienko odpowiedzi `Content-Type: application/xml` nagłówka i `Author` do pliku XML ma została wykonana serializacja obiektu.</span><span class="sxs-lookup"><span data-stu-id="01848-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="01848-180">Karta Composer służy do modyfikowania żądania, aby określić `application/json` w `Accept` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="01848-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="01848-181">Wykonanie żądania i odpowiedzi będą formatowane jako elementu JSON:</span><span class="sxs-lookup"><span data-stu-id="01848-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Konsola fiddler: Raw kartę dla żądania zawiera wartość nagłówka Accept jest application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="01848-184">Tego zrzutu ekranu widać żądania ustawia nagłówek z `Accept: application/json` i określa odpowiedź, taka sama jak jego `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="01848-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="01848-185">`Author` Obiektu jest wyświetlany w treści odpowiedzi w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="01848-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="01848-186">Wymuszanie w określonym formacie</span><span class="sxs-lookup"><span data-stu-id="01848-186">Forcing a Particular Format</span></span>

<span data-ttu-id="01848-187">Jeśli chcesz ograniczyć formatów odpowiedzi dla określonej akcji można wykonywać następujące czynności, możesz zastosować `[Produces]` filtru.</span><span class="sxs-lookup"><span data-stu-id="01848-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="01848-188">`[Produces]` Filtru określa format odpowiedzi dla danego działania (lub kontrolera).</span><span class="sxs-lookup"><span data-stu-id="01848-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="01848-189">Większość, takich jak [filtry](xref:mvc/controllers/filters), może to być zastosowane na działania, kontrolera lub zakresie globalnym.</span><span class="sxs-lookup"><span data-stu-id="01848-189">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="01848-190">`[Produces]` Filtru wymusi wszystkich akcji w ramach `AuthorsController` do zwracania odpowiedzi w formacie JSON, nawet jeśli inne elementy formatujące zostały skonfigurowane dla aplikacji i klient dostarczył `Accept` nagłówka żądania różnych, dostępne Format.</span><span class="sxs-lookup"><span data-stu-id="01848-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="01848-191">Zobacz [filtry](xref:mvc/controllers/filters) Aby dowiedzieć się więcej, łącznie ze sposobem stosowania filtrów globalnie.</span><span class="sxs-lookup"><span data-stu-id="01848-191">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="01848-192">Specjalne elementy formatujące Case</span><span class="sxs-lookup"><span data-stu-id="01848-192">Special Case Formatters</span></span>

<span data-ttu-id="01848-193">Niektóre specjalne przypadki są implementowane za pomocą wbudowanych elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="01848-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="01848-194">Domyślnie `string` zwracane typy będą formatowane jako *zwykły tekst* (*tekst i html* żądanie za pośrednictwem `Accept` nagłówka).</span><span class="sxs-lookup"><span data-stu-id="01848-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="01848-195">To zachowanie można usunąć przez usunięcie `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="01848-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="01848-196">Usuń elementy formatujące w `Configure` metody w *Startup.cs* (pokazana poniżej).</span><span class="sxs-lookup"><span data-stu-id="01848-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="01848-197">Akcje, które mają obiektu modelu zwracany typ zwróci 204 zawartości odpowiedzi wracając `null`.</span><span class="sxs-lookup"><span data-stu-id="01848-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="01848-198">To zachowanie można usunąć przez usunięcie `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="01848-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="01848-199">Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="01848-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="01848-200">Bez `TextOutputFormatter`, `string` zwracać typów zwracać 406 nie do przyjęcia, na przykład.</span><span class="sxs-lookup"><span data-stu-id="01848-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="01848-201">Należy pamiętać, że jeśli element formatujący XML istnieje, jej sformatuje `string` typy zwracane, jeżeli `TextOutputFormatter` zostanie usunięta.</span><span class="sxs-lookup"><span data-stu-id="01848-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="01848-202">Bez `HttpNoContentOutputFormatter`, obiektów null są sformatowane przy użyciu skonfigurowanego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="01848-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="01848-203">Na przykład program formatujący JSON po prostu zwróci odpowiedzi jednostki `null`, gdy element formatujący XML zwróci pustego elementu XML z atrybutem `xsi:nil="true"` ustawiony.</span><span class="sxs-lookup"><span data-stu-id="01848-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="01848-204">Mapowania adresów URL Format odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="01848-204">Response Format URL Mappings</span></span>

<span data-ttu-id="01848-205">Klienci mogą żądać określony format jako część adresu URL, takie jak w ciągu zapytania lub część ścieżki lub przy użyciu rozszerzenia nazwy pliku specyficzne dla formatu takiego jak XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="01848-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="01848-206">Powinny być określone mapowanie ścieżki żądania w trasy, który używa interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="01848-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="01848-207">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="01848-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="01848-208">Żądany format można określić jako opcjonalny plik rozszerzenie umożliwia tej trasy.</span><span class="sxs-lookup"><span data-stu-id="01848-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="01848-209">`[FormatFilter]` Atrybutu sprawdza istnienie wartości formatu `RouteData` i przypisze format odpowiedzi do odpowiedniego obiektu formatującego po utworzeniu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="01848-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>


|           <span data-ttu-id="01848-210">Trasy</span><span class="sxs-lookup"><span data-stu-id="01848-210">Route</span></span>            |             <span data-ttu-id="01848-211">Program formatujący</span><span class="sxs-lookup"><span data-stu-id="01848-211">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="01848-212">Domyślny element formatujący danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="01848-212">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="01848-213">Program formatujący JSON (jeśli jest skonfigurowane)</span><span class="sxs-lookup"><span data-stu-id="01848-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="01848-214">Element formatujący XML (jeśli jest skonfigurowane)</span><span class="sxs-lookup"><span data-stu-id="01848-214">The XML formatter (if configured)</span></span>  |

