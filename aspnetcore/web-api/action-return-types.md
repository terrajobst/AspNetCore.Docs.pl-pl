---
title: Typy zwracane akcje kontrolera w ASP.NET Core Web API
author: scottaddie
description: Dowiedz się więcej o używaniu różnych typów zwracanych metody akcji kontrolera w interfejsie API sieci Web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 02/03/2020
uid: web-api/action-return-types
ms.openlocfilehash: aeea005abfcfd45a6fc94dfddfd65e60ffb15df8
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089191"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="39d8e-103">Typy zwracane akcje kontrolera w ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="39d8e-103">Controller action return types in ASP.NET Core web API</span></span>

<span data-ttu-id="39d8e-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="39d8e-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="39d8e-105">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="39d8e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="39d8e-106">ASP.NET Core oferuje następujące opcje dla zwracanych typów akcji kontrolera API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="39d8e-106">ASP.NET Core offers the following options for web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="39d8e-107">Określony typ</span><span class="sxs-lookup"><span data-stu-id="39d8e-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="39d8e-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="39d8e-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="39d8e-109">ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="39d8e-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="39d8e-110">Określony typ</span><span class="sxs-lookup"><span data-stu-id="39d8e-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="39d8e-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="39d8e-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="39d8e-112">Ten dokument wyjaśnia, kiedy jest najbardziej odpowiedni do użycia każdego typu zwracanego.</span><span class="sxs-lookup"><span data-stu-id="39d8e-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="39d8e-113">Określony typ</span><span class="sxs-lookup"><span data-stu-id="39d8e-113">Specific type</span></span>

<span data-ttu-id="39d8e-114">Najprostsza akcja zwraca typ danych pierwotnych lub złożonych (na przykład `string` lub niestandardowy typ obiektu).</span><span class="sxs-lookup"><span data-stu-id="39d8e-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="39d8e-115">Rozważ poniższą akcję, która zwraca kolekcję niestandardowych obiektów `Product`:</span><span class="sxs-lookup"><span data-stu-id="39d8e-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="39d8e-116">Bez znanych warunków zabezpieczających przed wykonaniem akcji, zwrócenie określonego typu może być wystarczające.</span><span class="sxs-lookup"><span data-stu-id="39d8e-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="39d8e-117">Poprzednia akcja nie akceptuje żadnych parametrów, więc Walidacja ograniczeń parametrów nie jest konieczna.</span><span class="sxs-lookup"><span data-stu-id="39d8e-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="39d8e-118">Gdy w akcji musi być uwzględnione znane warunki, zostaną wprowadzone wiele ścieżek powrotu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="39d8e-119">W takim przypadku typowym sposobem jest mieszanie <xref:Microsoft.AspNetCore.Mvc.ActionResult> zwracanego typu z typem pierwotnym lub złożonym typu zwracanego.</span><span class="sxs-lookup"><span data-stu-id="39d8e-119">In such a case, it's common to mix an <xref:Microsoft.AspNetCore.Mvc.ActionResult> return type with the primitive or complex return type.</span></span> <span data-ttu-id="39d8e-120">[IActionResult](#iactionresult-type) lub [ActionResult\<> t](#actionresultt-type) są niezbędne do uwzględnienia tego typu akcji.</span><span class="sxs-lookup"><span data-stu-id="39d8e-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

### <a name="return-ienumerablet-or-iasyncenumerablet"></a><span data-ttu-id="39d8e-121">Zwróć\<t > lub IAsyncEnumerable\<T ></span><span class="sxs-lookup"><span data-stu-id="39d8e-121">Return IEnumerable\<T> or IAsyncEnumerable\<T></span></span>

<span data-ttu-id="39d8e-122">W ASP.NET Core 2,2 i starszych, zwrócenie <xref:System.Collections.Generic.IEnumerable%601> z akcji powoduje synchroniczną iterację kolekcji przez serializator.</span><span class="sxs-lookup"><span data-stu-id="39d8e-122">In ASP.NET Core 2.2 and earlier, returning <xref:System.Collections.Generic.IEnumerable%601> from an action results in synchronous collection iteration by the serializer.</span></span> <span data-ttu-id="39d8e-123">Wynikiem jest blokowanie wywołań i potencjalna blokada puli wątków.</span><span class="sxs-lookup"><span data-stu-id="39d8e-123">The result is the blocking of calls and a potential for thread pool starvation.</span></span> <span data-ttu-id="39d8e-124">Aby zilustrować, Wyobraź sobie, że Entity Framework (EF) rdzeń jest używany do potrzeb dostępu do danych interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="39d8e-124">To illustrate, imagine that Entity Framework (EF) Core is being used for the web API's data access needs.</span></span> <span data-ttu-id="39d8e-125">Zwracany typ następującego działania jest synchronicznie wyliczany podczas serializacji:</span><span class="sxs-lookup"><span data-stu-id="39d8e-125">The following action's return type is synchronously enumerated during serialization:</span></span>

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

