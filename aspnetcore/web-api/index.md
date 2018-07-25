---
title: Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web
author: scottaddie
description: Informacje o funkcjach dostępnych podczas tworzenia internetowego interfejsu API w programie ASP.NET Core i moment jest właściwy użyć każdej funkcji.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: ccee4f7bae0abe1b36088d58e5c1e1362d8de9f0
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243100"
---
# <a name="build-web-apis-with-aspnet-core"></a>Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web

Przez [Scott Addie](https://github.com/scottaddie)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

W tym dokumencie wyjaśniono, jak tworzenie internetowego interfejsu API w programie ASP.NET Core i jest najbardziej odpowiednie do użycia każdej funkcji.

## <a name="derive-class-from-controllerbase"></a>Dziedziczyć klasy ControllerBase

Dziedzicz [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) klasy w kontrolerze, który ma pełnić rolę interfejsu API sieci web. Na przykład:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` Klasa udostępnia kilka właściwości i metod. W poprzednim kodzie przykłady [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) i [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Te metody są wywoływane w ramach metod akcji do zwrócenia HTTP 400 i 201 kodów stanu, odpowiednio. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) także podana przez właściwość `ControllerBase`, jest dostępny do obsługi żądania weryfikacji modelu.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute

Platforma ASP.NET Core 2.1 wprowadzono [[klasy ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu do oznaczania klasa formantu API sieci web. Na przykład:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Zgodność wersji 2.1 lub nowszej, ustawić za pomocą [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), jest wymagana do używania tego atrybutu. Na przykład, wyróżniony kod w *Startup.ConfigureServices* ustawia 2.1 flagi zgodności:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

`[ApiController]` Atrybutu często jest sprzężona z `ControllerBase` Aby włączyć zachowanie REST specyficzne dla kontrolerów. `ControllerBase` zapewnia dostęp do metod, takich jak [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) i [pliku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Innym rozwiązaniem jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

W poniższych sekcjach opisano wygodnych funkcji dodane przez atrybut.

### <a name="automatic-http-400-responses"></a>Automatyczne odpowiedzi HTTP 400

Błędy sprawdzania poprawności automatycznie wyzwoli odpowiedź HTTP 400. Poniższy kod staje się niepotrzebne w swoje działania:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Domyślnym zachowaniem jest wyłączona po [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) właściwość jest ustawiona na `true`. Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Powiązanie źródła parametru wnioskowania

Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji. Istnieją następujące atrybuty źródło powiązania:

|Atrybut|Źródło wiążące |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Treść żądania |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Dane formularza w treści żądania |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Nagłówek żądania |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Parametr ciągu zapytania żądania |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Dane trasy z bieżącego żądania |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Usługa żądania wprowadzony jako parametru akcji |

> [!WARNING]
> Nie używaj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`). `%2f` nie będzie unescaped do `/`. Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.

Bez `[ApiController]` atrybutu, powiązanie źródła jawnie zdefiniowanych atrybutów. W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji. Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji. Powiązania atrybutów źródłowych zachowują się w następujący sposób:

* **[FromBody]**  jest wnioskowany dla parametrów typu złożonego. Wyjątkiem od tej reguły jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) i [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne. Kiedy akcja ma więcej niż jeden parametr określony jawnie (za pośrednictwem `[FromBody]`) lub wywnioskowane jako powiązanej z treści żądania, zgłaszany jest wyjątek. Na przykład następujące podpisy akcji może spowodować wyjątek:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]**  jest wnioskowany dla parametrach akcji danego typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.
* **[FromRoute]**  jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy. Gdy więcej niż jedna trasa jest zgodna z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.
* **[FromQuery]**  jest wnioskowany dla innych parametrów akcji.

Zasady wnioskowania domyślne są wyłączone podczas [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) właściwość jest ustawiona na `true`. Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Wnioskowanie multipart/formularza data żądania

Gdy parametr akcji jest oznaczony za pomocą [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atrybutu `multipart/form-data` żądania jest wnioskowany typ zawartości.

Domyślnym zachowaniem jest wyłączona po [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) właściwość jest ustawiona na `true`. Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Wymagania routingu atrybutu

Routing atrybutu staje się wymagania. Na przykład:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) lub [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) w *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
