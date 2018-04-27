---
title: Tworzenie interfejsów API z platformy ASP.NET Core sieci web
author: scottaddie
description: Więcej informacji na temat funkcji dostępnych do tworzenia składnika web API platformy ASP.NET Core i jest odpowiednie do użycia w każdej funkcji.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: web-api/index
ms.openlocfilehash: 017bcc1ed65b1baa92408db07201d1c7bab2849d
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="build-web-apis-with-aspnet-core"></a>Tworzenie interfejsów API z platformy ASP.NET Core sieci web

Przez [Scott Addie](https://github.com/scottaddie)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Tym dokumencie opisano sposób tworzenia składnika web API platformy ASP.NET Core i jest najbardziej odpowiednie do użycia w każdej funkcji.

## <a name="derive-class-from-controllerbase"></a>Klasa wyprowadzona z ControllerBase

Dziedzicz [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) klasy w kontrolerze, które mają służyć jako interfejs API sieci web. Na przykład:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

`ControllerBase` Klasy zapewnia dostęp do wielu właściwości i metody. W poprzednim przykładzie, niektóre z tych metod uwzględnić [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) i [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Te metody są wywoływane w ramach metod akcji, aby zwrócić HTTP 400 i kodów stanu 201, odpowiednio. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) właściwości w nim również podane przez `ControllerBase`, jest dostępny do wykonania żądania weryfikacji modelu.

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a>Dodawanie adnotacji z ApiControllerAttribute — klasa

Platformy ASP.NET Core 2.1 wprowadzono `[ApiController]` atrybut określający Klasa kontrolera interfejsu API sieci web. Na przykład:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Ten atrybut jest często połączone z `ControllerBase` do uzyskania dostępu do właściwości i metod przydatne. `ControllerBase` zapewnia dostęp do metod, takich jak [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) i [pliku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Innym rozwiązaniem jest utworzenie klasy podstawowej kontrolera niestandardowego opatrzoną `[ApiController]` atrybutu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

W poniższych sekcjach opisano funkcje wygody dodane przez atrybut.

### <a name="automatic-http-400-responses"></a>Automatyczne odpowiedzi HTTP 400

Błędy sprawdzania poprawności automatycznie wyzwoli odpowiedź HTTP 400. Poniższy kod staje się niepotrzebne w akcji:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

To domyślne zachowanie jest wyłączona w następującym kodem *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Powiązania źródła parametru wnioskowania

Atrybut źródłowy powiązanie definiuje lokalizacji, w którym znajduje się wartość parametru akcji. Istnieją następujące atrybuty źródło powiązania:

|Atrybut|Powiązania źródła |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Treść żądania |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Dane formularza w treści żądania |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Nagłówek żądania |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Parametr ciągu zapytania żądania |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Dane trasy z bieżącego żądania |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Usługa żądania dodane jako parametru akcji |

Bez `[ApiController]` atrybut powiązania źródła jawnie zdefiniowanych atrybutów. W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` wartość parametru jest podana w ciągu zapytania w adresie URL żądania:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji. Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji. Atrybuty źródło powiązania zachowują się w następujący sposób:

* **[FromBody]**  jest wywnioskowany dla parametrów typu złożonego. Wyjątek od tej reguły jest dowolnego typu złożonego, wbudowane o specjalnym znaczeniu, takie jak [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) i [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Wnioskowanie kod źródłowy powiązanie ignoruje te typy specjalne. Jeśli akcja ma więcej niż jeden parametr jawnie określony (za pośrednictwem `[FromBody]`) lub wywnioskować jako powiązane z treści żądania, jest zgłaszany wyjątek. Na przykład następujące podpisy akcji może spowodować wyjątek:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  jest wywnioskowane dla parametrów akcji typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Nie jest wywnioskować dla wszystkich typów prostych lub zdefiniowanej przez użytkownika.
* **[FromRoute]**  jest wywnioskowany dla dowolnej nazwy parametru akcji pasującej do parametru w szablonie trasy. Gdy wiele tras zgodne z parametrem akcji, wartości trasy jest uznawany za `[FromRoute]`.
* **[FromQuery]**  jest wywnioskowany dla innych parametrów akcji.

Zasady wnioskowania domyślne są wyłączone w następującym kodem *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Wnioskowanie multipart/dane formularza żądania

Jeśli parametr akcji jest oznaczony za pomocą [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atrybutu `multipart/form-data` żądania jest wywnioskowany typ zawartości.

Domyślnym zachowaniem jest wyłączona w następującym kodem *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Atrybut wymaganie routingu

Atrybut routingu staje się wymagania. Na przykład:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Akcje są niedostępne za pośrednictwem [trasy z konwencjonalnej](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) lub [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) w *Startup.Configure*.
::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zwracane typy akcji kontrolera](xref:web-api/action-return-types)
* [Niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters)
* [Formatowanie danych odpowiedzi](xref:web-api/advanced/formatting)
* [Strony pomocy przy użyciu programu Swagger](xref:tutorials/web-api-help-pages-using-swagger)
* [Routing do akcji kontrolera](xref:mvc/controllers/routing)