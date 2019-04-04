---
title: JsonPatch w interfejsie web API platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak obsługiwać żądania poprawki JSON w interfejsie web API platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: c5330f778ef202d366db5cdff5345d0db43a5c1b
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59013215"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="d1de3-103">JsonPatch w interfejsie web API platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d1de3-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="d1de3-104">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d1de3-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d1de3-105">W tym artykule opisano sposób obsługi żądań poprawki JSON w interfejsie web API platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d1de3-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="d1de3-106">Metoda żądania PATCH HTTP</span><span class="sxs-lookup"><span data-stu-id="d1de3-106">PATCH HTTP request method</span></span>

<span data-ttu-id="d1de3-107">PUT i [PATCH](https://tools.ietf.org/html/rfc5789) metody są używane do aktualizowania istniejącego zasobu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-107">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="d1de3-108">Różnica między nimi polega na tym, PUT zamienia cały zasób, podczas gdy poprawka określa tylko zmiany.</span><span class="sxs-lookup"><span data-stu-id="d1de3-108">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="d1de3-109">Poprawka JSON</span><span class="sxs-lookup"><span data-stu-id="d1de3-109">JSON Patch</span></span>

<span data-ttu-id="d1de3-110">[Poprawka JSON](https://tools.ietf.org/html/rfc6902) to format służącą do aktualizacji, które mają być stosowane do zasobu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="d1de3-111">Dokument poprawki JSON zawiera tablicę *operacji*.</span><span class="sxs-lookup"><span data-stu-id="d1de3-111">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="d1de3-112">Każda operacja identyfikuje określonego typu zmiany, takie jak dodawanie elementu tablicy lub Zastąp wartość właściwości.</span><span class="sxs-lookup"><span data-stu-id="d1de3-112">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="d1de3-113">Na przykład następujące dokumenty JSON reprezentują zasobem, dokument poprawki JSON dla danego zasobu i wynik zastosowania operacji patch.</span><span class="sxs-lookup"><span data-stu-id="d1de3-113">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="d1de3-114">Przykład zasobów</span><span class="sxs-lookup"><span data-stu-id="d1de3-114">Resource example</span></span>

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="d1de3-115">Przykład kodu JSON z poprawek</span><span class="sxs-lookup"><span data-stu-id="d1de3-115">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="d1de3-116">W poprzednim formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="d1de3-116">In the preceding JSON:</span></span>

* <span data-ttu-id="d1de3-117">`op` Właściwość wskazuje typ operacji.</span><span class="sxs-lookup"><span data-stu-id="d1de3-117">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="d1de3-118">`path` Właściwość wskazuje element do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="d1de3-118">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="d1de3-119">`value` Dostarcza nową wartość.</span><span class="sxs-lookup"><span data-stu-id="d1de3-119">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="d1de3-120">Zasób po poprawki</span><span class="sxs-lookup"><span data-stu-id="d1de3-120">Resource after patch</span></span>

<span data-ttu-id="d1de3-121">Oto zasób, po zastosowaniu poprzedni dokument poprawki JSON:</span><span class="sxs-lookup"><span data-stu-id="d1de3-121">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="d1de3-122">Zmiany wprowadzone przez zastosowanie dokumentu poprawki JSON do zasobu są niepodzielne: w przypadku niepowodzenia żadnych operacji na liście jest stosowana żadna operacja na liście.</span><span class="sxs-lookup"><span data-stu-id="d1de3-122">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="d1de3-123">Składnia ścieżki</span><span class="sxs-lookup"><span data-stu-id="d1de3-123">Path syntax</span></span>

<span data-ttu-id="d1de3-124">[Ścieżki](http://tools.ietf.org/html/rfc6901) właściwości obiektu operacja ma ukośników poziomów.</span><span class="sxs-lookup"><span data-stu-id="d1de3-124">The [path](http://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="d1de3-125">Na przykład `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-125">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="d1de3-126">Liczony od zera indeksy są używane do określenia elementów tablicy.</span><span class="sxs-lookup"><span data-stu-id="d1de3-126">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="d1de3-127">Pierwszy element `addresses` tablicy byłoby w `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-127">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="d1de3-128">Aby `add` do końca tablicy, należy użyć łącznika (-) zamiast numer indeksu: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-128">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="d1de3-129">Operacje</span><span class="sxs-lookup"><span data-stu-id="d1de3-129">Operations</span></span>

<span data-ttu-id="d1de3-130">W poniższej tabeli przedstawiono obsługiwane operacje, zgodnie z definicją w [specyfikacji poprawki JSON](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="d1de3-130">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="d1de3-131">Operacja</span><span class="sxs-lookup"><span data-stu-id="d1de3-131">Operation</span></span>  | <span data-ttu-id="d1de3-132">Uwagi</span><span class="sxs-lookup"><span data-stu-id="d1de3-132">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="d1de3-133">Dodaj właściwość lub element tablicy.</span><span class="sxs-lookup"><span data-stu-id="d1de3-133">Add a property or array element.</span></span> <span data-ttu-id="d1de3-134">Dla istniejących właściwości: Ustaw wartość.</span><span class="sxs-lookup"><span data-stu-id="d1de3-134">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="d1de3-135">Usuń właściwość lub element tablicy.</span><span class="sxs-lookup"><span data-stu-id="d1de3-135">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="d1de3-136">Taki sam jak `remove` następuje `add` w tej samej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="d1de3-136">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="d1de3-137">Taki sam jak `remove` ze źródła, a następnie `add` do miejsca docelowego przy użyciu wartości ze źródła.</span><span class="sxs-lookup"><span data-stu-id="d1de3-137">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="d1de3-138">Taki sam jak `add` do miejsca docelowego przy użyciu wartości ze źródła.</span><span class="sxs-lookup"><span data-stu-id="d1de3-138">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="d1de3-139">Zwraca kod stanu powodzenia, jeśli wartość `path` = pod warunkiem `value`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-139">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="d1de3-140">JsonPatch in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d1de3-140">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="d1de3-141">Implementacja programu ASP.NET Core poprawki JSON jest udostępniany w [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="d1de3-141">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="d1de3-142">Pakiet znajduje się w [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) meta Microsoft.aspnetcore.all.</span><span class="sxs-lookup"><span data-stu-id="d1de3-142">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="d1de3-143">Kod metody akcji</span><span class="sxs-lookup"><span data-stu-id="d1de3-143">Action method code</span></span>

<span data-ttu-id="d1de3-144">W kontrolerze interfejsu API metody akcji poprawki JSON:</span><span class="sxs-lookup"><span data-stu-id="d1de3-144">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="d1de3-145">Jest oznaczony za pomocą `HttpPatch` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-145">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="d1de3-146">Akceptuje `JsonPatchDocument<T>`, zwykle z [FromBody].</span><span class="sxs-lookup"><span data-stu-id="d1de3-146">Accepts a `JsonPatchDocument<T>`, typically with [FromBody].</span></span>
* <span data-ttu-id="d1de3-147">Wywołania `ApplyTo` na dokument poprawki, aby zastosować zmiany.</span><span class="sxs-lookup"><span data-stu-id="d1de3-147">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="d1de3-148">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="d1de3-148">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="d1de3-149">Ten kod z przykładowej aplikacji współdziała z następującymi `Customer` modelu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-149">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="d1de3-150">Przykładowa metoda akcji:</span><span class="sxs-lookup"><span data-stu-id="d1de3-150">The sample action method:</span></span>

* <span data-ttu-id="d1de3-151">Konstruuje `Customer`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-151">Constructs a `Customer`.</span></span>
* <span data-ttu-id="d1de3-152">Stosuje poprawki.</span><span class="sxs-lookup"><span data-stu-id="d1de3-152">Applies the patch.</span></span>
* <span data-ttu-id="d1de3-153">Zwraca wynik w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d1de3-153">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="d1de3-154">W rzeczywistej aplikacji kod będzie pobierać dane z magazynu, takich jak bazy danych i aktualizują bazę danych po zastosowaniu poprawki.</span><span class="sxs-lookup"><span data-stu-id="d1de3-154">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="d1de3-155">Stan modelu</span><span class="sxs-lookup"><span data-stu-id="d1de3-155">Model state</span></span>

<span data-ttu-id="d1de3-156">W poprzednim przykładzie metoda akcji wywołuje przeciążenia `ApplyTo` która pobiera stan modelu jako jeden z jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="d1de3-156">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="d1de3-157">Po wybraniu tej opcji można uzyskać komunikaty o błędach w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d1de3-157">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="d1de3-158">W poniższym przykładzie pokazano treści 400 Niewłaściwe żądanie odpowiedzi dla `test` operacji:</span><span class="sxs-lookup"><span data-stu-id="d1de3-158">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="d1de3-159">Obiekty dynamiczne</span><span class="sxs-lookup"><span data-stu-id="d1de3-159">Dynamic objects</span></span>

<span data-ttu-id="d1de3-160">W poniższym przykładzie metoda akcji pokazano, jak można zastosować poprawki do obiekt dynamiczny.</span><span class="sxs-lookup"><span data-stu-id="d1de3-160">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="d1de3-161">Operacji dodawania</span><span class="sxs-lookup"><span data-stu-id="d1de3-161">The add operation</span></span>

* <span data-ttu-id="d1de3-162">Jeśli `path` wskazuje elementu tablicy: Wstawia nowy element przed określonego przez `path`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-162">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="d1de3-163">Jeśli `path` wskazuje właściwość: ustawia wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="d1de3-163">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="d1de3-164">Jeśli `path` nieistniejącej lokalizację:</span><span class="sxs-lookup"><span data-stu-id="d1de3-164">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="d1de3-165">Jeśli zasób zastosowania poprawki jest to obiekt dynamiczny: Dodaje właściwość typu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-165">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="d1de3-166">Jeśli zasób do poprawki jest obiektem statyczne: żądanie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="d1de3-166">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="d1de3-167">Następującego przykładowego dokumentu poprawki ustawia wartość `CustomerName` i dodaje `Order` obiekt na koniec `Orders` tablicy.</span><span class="sxs-lookup"><span data-stu-id="d1de3-167">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="d1de3-168">Operacja usuwania</span><span class="sxs-lookup"><span data-stu-id="d1de3-168">The remove operation</span></span>

* <span data-ttu-id="d1de3-169">Jeśli `path` wskazuje elementu tablicy: usuwa element.</span><span class="sxs-lookup"><span data-stu-id="d1de3-169">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="d1de3-170">Jeśli `path` wskazuje właściwość:</span><span class="sxs-lookup"><span data-stu-id="d1de3-170">If `path` points to a property:</span></span>
  * <span data-ttu-id="d1de3-171">Jeśli zasób zastosowania poprawki jest to obiekt dynamiczny: Usuwa właściwość.</span><span class="sxs-lookup"><span data-stu-id="d1de3-171">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="d1de3-172">Jeśli zasób do poprawki obiektu statycznego:</span><span class="sxs-lookup"><span data-stu-id="d1de3-172">If resource to patch is a static object:</span></span> 
    * <span data-ttu-id="d1de3-173">Jeśli właściwość ma wartość null: ustawia ją na wartość null.</span><span class="sxs-lookup"><span data-stu-id="d1de3-173">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="d1de3-174">Jeśli właściwość ma wartość null, ustawia ją na `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-174">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="d1de3-175">Następujące przykładowe zestawy dokumentów poprawek `CustomerName` o wartości null i usuwa `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-175">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="d1de3-176">Operacja zamiany</span><span class="sxs-lookup"><span data-stu-id="d1de3-176">The replace operation</span></span>

<span data-ttu-id="d1de3-177">Ta operacja jest funkcjonalnie taka sama jak `remove` następuje `add`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-177">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="d1de3-178">Następującego przykładowego dokumentu poprawki ustawia wartość `CustomerName` i zastępuje `Orders[0]`dzięki nowemu `Order` obiektu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-178">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="d1de3-179">Operacji przenoszenia</span><span class="sxs-lookup"><span data-stu-id="d1de3-179">The move operation</span></span>

* <span data-ttu-id="d1de3-180">Jeśli `path` wskazuje elementu tablicy: kopie `from` elementu do lokalizacji `path` elementu, następnie uruchamia `remove` operacja `from` elementu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-180">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="d1de3-181">Jeśli `path` wskazuje właściwość: kopiuje wartość `from` właściwości `path` właściwości, następnie uruchamia `remove` operacja `from` właściwości.</span><span class="sxs-lookup"><span data-stu-id="d1de3-181">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="d1de3-182">Jeśli `path` wskazuje nieistniejącą właściwość:</span><span class="sxs-lookup"><span data-stu-id="d1de3-182">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="d1de3-183">Jeśli zasób do poprawki jest obiektem statyczne: żądanie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="d1de3-183">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="d1de3-184">Jeśli zasób zastosowania poprawki jest to obiekt dynamiczny: kopie `from` właściwość lokalizacji wskazanej przez `path`, następnie uruchamia `remove` operacja `from` właściwości.</span><span class="sxs-lookup"><span data-stu-id="d1de3-184">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="d1de3-185">Następującego przykładowego dokumentu poprawki:</span><span class="sxs-lookup"><span data-stu-id="d1de3-185">The following sample patch document:</span></span>

* <span data-ttu-id="d1de3-186">Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-186">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="d1de3-187">Zestawy `Orders[0].OrderName` na wartość null.</span><span class="sxs-lookup"><span data-stu-id="d1de3-187">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="d1de3-188">Przenosi `Orders[1]` przed `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-188">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="d1de3-189">Operacja kopiowania</span><span class="sxs-lookup"><span data-stu-id="d1de3-189">The copy operation</span></span>

<span data-ttu-id="d1de3-190">Ta operacja jest funkcjonalnie taka sama jak `move` operację bez końcowe `remove` kroku.</span><span class="sxs-lookup"><span data-stu-id="d1de3-190">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="d1de3-191">Następującego przykładowego dokumentu poprawki:</span><span class="sxs-lookup"><span data-stu-id="d1de3-191">The following sample patch document:</span></span>

* <span data-ttu-id="d1de3-192">Kopiuje wartość `Orders[0].OrderName` do `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-192">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="d1de3-193">Wstawia kopię `Orders[1]` przed `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="d1de3-193">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="d1de3-194">Operacja testowania</span><span class="sxs-lookup"><span data-stu-id="d1de3-194">The test operation</span></span>

<span data-ttu-id="d1de3-195">Jeśli wartość w miejscu wskazywanym przez `path` różni się od podanej wartości w `value`, żądanie kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="d1de3-195">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="d1de3-196">W takim przypadku całego żądanie PATCH nie powiedzie się nawet wtedy, gdy wszystkie inne operacje w dokumencie poprawki, w przeciwnym razie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="d1de3-196">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="d1de3-197">`test` Operacji jest najczęściej używany do uniknięcia aktualizacji, gdy występuje konflikt współbieżności.</span><span class="sxs-lookup"><span data-stu-id="d1de3-197">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="d1de3-198">Następującego przykładowego dokumentu poprawki nie obowiązuje, jeśli wartość początkową `CustomerName` jest "John", ponieważ test zakończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="d1de3-198">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="d1de3-199">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="d1de3-199">Get the code</span></span>

<span data-ttu-id="d1de3-200">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="d1de3-200">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="d1de3-201">([Sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d1de3-201">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="d1de3-202">Aby przetestować próbki, należy uruchomić aplikację i wysyłać żądania HTTP z następującymi ustawieniami:</span><span class="sxs-lookup"><span data-stu-id="d1de3-202">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="d1de3-203">ADRES URL:</span><span class="sxs-lookup"><span data-stu-id="d1de3-203">URL:</span></span> `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* <span data-ttu-id="d1de3-204">Metoda HTTP:</span><span class="sxs-lookup"><span data-stu-id="d1de3-204">HTTP method:</span></span> `PATCH`
* <span data-ttu-id="d1de3-205">Nagłówek:</span><span class="sxs-lookup"><span data-stu-id="d1de3-205">Header:</span></span> `Content-Type: application/json-patch+json`
* <span data-ttu-id="d1de3-206">Body: Skopiuj i Wklej jeden z przykładów dokumentu poprawki JSON z *JSON* folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="d1de3-206">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1de3-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d1de3-207">Additional resources</span></span>

* [<span data-ttu-id="d1de3-208">Specyfikacja IETF RFC 5789 PATCH — metoda</span><span class="sxs-lookup"><span data-stu-id="d1de3-208">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="d1de3-209">Specyfikacja IETF RFC 6902 JSON poprawki</span><span class="sxs-lookup"><span data-stu-id="d1de3-209">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="d1de3-210">Specyfikacja formatu ścieżki poprawki JSON 6901 RFC organizacji IETF</span><span class="sxs-lookup"><span data-stu-id="d1de3-210">IETF RFC 6901 JSON Patch path format spec</span></span>](http://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="d1de3-211">[Dokumentacja poprawki JSON](http://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="d1de3-211">[JSON Patch documentation](http://jsonpatch.com/).</span></span> <span data-ttu-id="d1de3-212">Zawiera łącza do zasobów dotyczących tworzenia dokumentów poprawki JSON.</span><span class="sxs-lookup"><span data-stu-id="d1de3-212">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="d1de3-213">Kod źródłowy poprawki JSON Core ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d1de3-213">ASP.NET Core JSON Patch source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
