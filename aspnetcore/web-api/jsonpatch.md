---
title: JsonPatch w interfejsie Web API ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obsługiwać żądania poprawek w formacie JSON w ASP.NET Core internetowym interfejsie API.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: e57556e4b3fba55c6c187092593ffab4e31ee2d9
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727024"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="9e8eb-103">JsonPatch w interfejsie Web API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e8eb-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="9e8eb-104">Autorzy [Dykstra](https://github.com/tdykstra) i [Kirka Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="9e8eb-104">By [Tom Dykstra](https://github.com/tdykstra) and [Kirk Larkin](https://github.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9e8eb-105">W tym artykule wyjaśniono, jak obsłużyć żądania poprawek w formacie JSON w ASP.NET Core interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="package-installation"></a><span data-ttu-id="9e8eb-106">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="9e8eb-106">Package installation</span></span>

<span data-ttu-id="9e8eb-107">Obsługa JsonPatch jest włączona przy użyciu pakietu `Microsoft.AspNetCore.Mvc.NewtonsoftJson`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-107">Support for JsonPatch is enabled using the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package.</span></span> <span data-ttu-id="9e8eb-108">Aby włączyć tę funkcję, aplikacje muszą:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-108">To enable this feature, apps must:</span></span>

* <span data-ttu-id="9e8eb-109">Zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) .</span><span class="sxs-lookup"><span data-stu-id="9e8eb-109">Install the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package.</span></span>
* <span data-ttu-id="9e8eb-110">Zaktualizuj metodę `Startup.ConfigureServices` projektu w celu uwzględnienia wywołania `AddNewtonsoftJson`:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-110">Update the project's `Startup.ConfigureServices` method to include a call to `AddNewtonsoftJson`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

<span data-ttu-id="9e8eb-111">`AddNewtonsoftJson` jest zgodna z metodami rejestracji usługi MVC:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-111">`AddNewtonsoftJson` is compatible with the MVC service registration methods:</span></span>

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a><span data-ttu-id="9e8eb-112">JsonPatch, AddNewtonsoftJson i system. Text. JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-112">JsonPatch, AddNewtonsoftJson, and System.Text.Json</span></span>
  
<span data-ttu-id="9e8eb-113">`AddNewtonsoftJson` zastępuje oparte na `System.Text.Json` wejściowe i wyjściowe elementy formatujące używane do formatowania **całej** zawartości JSON.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-113">`AddNewtonsoftJson` replaces the `System.Text.Json` based input and output formatters used for formatting **all** JSON content.</span></span> <span data-ttu-id="9e8eb-114">Aby dodać obsługę `JsonPatch` przy użyciu `Newtonsoft.Json`, pozostawiając inne elementy formatujące bez zmian, zaktualizuj `Startup.ConfigureServices` projektu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-114">To add support for `JsonPatch` using `Newtonsoft.Json`, while leaving the other formatters unchanged, update the project's `Startup.ConfigureServices` as follows:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="9e8eb-115">Poprzedzający kod wymaga odwołania do elementu [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) i następujących instrukcji using:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-115">The preceding code requires a reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) and the following using statements:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a><span data-ttu-id="9e8eb-116">Poprawka metody żądania HTTP</span><span class="sxs-lookup"><span data-stu-id="9e8eb-116">PATCH HTTP request method</span></span>

