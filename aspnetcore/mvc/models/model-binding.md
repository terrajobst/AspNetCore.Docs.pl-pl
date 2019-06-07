---
title: Wiązanie modelu w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak działa powiązanie modelu w programie ASP.NET Core oraz dostosować jego zachowanie.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 7d62ccecdacbd34a38a1fd8c58979a9b09cf86e8
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750197"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="66a80-103">Wiązanie modelu w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66a80-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="66a80-104">W tym artykule opisano, jakie wiązanie modelu przebiegło, jak działa i jak dostosować jego zachowanie.</span><span class="sxs-lookup"><span data-stu-id="66a80-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="66a80-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="66a80-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="66a80-106">Co to jest wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="66a80-106">What is Model binding</span></span>

<span data-ttu-id="66a80-107">Kontrolery i stron Razor pracować z danymi, które pochodzą z żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="66a80-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="66a80-108">Na przykład dane trasy może dostarczyć klucza rekordu, a pola przesłanego formularza może podać wartości dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="66a80-109">Pisanie kodu w celu pobrania każdej z tych wartości i konwertować z ciągów na typy .NET będzie uciążliwe i podatne na błędy.</span><span class="sxs-lookup"><span data-stu-id="66a80-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="66a80-110">Wiązanie modelu pozwala zautomatyzować ten proces.</span><span class="sxs-lookup"><span data-stu-id="66a80-110">Model binding automates this process.</span></span> <span data-ttu-id="66a80-111">System powiązań modelu:</span><span class="sxs-lookup"><span data-stu-id="66a80-111">The model binding system:</span></span>

* <span data-ttu-id="66a80-112">Pobiera dane z różnych źródeł, takie jak przekierowywanie danych, pola formularza i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="66a80-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="66a80-113">Udostępnia dane do kontrolerów i stronami Razor w parametrów metod i właściwości publiczne.</span><span class="sxs-lookup"><span data-stu-id="66a80-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="66a80-114">Konwertuje ciąg danych do typów .NET.</span><span class="sxs-lookup"><span data-stu-id="66a80-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="66a80-115">Aktualizuje właściwości typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="66a80-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="66a80-116">Przykład</span><span class="sxs-lookup"><span data-stu-id="66a80-116">Example</span></span>

