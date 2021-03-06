---
title: Typy zwracane akcje kontrolera w ASP.NET Core Web API
author: scottaddie
description: Dowiedz się więcej o używaniu różnych typów zwracanych metody akcji kontrolera w interfejsie API sieci Web ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 02/03/2020
uid: web-api/action-return-types
ms.openlocfilehash: 17e290d3aba4f724fcbd1693af371017c4d3f03a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655247"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Typy zwracane akcje kontrolera w ASP.NET Core Web API

Przez [Scott Addie](https://github.com/scottaddie)

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

ASP.NET Core oferuje następujące opcje dla zwracanych typów akcji kontrolera API sieci Web:

::: moniker range=">= aspnetcore-2.1"

* [Określony typ](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [Określony typ](#specific-type)
* [IActionResult](#iactionresult-type)

::: moniker-end

Ten dokument wyjaśnia, kiedy jest najbardziej odpowiedni do użycia każdego typu zwracanego.

## <a name="specific-type"></a>Określony typ

Najprostsza akcja zwraca typ danych pierwotnych lub złożonych (na przykład `string` lub niestandardowy typ obiektu). Rozważ poniższą akcję, która zwraca kolekcję niestandardowych obiektów `Product`:

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Bez znanych warunków zabezpieczających przed wykonaniem akcji, zwrócenie określonego typu może być wystarczające. Poprzednia akcja nie akceptuje żadnych parametrów, więc Walidacja ograniczeń parametrów nie jest konieczna.

Gdy w akcji musi być uwzględnione znane warunki, zostaną wprowadzone wiele ścieżek powrotu. W takim przypadku typowym sposobem jest mieszanie <xref:Microsoft.AspNetCore.Mvc.ActionResult> zwracanego typu z typem pierwotnym lub złożonym typu zwracanego. [IActionResult](#iactionresult-type) lub [ActionResult\<> t](#actionresultt-type) są niezbędne do uwzględnienia tego typu akcji.

### <a name="return-ienumerablet-or-iasyncenumerablet"></a>Zwróć\<t > lub IAsyncEnumerable\<T >

W ASP.NET Core 2,2 i starszych, zwrócenie <xref:System.Collections.Generic.IEnumerable%601> z akcji powoduje synchroniczną iterację kolekcji przez serializator. Wynikiem jest blokowanie wywołań i potencjalna blokada puli wątków. Aby zilustrować, Wyobraź sobie, że Entity Framework (EF) rdzeń jest używany do potrzeb dostępu do danych interfejsu API sieci Web. Zwracany typ następującego działania jest synchronicznie wyliczany podczas serializacji:

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

Aby uniknąć synchronicznego wyliczania i blokowania w bazie danych w ASP.NET Core 2,2 i wcześniejszych, wywołaj `ToListAsync`:

```csharp
public async Task<IEnumerable<Product>> GetOnSaleProducts() =>
    await _context.Products.Where(p => p.IsOnSale).ToListAsync();
```

W ASP.NET Core 3,0 i nowszych zwraca `IAsyncEnumerable<T>` z akcji:

* Nie jest już wynikiem iteracji synchronicznej.
* Staną się tak wydajne jak zwracanie <xref:System.Collections.Generic.IEnumerable%601>.

ASP.NET Core 3,0 i później buforuje wynik poniższego działania przed dostarczeniem go do serializatora:

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

Rozważ zadeklarowanie typu zwracanego sygnatury akcji jako `IAsyncEnumerable<T>` w celu zagwarantowania asynchronicznej iteracji. Ostatecznie tryb iteracji jest oparty na zwracanym konkretnym typie. MVC automatycznie buforuje każdy konkretny typ, który implementuje `IAsyncEnumerable<T>`.

Należy wziąć pod uwagę następujące działania, które zwracają rekordy produktu z ceną sprzedaży jako `IEnumerable<Product>`:

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProducts)]

`IAsyncEnumerable<Product>` odpowiednikiem poprzedniej akcji jest:

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProductsAsync)]

Obie powyższe akcje nie są blokowane w przypadku ASP.NET Core 3,0.

## <a name="iactionresult-type"></a>Typ IActionResult

Typ zwracany <xref:Microsoft.AspNetCore.Mvc.IActionResult> jest odpowiedni, gdy w akcji istnieje wiele typów zwracanych `ActionResult`. Typy `ActionResult` reprezentują różne kody stanu HTTP. Każda Klasa nieabstrakcyjna pochodna z `ActionResult` jest zgodna z prawidłowym zwracanym typem. Niektóre typowe typy zwracane w tej kategorii to <xref:Microsoft.AspNetCore.Mvc.BadRequestResult> (400), <xref:Microsoft.AspNetCore.Mvc.NotFoundResult> (404) i <xref:Microsoft.AspNetCore.Mvc.OkObjectResult> (200). Alternatywnie, wygodne metody klasy <xref:Microsoft.AspNetCore.Mvc.ControllerBase> mogą być używane do zwracania typów `ActionResult` z akcji. Na przykład `return BadRequest();` jest skróconą formą `return new BadRequestResult();`.

Ponieważ w tym typie akcji istnieje wiele typów zwracanych i ścieżek, konieczne jest korzystanie z zliberalizowanego atrybutu [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) . Ten atrybut zawiera bardziej szczegółowe szczegóły odpowiedzi dla stron pomocy interfejsu API sieci Web generowanych przez narzędzia takie jak [Swagger](xref:tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` wskazuje znane typy i kody stanu HTTP do zwrócenia przez akcję.

### <a name="synchronous-action"></a>Akcja synchroniczna

Rozważ wykonanie następującej akcji synchronicznej, w której istnieją dwa możliwe typy zwracane:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdIActionResult&highlight=8,11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

::: moniker-end

W poprzedniej akcji:

* Kod stanu 404 jest zwracany, gdy produkt reprezentowany przez `id` nie istnieje w źródłowym magazynie danych. Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> wygodna jest wywoływana jako skrót dla `return new NotFoundResult();`.
* Kod stanu 200 jest zwracany z obiektem `Product`, gdy produkt jest istniejący. Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> wygodna jest wywoływana jako skrót dla `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Akcja asynchroniczna

Rozważ wykonanie następującej akcji asynchronicznej, w której istnieją dwa możliwe typy zwracane:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncIActionResult&highlight=9,14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=9,14)]

