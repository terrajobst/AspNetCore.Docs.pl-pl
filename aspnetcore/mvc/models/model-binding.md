---
title: Powiązanie modelu w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak działa powiązanie modelu w ASP.NET Core i jak dostosować jego zachowanie.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 11/21/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 823d92c279454fc6c744eebbecf4268412774eba
ms.sourcegitcommit: a104ba258ae7c0b3ee7c6fa7eaea1ddeb8b6eb73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/25/2019
ms.locfileid: "74478710"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="16eb2-103">Powiązanie modelu w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16eb2-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="16eb2-104">W tym artykule wyjaśniono, co to jest powiązanie modelu, jak to działa i jak dostosować jego zachowanie.</span><span class="sxs-lookup"><span data-stu-id="16eb2-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="16eb2-105">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="16eb2-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="16eb2-106">Co to jest powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="16eb2-106">What is Model binding</span></span>

<span data-ttu-id="16eb2-107">Kontrolery i strony Razor współpracują z danymi, które pochodzą z żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="16eb2-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="16eb2-108">Na przykład dane trasy mogą dostarczyć klucz rekordu, a pola ogłoszone formularza mogą podawać wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="16eb2-109">Pisanie kodu w celu pobrania każdej z tych wartości i przekonwertowania ich z ciągów na typy .NET byłoby żmudnym i podatne na błędy.</span><span class="sxs-lookup"><span data-stu-id="16eb2-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="16eb2-110">Powiązanie modelu automatyzuje ten proces.</span><span class="sxs-lookup"><span data-stu-id="16eb2-110">Model binding automates this process.</span></span> <span data-ttu-id="16eb2-111">System powiązań modelu:</span><span class="sxs-lookup"><span data-stu-id="16eb2-111">The model binding system:</span></span>

* <span data-ttu-id="16eb2-112">Pobiera dane z różnych źródeł, takich jak dane tras, pola formularzy i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="16eb2-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="16eb2-113">Dostarcza dane do kontrolerów i stron Razor w parametrach metod i właściwościach publicznych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="16eb2-114">Konwertuje dane ciągu na typy .NET.</span><span class="sxs-lookup"><span data-stu-id="16eb2-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="16eb2-115">Aktualizuje właściwości typów złożonych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="16eb2-116">Przykład</span><span class="sxs-lookup"><span data-stu-id="16eb2-116">Example</span></span>