<span data-ttu-id="9e8eb-117">Metody PUT i [patch](https://tools.ietf.org/html/rfc5789) są używane do aktualizowania istniejącego zasobu.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-117">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="9e8eb-118">Różnica między nimi polega na tym, że zastępuje cały zasób, podczas gdy poprawka określa tylko te zmiany.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-118">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="9e8eb-119">Poprawka JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-119">JSON Patch</span></span>

<span data-ttu-id="9e8eb-120">[Poprawka JSON](https://tools.ietf.org/html/rfc6902) to format służący do określania aktualizacji, które mają zostać zastosowane do zasobu.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="9e8eb-121">Dokument poprawki JSON zawiera tablicę *operacji*.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-121">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="9e8eb-122">Każda operacja identyfikuje określony typ zmiany, na przykład Dodaj element tablicy lub Zastąp wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-122">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="9e8eb-123">Na przykład następujące dokumenty JSON reprezentują zasób, dokument poprawki JSON dla zasobu oraz wynik zastosowania operacji patch.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-123">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="9e8eb-124">Przykład zasobu</span><span class="sxs-lookup"><span data-stu-id="9e8eb-124">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="9e8eb-125">Przykład poprawki JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-125">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="9e8eb-126">W powyższym formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-126">In the preceding JSON:</span></span>

* <span data-ttu-id="9e8eb-127">Właściwość `op` wskazuje typ operacji.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-127">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="9e8eb-128">Właściwość `path` wskazuje element do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-128">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="9e8eb-129">Właściwość `value` udostępnia nową wartość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-129">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="9e8eb-130">Zasób po zastosowaniu poprawki</span><span class="sxs-lookup"><span data-stu-id="9e8eb-130">Resource after patch</span></span>

<span data-ttu-id="9e8eb-131">Poniżej znajduje się zasób po zastosowaniu poprzedniego dokumentu poprawki JSON:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-131">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="9e8eb-132">Zmiany wprowadzone przez zastosowanie dokumentu poprawek JSON do zasobu są niepodzielne: Jeśli jakakolwiek operacja na liście nie powiedzie się, nie zostanie zastosowana żadna operacja na liście.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-132">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="9e8eb-133">Składnia ścieżki</span><span class="sxs-lookup"><span data-stu-id="9e8eb-133">Path syntax</span></span>

<span data-ttu-id="9e8eb-134">Właściwość [Path](https://tools.ietf.org/html/rfc6901) obiektu operacji ma ukośniki między poziomami.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-134">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="9e8eb-135">Na przykład `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-135">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="9e8eb-136">W celu określenia elementów tablicy są używane indeksy oparte na wartości zero.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-136">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="9e8eb-137">Pierwszy element tablicy `addresses` będzie miał `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-137">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="9e8eb-138">Aby `add` do końca tablicy, użyj łącznika (-), a nie numeru indeksu: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-138">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="9e8eb-139">Operacje</span><span class="sxs-lookup"><span data-stu-id="9e8eb-139">Operations</span></span>

<span data-ttu-id="9e8eb-140">W poniższej tabeli przedstawiono obsługiwane operacje zgodnie z definicją w [specyfikacji poprawek JSON](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="9e8eb-140">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="9e8eb-141">Operacja</span><span class="sxs-lookup"><span data-stu-id="9e8eb-141">Operation</span></span>  | <span data-ttu-id="9e8eb-142">Uwagi</span><span class="sxs-lookup"><span data-stu-id="9e8eb-142">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="9e8eb-143">Dodaj właściwość lub element tablicy.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-143">Add a property or array element.</span></span> <span data-ttu-id="9e8eb-144">Dla istniejącej właściwości: Ustaw wartość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-144">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="9e8eb-145">Usuń właściwość lub element tablicy.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-145">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="9e8eb-146">Taka sama jak `remove`, a następnie `add` w tej samej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-146">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="9e8eb-147">Taka sama jak `remove` ze źródła, po której następuje `add` do miejsca docelowego przy użyciu wartości ze źródła.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-147">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="9e8eb-148">Analogicznie jak `add` do miejsca docelowego przy użyciu wartości ze źródła.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-148">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="9e8eb-149">Zwróć kod stanu sukcesu, jeśli wartość w `path` = dostarczona `value`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-149">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="9e8eb-150">JsonPatch w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e8eb-150">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="9e8eb-151">ASP.NET Core implementacja poprawki JSON jest dostępna w pakiecie NuGet [Microsoft. AspNetCore. JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) .</span><span class="sxs-lookup"><span data-stu-id="9e8eb-151">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="9e8eb-152">Kod metody akcji</span><span class="sxs-lookup"><span data-stu-id="9e8eb-152">Action method code</span></span>

<span data-ttu-id="9e8eb-153">W kontrolerze interfejsu API Metoda akcji dla poprawki JSON:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-153">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="9e8eb-154">Ma adnotację z atrybutem `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-154">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="9e8eb-155">Akceptuje `JsonPatchDocument<T>`, zazwyczaj z `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-155">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="9e8eb-156">Wywołuje `ApplyTo` w dokumencie poprawki, aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-156">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="9e8eb-157">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-157">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="9e8eb-158">Ten kod z przykładowej aplikacji współdziała z poniższym modelem `Customer`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-158">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="9e8eb-159">Przykładowa Metoda akcji:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-159">The sample action method:</span></span>

* <span data-ttu-id="9e8eb-160">Konstruuje `Customer`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-160">Constructs a `Customer`.</span></span>
* <span data-ttu-id="9e8eb-161">Stosuje poprawkę.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-161">Applies the patch.</span></span>
* <span data-ttu-id="9e8eb-162">Zwraca wynik w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-162">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="9e8eb-163">W rzeczywistej aplikacji kod pobiera dane z magazynu, takiego jak baza danych, i aktualizuje bazę danych po zastosowaniu poprawki.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-163">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="9e8eb-164">Stan modelu</span><span class="sxs-lookup"><span data-stu-id="9e8eb-164">Model state</span></span>

<span data-ttu-id="9e8eb-165">Poprzednia metoda działania przykład wywołuje Przeciążenie `ApplyTo`, które pobiera stan modelu jako jeden z jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-165">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="9e8eb-166">W przypadku tej opcji można odbierać komunikaty o błędach w odpowiedziach.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-166">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="9e8eb-167">Poniższy przykład przedstawia treść nieprawidłowej odpowiedzi na żądanie 400 dla operacji `test`:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-167">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="9e8eb-168">Obiekty dynamiczne</span><span class="sxs-lookup"><span data-stu-id="9e8eb-168">Dynamic objects</span></span>

<span data-ttu-id="9e8eb-169">W poniższym przykładzie metody akcji pokazano, jak zastosować poprawkę do obiektu dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-169">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="9e8eb-170">Operacja dodawania</span><span class="sxs-lookup"><span data-stu-id="9e8eb-170">The add operation</span></span>

* <span data-ttu-id="9e8eb-171">Jeśli `path` wskazuje element tablicy: wstawia nowy element przed określony przez `path`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-171">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="9e8eb-172">Jeśli `path` wskazuje Właściwość: ustawia wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-172">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="9e8eb-173">Jeśli `path` wskazuje nieistniejącą lokalizację:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-173">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="9e8eb-174">Jeśli zasób do poprawki jest obiektem dynamicznym: dodaje właściwość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-174">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="9e8eb-175">Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-175">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="9e8eb-176">Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i dodaje obiekt `Order` do końca tablicy `Orders`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-176">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="9e8eb-177">Operacja usuwania</span><span class="sxs-lookup"><span data-stu-id="9e8eb-177">The remove operation</span></span>

* <span data-ttu-id="9e8eb-178">Jeśli `path` wskazuje element tablicy: usuwa element.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-178">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="9e8eb-179">Jeśli `path` wskazuje Właściwość:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-179">If `path` points to a property:</span></span>
  * <span data-ttu-id="9e8eb-180">Jeśli zasób do poprawki jest obiektem dynamicznym: usuwa właściwość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-180">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="9e8eb-181">Jeśli zasób do poprawki jest obiektem statycznym:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-181">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="9e8eb-182">Jeśli właściwość dopuszcza wartość null: ustawia ją na wartość null.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-182">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="9e8eb-183">Jeśli właściwość nie dopuszcza wartości null, ustawia ją na `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-183">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="9e8eb-184">Następujący przykładowy dokument poprawek ustawia `CustomerName` na null i usuwa `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-184">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="9e8eb-185">Operacja zamiany</span><span class="sxs-lookup"><span data-stu-id="9e8eb-185">The replace operation</span></span>

<span data-ttu-id="9e8eb-186">Ta operacja jest funkcjonalnie taka sama jak `remove` i `add`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-186">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="9e8eb-187">Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i zastępuje `Orders[0]`nowym obiektem `Order`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-187">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="9e8eb-188">Operacja przenoszenia</span><span class="sxs-lookup"><span data-stu-id="9e8eb-188">The move operation</span></span>

* <span data-ttu-id="9e8eb-189">Jeśli `path` wskazuje element tablicy: kopiuje `from` element do lokalizacji `path` elementu, a następnie uruchamia operację `remove` na `from` elemencie.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-189">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="9e8eb-190">Jeśli `path` wskazuje Właściwość: kopiuje wartość właściwości `from` do właściwości `path`, a następnie uruchamia operację `remove` na właściwości `from`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-190">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="9e8eb-191">Jeśli `path` wskazuje nieistniejącą Właściwość:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-191">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="9e8eb-192">Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-192">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="9e8eb-193">Jeśli zasób do naprawienia jest obiektem dynamicznym: kopiuje Właściwość `from` do lokalizacji wskazywanej przez `path`, a następnie uruchamia operację `remove` na właściwości `from`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-193">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="9e8eb-194">Następujący przykładowy dokument poprawek:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-194">The following sample patch document:</span></span>

* <span data-ttu-id="9e8eb-195">Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-195">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="9e8eb-196">Ustawia `Orders[0].OrderName` na wartość null.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-196">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="9e8eb-197">Przenosi `Orders[1]` do przed `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-197">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="9e8eb-198">Operacja kopiowania</span><span class="sxs-lookup"><span data-stu-id="9e8eb-198">The copy operation</span></span>

<span data-ttu-id="9e8eb-199">Ta operacja jest funkcjonalnie taka sama jak operacja `move` bez kroku `remove` końcowego.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-199">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="9e8eb-200">Następujący przykładowy dokument poprawek:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-200">The following sample patch document:</span></span>

* <span data-ttu-id="9e8eb-201">Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-201">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="9e8eb-202">Wstawia kopię `Orders[1]` przed `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-202">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="9e8eb-203">Operacja testowa</span><span class="sxs-lookup"><span data-stu-id="9e8eb-203">The test operation</span></span>

<span data-ttu-id="9e8eb-204">Jeśli wartość w lokalizacji wskazywanej przez `path` różni się od wartości podanej w `value`, żądanie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-204">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="9e8eb-205">W takim przypadku całe żądanie PATCH kończy się niepowodzeniem, nawet jeśli wszystkie inne operacje w dokumencie poprawki zakończyły się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-205">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="9e8eb-206">Operacja `test` jest często używana do zapobiegania aktualizacji, gdy występuje konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-206">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="9e8eb-207">Następujący przykładowy dokument poprawek nie ma wpływu, jeśli początkowa wartość `CustomerName` to "Jan", ponieważ test kończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-207">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="9e8eb-208">Uzyskiwanie kodu</span><span class="sxs-lookup"><span data-stu-id="9e8eb-208">Get the code</span></span>

<span data-ttu-id="9e8eb-209">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="9e8eb-209">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="9e8eb-210">([Jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9e8eb-210">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="9e8eb-211">Aby przetestować przykład, uruchom aplikację i Wyślij żądania HTTP z następującymi ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-211">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="9e8eb-212">Adres URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="9e8eb-212">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="9e8eb-213">Metoda HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="9e8eb-213">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="9e8eb-214">Nagłówek: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="9e8eb-214">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="9e8eb-215">Treść: Skopiuj i wklej jeden z przykładów dokumentu poprawki JSON z folderu projektu *JSON* .</span><span class="sxs-lookup"><span data-stu-id="9e8eb-215">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e8eb-216">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9e8eb-216">Additional resources</span></span>

* [<span data-ttu-id="9e8eb-217">IETF RFC 5789 Specyfikacja metody poprawek</span><span class="sxs-lookup"><span data-stu-id="9e8eb-217">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="9e8eb-218">IETF RFC 6902 — Specyfikacja poprawek JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-218">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="9e8eb-219">IETF RFC 6901 JSON Format ścieżki poprawek</span><span class="sxs-lookup"><span data-stu-id="9e8eb-219">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="9e8eb-220">[Dokumentacja poprawek JSON](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="9e8eb-220">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="9e8eb-221">Zawiera linki do zasobów do tworzenia dokumentów poprawek JSON.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-221">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="9e8eb-222">ASP.NET Core kod źródłowy poprawki JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-222">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9e8eb-223">W tym artykule wyjaśniono, jak obsłużyć żądania poprawek w formacie JSON w ASP.NET Core interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-223">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="9e8eb-224">Poprawka metody żądania HTTP</span><span class="sxs-lookup"><span data-stu-id="9e8eb-224">PATCH HTTP request method</span></span>

<span data-ttu-id="9e8eb-225">Metody PUT i [patch](https://tools.ietf.org/html/rfc5789) są używane do aktualizowania istniejącego zasobu.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-225">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="9e8eb-226">Różnica między nimi polega na tym, że zastępuje cały zasób, podczas gdy poprawka określa tylko te zmiany.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-226">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="9e8eb-227">Poprawka JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-227">JSON Patch</span></span>

<span data-ttu-id="9e8eb-228">[Poprawka JSON](https://tools.ietf.org/html/rfc6902) to format służący do określania aktualizacji, które mają zostać zastosowane do zasobu.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="9e8eb-229">Dokument poprawki JSON zawiera tablicę *operacji*.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-229">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="9e8eb-230">Każda operacja identyfikuje określony typ zmiany, na przykład Dodaj element tablicy lub Zastąp wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-230">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="9e8eb-231">Na przykład następujące dokumenty JSON reprezentują zasób, dokument poprawki JSON dla zasobu oraz wynik zastosowania operacji patch.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-231">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="9e8eb-232">Przykład zasobu</span><span class="sxs-lookup"><span data-stu-id="9e8eb-232">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="9e8eb-233">Przykład poprawki JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-233">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="9e8eb-234">W powyższym formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-234">In the preceding JSON:</span></span>

* <span data-ttu-id="9e8eb-235">Właściwość `op` wskazuje typ operacji.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-235">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="9e8eb-236">Właściwość `path` wskazuje element do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-236">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="9e8eb-237">Właściwość `value` udostępnia nową wartość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-237">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="9e8eb-238">Zasób po zastosowaniu poprawki</span><span class="sxs-lookup"><span data-stu-id="9e8eb-238">Resource after patch</span></span>

<span data-ttu-id="9e8eb-239">Poniżej znajduje się zasób po zastosowaniu poprzedniego dokumentu poprawki JSON:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-239">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="9e8eb-240">Zmiany wprowadzone przez zastosowanie dokumentu poprawek JSON do zasobu są niepodzielne: Jeśli jakakolwiek operacja na liście nie powiedzie się, nie zostanie zastosowana żadna operacja na liście.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-240">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="9e8eb-241">Składnia ścieżki</span><span class="sxs-lookup"><span data-stu-id="9e8eb-241">Path syntax</span></span>

<span data-ttu-id="9e8eb-242">Właściwość [Path](https://tools.ietf.org/html/rfc6901) obiektu operacji ma ukośniki między poziomami.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-242">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="9e8eb-243">Na przykład `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-243">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="9e8eb-244">W celu określenia elementów tablicy są używane indeksy oparte na wartości zero.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-244">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="9e8eb-245">Pierwszy element tablicy `addresses` będzie miał `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-245">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="9e8eb-246">Aby `add` do końca tablicy, użyj łącznika (-), a nie numeru indeksu: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-246">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="9e8eb-247">Operacje</span><span class="sxs-lookup"><span data-stu-id="9e8eb-247">Operations</span></span>

<span data-ttu-id="9e8eb-248">W poniższej tabeli przedstawiono obsługiwane operacje zgodnie z definicją w [specyfikacji poprawek JSON](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="9e8eb-248">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="9e8eb-249">Operacja</span><span class="sxs-lookup"><span data-stu-id="9e8eb-249">Operation</span></span>  | <span data-ttu-id="9e8eb-250">Uwagi</span><span class="sxs-lookup"><span data-stu-id="9e8eb-250">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="9e8eb-251">Dodaj właściwość lub element tablicy.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-251">Add a property or array element.</span></span> <span data-ttu-id="9e8eb-252">Dla istniejącej właściwości: Ustaw wartość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-252">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="9e8eb-253">Usuń właściwość lub element tablicy.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-253">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="9e8eb-254">Taka sama jak `remove`, a następnie `add` w tej samej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-254">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="9e8eb-255">Taka sama jak `remove` ze źródła, po której następuje `add` do miejsca docelowego przy użyciu wartości ze źródła.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-255">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="9e8eb-256">Analogicznie jak `add` do miejsca docelowego przy użyciu wartości ze źródła.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-256">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="9e8eb-257">Zwróć kod stanu sukcesu, jeśli wartość w `path` = dostarczona `value`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-257">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="9e8eb-258">JsonPatch w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e8eb-258">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="9e8eb-259">ASP.NET Core implementacja poprawki JSON jest dostępna w pakiecie NuGet [Microsoft. AspNetCore. JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) .</span><span class="sxs-lookup"><span data-stu-id="9e8eb-259">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="9e8eb-260">Pakiet jest zawarty w pakiecie [Microsoft. AspnetCore. app](xref:fundamentals/metapackage-app) .</span><span class="sxs-lookup"><span data-stu-id="9e8eb-260">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="9e8eb-261">Kod metody akcji</span><span class="sxs-lookup"><span data-stu-id="9e8eb-261">Action method code</span></span>

<span data-ttu-id="9e8eb-262">W kontrolerze interfejsu API Metoda akcji dla poprawki JSON:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-262">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="9e8eb-263">Ma adnotację z atrybutem `HttpPatch`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-263">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="9e8eb-264">Akceptuje `JsonPatchDocument<T>`, zazwyczaj z `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-264">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="9e8eb-265">Wywołuje `ApplyTo` w dokumencie poprawki, aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-265">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="9e8eb-266">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-266">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="9e8eb-267">Ten kod z przykładowej aplikacji współdziała z poniższym modelem `Customer`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-267">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="9e8eb-268">Przykładowa Metoda akcji:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-268">The sample action method:</span></span>

* <span data-ttu-id="9e8eb-269">Konstruuje `Customer`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-269">Constructs a `Customer`.</span></span>
* <span data-ttu-id="9e8eb-270">Stosuje poprawkę.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-270">Applies the patch.</span></span>
* <span data-ttu-id="9e8eb-271">Zwraca wynik w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-271">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="9e8eb-272">W rzeczywistej aplikacji kod pobiera dane z magazynu, takiego jak baza danych, i aktualizuje bazę danych po zastosowaniu poprawki.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-272">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="9e8eb-273">Stan modelu</span><span class="sxs-lookup"><span data-stu-id="9e8eb-273">Model state</span></span>

<span data-ttu-id="9e8eb-274">Poprzednia metoda działania przykład wywołuje Przeciążenie `ApplyTo`, które pobiera stan modelu jako jeden z jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-274">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="9e8eb-275">W przypadku tej opcji można odbierać komunikaty o błędach w odpowiedziach.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-275">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="9e8eb-276">Poniższy przykład przedstawia treść nieprawidłowej odpowiedzi na żądanie 400 dla operacji `test`:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-276">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="9e8eb-277">Obiekty dynamiczne</span><span class="sxs-lookup"><span data-stu-id="9e8eb-277">Dynamic objects</span></span>

<span data-ttu-id="9e8eb-278">W poniższym przykładzie metody akcji pokazano, jak zastosować poprawkę do obiektu dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-278">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="9e8eb-279">Operacja dodawania</span><span class="sxs-lookup"><span data-stu-id="9e8eb-279">The add operation</span></span>

* <span data-ttu-id="9e8eb-280">Jeśli `path` wskazuje element tablicy: wstawia nowy element przed określony przez `path`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-280">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="9e8eb-281">Jeśli `path` wskazuje Właściwość: ustawia wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-281">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="9e8eb-282">Jeśli `path` wskazuje nieistniejącą lokalizację:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-282">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="9e8eb-283">Jeśli zasób do poprawki jest obiektem dynamicznym: dodaje właściwość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-283">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="9e8eb-284">Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-284">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="9e8eb-285">Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i dodaje obiekt `Order` do końca tablicy `Orders`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-285">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="9e8eb-286">Operacja usuwania</span><span class="sxs-lookup"><span data-stu-id="9e8eb-286">The remove operation</span></span>

* <span data-ttu-id="9e8eb-287">Jeśli `path` wskazuje element tablicy: usuwa element.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-287">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="9e8eb-288">Jeśli `path` wskazuje Właściwość:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-288">If `path` points to a property:</span></span>
  * <span data-ttu-id="9e8eb-289">Jeśli zasób do poprawki jest obiektem dynamicznym: usuwa właściwość.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-289">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="9e8eb-290">Jeśli zasób do poprawki jest obiektem statycznym:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-290">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="9e8eb-291">Jeśli właściwość dopuszcza wartość null: ustawia ją na wartość null.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-291">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="9e8eb-292">Jeśli właściwość nie dopuszcza wartości null, ustawia ją na `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-292">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="9e8eb-293">Następujący przykładowy dokument poprawek ustawia `CustomerName` na null i usuwa `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-293">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="9e8eb-294">Operacja zamiany</span><span class="sxs-lookup"><span data-stu-id="9e8eb-294">The replace operation</span></span>

<span data-ttu-id="9e8eb-295">Ta operacja jest funkcjonalnie taka sama jak `remove` i `add`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-295">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="9e8eb-296">Następujący przykładowy dokument poprawek ustawia wartość `CustomerName` i zastępuje `Orders[0]`nowym obiektem `Order`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-296">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="9e8eb-297">Operacja przenoszenia</span><span class="sxs-lookup"><span data-stu-id="9e8eb-297">The move operation</span></span>

* <span data-ttu-id="9e8eb-298">Jeśli `path` wskazuje element tablicy: kopiuje `from` element do lokalizacji `path` elementu, a następnie uruchamia operację `remove` na `from` elemencie.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-298">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="9e8eb-299">Jeśli `path` wskazuje Właściwość: kopiuje wartość właściwości `from` do właściwości `path`, a następnie uruchamia operację `remove` na właściwości `from`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-299">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="9e8eb-300">Jeśli `path` wskazuje nieistniejącą Właściwość:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-300">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="9e8eb-301">Jeśli zasób do poprawki jest obiektem statycznym: żądanie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-301">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="9e8eb-302">Jeśli zasób do naprawienia jest obiektem dynamicznym: kopiuje Właściwość `from` do lokalizacji wskazywanej przez `path`, a następnie uruchamia operację `remove` na właściwości `from`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-302">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="9e8eb-303">Następujący przykładowy dokument poprawek:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-303">The following sample patch document:</span></span>

* <span data-ttu-id="9e8eb-304">Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-304">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="9e8eb-305">Ustawia `Orders[0].OrderName` na wartość null.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-305">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="9e8eb-306">Przenosi `Orders[1]` do przed `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-306">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="9e8eb-307">Operacja kopiowania</span><span class="sxs-lookup"><span data-stu-id="9e8eb-307">The copy operation</span></span>

<span data-ttu-id="9e8eb-308">Ta operacja jest funkcjonalnie taka sama jak operacja `move` bez kroku `remove` końcowego.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-308">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="9e8eb-309">Następujący przykładowy dokument poprawek:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-309">The following sample patch document:</span></span>

* <span data-ttu-id="9e8eb-310">Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-310">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="9e8eb-311">Wstawia kopię `Orders[1]` przed `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-311">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="9e8eb-312">Operacja testowa</span><span class="sxs-lookup"><span data-stu-id="9e8eb-312">The test operation</span></span>

<span data-ttu-id="9e8eb-313">Jeśli wartość w lokalizacji wskazywanej przez `path` różni się od wartości podanej w `value`, żądanie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-313">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="9e8eb-314">W takim przypadku całe żądanie PATCH kończy się niepowodzeniem, nawet jeśli wszystkie inne operacje w dokumencie poprawki zakończyły się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-314">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="9e8eb-315">Operacja `test` jest często używana do zapobiegania aktualizacji, gdy występuje konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-315">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="9e8eb-316">Następujący przykładowy dokument poprawek nie ma wpływu, jeśli początkowa wartość `CustomerName` to "Jan", ponieważ test kończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-316">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="9e8eb-317">Uzyskiwanie kodu</span><span class="sxs-lookup"><span data-stu-id="9e8eb-317">Get the code</span></span>

<span data-ttu-id="9e8eb-318">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="9e8eb-318">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="9e8eb-319">([Jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9e8eb-319">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="9e8eb-320">Aby przetestować przykład, uruchom aplikację i Wyślij żądania HTTP z następującymi ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="9e8eb-320">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="9e8eb-321">Adres URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="9e8eb-321">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="9e8eb-322">Metoda HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="9e8eb-322">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="9e8eb-323">Nagłówek: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="9e8eb-323">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="9e8eb-324">Treść: Skopiuj i wklej jeden z przykładów dokumentu poprawki JSON z folderu projektu *JSON* .</span><span class="sxs-lookup"><span data-stu-id="9e8eb-324">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e8eb-325">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9e8eb-325">Additional resources</span></span>

* [<span data-ttu-id="9e8eb-326">IETF RFC 5789 Specyfikacja metody poprawek</span><span class="sxs-lookup"><span data-stu-id="9e8eb-326">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="9e8eb-327">IETF RFC 6902 — Specyfikacja poprawek JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-327">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="9e8eb-328">IETF RFC 6901 JSON Format ścieżki poprawek</span><span class="sxs-lookup"><span data-stu-id="9e8eb-328">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="9e8eb-329">[Dokumentacja poprawek JSON](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="9e8eb-329">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="9e8eb-330">Zawiera linki do zasobów do tworzenia dokumentów poprawek JSON.</span><span class="sxs-lookup"><span data-stu-id="9e8eb-330">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="9e8eb-331">ASP.NET Core kod źródłowy poprawki JSON</span><span class="sxs-lookup"><span data-stu-id="9e8eb-331">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