<span data-ttu-id="66a80-117">Załóżmy, że masz następujące metody akcji:</span><span class="sxs-lookup"><span data-stu-id="66a80-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="66a80-118">I aplikacja odbiera żądanie z tym adresem URL:</span><span class="sxs-lookup"><span data-stu-id="66a80-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="66a80-119">Wiązanie modelu wykracza jednak poniższe kroki, po system routingu wybiera metody akcji:</span><span class="sxs-lookup"><span data-stu-id="66a80-119">Model binding goes though the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="66a80-120">Wyszukuje pierwszy parametr `GetByID`, liczbą całkowitą o nazwie `id`.</span><span class="sxs-lookup"><span data-stu-id="66a80-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="66a80-121">Przegląda dostępne źródła w żądaniu HTTP i wyszukuje `id` = "2" w danych trasy.</span><span class="sxs-lookup"><span data-stu-id="66a80-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="66a80-122">Konwertuje ciąg "2" do liczby całkowitej 2.</span><span class="sxs-lookup"><span data-stu-id="66a80-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="66a80-123">Wyszukuje następny parametr `GetByID`, wartość logiczna o nazwie `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="66a80-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="66a80-124">Przegląda źródeł i umożliwia znalezienie "DogsOnly = true" w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="66a80-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="66a80-125">Dopasowywanie nazw nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="66a80-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="66a80-126">Konwertuje ciąg "true" na wartość logiczną `true`.</span><span class="sxs-lookup"><span data-stu-id="66a80-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="66a80-127">Struktura następnie wywołuje `GetById` jest metoda w wersji 2 dla `id` parametru i `true` dla `dogsOnly` parametru.</span><span class="sxs-lookup"><span data-stu-id="66a80-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="66a80-128">W powyższym przykładzie cele powiązanie modelu to parametry metody, które są typy proste.</span><span class="sxs-lookup"><span data-stu-id="66a80-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="66a80-129">Obiekty docelowe mogą być również właściwości typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="66a80-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="66a80-130">Po każdej właściwości pomyślnie jest związany, [Walidacja modelu](xref:mvc/models/validation) występuje dla tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="66a80-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="66a80-131">Rekord danych jest powiązany z modelu, a wszelkie błędy powiązania lub sprawdzania poprawności są przechowywane w [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) lub [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="66a80-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="66a80-132">Aby dowiedzieć się, jeśli ten proces zakończył się pomyślnie, aplikacja sprawdza, czy [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flagi.</span><span class="sxs-lookup"><span data-stu-id="66a80-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="66a80-133">Obiekty docelowe</span><span class="sxs-lookup"><span data-stu-id="66a80-133">Targets</span></span>

<span data-ttu-id="66a80-134">Wiązanie modelu próbuje znaleźć wartości dla następujących rodzajów elementów docelowych:</span><span class="sxs-lookup"><span data-stu-id="66a80-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="66a80-135">Parametry metody akcji kontrolera, że żądanie jest kierowane do.</span><span class="sxs-lookup"><span data-stu-id="66a80-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="66a80-136">Parametry metody obsługi stron Razor, który żądanie jest kierowane do.</span><span class="sxs-lookup"><span data-stu-id="66a80-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="66a80-137">Właściwości publiczne, kontrolera lub `PageModel` klasy, jeśli określony przez atrybuty.</span><span class="sxs-lookup"><span data-stu-id="66a80-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="66a80-138">Atrybut [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="66a80-138">[BindProperty] attribute</span></span>

<span data-ttu-id="66a80-139">Można zastosować do właściwości publicznej kontrolera lub `PageModel` klasy, aby spowodować, że wiązanie modelu pod kątem tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="66a80-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="66a80-140">Atrybut [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="66a80-140">[BindProperties] attribute</span></span>

<span data-ttu-id="66a80-141">Dostępne w programie ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="66a80-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="66a80-142">Mogą być stosowane do kontrolera lub `PageModel` klasy, aby poinformować wiązania modelu pod kątem wszystkie publiczne właściwości klasy:</span><span class="sxs-lookup"><span data-stu-id="66a80-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="66a80-143">Wiązanie dla żądania HTTP GET modelu</span><span class="sxs-lookup"><span data-stu-id="66a80-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="66a80-144">Domyślnie właściwości nie są związane na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="66a80-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="66a80-145">Zazwyczaj jest wszystko, czego potrzebujesz do żądania GET rekordów parametru Identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="66a80-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="66a80-146">Identyfikator rekordu jest używany do wyszukiwania elementów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="66a80-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="66a80-147">W związku z tym nie ma potrzeby do powiązania właściwość, która posiada wystąpienie modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="66a80-148">W scenariuszach, którego właściwości powiązane z danymi z żądania GET, należy ustawić `SupportsGet` właściwości `true`:</span><span class="sxs-lookup"><span data-stu-id="66a80-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="66a80-149">Źródła</span><span class="sxs-lookup"><span data-stu-id="66a80-149">Sources</span></span>

<span data-ttu-id="66a80-150">Domyślnie powiązanie modelu pobiera dane w postaci par klucz wartość z następujących źródeł w żądaniu HTTP:</span><span class="sxs-lookup"><span data-stu-id="66a80-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="66a80-151">Pola formularza</span><span class="sxs-lookup"><span data-stu-id="66a80-151">Form fields</span></span> 
1. <span data-ttu-id="66a80-152">Treść żądania (dla [kontrolery, które mają atrybut [klasy ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="66a80-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="66a80-153">Dane trasy</span><span class="sxs-lookup"><span data-stu-id="66a80-153">Route data</span></span>
1. <span data-ttu-id="66a80-154">Parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="66a80-154">Query string parameters</span></span>
1. <span data-ttu-id="66a80-155">Przekazane pliki</span><span class="sxs-lookup"><span data-stu-id="66a80-155">Uploaded files</span></span> 

<span data-ttu-id="66a80-156">Dla każdego parametru target lub właściwość źródła są skanowane w kolejności wskazanej na tej liście.</span><span class="sxs-lookup"><span data-stu-id="66a80-156">For each target parameter or property, the sources are scanned in the order indicated in this list.</span></span> <span data-ttu-id="66a80-157">Istnieje kilka wyjątków:</span><span class="sxs-lookup"><span data-stu-id="66a80-157">There are a few exceptions:</span></span>

* <span data-ttu-id="66a80-158">Trasy, danych i zapytań, wartościami ciągów są używane tylko dla typów prostych.</span><span class="sxs-lookup"><span data-stu-id="66a80-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="66a80-159">Przekazane pliki są powiązane tylko do typów docelowych, które implementują `IFormFile` lub `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="66a80-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="66a80-160">Jeśli domyślne zachowanie nie zapewnia odpowiednich wyników, można użyć jednej z następujących atrybutów, aby określić źródło do użycia w dowolnym danym obiektem docelowym.</span><span class="sxs-lookup"><span data-stu-id="66a80-160">If the default behavior doesn't give the right results, you can use one of the following attributes to specify the source to use for any given target.</span></span> 

* <span data-ttu-id="66a80-161">[[FromQuery] ](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) — Pobiera wartości z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="66a80-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="66a80-162">[[FromRoute] ](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) — Pobiera wartości z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="66a80-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="66a80-163">[[FromForm] ](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) — Pobiera wartości z pól przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="66a80-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="66a80-164">[[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) — Pobiera wartości z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="66a80-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="66a80-165">[[FromHeader] ](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) — Pobiera wartości z nagłówków HTTP.</span><span class="sxs-lookup"><span data-stu-id="66a80-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="66a80-166">Te atrybuty:</span><span class="sxs-lookup"><span data-stu-id="66a80-166">These attributes:</span></span>

* <span data-ttu-id="66a80-167">Zostaną dodane do właściwości modelu indywidualnie (nie do klasy modelu), jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="66a80-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="66a80-168">Opcjonalnie akceptuje wartości Nazwa modelu w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="66a80-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="66a80-169">Ta opcja znajduje się w przypadku, gdy nazwa właściwości nie jest zgodna wartość w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="66a80-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="66a80-170">Na przykład wartość w żądaniu może być nagłówka z łącznikiem w nazwie, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="66a80-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="66a80-171">Atrybut [FromBody]</span><span class="sxs-lookup"><span data-stu-id="66a80-171">[FromBody] attribute</span></span>

<span data-ttu-id="66a80-172">Dane treści żądania jest analizowany przy użyciu danych wejściowych elementy formatujące, które są specyficzne dla typu zawartości żądania.</span><span class="sxs-lookup"><span data-stu-id="66a80-172">The request body data is parsed by using input formatters specific to the content type of the request.</span></span> <span data-ttu-id="66a80-173">Opisano elementy formatujące danych wejściowych [w dalszej części tego artykułu](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="66a80-173">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="66a80-174">Nie stosuj `[FromBody]` do więcej niż jeden parametr, dla każdej metody akcji.</span><span class="sxs-lookup"><span data-stu-id="66a80-174">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="66a80-175">Środowisko uruchomieniowe programu ASP.NET Core deleguje odpowiedzialność odczytywania strumienia żądania do wejściowego elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="66a80-175">The ASP.NET Core runtime delegates the responsibility of reading the request stream to the input formatter.</span></span> <span data-ttu-id="66a80-176">Gdy strumienia żądania jest do odczytu, nie jest już dostępny do odczytu ponownie dla powiązania innych `[FromBody]` parametrów.</span><span class="sxs-lookup"><span data-stu-id="66a80-176">Once the request stream is read, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="66a80-177">Dodatkowe źródła</span><span class="sxs-lookup"><span data-stu-id="66a80-177">Additional sources</span></span>

<span data-ttu-id="66a80-178">Źródło danych znajduje się w systemie wiązania modelu przez *wartość dostawców*.</span><span class="sxs-lookup"><span data-stu-id="66a80-178">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="66a80-179">Można napisać i zarejestrować dostawców wartości niestandardowych, które pobierają dane do wiązania modelu z innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="66a80-179">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="66a80-180">Na przykład możesz zechcieć danych z plików cookie lub stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="66a80-180">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="66a80-181">Aby uzyskać dane z nowego źródła:</span><span class="sxs-lookup"><span data-stu-id="66a80-181">To get data from a new source:</span></span>

* <span data-ttu-id="66a80-182">Utwórz klasę, która implementuje `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="66a80-182">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="66a80-183">Utwórz klasę, która implementuje `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="66a80-183">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="66a80-184">Rejestrowanie klasy fabryki w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="66a80-184">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="66a80-185">Przykładowa aplikacja zawiera [dostawcy wartości](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) i [fabryki](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) przykładu, który pobiera wartości z plików cookie.</span><span class="sxs-lookup"><span data-stu-id="66a80-185">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="66a80-186">Oto kod rejestracyjny w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="66a80-186">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="66a80-187">Kod przedstawiony umieszcza dostawcy wartości niestandardowej po wszystkich dostawców wartości wbudowanej.</span><span class="sxs-lookup"><span data-stu-id="66a80-187">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="66a80-188">Aby stał się pierwszy na liście wywołań `Insert(0, new CookieValueProviderFactory())` zamiast `Add`.</span><span class="sxs-lookup"><span data-stu-id="66a80-188">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="66a80-189">Nie źródła dla właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="66a80-189">No source for a model property</span></span>

<span data-ttu-id="66a80-190">Domyślnie błąd stanu modelu nie jest tworzone, jeśli wartość nie zostanie znaleziony dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-190">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="66a80-191">Dla właściwości ustawiono na wartość null lub wartość domyślna:</span><span class="sxs-lookup"><span data-stu-id="66a80-191">The property is set to null or a default value:</span></span>

* <span data-ttu-id="66a80-192">Typy dopuszczające wartości zerowe proste są ustawione na `null`.</span><span class="sxs-lookup"><span data-stu-id="66a80-192">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="66a80-193">Typy nieprzyjmujące wartości są ustawione na `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="66a80-193">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="66a80-194">Na przykład parametr `int id` jest równa 0.</span><span class="sxs-lookup"><span data-stu-id="66a80-194">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="66a80-195">W przypadku typów złożonych wiązania modelu tworzy wystąpienie przy użyciu domyślnego konstruktora bez konieczności ustawiania właściwości.</span><span class="sxs-lookup"><span data-stu-id="66a80-195">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="66a80-196">Tablice są ustawione na `Array.Empty<T>()`, chyba że `byte[]` tablice są ustawione na `null`.</span><span class="sxs-lookup"><span data-stu-id="66a80-196">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="66a80-197">Jeśli stan modelu należy unieważniony, jeśli nic nie zostanie znaleziony w pola formularza dla właściwości modelu, należy użyć [atrybutu [BindRequired]](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="66a80-197">If model state should be invalidated when nothing is found in form fields for a model property, use the [[BindRequired] attribute](#bindrequired-attribute).</span></span>

<span data-ttu-id="66a80-198">Uwaga że `[BindRequired]` zachowanie odnosi się do wiązania modelu z danych przesłanego formularza, a nie dane JSON lub XML w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="66a80-198">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="66a80-199">Dane treści żądania jest obsługiwany przez [wejściowych elementy formatujące](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="66a80-199">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="66a80-200">Błędy konwersji typu</span><span class="sxs-lookup"><span data-stu-id="66a80-200">Type conversion errors</span></span>

<span data-ttu-id="66a80-201">Jeśli źródło zostanie znaleziony, ale nie można przekonwertować na typ docelowy, stan modelu został oznaczony jako nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="66a80-201">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="66a80-202">Jako parametru target lub właściwości jest równa null lub wartość domyślną, jak wspomniano w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="66a80-202">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="66a80-203">W kontrolerze interfejsu API, który ma `[ApiController]` atrybutu nieprawidłowy model stanu powoduje automatyczne odpowiedzi HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="66a80-203">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="66a80-204">Na stronie Razor ponowne wyświetlenie strony zawierającej komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="66a80-204">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="66a80-205">Weryfikacja po stronie klienta przechwytuje większość złe dane, które w przeciwnym razie będzie można przesłać formularza stron Razor.</span><span class="sxs-lookup"><span data-stu-id="66a80-205">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="66a80-206">Tej weryfikacji sprawia, że trudno wyzwolić poprzedniego wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="66a80-206">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="66a80-207">Przykładowa aplikacja zawiera **Prześlij z nieprawidłową datę** przycisku, który umieszcza złe dane w **Data zatrudnienia** pola, a następnie przesyła formularz.</span><span class="sxs-lookup"><span data-stu-id="66a80-207">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="66a80-208">Ten przycisk, przedstawiono sposób wyświetlania strony w kodzie po wystąpieniu błędów konwersji danych.</span><span class="sxs-lookup"><span data-stu-id="66a80-208">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="66a80-209">Po stronie zostanie wyświetlony ponownie, poprzedni kod, nieprawidłowe dane wejściowe nie jest wyświetlany w polu formularza.</span><span class="sxs-lookup"><span data-stu-id="66a80-209">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="66a80-210">Jest to spowodowane właściwość modelu została ustawiona na wartość null lub wartość domyślną.</span><span class="sxs-lookup"><span data-stu-id="66a80-210">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="66a80-211">Nieprawidłowe dane wejściowe są wyświetlane w komunikacie o błędzie.</span><span class="sxs-lookup"><span data-stu-id="66a80-211">The invalid input does appear in an error message.</span></span> <span data-ttu-id="66a80-212">Ale jeśli chcesz ponownie wyświetlić złe dane w polu formularza, należy wziąć pod uwagę co właściwość modelu ciągu oraz wykonywania konwersji danych ręcznie.</span><span class="sxs-lookup"><span data-stu-id="66a80-212">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="66a80-213">Tej samej strategii jest zalecane, jeśli nie chcesz, aby błędy konwersji typu, aby spowodować błędy stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-213">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="66a80-214">Ciąg w takiej sytuacji należy właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-214">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="66a80-215">Typy proste</span><span class="sxs-lookup"><span data-stu-id="66a80-215">Simple types</span></span>

<span data-ttu-id="66a80-216">Proste typy, które integratora modelu można przekonwertować ciągi źródłowe do są następujące:</span><span class="sxs-lookup"><span data-stu-id="66a80-216">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="66a80-217">Boolean</span><span class="sxs-lookup"><span data-stu-id="66a80-217">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="66a80-218">[Bajt](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="66a80-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="66a80-219">Char</span><span class="sxs-lookup"><span data-stu-id="66a80-219">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="66a80-220">DateTime</span><span class="sxs-lookup"><span data-stu-id="66a80-220">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="66a80-221">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="66a80-221">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="66a80-222">Decimal</span><span class="sxs-lookup"><span data-stu-id="66a80-222">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="66a80-223">Double</span><span class="sxs-lookup"><span data-stu-id="66a80-223">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="66a80-224">Enum</span><span class="sxs-lookup"><span data-stu-id="66a80-224">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="66a80-225">Identyfikator GUID</span><span class="sxs-lookup"><span data-stu-id="66a80-225">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="66a80-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="66a80-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="66a80-227">Single</span><span class="sxs-lookup"><span data-stu-id="66a80-227">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="66a80-228">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="66a80-228">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="66a80-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="66a80-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="66a80-230">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="66a80-230">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="66a80-231">Wersja</span><span class="sxs-lookup"><span data-stu-id="66a80-231">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="66a80-232">Typy złożone</span><span class="sxs-lookup"><span data-stu-id="66a80-232">Complex types</span></span>

<span data-ttu-id="66a80-233">Typ złożony musi mieć publicznego konstruktora domyślnego i publicznego modyfikowalne właściwości do powiązania.</span><span class="sxs-lookup"><span data-stu-id="66a80-233">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="66a80-234">W przypadku tworzenia powiązania modelu tworzenia wystąpienia klasy przy użyciu publicznego konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="66a80-234">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="66a80-235">Dla każdej właściwości typu złożonego wiązania modelu przegląda źródeł dla wzorca nazwy *prefix.property_name*.</span><span class="sxs-lookup"><span data-stu-id="66a80-235">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="66a80-236">Jeśli nic nie zostanie znaleziony, szuka po prostu *property_name* bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="66a80-236">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="66a80-237">Powiązanie parametru, prefiks jest nazwą parametru.</span><span class="sxs-lookup"><span data-stu-id="66a80-237">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="66a80-238">Powiązanie `PageModel` właściwość publiczną, prefiks jest nazwą właściwości publicznej.</span><span class="sxs-lookup"><span data-stu-id="66a80-238">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="66a80-239">Niektóre atrybuty mają `Prefix` właściwość, która umożliwia zastąpienie użycie domyślnego parametru lub nazwę właściwości.</span><span class="sxs-lookup"><span data-stu-id="66a80-239">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="66a80-240">Załóżmy, że typ złożony jest następująca `Instructor` klasy:</span><span class="sxs-lookup"><span data-stu-id="66a80-240">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="66a80-241">Prefiks = Nazwa parametru</span><span class="sxs-lookup"><span data-stu-id="66a80-241">Prefix = parameter name</span></span>

<span data-ttu-id="66a80-242">Czy model, który ma zostać powiązany parametr o nazwie `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="66a80-242">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="66a80-243">Model rozpoczyna się wiązanie przeglądając źródeł dla klucza `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="66a80-243">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="66a80-244">Jeśli, nie zostanie znaleziona, szuka `ID` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="66a80-244">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="66a80-245">Prefiks = nazwa właściwości</span><span class="sxs-lookup"><span data-stu-id="66a80-245">Prefix = property name</span></span>

<span data-ttu-id="66a80-246">Jeśli model, który ma zostać powiązany jest właściwość o nazwie `Instructor` kontrolera lub `PageModel` klasy:</span><span class="sxs-lookup"><span data-stu-id="66a80-246">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="66a80-247">Model rozpoczyna się wiązanie przeglądając źródeł dla klucza `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="66a80-247">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="66a80-248">Jeśli, nie zostanie znaleziona, szuka `ID` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="66a80-248">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="66a80-249">Prefiks niestandardowy</span><span class="sxs-lookup"><span data-stu-id="66a80-249">Custom prefix</span></span>

<span data-ttu-id="66a80-250">Czy model, który ma zostać powiązany parametr o nazwie `instructorToUpdate` i `Bind` Określa atrybut `Instructor` jako prefiksu:</span><span class="sxs-lookup"><span data-stu-id="66a80-250">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="66a80-251">Model rozpoczyna się wiązanie przeglądając źródeł dla klucza `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="66a80-251">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="66a80-252">Jeśli, nie zostanie znaleziona, szuka `ID` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="66a80-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="66a80-253">Atrybuty dla celów typu złożonego</span><span class="sxs-lookup"><span data-stu-id="66a80-253">Attributes for complex type targets</span></span>

<span data-ttu-id="66a80-254">Kilka wbudowanych atrybutów są dostępne do kontrolowania wiązania modelu złożonych typów:</span><span class="sxs-lookup"><span data-stu-id="66a80-254">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="66a80-255">Te atrybuty mają wpływ na model powiązań gdy opublikowane dane formularza źródła wartości.</span><span class="sxs-lookup"><span data-stu-id="66a80-255">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="66a80-256">Nie wpływają na elementy formatujące wejściowego, który proces opublikowane treść żądań JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="66a80-256">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="66a80-257">Opisano elementy formatujące danych wejściowych [w dalszej części tego artykułu](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="66a80-257">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="66a80-258">Zawiera również omówienie `[Required]` atrybutu w [Walidacja modelu](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="66a80-258">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="66a80-259">Atrybut [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="66a80-259">[BindRequired] attribute</span></span>

<span data-ttu-id="66a80-260">Można zastosować tylko do właściwości modelu, aby nie parametry metody.</span><span class="sxs-lookup"><span data-stu-id="66a80-260">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="66a80-261">Powoduje, że model można dodać błąd stanu modelu, w przypadku powiązania nie można powiązać właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-261">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="66a80-262">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="66a80-262">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="66a80-263">Atrybut [BindNever]</span><span class="sxs-lookup"><span data-stu-id="66a80-263">[BindNever] attribute</span></span>

<span data-ttu-id="66a80-264">Można zastosować tylko do właściwości modelu, aby nie parametry metody.</span><span class="sxs-lookup"><span data-stu-id="66a80-264">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="66a80-265">Zapobiega wiązanie modelu z ustawienie dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-265">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="66a80-266">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="66a80-266">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="66a80-267">Atrybut [powiązania]</span><span class="sxs-lookup"><span data-stu-id="66a80-267">[Bind] attribute</span></span>

<span data-ttu-id="66a80-268">Mogą być stosowane do klasy lub parametru metody.</span><span class="sxs-lookup"><span data-stu-id="66a80-268">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="66a80-269">Określa właściwości modelu, które powinny zostać uwzględnione w wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-269">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="66a80-270">W poniższym przykładzie, tylko określonej właściwości elementu `Instructor` modelu są powiązane, po wywołaniu dowolnej procedury obsługi lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="66a80-270">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="66a80-271">W poniższym przykładzie, tylko określonej właściwości elementu `Instructor` modelu są powiązane po `OnPost` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="66a80-271">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="66a80-272">`[Bind]` Atrybut może służyć do ochrony przed overposting w *tworzenie* scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="66a80-272">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="66a80-273">Nie działa dobrze w scenariuszach edycji wykluczone właściwości są ustawione na wartość null lub wartość domyślną, zamiast pozostał niezmieniony.</span><span class="sxs-lookup"><span data-stu-id="66a80-273">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="66a80-274">Do obrony przed overposting, zaleca się wyświetlanie modeli zamiast `[Bind]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="66a80-274">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="66a80-275">Aby uzyskać więcej informacji, zobacz [Uwaga dotycząca zabezpieczeń o overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="66a80-275">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="66a80-276">Kolekcje</span><span class="sxs-lookup"><span data-stu-id="66a80-276">Collections</span></span>

<span data-ttu-id="66a80-277">Dla celów, które są kolekcjami typów prostych, wiązanie modelu szuka dopasowań, który ma *parameter_name* lub *property_name*.</span><span class="sxs-lookup"><span data-stu-id="66a80-277">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="66a80-278">Jeśli nie zostanie znalezione dopasowanie, szuka w jednym z obsługiwanych formatów, bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="66a80-278">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="66a80-279">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="66a80-279">For example:</span></span>

* <span data-ttu-id="66a80-280">Załóżmy, że można powiązać parametr jest tablicą o nazwie `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="66a80-280">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="66a80-281">Dane ciągu formularza lub zapytanie może być w jednym z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="66a80-281">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="66a80-282">Następujący format jest obsługiwany tylko w formie danych:</span><span class="sxs-lookup"><span data-stu-id="66a80-282">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="66a80-283">Dla wszystkich poprzedni przykład formatuje wiązania modelu przekazuje tablicę dwa elementy, które `selectedCourses` parametru:</span><span class="sxs-lookup"><span data-stu-id="66a80-283">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="66a80-284">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="66a80-284">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="66a80-285">selectedCourses [1] = 2000</span><span class="sxs-lookup"><span data-stu-id="66a80-285">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="66a80-286">Dane formatuje tego użycie indeksu dolnego liczby (...) [0]... [1]...) należy się upewnić, że ich są numerowane kolejno od zera.</span><span class="sxs-lookup"><span data-stu-id="66a80-286">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="66a80-287">Jeśli ma żadnych przerw w numeracji indeksu dolnego, wszystkie elementy po przerwa zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="66a80-287">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="66a80-288">Na przykład jeśli indeksy dolne 0 i 2 zamiast 0 i 1, drugi element jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="66a80-288">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="66a80-289">słowniki</span><span class="sxs-lookup"><span data-stu-id="66a80-289">Dictionaries</span></span>

<span data-ttu-id="66a80-290">Aby uzyskać `Dictionary` lokalizacjach docelowych oraz wiązania modelu szuka dopasowań, który ma *parameter_name* lub *property_name*.</span><span class="sxs-lookup"><span data-stu-id="66a80-290">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="66a80-291">Jeśli nie zostanie znalezione dopasowanie, szuka w jednym z obsługiwanych formatów, bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="66a80-291">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="66a80-292">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="66a80-292">For example:</span></span>

* <span data-ttu-id="66a80-293">Załóżmy, że parametr docelowy jest `Dictionary<string, string>` o nazwie `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="66a80-293">Suppose the target parameter is a `Dictionary<string, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="66a80-294">Przesłane dane ciągu formularza lub zapytania może wyglądać jak jeden z poniższych przykładów:</span><span class="sxs-lookup"><span data-stu-id="66a80-294">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="66a80-295">Dla wszystkich poprzedni przykład formatuje wiązania modelu przekazuje słownika zawierającego dwa elementy, które `selectedCourses` parametru:</span><span class="sxs-lookup"><span data-stu-id="66a80-295">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="66a80-296">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="66a80-296">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="66a80-297">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="66a80-297">selectedCourses["2000"]="Economics"</span></span>

## <a name="special-data-types"></a><span data-ttu-id="66a80-298">Specjalne typy danych</span><span class="sxs-lookup"><span data-stu-id="66a80-298">Special data types</span></span>

<span data-ttu-id="66a80-299">Istnieją pewne specjalne typy danych, które może obsłużyć wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="66a80-299">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="66a80-300">IFormFile i IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="66a80-300">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="66a80-301">Przekazany plik uwzględnione w żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="66a80-301">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="66a80-302">Jest również obsługiwane `IEnumerable<IFormFile>` dla wielu plików.</span><span class="sxs-lookup"><span data-stu-id="66a80-302">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="66a80-303">Token anulowania</span><span class="sxs-lookup"><span data-stu-id="66a80-303">CancellationToken</span></span>

<span data-ttu-id="66a80-304">Używane, aby anulować działanie w asynchronicznej kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="66a80-304">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="66a80-305">FormCollection</span><span class="sxs-lookup"><span data-stu-id="66a80-305">FormCollection</span></span>

<span data-ttu-id="66a80-306">Używany do pobierania wszystkich wartości z formularza przesłanych danych.</span><span class="sxs-lookup"><span data-stu-id="66a80-306">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="66a80-307">Programy formatujące danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="66a80-307">Input formatters</span></span>

<span data-ttu-id="66a80-308">Dane w treści żądania mogą być w formacie JSON, XML lub innym formacie.</span><span class="sxs-lookup"><span data-stu-id="66a80-308">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="66a80-309">Aby analizować te dane, model wiązania używa *wejściowego elementu formatującego* skonfigurowanego do obsługi określonego typu zawartości.</span><span class="sxs-lookup"><span data-stu-id="66a80-309">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="66a80-310">Domyślnie platformy ASP.NET Core obejmuje elementy formatujące wejściowych opartych na notacji JSON do obsługi danych JSON.</span><span class="sxs-lookup"><span data-stu-id="66a80-310">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="66a80-311">Możesz dodać inne elementy formatujące dla innych typów zawartości.</span><span class="sxs-lookup"><span data-stu-id="66a80-311">You can add other formatters for other content types.</span></span>

<span data-ttu-id="66a80-312">Platforma ASP.NET Core wybiera elementy formatujące danych wejściowych, na podstawie [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="66a80-312">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="66a80-313">Jeśli atrybut nie jest obecny, używa [nagłówek Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="66a80-313">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="66a80-314">Aby użyć wbudowanych formatujących danych wejściowych XML:</span><span class="sxs-lookup"><span data-stu-id="66a80-314">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="66a80-315">Zainstaluj `Microsoft.AspNetCore.Mvc.Formatters.Xml` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="66a80-315">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="66a80-316">W `Startup.ConfigureServices`, wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> lub <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="66a80-316">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="66a80-317">Zastosuj `Consumes` atrybutów do klasy kontrolera lub metody akcji, które należy się spodziewać XML w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="66a80-317">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="66a80-318">Aby uzyskać więcej informacji, zobacz [wprowadzenie do serializacji XML](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="66a80-318">For more information, see [Introducing XML Serialization](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="66a80-319">Wyklucz określonych typów z wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="66a80-319">Exclude specified types from model binding</span></span>

<span data-ttu-id="66a80-320">Model powiązania i sprawdzanie poprawności systems zachowanie jest wymuszany przez [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="66a80-320">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="66a80-321">Można dostosować `ModelMetadata` przez dodanie dostawcy szczegóły [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="66a80-321">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="66a80-322">Szczegóły wbudowane są dostępni dostawcy dla wyłączenie wiązania modelu lub sprawdzania poprawności dla określonych typów.</span><span class="sxs-lookup"><span data-stu-id="66a80-322">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="66a80-323">Aby wyłączyć wiązania modelu wszystkich modeli określonego typu, należy dodać <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="66a80-323">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="66a80-324">Na przykład do wiązania modelu wyłączenie wszystkich modeli typu `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="66a80-324">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="66a80-325">Aby wyłączyć sprawdzanie poprawności właściwości określonego typu, należy dodać <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="66a80-325">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="66a80-326">Na przykład, aby wyłączyć sprawdzanie poprawności we właściwościach typu `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="66a80-326">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="66a80-327">Integratorów modelu niestandardowego</span><span class="sxs-lookup"><span data-stu-id="66a80-327">Custom model binders</span></span>

<span data-ttu-id="66a80-328">Możesz rozszerzyć wiązania modelu przez napisanie niestandardowego integratora i przy użyciu modelu `[ModelBinder]` atrybutu, aby ją wybrać dla danego obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="66a80-328">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="66a80-329">Dowiedz się więcej o [niestandardowe wiązanie modelu](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="66a80-329">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="66a80-330">Wiązanie modelu ręczne</span><span class="sxs-lookup"><span data-stu-id="66a80-330">Manual model binding</span></span>

<span data-ttu-id="66a80-331">Wiązanie modelu można uruchomić ręcznie za pomocą <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> metody.</span><span class="sxs-lookup"><span data-stu-id="66a80-331">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="66a80-332">Metoda jest określona zarówno `ControllerBase` i `PageModel` klasy.</span><span class="sxs-lookup"><span data-stu-id="66a80-332">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="66a80-333">Przeciążenia metody umożliwiają określenie dostawca prefiksu i wartości do użycia.</span><span class="sxs-lookup"><span data-stu-id="66a80-333">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="66a80-334">Metoda ta zwraca `false` Jeśli wiązanie modelu nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="66a80-334">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="66a80-335">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="66a80-335">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="66a80-336">Atrybut [FromServices]</span><span class="sxs-lookup"><span data-stu-id="66a80-336">[FromServices] attribute</span></span>

<span data-ttu-id="66a80-337">Nazwa tego atrybutu jest zgodna z wzorcem atrybutów powiązanie modelu, które określić źródło danych.</span><span class="sxs-lookup"><span data-stu-id="66a80-337">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="66a80-338">Ale nie chodzi o powiązanie danych z dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="66a80-338">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="66a80-339">Pobiera wystąpienie typu z [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera.</span><span class="sxs-lookup"><span data-stu-id="66a80-339">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="66a80-340">Jej celem jest zapewnienie alternatywa iniekcji konstruktora dla, gdy niezbędna jest usługa tylko wtedy, gdy wywoływana jest metoda określonego.</span><span class="sxs-lookup"><span data-stu-id="66a80-340">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66a80-341">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="66a80-341">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