<span data-ttu-id="39d8e-126">Aby uniknąć synchronicznego wyliczania i blokowania w bazie danych w ASP.NET Core 2,2 i wcześniejszych, wywołaj `ToListAsync`:</span><span class="sxs-lookup"><span data-stu-id="39d8e-126">To avoid synchronous enumeration and blocking waits on the database in ASP.NET Core 2.2 and earlier, invoke `ToListAsync`:</span></span>

```csharp
public async Task<IEnumerable<Product>> GetOnSaleProducts() =>
    await _context.Products.Where(p => p.IsOnSale).ToListAsync();
```

<span data-ttu-id="39d8e-127">W ASP.NET Core 3,0 i nowszych zwraca `IAsyncEnumerable<T>` z akcji:</span><span class="sxs-lookup"><span data-stu-id="39d8e-127">In ASP.NET Core 3.0 and later, returning `IAsyncEnumerable<T>` from an action:</span></span>

* <span data-ttu-id="39d8e-128">Nie jest już wynikiem iteracji synchronicznej.</span><span class="sxs-lookup"><span data-stu-id="39d8e-128">No longer results in synchronous iteration.</span></span>
* <span data-ttu-id="39d8e-129">Staną się tak wydajne jak zwracanie <xref:System.Collections.Generic.IEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="39d8e-129">Becomes as efficient as returning <xref:System.Collections.Generic.IEnumerable%601>.</span></span>

<span data-ttu-id="39d8e-130">ASP.NET Core 3,0 i później buforuje wynik poniższego działania przed dostarczeniem go do serializatora:</span><span class="sxs-lookup"><span data-stu-id="39d8e-130">ASP.NET Core 3.0 and later buffers the result of the following action before providing it to the serializer:</span></span>

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

<span data-ttu-id="39d8e-131">Rozważ zadeklarowanie typu zwracanego sygnatury akcji jako `IAsyncEnumerable<T>` w celu zagwarantowania asynchronicznej iteracji.</span><span class="sxs-lookup"><span data-stu-id="39d8e-131">Consider declaring the action signature's return type as `IAsyncEnumerable<T>` to guarantee the asynchronous iteration.</span></span> <span data-ttu-id="39d8e-132">Ostatecznie tryb iteracji jest oparty na zwracanym konkretnym typie.</span><span class="sxs-lookup"><span data-stu-id="39d8e-132">Ultimately, the iteration mode is based on the underlying concrete type being returned.</span></span> <span data-ttu-id="39d8e-133">MVC automatycznie buforuje każdy konkretny typ, który implementuje `IAsyncEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-133">MVC automatically buffers any concrete type that implements `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="39d8e-134">Należy wziąć pod uwagę następujące działania, które zwracają rekordy produktu z ceną sprzedaży jako `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="39d8e-134">Consider the following action, which returns sale-priced product records as `IEnumerable<Product>`:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProducts)]

<span data-ttu-id="39d8e-135">`IAsyncEnumerable<Product>` odpowiednikiem poprzedniej akcji jest:</span><span class="sxs-lookup"><span data-stu-id="39d8e-135">The `IAsyncEnumerable<Product>` equivalent of the preceding action is:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProductsAsync)]

<span data-ttu-id="39d8e-136">Obie powyższe akcje nie są blokowane w przypadku ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="39d8e-136">Both of the preceding actions are non-blocking as of ASP.NET Core 3.0.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="39d8e-137">Typ IActionResult</span><span class="sxs-lookup"><span data-stu-id="39d8e-137">IActionResult type</span></span>

