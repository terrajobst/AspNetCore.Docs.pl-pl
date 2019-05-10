---
title: Zwracane typy akcji kontrolera platformy ASP.NET Core Web API
author: scottaddie
description: Więcej informacji na temat w interfejsie API sieci Web platformy ASP.NET Core przy użyciu różnych metod zwracane typy akcji kontrolera.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: 180d76c2c2e53dbf64b8fcc5cdc6d2b6f4dab6eb
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900685"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="a09db-103">Zwracane typy akcji kontrolera platformy ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="a09db-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="a09db-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="a09db-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="a09db-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a09db-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a09db-106">Platforma ASP.NET Core oferuje następujące opcje dla akcji kontrolera interfejsu API sieci Web zwracane typy:</span><span class="sxs-lookup"><span data-stu-id="a09db-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="a09db-107">Określony typ</span><span class="sxs-lookup"><span data-stu-id="a09db-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="a09db-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="a09db-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="a09db-109">ActionResult\<T></span><span class="sxs-lookup"><span data-stu-id="a09db-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="a09db-110">Określony typ</span><span class="sxs-lookup"><span data-stu-id="a09db-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="a09db-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="a09db-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="a09db-112">W tym dokumencie wyjaśniono, gdy jest najbardziej odpowiednie do użycia każdy typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="a09db-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="a09db-113">Określony typ</span><span class="sxs-lookup"><span data-stu-id="a09db-113">Specific type</span></span>

