---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Wiązanie parametrów interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97785db962691c25bac03f904924321af2f6b500
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810085"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="90207-102">Wiązanie parametrów interfejsie Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="90207-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="90207-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="90207-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="90207-104">Gdy interfejs API sieci Web wywołuje metodę dla kontrolera, należy ustawić w niej wartości parametrów w procesie zwanym *powiązania*.</span><span class="sxs-lookup"><span data-stu-id="90207-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="90207-105">W tym artykule opisano, jak interfejs API sieci Web tworzy powiązania parametrów i w jaki sposób dostosować proces wiązania.</span><span class="sxs-lookup"><span data-stu-id="90207-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="90207-106">Domyślnie by powiązać parametry interfejsu API sieci Web stosowane są następujące reguły:</span><span class="sxs-lookup"><span data-stu-id="90207-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="90207-107">Jeśli parametr jest typu "prosta", interfejs API sieci Web próbuje pobrać wartość z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="90207-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="90207-108">Proste typy obejmują .NET [typów pierwotnych](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, i tak dalej), oraz **TimeSpan**, **Daty/godziny**, **Guid**, **dziesiętna**, i **ciąg**, *oraz* wszystkie Typ konwertera typów, który można przekonwertować ciągu.</span><span class="sxs-lookup"><span data-stu-id="90207-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="90207-109">(Więcej informacji na temat konwerterów typów później.)</span><span class="sxs-lookup"><span data-stu-id="90207-109">(More about type converters later.)</span></span>
- <span data-ttu-id="90207-110">Dla typów złożonych, interfejs API sieci Web próbuje odczytać wartości z treści wiadomości, za pomocą [program formatujący typ nośnika](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="90207-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="90207-111">Na przykład poniżej przedstawiono typowe metody kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="90207-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="90207-112">*Identyfikator* parametr jest &quot;proste&quot; typ, dzięki czemu próbuje pobrać wartość z identyfikatora URI żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="90207-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="90207-113">*Elementu* parametr jest typem złożonym, dzięki czemu można odczytać wartości z treści żądania interfejsu API sieci Web korzysta z elementu formatującego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="90207-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="90207-114">Aby uzyskać wartość z identyfikatora URI, internetowy interfejs API wygląda w danych trasy i ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="90207-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="90207-115">Dane trasy jest wypełniana w przypadku routingu system analizuje identyfikator URI i dopasowuje je do trasy.</span><span class="sxs-lookup"><span data-stu-id="90207-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="90207-116">Aby uzyskać więcej informacji, zobacz [Routing i wybieranie akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="90207-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="90207-117">W dalszej części tego artykułu I pokazano, w jaki sposób dostosować procesie powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="90207-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="90207-118">W przypadku typów złożonych jednak należy rozważyć użycie programy formatujące typy nośnika, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="90207-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="90207-119">Klucza zasada HTTP jest, że zasoby są wysyłane w treści wiadomości, za pomocą negocjowania zawartości w celu określania reprezentację zasobu.</span><span class="sxs-lookup"><span data-stu-id="90207-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="90207-120">Programy formatujące typy nośnika zostały zaprojektowane do dokładnie tego celu.</span><span class="sxs-lookup"><span data-stu-id="90207-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="90207-121">Przy użyciu [FromUri]</span><span class="sxs-lookup"><span data-stu-id="90207-121">Using [FromUri]</span></span>

<span data-ttu-id="90207-122">Aby wymusić interfejsu API sieci Web można odczytać to typ złożony z identyfikatora URI, należy dodać **[FromUri]** atrybutu do parametru.</span><span class="sxs-lookup"><span data-stu-id="90207-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="90207-123">W poniższym przykładzie zdefiniowano `GeoPoint` typu wraz z metody kontrolera, który pobiera `GeoPoint` z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="90207-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="90207-124">Klienta można umieścić wartości długości i szerokości geograficznej w ciągu zapytania i interfejsu API sieci Web będzie konstruowania za ich pomocą `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="90207-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="90207-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="90207-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="90207-126">Przy użyciu [FromBody]</span><span class="sxs-lookup"><span data-stu-id="90207-126">Using [FromBody]</span></span>

<span data-ttu-id="90207-127">Aby wymusić odczytu typu prostego z treści żądania interfejsu API sieci Web, należy dodać **[FromBody]** atrybutu do parametru:</span><span class="sxs-lookup"><span data-stu-id="90207-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="90207-128">W tym przykładzie interfejs API sieci Web użyje elementu formatującego typu nośnika można odczytać wartości z *nazwa* z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="90207-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="90207-129">Oto przykładowe żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="90207-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="90207-130">Jeśli parametr ma [FromBody], interfejs API sieci Web używa nagłówka Content-Type, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="90207-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="90207-131">W tym przykładzie typem zawartości jest &quot;application/json&quot; i treść żądania jest nieprzetworzonego ciągu JSON (a nie obiekt JSON).</span><span class="sxs-lookup"><span data-stu-id="90207-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="90207-132">Co najwyżej jeden parametr może odczytywać treść komunikatu.</span><span class="sxs-lookup"><span data-stu-id="90207-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="90207-133">Dlatego ta nie będzie działać:</span><span class="sxs-lookup"><span data-stu-id="90207-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="90207-134">Przyczyna, dla tej reguły jest, że treść żądania mogą być przechowywane w niebuforowanym strumienia, który może zostać odczytany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="90207-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="90207-135">Konwertery typu</span><span class="sxs-lookup"><span data-stu-id="90207-135">Type Converters</span></span>

<span data-ttu-id="90207-136">Ułatwia traktować klasę jako typu prostego (dzięki czemu interfejs API sieci Web spróbuje powiązać go z identyfikatora URI) interfejsu API sieci Web, tworząc **TypeConverter** i zapewniając konwersji ciągu.</span><span class="sxs-lookup"><span data-stu-id="90207-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="90207-137">Poniższy kod przedstawia `GeoPoint` klasa, która reprezentuje punkt geograficzne oraz **TypeConverter** konwersji z ciągów do `GeoPoint` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="90207-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="90207-138">`GeoPoint` Klasy zostanie nadany **[TypeConverter]** atrybutu, aby określić konwertera typów.</span><span class="sxs-lookup"><span data-stu-id="90207-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="90207-139">(W tym przykładzie został ZAINSPIROWANE wpis w blogu wstrzymania Mike [sposobu tworzenia powiązania do obiektów niestandardowych w podpisach akcji w MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="90207-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="90207-140">Teraz interfejs API sieci Web będą traktować `GeoPoint` jako typ prosty, czyli podejmie próbę powiązania `GeoPoint` parametry z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="90207-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="90207-141">Nie musisz uwzględniać **[FromUri]** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="90207-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="90207-142">Klient może wywołać metody za pomocą identyfikatora URI w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="90207-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="90207-143">Integratorów modelu</span><span class="sxs-lookup"><span data-stu-id="90207-143">Model Binders</span></span>

<span data-ttu-id="90207-144">Opcja bardziej elastyczne niż konwertera typów polega na utworzeniu niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="90207-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="90207-145">Za pomocą integratora modelu masz dostęp do elementów, takich jak żądania HTTP, opis akcji i wartości w wierszach z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="90207-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="90207-146">Aby utworzyć obiekt wiążący modelu, należy zaimplementować **interfejs IModelBinder** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="90207-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="90207-147">Ten interfejs definiuje jedną metodę **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="90207-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="90207-148">Oto integratora modelu dla `GeoPoint` obiektów.</span><span class="sxs-lookup"><span data-stu-id="90207-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="90207-149">Integrator modelu pobiera nieprzetworzone wartości wejściowe z *dostawcy wartości*.</span><span class="sxs-lookup"><span data-stu-id="90207-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="90207-150">W tym projekcie oddziela dwie odrębne funkcje:</span><span class="sxs-lookup"><span data-stu-id="90207-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="90207-151">Dostawca wartości przyjmuje żądania HTTP i wypełnia słownik par klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="90207-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="90207-152">Integrator modelu użyje tego słownika, aby wypełnić modelu.</span><span class="sxs-lookup"><span data-stu-id="90207-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="90207-153">Dostawca wartości domyślne w interfejsie API sieci Web pobiera wartości z danych trasy i ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="90207-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="90207-154">Na przykład, jeśli identyfikator URI jest `http://localhost/api/values/1?location=48,-122`, dostawca wartości tworzy następujące pary klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="90207-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="90207-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="90207-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="90207-156">Lokalizacja = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="90207-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="90207-157">(Czy jestem zakładając, że szablon trasy domyślne, czyli &quot;interfejsu api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="90207-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="90207-158">Nazwa parametru do powiązania są przechowywane w **ModelBindingContext.ModelName** właściwości.</span><span class="sxs-lookup"><span data-stu-id="90207-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="90207-159">Integrator modelu szuka klucza o tej wartości w słowniku.</span><span class="sxs-lookup"><span data-stu-id="90207-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="90207-160">Jeśli wartość istnieje i mogą być konwertowane na `GeoPoint`, integratora modelu przypisuje wartość powiązanych **ModelBindingContext.Model** właściwości.</span><span class="sxs-lookup"><span data-stu-id="90207-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="90207-161">Należy zauważyć, że integratora modelu nie jest ograniczona do konwersji typu prostego.</span><span class="sxs-lookup"><span data-stu-id="90207-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="90207-162">W tym przykładzie integratora modelu najpierw przeszukuje tabelę znanej lokalizacji, a jeśli ono zawiedzie, używa konwersji typu.</span><span class="sxs-lookup"><span data-stu-id="90207-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="90207-163">**Ustawienie integratora modelu**</span><span class="sxs-lookup"><span data-stu-id="90207-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="90207-164">Istnieje kilka sposobów, aby ustawić integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="90207-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="90207-165">Po pierwsze, możesz dodać **[ModelBinder]** atrybutu do parametru.</span><span class="sxs-lookup"><span data-stu-id="90207-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="90207-166">Można również dodać **[ModelBinder]** atrybutu typu.</span><span class="sxs-lookup"><span data-stu-id="90207-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="90207-167">Interfejs API sieci Web użyje integratora modelu określonego dla wszystkich parametrów tego typu.</span><span class="sxs-lookup"><span data-stu-id="90207-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="90207-168">Na koniec można dodać dostawcę integratora modelu do **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="90207-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="90207-169">Dostawca integratora modelu to po prostu klasę fabryki, która tworzy integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="90207-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="90207-170">Możesz utworzyć dostawcę, wynikające z [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="90207-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="90207-171">Jednak jeśli Twoja integratora modelu obsługuje jeden typ, łatwiej jest użyć wbudowanego **SimpleModelBinderProvider**, który jest przeznaczony do tego celu.</span><span class="sxs-lookup"><span data-stu-id="90207-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="90207-172">Poniższy kod pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="90207-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="90207-173">Za pomocą dostawcy powiązanie modelu, nadal musisz dodać **[ModelBinder]** atrybutu do parametru mówić interfejsu API sieci Web, że należy używać w integratora modelu i nie element formatujący typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="90207-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="90207-174">Ale teraz nie musisz określić typ integratora modelu w atrybucie:</span><span class="sxs-lookup"><span data-stu-id="90207-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="90207-175">Dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="90207-175">Value Providers</span></span>

<span data-ttu-id="90207-176">Wspomniałem, że obiekt wiążący modelu pobiera wartości z dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="90207-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="90207-177">Aby zapisać dostawcę wartości niestandardowych, należy zaimplementować **IValueProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="90207-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="90207-178">Oto przykład, która pobiera wartości z plików cookie żądania:</span><span class="sxs-lookup"><span data-stu-id="90207-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="90207-179">Należy również utworzyć fabryki dostawców wartości, wynikające z **ValueProviderFactory** klasy.</span><span class="sxs-lookup"><span data-stu-id="90207-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="90207-180">Fabryki dostawców wartości, aby dodać **HttpConfiguration** w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="90207-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="90207-181">Interfejs API sieci Web Redaguj wszystkich dostawców wartości, więc kiedy wywołuje obiekt wiążący modelu **ValueProvider.GetValue**, integratora modelu otrzymuje wartość z pierwszym dostawcy wartości, które może wygenerować go.</span><span class="sxs-lookup"><span data-stu-id="90207-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="90207-182">Alternatywnie, można ustawić fabryki dostawców wartości na poziomie parametru przy użyciu **ValueProvider** atrybutu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="90207-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="90207-183">Oznacza to, internetowy interfejs API wiązania modelu za pomocą fabryki dostawców określoną wartość, a nie używać żadnych innych dostawców zarejestrowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="90207-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="90207-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="90207-184">HttpParameterBinding</span></span>

<span data-ttu-id="90207-185">Integratorów modelu są konkretne wystąpienie mechanizmu bardziej ogólnych.</span><span class="sxs-lookup"><span data-stu-id="90207-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="90207-186">Jeśli przyjrzymy się **[ModelBinder]** atrybutu, zobaczysz, że pochodzi od abstrakcyjnej **ParameterBindingAttribute** klasy.</span><span class="sxs-lookup"><span data-stu-id="90207-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="90207-187">Ta klasa definiuje jedną metodę **GetBinding**, co powoduje zwrócenie **HttpParameterBinding** obiektu:</span><span class="sxs-lookup"><span data-stu-id="90207-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="90207-188">**HttpParameterBinding** jest odpowiedzialny za powiązanie parametru z wartością.</span><span class="sxs-lookup"><span data-stu-id="90207-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="90207-189">W przypadku właściwości **[ModelBinder]**, zwraca ten atrybut **HttpParameterBinding** implementację, która używa **interfejs IModelBinder** przeprowadzić faktycznego wiązania.</span><span class="sxs-lookup"><span data-stu-id="90207-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="90207-190">Można też wdrożyć własną **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="90207-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="90207-191">Załóżmy, że chcesz pobrać elementów etag z `if-match` i `if-none-match` nagłówki w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="90207-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="90207-192">Rozpoczniemy od zdefiniowania klasy do reprezentowania elementów ETag.</span><span class="sxs-lookup"><span data-stu-id="90207-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="90207-193">Firma Microsoft będzie również zdefiniować wyliczenie, aby wskazać, czy można pobrać elementu ETag z `if-match` nagłówek lub `if-none-match` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="90207-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="90207-194">Oto **HttpParameterBinding** , pobiera element ETag z żądanego nagłówka i wiąże ją do parametru typu tagu ETag:</span><span class="sxs-lookup"><span data-stu-id="90207-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="90207-195">**ExecuteBindingAsync** metoda wykonuje wiązanie.</span><span class="sxs-lookup"><span data-stu-id="90207-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="90207-196">W ramach tej metody, Dodaj wartość powiązano parametr **ActionArgument** słownika **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="90207-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="90207-197">Jeśli Twoje **ExecuteBindingAsync** metoda odczytuje treści komunikatu żądania, Zastąp **WillReadBody** właściwości, aby zwracała wartość true.</span><span class="sxs-lookup"><span data-stu-id="90207-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="90207-198">Treść żądania może być Niebuforowane strumienia, które mogą być odczytane tylko raz, więc interfejsu API sieci Web wymusza reguły co najwyżej jednego powiązanie można odczytać treści komunikatu.</span><span class="sxs-lookup"><span data-stu-id="90207-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="90207-199">Aby zastosować niestandardowy **HttpParameterBinding**, można zdefiniować atrybut, który pochodzi od klasy **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="90207-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="90207-200">Aby uzyskać `ETagParameterBinding`, zdefiniujemy dwa atrybuty, jeden dla `if-match` nagłówków i jeden dla `if-none-match` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="90207-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="90207-201">Abstrakcyjna klasa bazowa zarówno dziedziczyć.</span><span class="sxs-lookup"><span data-stu-id="90207-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="90207-202">Poniżej przedstawiono metody kontrolera, który używa `[IfNoneMatch]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="90207-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="90207-203">Oprócz **ParameterBindingAttribute**, jest innym haku dodawania niestandardowego **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="90207-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="90207-204">Na **HttpConfiguration** obiektu **ParameterBindingRules** właściwości to zbiór funkcji anomymous typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="90207-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="90207-205">Na przykład można dodać regułę, która korzysta z żadnych parametrów element ETag dla metody GET `ETagParameterBinding` z `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="90207-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="90207-206">Funkcja powinna zwrócić `null` parametrów, których powiązanie nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="90207-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="90207-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="90207-207">IActionValueBinder</span></span>

<span data-ttu-id="90207-208">Cały proces wiązanie parametru jest kontrolowany przez podłączane usługę **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="90207-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="90207-209">Domyślna implementacja klasy **IActionValueBinder** wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="90207-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="90207-210">Wyszukaj **ParameterBindingAttribute** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="90207-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="90207-211">Obejmuje to **[FromBody]**, **[FromUri]**, i **[ModelBinder]**, lub atrybutów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="90207-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="90207-212">W przeciwnym razie Szukaj w **HttpConfiguration.ParameterBindingRules** dla funkcji, która zwraca wartość inną niż null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="90207-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="90207-213">W przeciwnym razie Użyj domyślnych reguł, które mogę opisane wcześniej.</span><span class="sxs-lookup"><span data-stu-id="90207-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="90207-214">Jeśli typ parametru jest "prosta" lub ma konwertera typów, należy powiązać z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="90207-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="90207-215">Jest to równoważne umieszczenie **[FromUri]** atrybutu w parametrze.</span><span class="sxs-lookup"><span data-stu-id="90207-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="90207-216">W przeciwnym razie spróbuj odczytywać parametr treści komunikatu.</span><span class="sxs-lookup"><span data-stu-id="90207-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="90207-217">Jest to równoważne umieszczenie **[FromBody]** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="90207-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="90207-218">Jeśli chcesz, możesz zamienić całą **IActionValueBinder** usługi przy użyciu niestandardowych implementacji.</span><span class="sxs-lookup"><span data-stu-id="90207-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90207-219">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="90207-219">Additional Resources</span></span>

[<span data-ttu-id="90207-220">Przykład powiązania parametru niestandardowego</span><span class="sxs-lookup"><span data-stu-id="90207-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="90207-221">Mike wstrzymania napisał dobre serię wpisów w blogu o wiązanie parametru interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="90207-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="90207-222">Jak działa interfejs API sieci Web, wiązanie parametrów</span><span class="sxs-lookup"><span data-stu-id="90207-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="90207-223">Styl MVC wiązanie parametru dla interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="90207-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="90207-224">Jak powiązać do obiektów niestandardowych w podpisach akcji w interfejsie API sieci Web i MVC</span><span class="sxs-lookup"><span data-stu-id="90207-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="90207-225">Jak utworzyć dostawcę wartości niestandardowe w interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="90207-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="90207-226">Wiązanie parametru internetowy interfejs API kulisy</span><span class="sxs-lookup"><span data-stu-id="90207-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
