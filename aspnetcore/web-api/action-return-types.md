---
title: Zwracane typy akcji kontrolera platformy ASP.NET Core Web API
author: scottaddie
description: Więcej informacji na temat w interfejsie API sieci Web platformy ASP.NET Core przy użyciu różnych metod zwracane typy akcji kontrolera.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
uid: web-api/action-return-types
ms.openlocfilehash: 422db97da222fb5e742e1d8e6ae410edc90dbc18
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2018
ms.locfileid: "36273559"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Zwracane typy akcji kontrolera platformy ASP.NET Core Web API

Przez [Scott Addie](https://github.com/scottaddie)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Platforma ASP.NET Core oferuje następujące opcje dla akcji kontrolera interfejsu API sieci Web zwracane typy:

::: moniker range="<= aspnetcore-2.0"
* [Określony typ](#specific-type)
* [IActionResult](#iactionresult-type)
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
* [Określony typ](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)
::: moniker-end

W tym dokumencie wyjaśniono, gdy jest najbardziej odpowiednie do użycia każdy typ zwracany.

## <a name="specific-type"></a>Określony typ

Najprostsza akcji zwraca typ danych pierwotnych lub złożonych (na przykład `string` lub typ niestandardowy obiekt). Należy wziąć pod uwagę następujące akcji, która zwraca kolekcję obiektów niestandardowych `Product` obiektów:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Bez znanych warunków, aby zabezpieczyć się przed podczas wykonywania akcji zwracając określonego typu można wystarczające. Poprzednią akcję przyjmuje żadnych parametrów, więc Walidacja ograniczenia parametru nie jest wymagane.

Jeśli wiadomo, warunki, które należy wyjaśnić dla akcji, wprowadzono wiele ścieżek zwrotu. W takim przypadku jest często mieszać [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) zwracany typ z typem zwracanym pierwotnych lub złożonych. Albo [IActionResult](#iactionresult-type) lub [ActionResult\<T >](#actionresultt-type) są niezbędne uwzględnić ten typ akcji.

## <a name="iactionresult-type"></a>Typ IActionResult

[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) zwracany typ, jest odpowiednie, gdy wiele [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) zwraca typy są możliwe w akcji. `ActionResult` Typy reprezentują różne kody stanu HTTP. Niektóre typowe typy zwracane należących do tej kategorii [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) i [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Ponieważ istnieje wiele typów zwracanych i ścieżek w działaniu, liberalne użytkowania [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) atrybut jest konieczny. Ten atrybut daje bardziej opisowe szczegóły odpowiedzi dla stron pomocy interfejsu API wygenerowanych przez narzędzia, takie jak [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` Wskazuje, znanych typów oraz kodów stanu HTTP, które mają zostać zwrócone przez akcję.

### <a name="synchronous-action"></a>Akcja synchroniczne

Rozważmy następującą akcję synchroniczne, w którym istnieją dwie możliwe zwracane typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

W poprzednich akcji, kod stanu 404 jest zwracane, gdy produkt jest reprezentowany przez `id` nie istnieje w podstawowym magazynie danych. [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) pomocnika metoda jest wywoływana w celu szybkiego `return new NotFoundResult();`. Jeśli istnieje produktu, `Product` obiekt reprezentujący ładunek zwracany jest kod stanu 200. [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) metody pomocnika jest wywoływana jako formę skróconą `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Akcja asynchroniczna

Należy wziąć pod uwagę następujące akcję asynchroniczną, w której istnieją dwie możliwe zwracane typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

W poprzednim działaniu, zwracany jest kod stanu 400, podczas sprawdzania poprawności modelu nie powiedzie się i [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) wywoływana jest metoda pomocnika. Na przykład, następujący model wskazuje, że żądania należy podać `Name` właściwości i wartości. W związku z tym, nie można podać poprawną `Name` w żądaniu powoduje, że sprawdzanie poprawności modelu nie powiedzie się.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Akcji innych znanych kod powrotny poprzedzającego jest 201, który jest generowany przez [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) metody pomocnika. W tej ścieżce `Product` obiekt jest zwracany.

::: moniker range=">= aspnetcore-2.1"
## <a name="actionresultt-type"></a>ActionResult\<T > typu

Platforma ASP.NET Core 2.1 wprowadzono [ActionResult\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) zwracany typ dla akcji kontrolera interfejsu API sieci Web. Umożliwia ona zwracany typ pochodząca od [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) lub je zwracają [określonego typu](#specific-type). `ActionResult<T>` oferuje następujące korzyści za pośrednictwem [typu IActionResult](#iactionresult-type):

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) atrybutu `Type` właściwości mogą być wyłączone.
* [Operatory rzutowania niejawnego](/dotnet/csharp/language-reference/keywords/implicit) obsługi zarówno konwersji `T` i `ActionResult` do `ActionResult<T>`. `T` Konwertuje [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), co oznacza, że `return new ObjectResult(T);` została uproszczona w celu `return T;`.

Większość akcji ma określony typ zwracany. Nieoczekiwane warunki mogą wystąpić podczas wykonywania akcji, w którym nie jest zwracany w przypadku określonego typu. Na przykład parametr wejściowy akcja może zakończyć się niepowodzeniem weryfikacji modelu. W takim przypadku jest wspólne do zwrócenia odpowiedniego `ActionResult` typu zamiast określonego typu.

### <a name="synchronous-action"></a>Akcja synchroniczne

Należy wziąć pod uwagę synchroniczne akcji, w którym istnieją dwie możliwe zwracane typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

W poprzednim kodzie kod stanu 404 jest zwracane, gdy produkt nie istnieje w bazie danych. Jeśli istnieje produktu, odpowiedni `Product` obiekt jest zwracany. Przed platformy ASP.NET Core 2.1 `return product;` byłby wiersza `return Ok(product);`.

> [!TIP]
> Począwszy od platformy ASP.NET Core 2.1, wnioskowania źródła wiązania parametrów akcji jest włączane, gdy klasa kontrolera zostanie nadany `[ApiController]` atrybutu. Nazwa parametru pasujące do nazwy w szablonie trasy zostanie automatycznie powiązany za pomocą żądania danych trasy. W związku z tym, poprzedniej akcji `id` parametr jawnie nie jest oznaczony za pomocą [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) atrybutu.

### <a name="asynchronous-action"></a>Akcja asynchroniczna

Należy wziąć pod uwagę akcję asynchroniczną, w którym istnieją dwie możliwe zwracane typy:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

W przypadku niepowodzenia weryfikacji modelu [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) metoda jest wywoływana, aby zwrócić kod stanu 400. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) właściwości zawierające błędy sprawdzania poprawności określonego jest przekazywany do niego. Jeśli weryfikacja modelu zakończy się powodzeniem, produkt jest tworzony w bazie danych. Zwracany jest kod stanu 201.

> [!TIP]
> Począwszy od platformy ASP.NET Core 2.1, wnioskowania źródła wiązania parametrów akcji jest włączane, gdy klasa kontrolera zostanie nadany `[ApiController]` atrybutu. Parametry typu złożonego automatycznie powiązane przy użyciu treści żądania. W związku z tym, poprzedniej akcji `product` parametr jawnie nie jest oznaczony za pomocą [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybutu.
::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Akcji kontrolera](xref:mvc/controllers/actions)
* [Walidacja modelu](xref:mvc/models/validation)
* [Strony pomocy internetowego interfejsu API przy użyciu programu Swagger](xref:tutorials/web-api-help-pages-using-swagger)