<span data-ttu-id="39d8e-138">Typ zwracany <xref:Microsoft.AspNetCore.Mvc.IActionResult> jest odpowiedni, gdy w akcji istnieje wiele typów zwracanych `ActionResult`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-138">The <xref:Microsoft.AspNetCore.Mvc.IActionResult> return type is appropriate when multiple `ActionResult` return types are possible in an action.</span></span> <span data-ttu-id="39d8e-139">Typy `ActionResult` reprezentują różne kody stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="39d8e-139">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="39d8e-140">Każda Klasa nieabstrakcyjna pochodna z `ActionResult` jest zgodna z prawidłowym zwracanym typem.</span><span class="sxs-lookup"><span data-stu-id="39d8e-140">Any non-abstract class deriving from `ActionResult` qualifies as a valid return type.</span></span> <span data-ttu-id="39d8e-141">Niektóre typowe typy zwracane w tej kategorii to <xref:Microsoft.AspNetCore.Mvc.BadRequestResult> (400), <xref:Microsoft.AspNetCore.Mvc.NotFoundResult> (404) i <xref:Microsoft.AspNetCore.Mvc.OkObjectResult> (200).</span><span class="sxs-lookup"><span data-stu-id="39d8e-141">Some common return types in this category are <xref:Microsoft.AspNetCore.Mvc.BadRequestResult> (400), <xref:Microsoft.AspNetCore.Mvc.NotFoundResult> (404), and <xref:Microsoft.AspNetCore.Mvc.OkObjectResult> (200).</span></span> <span data-ttu-id="39d8e-142">Alternatywnie, wygodne metody klasy <xref:Microsoft.AspNetCore.Mvc.ControllerBase> mogą być używane do zwracania typów `ActionResult` z akcji.</span><span class="sxs-lookup"><span data-stu-id="39d8e-142">Alternatively, convenience methods in the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class can be used to return `ActionResult` types from an action.</span></span> <span data-ttu-id="39d8e-143">Na przykład `return BadRequest();` jest skróconą formą `return new BadRequestResult();`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-143">For example, `return BadRequest();` is a shorthand form of `return new BadRequestResult();`.</span></span>

<span data-ttu-id="39d8e-144">Ponieważ w tym typie akcji istnieje wiele typów zwracanych i ścieżek, konieczne jest korzystanie z zliberalizowanego atrybutu [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) .</span><span class="sxs-lookup"><span data-stu-id="39d8e-144">Because there are multiple return types and paths in this type of action, liberal use of the [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) attribute is necessary.</span></span> <span data-ttu-id="39d8e-145">Ten atrybut zawiera bardziej szczegółowe szczegóły odpowiedzi dla stron pomocy interfejsu API sieci Web generowanych przez narzędzia takie jak [Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="39d8e-145">This attribute produces more descriptive response details for web API help pages generated by tools like [Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="39d8e-146">`[ProducesResponseType]` wskazuje znane typy i kody stanu HTTP do zwrócenia przez akcję.</span><span class="sxs-lookup"><span data-stu-id="39d8e-146">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="39d8e-147">Akcja synchroniczna</span><span class="sxs-lookup"><span data-stu-id="39d8e-147">Synchronous action</span></span>

<span data-ttu-id="39d8e-148">Rozważ wykonanie następującej akcji synchronicznej, w której istnieją dwa możliwe typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="39d8e-148">Consider the following synchronous action in which there are two possible return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdIActionResult&highlight=8,11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

::: moniker-end

<span data-ttu-id="39d8e-149">W poprzedniej akcji:</span><span class="sxs-lookup"><span data-stu-id="39d8e-149">In the preceding action:</span></span>

* <span data-ttu-id="39d8e-150">Kod stanu 404 jest zwracany, gdy produkt reprezentowany przez `id` nie istnieje w źródłowym magazynie danych.</span><span class="sxs-lookup"><span data-stu-id="39d8e-150">A 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="39d8e-151">Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> wygodna jest wywoływana jako skrót dla `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-151">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> convenience method is invoked as shorthand for `return new NotFoundResult();`.</span></span>
* <span data-ttu-id="39d8e-152">Kod stanu 200 jest zwracany z obiektem `Product`, gdy produkt jest istniejący.</span><span class="sxs-lookup"><span data-stu-id="39d8e-152">A 200 status code is returned with the `Product` object when the product does exist.</span></span> <span data-ttu-id="39d8e-153">Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> wygodna jest wywoływana jako skrót dla `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-153">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> convenience method is invoked as shorthand for `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="39d8e-154">Akcja asynchroniczna</span><span class="sxs-lookup"><span data-stu-id="39d8e-154">Asynchronous action</span></span>

<span data-ttu-id="39d8e-155">Rozważ wykonanie następującej akcji asynchronicznej, w której istnieją dwa możliwe typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="39d8e-155">Consider the following asynchronous action in which there are two possible return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncIActionResult&highlight=9,14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=9,14)]

