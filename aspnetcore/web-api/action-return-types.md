---
title: Zwracane typy akcji kontrolera w interfejsu API platformy ASP.NET Core sieci Web
author: scottaddie
description: 'Informacje o używaniu różne metody akcji kontrolera: zwracane typy w interfejsie API sieci Web platformy ASP.NET Core.'
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: web-api/action-return-types
ms.openlocfilehash: 02a28425cc3b7b792e7275ebab37fa8eeafc74c1
ms.sourcegitcommit: 211ef03cf13f631dd77076de0c55863fe0cee2c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Zwracane typy akcji kontrolera w interfejsu API platformy ASP.NET Core sieci Web

Przez [Scott Addie](https://github.com/scottaddie)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Platformy ASP.NET Core oferują typy zwracane trzech opcji dla akcji kontrolera interfejsu API sieci Web:

* [Określony typ](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T>](#actionresultt-type)

W tym dokumencie opisano, gdy jest najbardziej odpowiednie do użycia w każdym zwracanego typu.

## <a name="specific-type"></a>Określony typ

Najprostsza akcji zwraca typ danych pierwotnych lub złożonych (na przykład `string` lub typu niestandardowego obiektu). Należy wziąć pod uwagę następujące akcji, która zwraca kolekcję niestandardowych `Product` obiektów:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Znane bezwarunkowo do ochrony przed podczas wykonywania akcji zwracając określonego typu można wystarczające. Poprzedniej akceptuje bez parametrów, więc sprawdzanie poprawności ograniczenia parametru nie jest wymagane.

Jeśli wiadomo, warunki, które należy wyjaśnić dla akcji, wprowadzono wiele ścieżek powrotu. W takim przypadku jest często stosowanym rozwiązaniem mieszać [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) zwracany typ z typem zwracanym pierwotnych lub złożonych. Albo [IActionResult](#iactionresult-type) lub [ActionResult\<T >](#actionresultt-type) są niezbędne do dostosowania tego typu działania.

## <a name="iactionresult-type"></a>Typ IActionResult

[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) zwracany typ jest odpowiednie w przypadku wielu [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) zwraca typy są możliwe w celu wykonania akcji. `ActionResult` Typy reprezentują różnych kodów stanu HTTP. Niektóre typowe zwracane typy należących do tej kategorii są [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) i [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Ponieważ istnieje wiele typów zwracanych i użycie ścieżki w działaniu, rozległych [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) jest potrzebny atrybut. Ten atrybut tworzy opisowej szczegóły odpowiedzi dla strony pomocy interfejsu API wygenerowanych przez narzędzia, takie jak [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` Wskazuje zwracaną przez działanie znane typy i kodów stanu HTTP.

### <a name="synchronous-action"></a>Synchroniczne akcji

Należy wziąć pod uwagę następujące akcji synchroniczne, w której istnieją dwa możliwe typy zwracane:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

W poprzednich operacji, kod stanu 404 zwracana jest produktu reprezentowane przez `id` nie istnieje w magazynie danych podstawowych. [NotFound](/dotnet/api/system.web.http.apicontroller.notfound) pomocnika metoda jest wywoływana w celu szybkiego `return new NotFoundResult();`. Jeśli istnieje produktu, `Product` obiekt reprezentujący ładunku jest zwracana z kodem stanu 200. [Ok](/dotnet/api/system.web.http.apicontroller.ok) jako formę skrótu wywoływana jest metoda pomocnika `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Akcja asynchroniczna

Należy wziąć pod uwagę następujące akcja asynchroniczna, w której istnieją dwa możliwe typy zwracane:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

W poprzednim akcji kod stanu 400 jest zwracany w przypadku niepowodzenia weryfikacji modelu i [element BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) wywołania metody pomocnika. Na przykład następujący model wskazuje Podaj żądań `Name` właściwości i wartości. W związku z tym braku odpowiedniego `Name` w żądaniu powoduje niepowodzenie sprawdzania poprawności modelu.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Poprzedniej akcji innych znanych kod powrotny jest 201, która jest generowana przez [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) metody pomocnika. W tej ścieżce `Product` obiekt jest zwracany.

## <a name="actionresultt-type"></a>ActionResult\<T > typ

Platformy ASP.NET Core 2.1 wprowadzono `ActionResult<T>` zwracany typ dla akcji kontrolera interfejsu API sieci Web. Umożliwia ona zwracany typ pochodny [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) lub zwróć [określonego typu](#specific-type). `ActionResult<T>` oferuje następujące korzyści za pośrednictwem [IActionResult typu](#iactionresult-type):

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) atrybutu `Type` właściwości mogą być wykluczone.
* [Niejawne rzutowanie operatory](/dotnet/csharp/language-reference/keywords/implicit) obsługuje konwersji zarówno `T` i `ActionResult` do `ActionResult<T>`. `T` Konwertuje [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), co oznacza, że `return new ObjectResult(T);` została uproszczona w celu `return T;`.

Większość akcji ma określonego typu zwracanego. Nieoczekiwane warunki mogą wystąpić podczas wykonywania akcji, w którym nie jest zwracana w przypadku określonego typu. Na przykład parametr wejściowy akcji może zakończyć się niepowodzeniem sprawdzania poprawności modelu. W takim przypadku jest często stosowanym rozwiązaniem Zwróć odpowiedni `ActionResult` typu zamiast określonego typu.

### <a name="synchronous-action"></a>Synchroniczne akcji

Należy wziąć pod uwagę synchroniczne akcji, w której istnieją dwa możliwe typy zwracane:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

W poprzednim kodzie kod stanu 404 jest zwracany, gdy produkt nie istnieje w bazie danych. Jeśli istnieje produktu odpowiednie `Product` obiekt jest zwracany. Przed platformy ASP.NET Core 2.1 `return product;` byłby wiersza `return Ok(product);`.

> [!TIP]
> Począwszy od platformy ASP.NET Core 2.1, wnioskowania źródła wiązania parametrów akcji jest włączona, gdy klasa kontrolera jest ozdobione `[ApiController]` atrybutu. Nazwa parametru pasujących do nazwy w szablonie trasy automatycznie jest powiązany za pomocą żądania danych trasy. W rezultacie poprzedniej akcji `id` parametr nie jest jawnie oznaczony za pomocą [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) atrybutu.

### <a name="asynchronous-action"></a>Akcja asynchroniczna

Akcja asynchroniczna, w której istnieją dwa możliwe typy zwracane należy wziąć pod uwagę:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

W przypadku niepowodzenia weryfikacji modelu [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) metoda wywoływana w celu kod 400 stanu powrotu. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) przekazana zawierające błędy sprawdzania poprawności określonej właściwości. Jeśli weryfikacja modelu zakończy się powodzeniem, produktu jest tworzony w bazie danych. Zwracany jest kod stanu 201.

> [!TIP]
> Począwszy od platformy ASP.NET Core 2.1, wnioskowania źródła wiązania parametrów akcji jest włączona, gdy klasa kontrolera jest ozdobione `[ApiController]` atrybutu. Parametry typu złożonego automatycznie powiązane przy użyciu treści żądania. W rezultacie poprzedniej akcji `product` parametr nie jest jawnie oznaczony za pomocą [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) atrybutu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Akcji kontrolera](xref:mvc/controllers/actions)
* [Walidacja modelu](xref:mvc/models/validation)
* [Strony pomocy interfejsu API sieci Web przy użyciu programu Swagger](xref:tutorials/web-api-help-pages-using-swagger)