::: moniker-end

W poprzedniej akcji:

* Kod stanu 400 jest zwracany, gdy opis produktu zawiera "widżet XYZ". Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> wygodna jest wywoływana jako skrót dla `return new BadRequestResult();`.
* Kod stanu 201 jest generowany przez <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> wygodną metodę podczas tworzenia produktu. Alternatywą dla wywoływania `CreatedAtAction` jest `return new CreatedAtActionResult(nameof(GetById), "Products", new { id = product.Id }, product);`. W tej ścieżce kodu obiekt `Product` jest udostępniany w treści odpowiedzi. Podano nagłówek odpowiedzi `Location` zawierający adres URL nowo utworzonego produktu.

Na przykład następujący model wskazuje, że żądania muszą zawierać `Name` i `Description` właściwości. Niedostarczenie `Name` i `Description` w żądaniu powoduje niepowodzenie walidacji modelu.

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

Jeśli zastosowano atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) w ASP.NET Core 2,1 lub nowszej, błędy walidacji modelu powodują powstanie kodu stanu 400. Aby uzyskać więcej informacji, zobacz [Automatyczne HTTP 400 odpowiedzi](xref:web-api/index#automatic-http-400-responses).

## <a name="actionresultt-type"></a>Typ > ActionResult\<

W ASP.NET Core 2,1 został wprowadzony typ zwracany [ActionResult\<t >](xref:Microsoft.AspNetCore.Mvc.ActionResult`1) dla akcji kontrolera interfejsu API sieci Web. Umożliwia zwrócenie typu pochodnego od <xref:Microsoft.AspNetCore.Mvc.ActionResult> lub zwrócenie [określonego typu](#specific-type). `ActionResult<T>` oferuje następujące korzyści w stosunku do [typu IActionResult](#iactionresult-type):

* Właściwość `Type` atrybutu [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) może być wykluczona. Na przykład `[ProducesResponseType(200, Type = typeof(Product))]` jest uproszczony do `[ProducesResponseType(200)]`. Oczekiwany zwracany typ akcji jest wywnioskowany na podstawie `T` w `ActionResult<T>`.
* [Operatory rzutowania niejawnego](/dotnet/csharp/language-reference/keywords/implicit) obsługują konwersję zarówno `T`, jak i `ActionResult` do `ActionResult<T>`. `T` konwertuje do <xref:Microsoft.AspNetCore.Mvc.ObjectResult>, co oznacza, że `return new ObjectResult(T);` jest uproszczony do `return T;`.

C#nie obsługuje operatorów rzutowania niejawnego w interfejsach. W związku z tym konwersja interfejsu do konkretnego typu jest niezbędna do użycia `ActionResult<T>`. Na przykład użycie `IEnumerable` w poniższym przykładzie nie działa:

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get() =>
    _repository.GetProducts();
```

Jedną z opcji naprawy powyższego kodu jest zwrócenie `_repository.GetProducts().ToList();`.

Większość akcji ma określony typ zwracany. Podczas wykonywania akcji mogą wystąpić nieoczekiwane warunki. w takim przypadku określony typ nie jest zwracany. Na przykład parametr wejściowy akcji może kończyć się niepowodzeniem walidacji modelu. W takim przypadku typowym rozwiązaniem jest zwrócenie odpowiedniego typu `ActionResult` zamiast określonego typu.

### <a name="synchronous-action"></a>Akcja synchroniczna

Należy rozważyć akcję synchroniczną, w której są dostępne dwa typy zwracane:

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdActionResult&highlight=8,11)]

W poprzedniej akcji:

* Kod stanu 404 jest zwracany, gdy produkt nie istnieje w bazie danych.
* Kod stanu 200 jest zwracany z odpowiednim obiektem `Product`, gdy produkt istnieje. Przed ASP.NET Core 2,1, wiersz `return product;` musiał być `return Ok(product);`.

### <a name="asynchronous-action"></a>Akcja asynchroniczna

Rozważ akcję asynchroniczną, w której istnieją dwa możliwe typy zwracane:

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncActionResult&highlight=9,14)]

W poprzedniej akcji:

* Kod stanu 400 (<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>) jest zwracany przez środowisko uruchomieniowe ASP.NET Core, gdy:
  * Atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) został zastosowany i walidacja modelu kończy się niepowodzeniem.
  * Opis produktu zawiera "widżet XYZ".
* Kod stanu 201 jest generowany przez metodę <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> podczas tworzenia produktu. W tej ścieżce kodu obiekt `Product` jest udostępniany w treści odpowiedzi. Podano nagłówek odpowiedzi `Location` zawierający adres URL nowo utworzonego produktu.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
