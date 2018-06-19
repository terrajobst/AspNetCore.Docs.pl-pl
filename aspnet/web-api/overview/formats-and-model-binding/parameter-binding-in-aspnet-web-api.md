---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parametr wiązania w składniku ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042433"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="df614-102">Parametr wiązania w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="df614-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="df614-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="df614-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="df614-104">Gdy interfejs API sieci Web wywołuje metodę dla kontrolera, należy ustawić wartości parametrów w procesie nazywanym *powiązania*.</span><span class="sxs-lookup"><span data-stu-id="df614-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="df614-105">W tym artykule opisano sposób interfejsu API sieci Web wiąże parametry, i jak można dostosować proces wiązania.</span><span class="sxs-lookup"><span data-stu-id="df614-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="df614-106">Domyślnie interfejsu API sieci Web używa następujące reguły można powiązać parametry:</span><span class="sxs-lookup"><span data-stu-id="df614-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="df614-107">Jeśli parametr jest typu "prosty", interfejsu API sieci Web próbuje pobrać wartość z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="df614-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="df614-108">Proste typy .NET [typów pierwotnych](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **podwójne**itd), oraz **TimeSpan**, **DateTime**, **Guid**, **dziesiętną**, i **ciąg**, *plus* żadnych Typ konwertera typów, który można przekonwertować ciągu.</span><span class="sxs-lookup"><span data-stu-id="df614-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="df614-109">(Więcej informacji na temat typów konwerterów później.)</span><span class="sxs-lookup"><span data-stu-id="df614-109">(More about type converters later.)</span></span>
- <span data-ttu-id="df614-110">Dla typów złożonych, interfejs API sieci Web próbuje odczytać wartości z treści wiadomości, przy użyciu [program formatujący typ nośnika](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="df614-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="df614-111">Na przykład poniżej przedstawiono typowe metody kontrolera interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="df614-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="df614-112">*Identyfikator* parametr jest &quot;proste&quot; typ, więc próbuje pobrać wartość z identyfikatora URI żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="df614-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="df614-113">*Elementu* parametr jest typu złożonego, więc można odczytać wartości z treści żądania interfejsu API sieci Web korzysta z elementu formatującego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="df614-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="df614-114">Aby uzyskać wartość z identyfikatora URI, interfejsu API sieci Web jest wyszukiwany w danych trasy i ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="df614-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="df614-115">Dane trasy jest wypełniana podczas routingu systemu analizuje identyfikator URI i dopasowuje je do trasy.</span><span class="sxs-lookup"><span data-stu-id="df614-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="df614-116">Aby uzyskać więcej informacji, zobacz [routingu i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="df614-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="df614-117">W dalszej części tego artykułu I opisano, jak można dostosować procesie powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="df614-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="df614-118">Dla typów złożonych jednak należy rozważyć użycie programy formatujące typy nośnika, jeśli to możliwe.</span><span class="sxs-lookup"><span data-stu-id="df614-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="df614-119">Klucza zasady HTTP jest, że zasoby są wysyłane w treści wiadomości, za pomocą negocjowania zawartości, aby określić reprezentacja zasobu.</span><span class="sxs-lookup"><span data-stu-id="df614-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="df614-120">Programy formatujące typy nośnika zostały zaprojektowane do dokładnie tego celu.</span><span class="sxs-lookup"><span data-stu-id="df614-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="df614-121">Przy użyciu [FromUri]</span><span class="sxs-lookup"><span data-stu-id="df614-121">Using [FromUri]</span></span>

<span data-ttu-id="df614-122">Aby wymusić interfejsu API sieci Web do odczytu typu złożonego z identyfikatora URI, Dodaj **[FromUri]** atrybutu parametru.</span><span class="sxs-lookup"><span data-stu-id="df614-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="df614-123">W poniższym przykładzie zdefiniowano `GeoPoint` typu wraz z metody kontrolera, który pobiera `GeoPoint` z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="df614-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="df614-124">Klienta można ustawić wartości szerokości i długości geograficznej w ciągu zapytania i interfejsu API sieci Web będą z nich korzystać do utworzenia `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="df614-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="df614-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="df614-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="df614-126">Przy użyciu [FromBody]</span><span class="sxs-lookup"><span data-stu-id="df614-126">Using [FromBody]</span></span>

<span data-ttu-id="df614-127">Aby wymusić interfejsu API sieci Web do odczytu treści żądania typu prostego, Dodaj **[FromBody]** atrybutu parametru:</span><span class="sxs-lookup"><span data-stu-id="df614-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="df614-128">W tym przykładzie interfejsu API sieci Web użyje elementu formatującego typu nośnika do odczytania wartości *nazwa* z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="df614-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="df614-129">Oto przykład żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="df614-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="df614-130">Jeśli parametr ma [FromBody], interfejsu API sieci Web używa nagłówka Content-Type, aby wybrać element formatujący.</span><span class="sxs-lookup"><span data-stu-id="df614-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="df614-131">W tym przykładzie typ zawartości jest &quot;application/json&quot; i treść żądania jest nieprzetworzonego ciągu JSON (nie obiekt JSON).</span><span class="sxs-lookup"><span data-stu-id="df614-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="df614-132">Co najwyżej jeden parametr może odczytywać treść komunikatu.</span><span class="sxs-lookup"><span data-stu-id="df614-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="df614-133">Dlatego to nie będzie działać:</span><span class="sxs-lookup"><span data-stu-id="df614-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="df614-134">Przyczyną dla tej reguły jest, że treść żądania mogą być przechowywane w niebuforowanym strumienia, który może zostać odczytany tylko raz.</span><span class="sxs-lookup"><span data-stu-id="df614-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="df614-135">Konwertery typu</span><span class="sxs-lookup"><span data-stu-id="df614-135">Type Converters</span></span>

<span data-ttu-id="df614-136">Możesz wprowadzić traktowanie klasy jako typu prostego (dzięki czemu interfejsu API sieci Web próbuje powiązać go z identyfikatora URI) interfejsu API sieci Web, tworząc **TypeConverter** i Konwersja ciągu.</span><span class="sxs-lookup"><span data-stu-id="df614-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="df614-137">Poniższy kod przedstawia `GeoPoint` Klasa reprezentująca punkt geograficznych oraz **TypeConverter** konwertująca z ciągów do `GeoPoint` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="df614-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="df614-138">`GeoPoint` Zostanie nadany klasy **[TypeConverter]** Podaj konwerter typów dla atrybutu.</span><span class="sxs-lookup"><span data-stu-id="df614-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="df614-139">(W tym przykładzie został inspirowana przez wstrzymania Jan wpis w blogu [temat wiązania niestandardowe obiekty w podpisach akcji w MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="df614-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="df614-140">Teraz interfejsu API sieci Web będzie traktować `GeoPoint` jako typu prostego, czyli spróbuje powiązać `GeoPoint` parametry z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="df614-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="df614-141">Nie trzeba uwzględnić **[FromUri]** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="df614-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="df614-142">Klienta można wywołać metody o identyfikatorze URI w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="df614-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="df614-143">Integratorów modeli</span><span class="sxs-lookup"><span data-stu-id="df614-143">Model Binders</span></span>

<span data-ttu-id="df614-144">Opcja bardziej elastyczne niż konwertera typów jest utworzenie niestandardowego integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="df614-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="df614-145">Z integratora modelu Ty masz dostęp do elementów, jak żądania HTTP, opis akcji i wartości w wierszach z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="df614-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="df614-146">Aby utworzyć obiekt wiążący modelu, należy zaimplementować **interfejs IModelBinder** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="df614-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="df614-147">Ten interfejs definiuje jedną metodę **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="df614-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="df614-148">Oto integratora modelu dla `GeoPoint` obiektów.</span><span class="sxs-lookup"><span data-stu-id="df614-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="df614-149">Integrator modelu pobiera nieprzetworzone wartości wejściowych z *dostawcy wartości*.</span><span class="sxs-lookup"><span data-stu-id="df614-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="df614-150">W tym projekcie oddziela dwa różne funkcje:</span><span class="sxs-lookup"><span data-stu-id="df614-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="df614-151">Dostawca wartości przyjmuje żądania HTTP i wypełnia słownik par klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="df614-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="df614-152">Integrator modelu użyje tego słownika, aby wypełnić modelu.</span><span class="sxs-lookup"><span data-stu-id="df614-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="df614-153">Domyślny dostawca wartości w interfejsie API sieci Web pobiera wartości z danych trasy i ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="df614-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="df614-154">Na przykład, jeśli identyfikator URI jest `http://localhost/api/values/1?location=48,-122`, dostawca wartości tworzy następujące pary klucz wartość:</span><span class="sxs-lookup"><span data-stu-id="df614-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="df614-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="df614-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="df614-156">Lokalizacja = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="df614-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="df614-157">(I używam zakładając, że szablon trasy domyślne, czyli &quot;interfejsu api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="df614-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="df614-158">Nazwa parametru do powiązania jest przechowywana w **ModelBindingContext.ModelName** właściwości.</span><span class="sxs-lookup"><span data-stu-id="df614-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="df614-159">Wyszukuje integratora modelu dla klucza z tej wartości w słowniku.</span><span class="sxs-lookup"><span data-stu-id="df614-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="df614-160">Jeśli wartość istnieje i mogą być konwertowane na `GeoPoint`, integratora modelu przypisuje wartości powiązanej do **ModelBindingContext.Model** właściwości.</span><span class="sxs-lookup"><span data-stu-id="df614-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="df614-161">Zwróć uwagę, że integratora modelu nie jest ograniczona do konwersji typu prostego.</span><span class="sxs-lookup"><span data-stu-id="df614-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="df614-162">W tym przykładzie integratora modelu najpierw wyszukiwana w tabeli znanej lokalizacji, a w przypadku niepowodzenia używa konwersji typu.</span><span class="sxs-lookup"><span data-stu-id="df614-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="df614-163">**Ustawienie integratora modelu**</span><span class="sxs-lookup"><span data-stu-id="df614-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="df614-164">Istnieje kilka sposobów, aby ustawić integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="df614-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="df614-165">Po pierwsze, można dodać **[ModelBinder]** atrybutu parametru.</span><span class="sxs-lookup"><span data-stu-id="df614-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="df614-166">Można również dodać **[ModelBinder]** atrybutu typu.</span><span class="sxs-lookup"><span data-stu-id="df614-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="df614-167">Interfejs API sieci Web użyje integratora modelu określonego dla wszystkich parametrów typu.</span><span class="sxs-lookup"><span data-stu-id="df614-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="df614-168">Na koniec można dodać dostawcę integratora modelu do **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="df614-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="df614-169">Dostawca integratora modelu to po prostu klasę fabryki, która tworzy integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="df614-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="df614-170">Można utworzyć dostawcę pochodny [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="df614-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="df614-171">Jednak jeśli Twoje integratora modelu obsługuje jednego typu, łatwiej jest użyć wbudowanych **SimpleModelBinderProvider**, które jest przeznaczone do tego celu.</span><span class="sxs-lookup"><span data-stu-id="df614-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="df614-172">Poniższy kod przedstawia, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="df614-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="df614-173">Z dostawcy wiązania modelu, nadal konieczne jest dodanie **[ModelBinder]** atrybutu do parametru powiedzieć interfejsu API sieci Web, aby go używać integratora modelu, a nie element formatujący typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="df614-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="df614-174">Jednak obecnie nie trzeba określić typ integratora modelu w atrybucie:</span><span class="sxs-lookup"><span data-stu-id="df614-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="df614-175">Dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="df614-175">Value Providers</span></span>

<span data-ttu-id="df614-176">Wymienione I że integratora modelu pobiera wartości z dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="df614-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="df614-177">Aby napisać dostawcę wartości niestandardowych, zaimplementuj **IValueProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="df614-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="df614-178">Oto przykład pobierający wartości z plików cookie w żądaniu:</span><span class="sxs-lookup"><span data-stu-id="df614-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="df614-179">Należy także utworzyć fabryki dostawców wartości przez wynikających z **ValueProviderFactory** klasy.</span><span class="sxs-lookup"><span data-stu-id="df614-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="df614-180">Fabryki dostawców wartości, aby dodać **HttpConfiguration** w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="df614-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="df614-181">Interfejs API sieci Web Redaguj wszystkich dostawców wartości, więc jeśli wywołuje integratora modelu **ValueProvider.GetValue**, integratora modelu otrzymuje wartość z pierwszego dostawcy wartości, który może go wygenerować.</span><span class="sxs-lookup"><span data-stu-id="df614-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="df614-182">Alternatywnie można ustawić fabryki dostawców wartości na poziomie parametr przy użyciu **ValueProvider** atrybutu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="df614-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="df614-183">Ta wartość informuje interfejsu API sieci Web do używania wiązania modelu z fabryki dostawców wartości określonej i nie można użyć dowolnego dostawców zarejestrowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="df614-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="df614-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="df614-184">HttpParameterBinding</span></span>

<span data-ttu-id="df614-185">Integratorów modeli są określone wystąpienie mechanizmu bardziej ogólne.</span><span class="sxs-lookup"><span data-stu-id="df614-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="df614-186">Jeśli przyjrzymy się **[ModelBinder]** atrybutu, zobaczysz, że pochodzi od klasy abstrakcyjnej **ParameterBindingAttribute** klasy.</span><span class="sxs-lookup"><span data-stu-id="df614-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="df614-187">Ta klasa definiuje jedną metodę **GetBinding**, która zwraca **HttpParameterBinding** obiektu:</span><span class="sxs-lookup"><span data-stu-id="df614-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="df614-188">**HttpParameterBinding** jest odpowiedzialny za wiązanie parametru na wartość.</span><span class="sxs-lookup"><span data-stu-id="df614-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="df614-189">W przypadku programu **[ModelBinder]**, zwraca ten atrybut **HttpParameterBinding** implementację, która używa **interfejs IModelBinder** przeprowadzić rzeczywistego powiązania.</span><span class="sxs-lookup"><span data-stu-id="df614-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="df614-190">Można też wdrożyć własne **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="df614-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="df614-191">Na przykład, załóżmy, że chcesz pobrać elementy etag z `if-match` i `if-none-match` nagłówków w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="df614-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="df614-192">Zaczniemy, definiując klasę do reprezentowania elementy ETag.</span><span class="sxs-lookup"><span data-stu-id="df614-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="df614-193">Zdefiniujemy również wyliczenia wskazująca, czy można uzyskać ETag z `if-match` nagłówka lub `if-none-match` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="df614-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="df614-194">Oto **HttpParameterBinding** czy pobiera tagu ETag z żądaną nagłówka i wiąże go z parametrem typu ETag:</span><span class="sxs-lookup"><span data-stu-id="df614-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="df614-195">**ExecuteBindingAsync** metoda wykonuje wiązanie.</span><span class="sxs-lookup"><span data-stu-id="df614-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="df614-196">W ramach tej metody, Dodaj wartość parametru powiązania **ActionArgument** słownika **element HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="df614-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="df614-197">Jeśli Twoje **ExecuteBindingAsync** metoda odczytuje treść komunikatu żądania, Zastąp **WillReadBody** właściwości, aby zwracała wartość true.</span><span class="sxs-lookup"><span data-stu-id="df614-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="df614-198">Treść żądania może być Niebuforowane strumienia, które mogą być odczytywane tylko raz, więc interfejsu API sieci Web wymusza reguły tego co najwyżej jeden powiązanie można odczytać treści wiadomości.</span><span class="sxs-lookup"><span data-stu-id="df614-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="df614-199">Aby zastosować niestandardowy **HttpParameterBinding**, można zdefiniować atrybut, który jest pochodną **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="df614-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="df614-200">Aby uzyskać `ETagParameterBinding`, zdefiniujemy dwa atrybuty, jeden dla `if-match` nagłówki i jeden dla `if-none-match` nagłówków.</span><span class="sxs-lookup"><span data-stu-id="df614-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="df614-201">Abstrakcyjna klasa podstawowa zarówno pochodną.</span><span class="sxs-lookup"><span data-stu-id="df614-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="df614-202">Oto metoda kontrolera, który używa `[IfNoneMatch]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="df614-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="df614-203">Oprócz **ParameterBindingAttribute**, istnieje inny haku dodawania niestandardowego **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="df614-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="df614-204">Na **HttpConfiguration** obiektu **ParameterBindingRules** właściwości to zbiór funkcji anomymous typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="df614-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="df614-205">Na przykład można dodać regułę, która korzysta z żadnych parametrów ETag dla metody GET `ETagParameterBinding` z `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="df614-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="df614-206">Funkcja powinna zwrócić `null` dla parametrów, których powiązanie nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="df614-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="df614-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="df614-207">IActionValueBinder</span></span>

<span data-ttu-id="df614-208">Cały proces wiązanie parametru jest kontrolowany przez usługę podłączania, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="df614-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="df614-209">Domyślna implementacja **IActionValueBinder** wykonuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="df614-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="df614-210">Wyszukaj **ParameterBindingAttribute** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="df614-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="df614-211">Obejmuje to **[FromBody]**, **[FromUri]**, i **[ModelBinder]**, lub atrybutów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="df614-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="df614-212">W przeciwnym razie Szukaj w **HttpConfiguration.ParameterBindingRules** dla funkcji, która zwraca wartość inną niż null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="df614-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="df614-213">W przeciwnym razie Użyj domyślnych reguł, które I opisane wcześniej.</span><span class="sxs-lookup"><span data-stu-id="df614-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="df614-214">Jeśli typ parametru jest "prosta" lub ma konwertera typów, należy powiązać z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="df614-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="df614-215">Jest to równoważne umieszczanie **[FromUri]** atrybutu w parametrze.</span><span class="sxs-lookup"><span data-stu-id="df614-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="df614-216">W przeciwnym razie spróbuj odczytywać parametr treści wiadomości.</span><span class="sxs-lookup"><span data-stu-id="df614-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="df614-217">Jest to równoważne umieszczanie **[FromBody]** w parametrze.</span><span class="sxs-lookup"><span data-stu-id="df614-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="df614-218">Jeśli potrzebujesz, można zastąpić całą **IActionValueBinder** usługi z implementacją niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="df614-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df614-219">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="df614-219">Additional Resources</span></span>

[<span data-ttu-id="df614-220">Przykładowe powiązanie parametru niestandardowego</span><span class="sxs-lookup"><span data-stu-id="df614-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="df614-221">Jan wstrzymania zapisano szereg dobrej wpisy na blogu dotyczące wiązanie parametru interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="df614-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="df614-222">Jak wiązanie parametru przez interfejs API sieci Web</span><span class="sxs-lookup"><span data-stu-id="df614-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="df614-223">Styl MVC wiązanie parametru dla interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="df614-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="df614-224">Jak chcesz powiązać niestandardowych obiektów w podpisach akcji w interfejsie API MVC/sieci Web</span><span class="sxs-lookup"><span data-stu-id="df614-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="df614-225">Jak utworzyć dostawcę wartości niestandardowe w interfejsie API sieci Web</span><span class="sxs-lookup"><span data-stu-id="df614-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="df614-226">Wiązanie parametru interfejsu API sieci Web kulisy</span><span class="sxs-lookup"><span data-stu-id="df614-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