::: moniker-end

<span data-ttu-id="39d8e-156">W poprzedniej akcji:</span><span class="sxs-lookup"><span data-stu-id="39d8e-156">In the preceding action:</span></span>

* <span data-ttu-id="39d8e-157">Kod stanu 400 jest zwracany, gdy opis produktu zawiera "widżet XYZ".</span><span class="sxs-lookup"><span data-stu-id="39d8e-157">A 400 status code is returned when the product description contains "XYZ Widget".</span></span> <span data-ttu-id="39d8e-158">Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> wygodna jest wywoływana jako skrót dla `return new BadRequestResult();`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-158">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> convenience method is invoked as shorthand for `return new BadRequestResult();`.</span></span>
* <span data-ttu-id="39d8e-159">Kod stanu 201 jest generowany przez <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> wygodną metodę podczas tworzenia produktu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-159">A 201 status code is generated by the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> convenience method when a product is created.</span></span> <span data-ttu-id="39d8e-160">Alternatywą dla wywoływania `CreatedAtAction` jest `return new CreatedAtActionResult(nameof(GetById), "Products", new { id = product.Id }, product);`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-160">An alternative to calling `CreatedAtAction` is `return new CreatedAtActionResult(nameof(GetById), "Products", new { id = product.Id }, product);`.</span></span> <span data-ttu-id="39d8e-161">W tej ścieżce kodu obiekt `Product` jest udostępniany w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="39d8e-161">In this code path, the `Product` object is provided in the response body.</span></span> <span data-ttu-id="39d8e-162">Podano nagłówek odpowiedzi `Location` zawierający adres URL nowo utworzonego produktu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-162">A `Location` response header containing the newly created product's URL is provided.</span></span>

<span data-ttu-id="39d8e-163">Na przykład następujący model wskazuje, że żądania muszą zawierać `Name` i `Description` właściwości.</span><span class="sxs-lookup"><span data-stu-id="39d8e-163">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="39d8e-164">Niedostarczenie `Name` i `Description` w żądaniu powoduje niepowodzenie walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-164">Failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="39d8e-165">Jeśli zastosowano atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) w ASP.NET Core 2,1 lub nowszej, błędy walidacji modelu powodują powstanie kodu stanu 400.</span><span class="sxs-lookup"><span data-stu-id="39d8e-165">If the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="39d8e-166">Aby uzyskać więcej informacji, zobacz [Automatyczne HTTP 400 odpowiedzi](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="39d8e-166">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="39d8e-167">Typ > ActionResult\<</span><span class="sxs-lookup"><span data-stu-id="39d8e-167">ActionResult\<T> type</span></span>