<span data-ttu-id="16eb2-117">Załóżmy, że masz następującą metodę działania:</span><span class="sxs-lookup"><span data-stu-id="16eb2-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="16eb2-118">A aplikacja odbiera żądanie przy użyciu tego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="16eb2-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="16eb2-119">Powiązanie modelu przechodzi przez następujące kroki, gdy system routingu wybierze metodę akcji:</span><span class="sxs-lookup"><span data-stu-id="16eb2-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="16eb2-120">Znajduje pierwszy parametr `GetByID`, liczbą całkowitą o nazwie `id`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="16eb2-121">Wyszukuje dostępne źródła w żądaniu HTTP i odnajduje `id` = "2" w temacie dane trasy.</span><span class="sxs-lookup"><span data-stu-id="16eb2-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="16eb2-122">Konwertuje ciąg "2" na liczbę całkowitą 2.</span><span class="sxs-lookup"><span data-stu-id="16eb2-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="16eb2-123">Znajduje następny parametr `GetByID`, wartość logiczna o nazwie `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="16eb2-124">Wyszukuje źródła i wyszukuje ciąg "DogsOnly = true" w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="16eb2-125">W dopasowaniu nazw nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="16eb2-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="16eb2-126">Konwertuje ciąg "true" na wartość logiczną `true`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="16eb2-127">Struktura następnie wywołuje metodę `GetById`, przekazując wartość 2 dla parametru `id` i `true` dla parametru `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="16eb2-128">W poprzednim przykładzie elementy docelowe powiązań modelu to parametry metody, które są typami prostymi.</span><span class="sxs-lookup"><span data-stu-id="16eb2-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="16eb2-129">Elementy docelowe mogą być również właściwościami typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="16eb2-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="16eb2-130">Po pomyślnym powiązaniu każdej właściwości [Walidacja modelu](xref:mvc/models/validation) jest wykonywana dla tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="16eb2-131">Rekord danych powiązanych z modelem oraz wszelkie błędy powiązań lub walidacji są przechowywane w [ControllerBase. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) lub [PageModel. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="16eb2-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="16eb2-132">Aby dowiedzieć się, czy ten proces zakończył się pomyślnie, aplikacja sprawdza flagę [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) .</span><span class="sxs-lookup"><span data-stu-id="16eb2-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="16eb2-133">Obiekty docelowe</span><span class="sxs-lookup"><span data-stu-id="16eb2-133">Targets</span></span>

<span data-ttu-id="16eb2-134">Powiązanie modelu próbuje znaleźć wartości dla następujących rodzajów obiektów docelowych:</span><span class="sxs-lookup"><span data-stu-id="16eb2-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="16eb2-135">Parametry metody akcji kontrolera, do której jest kierowane żądanie.</span><span class="sxs-lookup"><span data-stu-id="16eb2-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="16eb2-136">Parametry metody obsługi Razor Pages, do której jest kierowane żądanie.</span><span class="sxs-lookup"><span data-stu-id="16eb2-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="16eb2-137">Właściwości publiczne kontrolera lub klasy `PageModel`, jeśli określono przez atrybuty.</span><span class="sxs-lookup"><span data-stu-id="16eb2-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="16eb2-138">[BindProperty] — atrybut</span><span class="sxs-lookup"><span data-stu-id="16eb2-138">[BindProperty] attribute</span></span>

<span data-ttu-id="16eb2-139">Można zastosować do właściwości publicznej kontrolera lub klasy `PageModel`, aby spowodować powiązanie modelu z celem tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="16eb2-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="16eb2-140">[BindProperties] — atrybut</span><span class="sxs-lookup"><span data-stu-id="16eb2-140">[BindProperties] attribute</span></span>

<span data-ttu-id="16eb2-141">Dostępne w ASP.NET Core 2,1 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="16eb2-142">Można zastosować do kontrolera lub klasy `PageModel`, aby poinstruować model powiązania do wszystkich właściwości publicznych klasy:</span><span class="sxs-lookup"><span data-stu-id="16eb2-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="16eb2-143">Powiązanie modelu dla żądań HTTP GET</span><span class="sxs-lookup"><span data-stu-id="16eb2-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="16eb2-144">Domyślnie właściwości nie są powiązane z żądaniami HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="16eb2-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="16eb2-145">Zazwyczaj wszystkie potrzebne do żądania GET są parametrem identyfikatora rekordu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="16eb2-146">Identyfikator rekordu służy do wyszukiwania elementu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="16eb2-147">W związku z tym nie ma potrzeby powiązania właściwości, która przechowuje wystąpienie modelu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="16eb2-148">W scenariuszach, w których właściwości są powiązane z żądaniami GET, ustaw właściwość `SupportsGet` na `true`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="16eb2-149">Źródeł</span><span class="sxs-lookup"><span data-stu-id="16eb2-149">Sources</span></span>

<span data-ttu-id="16eb2-150">Domyślnie powiązanie modelu pobiera dane w postaci par klucz-wartość z następujących źródeł w żądaniu HTTP:</span><span class="sxs-lookup"><span data-stu-id="16eb2-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="16eb2-151">Pola formularza</span><span class="sxs-lookup"><span data-stu-id="16eb2-151">Form fields</span></span>
1. <span data-ttu-id="16eb2-152">Treść żądania (dla [kontrolerów, które mają atrybut [ApiController]](xref:web-api/index#binding-source-parameter-inference)).</span><span class="sxs-lookup"><span data-stu-id="16eb2-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="16eb2-153">Dane trasy</span><span class="sxs-lookup"><span data-stu-id="16eb2-153">Route data</span></span>
1. <span data-ttu-id="16eb2-154">Parametry ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="16eb2-154">Query string parameters</span></span>
1. <span data-ttu-id="16eb2-155">Przekazane pliki</span><span class="sxs-lookup"><span data-stu-id="16eb2-155">Uploaded files</span></span>

<span data-ttu-id="16eb2-156">Dla każdego parametru lub właściwości docelowej źródła są skanowane w kolejności wskazanej na poprzedniej liście.</span><span class="sxs-lookup"><span data-stu-id="16eb2-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="16eb2-157">Istnieje kilka wyjątków:</span><span class="sxs-lookup"><span data-stu-id="16eb2-157">There are a few exceptions:</span></span>

* <span data-ttu-id="16eb2-158">Dane trasy i wartości ciągu zapytania są używane tylko dla typów prostych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="16eb2-159">Przekazane pliki są powiązane tylko z typami docelowymi, które implementują `IFormFile` lub `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="16eb2-160">Jeśli źródło domyślne jest niepoprawne, użyj jednego z następujących atrybutów, aby określić Źródło:</span><span class="sxs-lookup"><span data-stu-id="16eb2-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="16eb2-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) — pobiera wartości z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="16eb2-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) — pobiera wartości z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="16eb2-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="16eb2-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) — pobiera wartości ze opublikowanych pól formularza.</span><span class="sxs-lookup"><span data-stu-id="16eb2-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="16eb2-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) — pobiera wartości z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="16eb2-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) — pobiera wartości z nagłówków HTTP.</span><span class="sxs-lookup"><span data-stu-id="16eb2-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="16eb2-166">Następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="16eb2-166">These attributes:</span></span>