<span data-ttu-id="a09db-114">Najprostsza akcji zwraca typ danych pierwotnych lub złożonych (na przykład `string` lub typ niestandardowy obiekt).</span><span class="sxs-lookup"><span data-stu-id="a09db-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="a09db-115">Należy wziąć pod uwagę następujące akcji, która zwraca kolekcję obiektów niestandardowych `Product` obiektów:</span><span class="sxs-lookup"><span data-stu-id="a09db-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="a09db-116">Bez znanych warunków, aby zabezpieczyć się przed podczas wykonywania akcji zwracając określonego typu można wystarczające.</span><span class="sxs-lookup"><span data-stu-id="a09db-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="a09db-117">Poprzednią akcję przyjmuje żadnych parametrów, więc Walidacja ograniczenia parametru nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="a09db-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="a09db-118">Jeśli wiadomo, warunki, które należy wyjaśnić dla akcji, wprowadzono wiele ścieżek zwrotu.</span><span class="sxs-lookup"><span data-stu-id="a09db-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="a09db-119">W takim przypadku jest często mieszać [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) zwracany typ z typem zwracanym pierwotnych lub złożonych.</span><span class="sxs-lookup"><span data-stu-id="a09db-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="a09db-120">Albo [IActionResult](#iactionresult-type) lub [ActionResult\<T >](#actionresultt-type) są niezbędne uwzględnić ten typ akcji.</span><span class="sxs-lookup"><span data-stu-id="a09db-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="a09db-121">Typ IActionResult</span><span class="sxs-lookup"><span data-stu-id="a09db-121">IActionResult type</span></span>

<span data-ttu-id="a09db-122">[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) zwracany typ, jest odpowiednie, gdy wiele [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) zwraca typy są możliwe w akcji.</span><span class="sxs-lookup"><span data-stu-id="a09db-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="a09db-123">`ActionResult` Typy reprezentują różne kody stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a09db-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="a09db-124">Niektóre typowe typy zwracane należących do tej kategorii [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) i [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="a09db-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="a09db-125">Ponieważ istnieje wiele typów zwracanych i ścieżek w działaniu, liberalne użytkowania [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) atrybut jest konieczny.</span><span class="sxs-lookup"><span data-stu-id="a09db-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="a09db-126">Ten atrybut daje bardziej opisowe szczegóły odpowiedzi dla stron pomocy interfejsu API wygenerowanych przez narzędzia, takie jak [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="a09db-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="a09db-127">`[ProducesResponseType]` Wskazuje, znanych typów oraz kodów stanu HTTP, które mają zostać zwrócone przez akcję.</span><span class="sxs-lookup"><span data-stu-id="a09db-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="a09db-128">Akcja synchroniczne</span><span class="sxs-lookup"><span data-stu-id="a09db-128">Synchronous action</span></span>

<span data-ttu-id="a09db-129">Rozważmy następującą akcję synchroniczne, w którym istnieją dwie możliwe zwracane typy:</span><span class="sxs-lookup"><span data-stu-id="a09db-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="a09db-130">W poprzednich akcji, kod stanu 404 jest zwracane, gdy produkt jest reprezentowany przez `id` nie istnieje w podstawowym magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="a09db-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="a09db-131">[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) pomocnika metoda jest wywoływana w celu szybkiego `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="a09db-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="a09db-132">Jeśli istnieje produktu, `Product` obiekt reprezentujący ładunek zwracany jest kod stanu 200.</span><span class="sxs-lookup"><span data-stu-id="a09db-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="a09db-133">[Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) metody pomocnika jest wywoływana jako formę skróconą `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="a09db-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="a09db-134">Akcja asynchroniczna</span><span class="sxs-lookup"><span data-stu-id="a09db-134">Asynchronous action</span></span>

<span data-ttu-id="a09db-135">Należy wziąć pod uwagę następujące akcję asynchroniczną, w której istnieją dwie możliwe zwracane typy:</span><span class="sxs-lookup"><span data-stu-id="a09db-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="a09db-136">W poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="a09db-136">In the preceding code:</span></span>

* <span data-ttu-id="a09db-137">Kod stanu 400 ([element BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) jest zwracany przez środowisko uruchomieniowe programu ASP.NET Core, gdy opis produktu zawiera "XYZ widżet".</span><span class="sxs-lookup"><span data-stu-id="a09db-137">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when the product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="a09db-138">Kod stanu 201 jest generowany przez [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) metody, gdy produkt jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="a09db-138">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="a09db-139">W tej ścieżce kodu `Product` obiekt jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="a09db-139">In this code path, the `Product` object is returned.</span></span>

<span data-ttu-id="a09db-140">Na przykład, następujący model wskazuje, że żądania muszą zawierać `Name` i `Description` właściwości.</span><span class="sxs-lookup"><span data-stu-id="a09db-140">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="a09db-141">W związku z tym, nie można podać `Name` i `Description` w żądaniu powoduje, że sprawdzanie poprawności modelu nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="a09db-141">Therefore, failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a09db-142">Jeśli [[klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) w platformy ASP.NET Core 2.1 lub nowszej jest stosowany, kod stanu 400 powodować błędy sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="a09db-142">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later  is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="a09db-143">Aby uzyskać więcej informacji, zobacz [odpowiedzi automatyczne HTTP 400](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="a09db-143">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="a09db-144">ActionResult\<T > typu</span><span class="sxs-lookup"><span data-stu-id="a09db-144">ActionResult\<T> type</span></span>

<span data-ttu-id="a09db-145">Platforma ASP.NET Core 2.1 wprowadzono [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) zwracany typ dla akcji kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a09db-145">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="a09db-146">Umożliwia ona zwracany typ pochodząca od [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) lub je zwracają [określonego typu](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="a09db-146">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="a09db-147">`ActionResult<T>` oferuje następujące korzyści za pośrednictwem [typu IActionResult](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="a09db-147">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="a09db-148">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) atrybutu `Type` właściwości mogą być wyłączone.</span><span class="sxs-lookup"><span data-stu-id="a09db-148">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="a09db-149">Na przykład `[ProducesResponseType(200, Type = typeof(Product))]` została uproszczona w celu `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="a09db-149">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="a09db-150">Akcja użytkownika oczekiwany typ zwracany zamiast tego jest wnioskowany z `T` w `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="a09db-150">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="a09db-151">[Operatory rzutowania niejawnego](/dotnet/csharp/language-reference/keywords/implicit) obsługi zarówno konwersji `T` i `ActionResult` do `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="a09db-151">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="a09db-152">`T` Konwertuje [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), co oznacza, że `return new ObjectResult(T);` została uproszczona w celu `return T;`.</span><span class="sxs-lookup"><span data-stu-id="a09db-152">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="a09db-153">C# nie obsługuje operatory niejawne Rzutowanie na interfejsach.</span><span class="sxs-lookup"><span data-stu-id="a09db-153">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="a09db-154">W związku z tym, konwersja interfejsu na konkretny typ jest niezbędne do korzystania z `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="a09db-154">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="a09db-155">Na przykład użycie `IEnumerable` nie działa w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="a09db-155">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="a09db-156">Jedną opcję, aby naprawić poprzedni kod ma zwrócić `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="a09db-156">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="a09db-157">Większość akcji ma określony typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="a09db-157">Most actions have a specific return type.</span></span> <span data-ttu-id="a09db-158">Nieoczekiwane warunki mogą wystąpić podczas wykonywania akcji, w którym nie jest zwracany w przypadku określonego typu.</span><span class="sxs-lookup"><span data-stu-id="a09db-158">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="a09db-159">Na przykład parametr wejściowy akcja może zakończyć się niepowodzeniem weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="a09db-159">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="a09db-160">W takim przypadku jest wspólne do zwrócenia odpowiedniego `ActionResult` typu zamiast określonego typu.</span><span class="sxs-lookup"><span data-stu-id="a09db-160">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="a09db-161">Akcja synchroniczne</span><span class="sxs-lookup"><span data-stu-id="a09db-161">Synchronous action</span></span>

<span data-ttu-id="a09db-162">Należy wziąć pod uwagę synchroniczne akcji, w którym istnieją dwie możliwe zwracane typy:</span><span class="sxs-lookup"><span data-stu-id="a09db-162">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="a09db-163">W poprzednim kodzie kod stanu 404 jest zwracane, gdy produkt nie istnieje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a09db-163">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="a09db-164">Jeśli istnieje produktu, odpowiedni `Product` obiekt jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="a09db-164">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="a09db-165">Przed platformy ASP.NET Core 2.1 `return product;` byłby wiersza `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="a09db-165">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="a09db-166">Począwszy od platformy ASP.NET Core 2.1, wnioskowania źródła wiązania parametrów akcji jest włączane, gdy klasa kontrolera zostanie nadany `[ApiController]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a09db-166">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="a09db-167">Nazwa parametru pasujące do nazwy w szablonie trasy zostanie automatycznie powiązany za pomocą żądania danych trasy.</span><span class="sxs-lookup"><span data-stu-id="a09db-167">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="a09db-168">W związku z tym, poprzedniej akcji `id` parametr jawnie nie jest oznaczony za pomocą [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a09db-168">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="a09db-169">Akcja asynchroniczna</span><span class="sxs-lookup"><span data-stu-id="a09db-169">Asynchronous action</span></span>

<span data-ttu-id="a09db-170">Należy wziąć pod uwagę akcję asynchroniczną, w którym istnieją dwie możliwe zwracane typy:</span><span class="sxs-lookup"><span data-stu-id="a09db-170">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="a09db-171">W poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="a09db-171">In the preceding code:</span></span>

* <span data-ttu-id="a09db-172">Kod stanu 400 ([element BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) jest zwracany przez środowisko uruchomieniowe programu ASP.NET Core po:</span><span class="sxs-lookup"><span data-stu-id="a09db-172">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="a09db-173">[[Klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) zastosowano atrybut i sprawdzanie poprawności modelu nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="a09db-173">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="a09db-174">Opis produktu zawiera "XYZ widżet".</span><span class="sxs-lookup"><span data-stu-id="a09db-174">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="a09db-175">Kod stanu 201 jest generowany przez [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) metody, gdy produkt jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="a09db-175">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="a09db-176">W tej ścieżce kodu `Product` obiekt jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="a09db-176">In this code path, the `Product` object is returned.</span></span>

> [!TIP]
> <span data-ttu-id="a09db-177">Począwszy od platformy ASP.NET Core 2.1, wnioskowania źródła wiązania parametrów akcji jest włączane, gdy klasa kontrolera zostanie nadany `[ApiController]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a09db-177">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="a09db-178">Parametry typu złożonego automatycznie powiązane przy użyciu treści żądania.</span><span class="sxs-lookup"><span data-stu-id="a09db-178">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="a09db-179">W związku z tym, poprzedniej akcji `product` parametr jawnie nie jest oznaczony za pomocą [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a09db-179">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a09db-180">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a09db-180">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