<span data-ttu-id="39d8e-168">W ASP.NET Core 2,1 został wprowadzony typ zwracany [ActionResult\<t >](xref:Microsoft.AspNetCore.Mvc.ActionResult`1) dla akcji kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="39d8e-168">ASP.NET Core 2.1 introduced the [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult`1) return type for web API controller actions.</span></span> <span data-ttu-id="39d8e-169">Umożliwia zwrócenie typu pochodnego od <xref:Microsoft.AspNetCore.Mvc.ActionResult> lub zwrócenie [określonego typu](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="39d8e-169">It enables you to return a type deriving from <xref:Microsoft.AspNetCore.Mvc.ActionResult> or return a [specific type](#specific-type).</span></span> <span data-ttu-id="39d8e-170">`ActionResult<T>` oferuje następujące korzyści w stosunku do [typu IActionResult](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="39d8e-170">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="39d8e-171">Właściwość `Type` atrybutu [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) może być wykluczona.</span><span class="sxs-lookup"><span data-stu-id="39d8e-171">The [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="39d8e-172">Na przykład `[ProducesResponseType(200, Type = typeof(Product))]` jest uproszczony do `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-172">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="39d8e-173">Oczekiwany zwracany typ akcji jest wywnioskowany na podstawie `T` w `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-173">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="39d8e-174">[Operatory rzutowania niejawnego](/dotnet/csharp/language-reference/keywords/implicit) obsługują konwersję zarówno `T`, jak i `ActionResult` do `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-174">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="39d8e-175">`T` konwertuje do <xref:Microsoft.AspNetCore.Mvc.ObjectResult>, co oznacza, że `return new ObjectResult(T);` jest uproszczony do `return T;`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-175">`T` converts to <xref:Microsoft.AspNetCore.Mvc.ObjectResult>, which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="39d8e-176">C#nie obsługuje operatorów rzutowania niejawnego w interfejsach.</span><span class="sxs-lookup"><span data-stu-id="39d8e-176">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="39d8e-177">W związku z tym konwersja interfejsu do konkretnego typu jest niezbędna do użycia `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-177">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="39d8e-178">Na przykład użycie `IEnumerable` w poniższym przykładzie nie działa:</span><span class="sxs-lookup"><span data-stu-id="39d8e-178">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get() =>
    _repository.GetProducts();
```

<span data-ttu-id="39d8e-179">Jedną z opcji naprawy powyższego kodu jest zwrócenie `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-179">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="39d8e-180">Większość akcji ma określony typ zwracany.</span><span class="sxs-lookup"><span data-stu-id="39d8e-180">Most actions have a specific return type.</span></span> <span data-ttu-id="39d8e-181">Podczas wykonywania akcji mogą wystąpić nieoczekiwane warunki. w takim przypadku określony typ nie jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="39d8e-181">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="39d8e-182">Na przykład parametr wejściowy akcji może kończyć się niepowodzeniem walidacji modelu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-182">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="39d8e-183">W takim przypadku typowym rozwiązaniem jest zwrócenie odpowiedniego typu `ActionResult` zamiast określonego typu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-183">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="39d8e-184">Akcja synchroniczna</span><span class="sxs-lookup"><span data-stu-id="39d8e-184">Synchronous action</span></span>

<span data-ttu-id="39d8e-185">Należy rozważyć akcję synchroniczną, w której są dostępne dwa typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="39d8e-185">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdActionResult&highlight=8,11)]

<span data-ttu-id="39d8e-186">W poprzedniej akcji:</span><span class="sxs-lookup"><span data-stu-id="39d8e-186">In the preceding action:</span></span>

* <span data-ttu-id="39d8e-187">Kod stanu 404 jest zwracany, gdy produkt nie istnieje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="39d8e-187">A 404 status code is returned when the product doesn't exist in the database.</span></span>
* <span data-ttu-id="39d8e-188">Kod stanu 200 jest zwracany z odpowiednim obiektem `Product`, gdy produkt istnieje.</span><span class="sxs-lookup"><span data-stu-id="39d8e-188">A 200 status code is returned with the corresponding `Product` object when the product does exist.</span></span> <span data-ttu-id="39d8e-189">Przed ASP.NET Core 2,1, wiersz `return product;` musiał być `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="39d8e-189">Before ASP.NET Core 2.1, the `return product;` line had to be `return Ok(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="39d8e-190">Akcja asynchroniczna</span><span class="sxs-lookup"><span data-stu-id="39d8e-190">Asynchronous action</span></span>

<span data-ttu-id="39d8e-191">Rozważ akcję asynchroniczną, w której istnieją dwa możliwe typy zwracane:</span><span class="sxs-lookup"><span data-stu-id="39d8e-191">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncActionResult&highlight=9,14)]

<span data-ttu-id="39d8e-192">W poprzedniej akcji:</span><span class="sxs-lookup"><span data-stu-id="39d8e-192">In the preceding action:</span></span>

* <span data-ttu-id="39d8e-193">Kod stanu 400 (<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>) jest zwracany przez środowisko uruchomieniowe ASP.NET Core, gdy:</span><span class="sxs-lookup"><span data-stu-id="39d8e-193">A 400 status code (<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="39d8e-194">Atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) został zastosowany i walidacja modelu kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="39d8e-194">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="39d8e-195">Opis produktu zawiera "widżet XYZ".</span><span class="sxs-lookup"><span data-stu-id="39d8e-195">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="39d8e-196">Kod stanu 201 jest generowany przez metodę <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> podczas tworzenia produktu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-196">A 201 status code is generated by the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method when a product is created.</span></span> <span data-ttu-id="39d8e-197">W tej ścieżce kodu obiekt `Product` jest udostępniany w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="39d8e-197">In this code path, the `Product` object is provided in the response body.</span></span> <span data-ttu-id="39d8e-198">Podano nagłówek odpowiedzi `Location` zawierający adres URL nowo utworzonego produktu.</span><span class="sxs-lookup"><span data-stu-id="39d8e-198">A `Location` response header containing the newly created product's URL is provided.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="39d8e-199">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="39d8e-199">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