* <span data-ttu-id="16eb2-167">Są dodawane do właściwości modelu pojedynczo (nie do klasy modelu), jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="16eb2-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="16eb2-168">Opcjonalnie Zaakceptuj wartość nazwy modelu w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="16eb2-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="16eb2-169">Ta opcja jest dostępna w przypadku, gdy nazwa właściwości nie jest zgodna z wartością w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="16eb2-170">Na przykład wartość w żądaniu może być nagłówkiem z łącznikiem w nazwie, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="16eb2-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="16eb2-171">[FromBody] — atrybut</span><span class="sxs-lookup"><span data-stu-id="16eb2-171">[FromBody] attribute</span></span>

<span data-ttu-id="16eb2-172">Zastosuj atrybut `[FromBody]` do parametru, aby wypełnić jego właściwości z treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="16eb2-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="16eb2-173">Środowisko uruchomieniowe ASP.NET Core deleguje odpowiedzialność za odczytanie treści do wejściowego programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="16eb2-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="16eb2-174">Dane wejściowe są wyjaśnione [w dalszej części tego artykułu](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="16eb2-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="16eb2-175">Gdy `[FromBody]` jest stosowany do parametru typu złożonego, wszystkie atrybuty źródła powiązań zastosowane do jego właściwości są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="16eb2-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="16eb2-176">Na przykład następująca akcja `Create` określa, że jej parametr `pet` jest wypełniany w treści:</span><span class="sxs-lookup"><span data-stu-id="16eb2-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="16eb2-177">Klasa `Pet` określa, że jej Właściwość `Breed` jest wypełniona z parametru ciągu zapytania:</span><span class="sxs-lookup"><span data-stu-id="16eb2-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="16eb2-178">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="16eb2-178">In the preceding example:</span></span>

* <span data-ttu-id="16eb2-179">Atrybut `[FromQuery]` jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="16eb2-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="16eb2-180">Właściwość `Breed` nie jest wypełniona z parametru ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="16eb2-181">W danych wejściowych programu formatującego są odczytywane tylko treści i nie są rozpoznawane atrybuty źródłowe powiązania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="16eb2-182">Jeśli w treści zostanie znaleziona odpowiednia wartość, ta wartość jest używana do wypełniania właściwości `Breed`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="16eb2-183">Nie stosuj `[FromBody]` do więcej niż jednego parametru na metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="16eb2-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="16eb2-184">Gdy strumień żądania jest odczytywany przez wejściowego programu formatującego, nie jest już dostępny do ponownego odczytywania dla powiązań innych `[FromBody]` parametrów.</span><span class="sxs-lookup"><span data-stu-id="16eb2-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="16eb2-185">Dodatkowe źródła</span><span class="sxs-lookup"><span data-stu-id="16eb2-185">Additional sources</span></span>

<span data-ttu-id="16eb2-186">Dane źródłowe są dostarczane do systemu powiązań modelu przez *dostawców wartości*.</span><span class="sxs-lookup"><span data-stu-id="16eb2-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="16eb2-187">Można napisać i zarejestrować dostawców wartości niestandardowych, którzy pobierają dane dla powiązania modelu z innych źródeł.</span><span class="sxs-lookup"><span data-stu-id="16eb2-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="16eb2-188">Możesz na przykład potrzebować danych z plików cookie lub stanu sesji.</span><span class="sxs-lookup"><span data-stu-id="16eb2-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="16eb2-189">Aby pobrać dane z nowego źródła:</span><span class="sxs-lookup"><span data-stu-id="16eb2-189">To get data from a new source:</span></span>

* <span data-ttu-id="16eb2-190">Utwórz klasę, która implementuje `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="16eb2-191">Utwórz klasę, która implementuje `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="16eb2-192">Zarejestruj klasę fabryki w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="16eb2-193">Przykładowa aplikacja zawiera [dostawcę wartości](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) i przykład [fabryki](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) , który pobiera wartości z plików cookie.</span><span class="sxs-lookup"><span data-stu-id="16eb2-193">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="16eb2-194">Oto kod rejestracyjny w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="16eb2-195">Pokazany kod umieszcza niestandardowego dostawcę wartości po wszystkich wbudowanych dostawcach wartości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="16eb2-196">Aby najpierw utworzyć ten element na liście, wywołaj `Insert(0, new CookieValueProviderFactory())`, a nie `Add`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="16eb2-197">Brak źródła dla właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="16eb2-197">No source for a model property</span></span>

<span data-ttu-id="16eb2-198">Domyślnie błąd stanu modelu nie jest tworzony, jeśli dla właściwości modelu nie znaleziono żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="16eb2-199">Właściwość jest ustawiona na null lub wartość domyślną:</span><span class="sxs-lookup"><span data-stu-id="16eb2-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="16eb2-200">Dla typów prostych dopuszczających wartość null są ustawiane `null`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="16eb2-201">Typy wartości, które nie są dopuszczane do wartości null, są ustawione na `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="16eb2-202">Na przykład parametr `int id` jest ustawiony na 0.</span><span class="sxs-lookup"><span data-stu-id="16eb2-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="16eb2-203">W przypadku typów złożonych powiązanie modelu tworzy wystąpienie przy użyciu domyślnego konstruktora bez ustawiania właściwości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="16eb2-204">Tablice są ustawione na `Array.Empty<T>()`, z tą różnicą, że `byte[]` tablice są ustawione na `null`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="16eb2-205">Jeśli stan modelu ma być unieważniony, gdy niczego nie znaleziono w polach formularza dla właściwości modelu, użyj [atrybutu [BindRequired]](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="16eb2-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [[BindRequired] attribute](#bindrequired-attribute).</span></span>

<span data-ttu-id="16eb2-206">Należy zauważyć, że to zachowanie `[BindRequired]` ma zastosowanie do powiązania modelu z ogłoszonych danych formularza, nie do danych JSON ani XML w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="16eb2-207">Dane treści żądania są obsługiwane przez elementy [formatujące dane wejściowe](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="16eb2-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="16eb2-208">Błędy konwersji typów</span><span class="sxs-lookup"><span data-stu-id="16eb2-208">Type conversion errors</span></span>

<span data-ttu-id="16eb2-209">Jeśli źródło zostanie znalezione, ale nie można go przekonwertować na typ docelowy, stan modelu jest oflagowany jako nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="16eb2-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="16eb2-210">Parametr lub właściwość docelowa jest ustawiona na null lub wartość domyślną, jak wskazano w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="16eb2-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="16eb2-211">W kontrolerze interfejsu API, który ma atrybut `[ApiController]`, nieprawidłowy stan modelu powoduje automatyczne odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="16eb2-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="16eb2-212">Na stronie Razor ponownie Wyświetl stronę z komunikatem o błędzie:</span><span class="sxs-lookup"><span data-stu-id="16eb2-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="16eb2-213">Weryfikacja po stronie klienta przechwytuje najbardziej złe dane, które mogłyby zostać przesłane do Razor Pages formularzu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="16eb2-214">Ta weryfikacja sprawia, że trudno jest wyzwolić poprzedni wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="16eb2-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="16eb2-215">Przykładowa aplikacja zawiera przycisk **Prześlij z nieprawidłowym dniem** , który umieszcza złe dane w polu **Data zatrudnienia** i przesyła formularz.</span><span class="sxs-lookup"><span data-stu-id="16eb2-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="16eb2-216">Ten przycisk pokazuje, w jaki sposób kod na potrzeby wyświetlania strony działa po wystąpieniu błędów konwersji danych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="16eb2-217">Gdy strona jest ponownie wyświetlana przez poprzedni kod, nieprawidłowe dane wejściowe nie są wyświetlane w polu formularza.</span><span class="sxs-lookup"><span data-stu-id="16eb2-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="16eb2-218">Wynika to z faktu, że właściwość model ma wartość null lub domyślną.</span><span class="sxs-lookup"><span data-stu-id="16eb2-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="16eb2-219">Nieprawidłowe dane wejściowe są wyświetlane w komunikacie o błędzie.</span><span class="sxs-lookup"><span data-stu-id="16eb2-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="16eb2-220">Jeśli jednak chcesz ponownie wyświetlić złe dane w polu formularza, rozważ, że właściwość model jest ciągiem i ręcznie wykonuje konwersję danych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="16eb2-221">Ta sama strategia jest zalecana, jeśli nie chcesz, aby Błędy konwersji typów powodowały błędy stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="16eb2-222">W takim przypadku należy zmienić wartość właściwości model na ciąg.</span><span class="sxs-lookup"><span data-stu-id="16eb2-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="16eb2-223">Typy proste</span><span class="sxs-lookup"><span data-stu-id="16eb2-223">Simple types</span></span>

<span data-ttu-id="16eb2-224">Typy proste, które tworzą spinacz modelu mogą konwertować ciągi źródłowe, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="16eb2-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="16eb2-225">Typu</span><span class="sxs-lookup"><span data-stu-id="16eb2-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="16eb2-226">[Byte, bajty](xref:System.ComponentModel.ByteConverter) [](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="16eb2-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="16eb2-227">Delikatn</span><span class="sxs-lookup"><span data-stu-id="16eb2-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="16eb2-228">Datę</span><span class="sxs-lookup"><span data-stu-id="16eb2-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="16eb2-229">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="16eb2-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="16eb2-230">Dokładności</span><span class="sxs-lookup"><span data-stu-id="16eb2-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="16eb2-231">Double</span><span class="sxs-lookup"><span data-stu-id="16eb2-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="16eb2-232">Podstawowe</span><span class="sxs-lookup"><span data-stu-id="16eb2-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="16eb2-233">Ident</span><span class="sxs-lookup"><span data-stu-id="16eb2-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="16eb2-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="16eb2-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="16eb2-235">Wiersz</span><span class="sxs-lookup"><span data-stu-id="16eb2-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="16eb2-236">Czasu</span><span class="sxs-lookup"><span data-stu-id="16eb2-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="16eb2-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="16eb2-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="16eb2-238">Adresu</span><span class="sxs-lookup"><span data-stu-id="16eb2-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="16eb2-239">Wersja</span><span class="sxs-lookup"><span data-stu-id="16eb2-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="16eb2-240">Typy złożone</span><span class="sxs-lookup"><span data-stu-id="16eb2-240">Complex types</span></span>

<span data-ttu-id="16eb2-241">Typ złożony musi mieć publiczny Konstruktor domyślny i publiczne właściwości do zapisu do powiązania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="16eb2-242">W przypadku wystąpienia powiązania modelu Klasa jest tworzona przy użyciu publicznego konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="16eb2-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="16eb2-243">Dla każdej właściwości typu złożonego powiązanie modelu przeszukuje źródła dla *prefiksu wzorca nazwy. property_name*.</span><span class="sxs-lookup"><span data-stu-id="16eb2-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="16eb2-244">Jeśli nic nie zostanie znalezione, szuka tylko *property_name* bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="16eb2-245">W przypadku powiązania z parametrem prefiks jest nazwą parametru.</span><span class="sxs-lookup"><span data-stu-id="16eb2-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="16eb2-246">W przypadku powiązania do `PageModel` publicznego Właściwość prefiks jest publiczną nazwą właściwości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="16eb2-247">Niektóre atrybuty mają właściwość `Prefix`, która pozwala zastąpić domyślne użycie parametru lub nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="16eb2-248">Na przykład, Załóżmy, że typ złożony jest następującą klasą `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="16eb2-249">Prefix = Nazwa parametru</span><span class="sxs-lookup"><span data-stu-id="16eb2-249">Prefix = parameter name</span></span>

<span data-ttu-id="16eb2-250">Jeśli modelem, który ma zostać powiązany, jest parametr o nazwie `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="16eb2-251">Powiązanie modelu rozpocznie się, przeglądając źródła klucza `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="16eb2-252">Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="16eb2-253">Prefix = nazwa właściwości</span><span class="sxs-lookup"><span data-stu-id="16eb2-253">Prefix = property name</span></span>

<span data-ttu-id="16eb2-254">Jeśli modelem, który ma zostać powiązany, jest właściwość o nazwie `Instructor` kontrolera lub klasy `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="16eb2-255">Powiązanie modelu rozpocznie się, przeglądając źródła klucza `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="16eb2-256">Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="16eb2-257">Prefiks niestandardowy</span><span class="sxs-lookup"><span data-stu-id="16eb2-257">Custom prefix</span></span>

<span data-ttu-id="16eb2-258">Jeśli modelem, który ma zostać powiązany, jest parametr o nazwie `instructorToUpdate`, a atrybut `Bind` określa `Instructor` jako prefiks:</span><span class="sxs-lookup"><span data-stu-id="16eb2-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="16eb2-259">Powiązanie modelu rozpocznie się, przeglądając źródła klucza `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="16eb2-260">Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="16eb2-261">Atrybuty dla obiektów docelowych typu złożonego</span><span class="sxs-lookup"><span data-stu-id="16eb2-261">Attributes for complex type targets</span></span>

<span data-ttu-id="16eb2-262">Dostępne są kilka wbudowanych atrybutów do kontrolowania powiązania modelu typów złożonych:</span><span class="sxs-lookup"><span data-stu-id="16eb2-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="16eb2-263">Te atrybuty wpływają na powiązanie modelu, gdy dane formularza ogłoszonego są źródłem wartości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="16eb2-264">Nie wpływają one na wejściowe elementy formatujące, które przetwarzają ogłoszone treści kodu JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="16eb2-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="16eb2-265">Dane wejściowe są wyjaśnione [w dalszej części tego artykułu](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="16eb2-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="16eb2-266">Zobacz również Omówienie atrybutu `[Required]` w [walidacji modelu](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="16eb2-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="16eb2-267">[BindRequired] — atrybut</span><span class="sxs-lookup"><span data-stu-id="16eb2-267">[BindRequired] attribute</span></span>

<span data-ttu-id="16eb2-268">Można stosować tylko do właściwości modelu, a nie do parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="16eb2-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="16eb2-269">Powoduje, że powiązanie modelu umożliwia dodanie błędu stanu modelu, Jeśli powiązanie nie może wystąpić dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="16eb2-270">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="16eb2-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="16eb2-271">[BindNever] — atrybut</span><span class="sxs-lookup"><span data-stu-id="16eb2-271">[BindNever] attribute</span></span>

<span data-ttu-id="16eb2-272">Można stosować tylko do właściwości modelu, a nie do parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="16eb2-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="16eb2-273">Uniemożliwia powiązanie modelu z ustawiania właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="16eb2-274">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="16eb2-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="16eb2-275">[Bind] — atrybut</span><span class="sxs-lookup"><span data-stu-id="16eb2-275">[Bind] attribute</span></span>

<span data-ttu-id="16eb2-276">Można zastosować do klasy lub parametru metody.</span><span class="sxs-lookup"><span data-stu-id="16eb2-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="16eb2-277">Określa, które właściwości modelu powinny być dołączone do powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="16eb2-278">W poniższym przykładzie tylko określone właściwości modelu `Instructor` są powiązane, gdy wywoływana jest jakakolwiek procedura obsługi lub metoda działania:</span><span class="sxs-lookup"><span data-stu-id="16eb2-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="16eb2-279">W poniższym przykładzie tylko określone właściwości modelu `Instructor` są powiązane, gdy wywoływana jest metoda `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="16eb2-280">Atrybut `[Bind]` może służyć do ochrony przed nadużyciem w scenariuszach *tworzenia* scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="16eb2-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="16eb2-281">Nie działa prawidłowo w scenariuszach edycji, ponieważ wykluczone właściwości mają ustawioną wartość null lub wartość domyślną, a nie jako pozostawione bez zmian.</span><span class="sxs-lookup"><span data-stu-id="16eb2-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="16eb2-282">W celu zapewnienia obrony przed przekroczeniem, zaleca się, aby zamiast atrybutu `[Bind]` uzyskać odpowiednie modele.</span><span class="sxs-lookup"><span data-stu-id="16eb2-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="16eb2-283">Aby uzyskać więcej informacji, zobacz [temat Security uwagi dotyczący przefinalizowania](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="16eb2-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="16eb2-284">Kolekcje</span><span class="sxs-lookup"><span data-stu-id="16eb2-284">Collections</span></span>

<span data-ttu-id="16eb2-285">Dla celów, które są kolekcjami typów prostych, powiązanie modelu wyszukuje dopasowania do *parameter_name* lub *property_name*.</span><span class="sxs-lookup"><span data-stu-id="16eb2-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="16eb2-286">Jeśli dopasowanie nie zostanie znalezione, szuka jednego z obsługiwanych formatów bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="16eb2-287">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="16eb2-287">For example:</span></span>

* <span data-ttu-id="16eb2-288">Załóżmy, że parametr ma być powiązany, jest tablicą o nazwie `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="16eb2-289">Dane formularza lub ciągu zapytania mogą znajdować się w jednym z następujących formatów:</span><span class="sxs-lookup"><span data-stu-id="16eb2-289">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="16eb2-290">Następujący format jest obsługiwany tylko w danych formularza:</span><span class="sxs-lookup"><span data-stu-id="16eb2-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="16eb2-291">We wszystkich powyższych formatach przykładowe powiązanie modelu przekazuje tablicę dwóch elementów do `selectedCourses` parametru:</span><span class="sxs-lookup"><span data-stu-id="16eb2-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="16eb2-292">selectedCourses [0] = 1050</span><span class="sxs-lookup"><span data-stu-id="16eb2-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="16eb2-293">selectedCourses [1] = 2000</span><span class="sxs-lookup"><span data-stu-id="16eb2-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="16eb2-294">Formaty danych używające liczb indeksów dolnych (... [0]... [1]...) należy upewnić się, że są numerowane sekwencyjnie, zaczynając od zera.</span><span class="sxs-lookup"><span data-stu-id="16eb2-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="16eb2-295">Jeśli występują luki w numerze indeksu dolnego, wszystkie elementy po przerwie zostaną zignorowane.</span><span class="sxs-lookup"><span data-stu-id="16eb2-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="16eb2-296">Na przykład, jeśli indeksy dolne są równe 0 i 2 zamiast 0 i 1, drugi element jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="16eb2-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="16eb2-297">Słownik</span><span class="sxs-lookup"><span data-stu-id="16eb2-297">Dictionaries</span></span>

<span data-ttu-id="16eb2-298">Dla `Dictionary` obiektów docelowych powiązanie modelu wyszukuje dopasowania do *parameter_name* lub *property_name*.</span><span class="sxs-lookup"><span data-stu-id="16eb2-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="16eb2-299">Jeśli dopasowanie nie zostanie znalezione, szuka jednego z obsługiwanych formatów bez prefiksu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="16eb2-300">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="16eb2-300">For example:</span></span>

* <span data-ttu-id="16eb2-301">Załóżmy, że parametr docelowy jest `Dictionary<int, string>` o nazwie `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="16eb2-302">Ogłoszone dane formularza lub ciągu zapytania mogą wyglądać jak w jednym z następujących przykładów:</span><span class="sxs-lookup"><span data-stu-id="16eb2-302">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="16eb2-303">We wszystkich powyższych formatach przykładowe powiązanie modelu przekazuje słownik dwóch elementów do `selectedCourses` parametru:</span><span class="sxs-lookup"><span data-stu-id="16eb2-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="16eb2-304">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="16eb2-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="16eb2-305">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="16eb2-305">selectedCourses["2000"]="Economics"</span></span>

## <a name="special-data-types"></a><span data-ttu-id="16eb2-306">Specjalne typy danych</span><span class="sxs-lookup"><span data-stu-id="16eb2-306">Special data types</span></span>

<span data-ttu-id="16eb2-307">Istnieją specjalne typy danych, które może obsłużyć powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="16eb2-307">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="16eb2-308">IFormFile i IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="16eb2-308">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="16eb2-309">Przekazany plik uwzględniony w żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="16eb2-309">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="16eb2-310">Obsługiwane są również `IEnumerable<IFormFile>` dla wielu plików.</span><span class="sxs-lookup"><span data-stu-id="16eb2-310">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="16eb2-311">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="16eb2-311">CancellationToken</span></span>

<span data-ttu-id="16eb2-312">Służy do anulowania działania w kontrolerach asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-312">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="16eb2-313">Formularz</span><span class="sxs-lookup"><span data-stu-id="16eb2-313">FormCollection</span></span>

<span data-ttu-id="16eb2-314">Służy do pobierania wszystkich wartości z ogłoszonych danych formularza.</span><span class="sxs-lookup"><span data-stu-id="16eb2-314">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="16eb2-315">Wejściowe elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="16eb2-315">Input formatters</span></span>

<span data-ttu-id="16eb2-316">Dane w treści żądania mogą być w formacie JSON, XML lub innym.</span><span class="sxs-lookup"><span data-stu-id="16eb2-316">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="16eb2-317">Aby przeanalizować te dane, powiązanie modelu korzysta z *wejściowego programu formatującego* , który jest skonfigurowany do obsługi określonego typu zawartości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-317">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="16eb2-318">Domyślnie ASP.NET Core zawiera dane wejściowe w formacie JSON na potrzeby obsługi danych JSON.</span><span class="sxs-lookup"><span data-stu-id="16eb2-318">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="16eb2-319">Można dodać inne elementy formatujące dla innych typów zawartości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-319">You can add other formatters for other content types.</span></span>

<span data-ttu-id="16eb2-320">ASP.NET Core wybiera wejściowe elementy formatujące na podstawie atrybutu [użycia](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) .</span><span class="sxs-lookup"><span data-stu-id="16eb2-320">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="16eb2-321">Jeśli atrybut nie jest obecny, używa [nagłówka Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="16eb2-321">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="16eb2-322">Aby użyć wbudowanych elementów formatujących dane wejściowe XML:</span><span class="sxs-lookup"><span data-stu-id="16eb2-322">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="16eb2-323">Zainstaluj pakiet NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-323">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="16eb2-324">W `Startup.ConfigureServices`Wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> lub <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="16eb2-324">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="16eb2-325">Zastosuj atrybut `Consumes` do klas kontrolera lub metod akcji, które powinny oczekiwać XML w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="16eb2-325">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="16eb2-326">Aby uzyskać więcej informacji, zobacz [wprowadzenie serializacji XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="16eb2-326">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="16eb2-327">Wyklucz określone typy z powiązania modelu</span><span class="sxs-lookup"><span data-stu-id="16eb2-327">Exclude specified types from model binding</span></span>

<span data-ttu-id="16eb2-328">Zachowanie modelu powiązań i systemów walidacji jest zależne od [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="16eb2-328">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="16eb2-329">`ModelMetadata` można dostosować, dodając dostawcę szczegółów do [MvcOptions. ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="16eb2-329">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="16eb2-330">Wbudowane dostawcy szczegółów są dostępne do wyłączenia powiązania modelu lub walidacji dla określonych typów.</span><span class="sxs-lookup"><span data-stu-id="16eb2-330">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="16eb2-331">Aby wyłączyć powiązanie modelu dla wszystkich modeli określonego typu, Dodaj <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-331">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="16eb2-332">Na przykład, aby wyłączyć powiązanie modelu dla wszystkich modeli typu `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-332">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="16eb2-333">Aby wyłączyć walidację właściwości określonego typu, Dodaj <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-333">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="16eb2-334">Na przykład aby wyłączyć walidację właściwości typu `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="16eb2-334">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="16eb2-335">Niestandardowe powiązania modelu</span><span class="sxs-lookup"><span data-stu-id="16eb2-335">Custom model binders</span></span>

<span data-ttu-id="16eb2-336">Można rozszerzać powiązania modelu, pisząc niestandardowy spinacz modelu i używając atrybutu `[ModelBinder]`, aby wybrać go dla danego elementu docelowego.</span><span class="sxs-lookup"><span data-stu-id="16eb2-336">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="16eb2-337">Dowiedz się więcej na temat [niestandardowego powiązania modelu](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="16eb2-337">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="16eb2-338">Ręczne powiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="16eb2-338">Manual model binding</span></span>

<span data-ttu-id="16eb2-339">Powiązanie modelu można wywołać ręcznie przy użyciu metody <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="16eb2-339">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="16eb2-340">Metoda jest definiowana dla klas `ControllerBase` i `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="16eb2-340">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="16eb2-341">Przeciążenia metod umożliwiają określenie prefiksu i dostawcy wartości do użycia.</span><span class="sxs-lookup"><span data-stu-id="16eb2-341">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="16eb2-342">Metoda zwraca `false`, Jeśli powiązanie z modelem nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="16eb2-342">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="16eb2-343">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="16eb2-343">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="16eb2-344">[FromServices] — atrybut</span><span class="sxs-lookup"><span data-stu-id="16eb2-344">[FromServices] attribute</span></span>

<span data-ttu-id="16eb2-345">Nazwa tego atrybutu jest zgodna ze wzorcem atrybutów powiązania modelu, które określają źródło danych.</span><span class="sxs-lookup"><span data-stu-id="16eb2-345">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="16eb2-346">Ale nie informacje o powiązaniu danych od dostawcy wartości.</span><span class="sxs-lookup"><span data-stu-id="16eb2-346">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="16eb2-347">Pobiera wystąpienie typu z kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="16eb2-347">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="16eb2-348">Jego celem jest zapewnienie alternatywy dla iniekcji konstruktorów, gdy potrzebna jest usługa tylko wtedy, gdy jest wywoływana konkretna metoda.</span><span class="sxs-lookup"><span data-stu-id="16eb2-348">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="16eb2-349">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="16eb2-349">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
