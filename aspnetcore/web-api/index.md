---
title: Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web
author: scottaddie
description: Informacje o funkcjach dostępnych podczas tworzenia internetowego interfejsu API w programie ASP.NET Core i moment jest właściwy użyć każdej funkcji.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
ms.openlocfilehash: bc8be67957a56a818a88e0496d45db1e7b7aed0e
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142367"
---
# <a name="build-web-apis-with-aspnet-core"></a>Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web

Przez [Scott Addie](https://github.com/scottaddie)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

W tym dokumencie wyjaśniono, jak tworzenie internetowego interfejsu API w programie ASP.NET Core i jest najbardziej odpowiednie do użycia każdej funkcji.

## <a name="derive-class-from-controllerbase"></a>Dziedziczyć klasy ControllerBase

Dziedzicz <xref:Microsoft.AspNetCore.Mvc.ControllerBase> klasy w kontrolerze, który ma pełnić rolę interfejsu API sieci web. Na przykład:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` Klasa udostępnia kilka właściwości i metod. W poprzednim kodzie przykłady <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Te metody są wywoływane w ramach metod akcji do zwrócenia HTTP 400 i 201 kodów stanu, odpowiednio. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Także podana przez właściwość `ControllerBase`, jest dostępny do obsługi żądania weryfikacji modelu.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a>Adnotacja z atrybutem klasy ApiController

Platforma ASP.NET Core 2.1 wprowadzono [[klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu do oznaczania klasa formantu API sieci web. Na przykład:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Zgodność wersji 2.1 lub nowszej, ustawić za pomocą <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, jest wymagana do używania tego atrybutu na poziomie kontrolera. Na przykład, wyróżniony kod w `Startup.ConfigureServices` ustawia 2.1 flagi zgodności:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

W programie ASP.NET Core 2.2 lub nowszej `[ApiController]` atrybut można stosować do zestawu. Adnotacja w ten sposób dotyczy wszystkich kontrolerów w zestawie zachowanie interfejsu API sieci web. Należy pamiętać, że nie ma możliwości można wycofać się dla poszczególnych kontrolerów. Jako zalecenie atrybuty poziomu zestawu powinny być stosowane do `Startup` klasy:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

Zgodność wersję 2,2 lub nowszego, ustawić za pomocą <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, jest wymagana do używania tego atrybutu na poziomie zestawu.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

`[ApiController]` Atrybutu często jest sprzężona z `ControllerBase` Aby włączyć zachowanie REST specyficzne dla kontrolerów. `ControllerBase` zapewnia dostęp do metod, takich jak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

Innym rozwiązaniem jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

W poniższych sekcjach opisano wygodnych funkcji dodane przez atrybut.

### <a name="automatic-http-400-responses"></a>Automatyczne odpowiedzi HTTP 400

Błędy sprawdzania poprawności modelu automatycznie wyzwoli odpowiedź HTTP 400. W związku z tym poniższy kod staje się niepotrzebne w swoje działania:

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Użyj <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> na wyniki wynikowe odpowiedzi.

Wyłączanie zachowanie domyślne jest przydatne w przypadku, gdy wybór może odzyskać sprawność po błędzie sprawdzania poprawności modelu. Domyślnym zachowaniem jest wyłączona po <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> właściwość jest ustawiona na `true`. Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Za pomocą flagi zgodności 2,2 lub nowszy, jest domyślny typ odpowiedzi na odpowiedzi HTTP 400 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. `ValidationProblemDetails` Typ jest zgodny z [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807). Ustaw `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` właściwości `true` zamiast zwracać platformy ASP.NET Core 2.1 format błąd <xref:Microsoft.AspNetCore.Mvc.SerializableError>. Dodaj następujący kod w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a>Powiązanie źródła parametru wnioskowania

Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji. Istnieją następujące atrybuty źródło powiązania:

|Atrybut|Źródło wiążące |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | Treść żądania |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | Dane formularza w treści żądania |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | Nagłówek żądania |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | Parametr ciągu zapytania żądania |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Dane trasy z bieżącego żądania |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Usługa żądania wprowadzony jako parametru akcji |

> [!WARNING]
> Nie używaj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`). `%2f` nie będzie unescaped do `/`. Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.

Bez `[ApiController]` atrybutu, powiązanie źródła jawnie zdefiniowanych atrybutów. Bez `[ApiController]` lub innych atrybutów źródło powiązania, takich jak `[FromQuery]`, środowisko uruchomieniowe programu ASP.NET Core podejmują próbę użycia integratora modelu obiektu złożonego. Integrator modelu obiektu złożonego ściąga dane z dostawców wartości, (które mają zdefiniowaną kolejność). Na przykład "body integratora modelu" jest zawsze zgadzaj.

W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji. Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji. Powiązania atrybutów źródłowych zachowują się w następujący sposób:

* **[FromBody]**  jest wnioskowany dla parametrów typu złożonego. Wyjątkiem od tej reguły jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>. Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne. `[FromBody]` nie jest wnioskowany dla typów prostych, takich jak `string` lub `int`. W związku z tym `[FromBody]` atrybut powinien być używany dla typów prostych, gdy te funkcje są potrzebne. Kiedy akcja ma więcej niż jeden parametr określony jawnie (za pośrednictwem `[FromBody]`) lub wywnioskowane jako powiązanej z treści żądania, zgłaszany jest wyjątek. Na przykład następujące podpisy akcji może spowodować wyjątek:

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > W programie ASP.NET Core 2.1 parametrów typu kolekcji, takie jak listy i tablice są niepoprawnie wnioskowane jako [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute). [[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) powinny być używane do tych parametrów, jeśli mają być powiązane z treści żądania. To zachowanie jest stała, w programie ASP.NET Core 2.2 lub nowszej, gdzie wywnioskowana, parametry typu kolekcji można powiązać z treści domyślnie.

* **[FromForm]**  jest wnioskowany dla parametrach akcji danego typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.
* **[FromRoute]**  jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy. Gdy więcej niż jedna trasa jest zgodna z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.
* **[FromQuery]**  jest wnioskowany dla innych parametrów akcji.

Zasady wnioskowania domyślne są wyłączone podczas <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> właściwość jest ustawiona na `true`. Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a>Wnioskowanie multipart/formularza data żądania

Gdy parametr akcji jest oznaczony za pomocą [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atrybutu `multipart/form-data` żądania jest wnioskowany typ zawartości.

Domyślnym zachowaniem jest wyłączona po <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> właściwość jest ustawiona na `true`.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Dodaj następujący kod w `Startup.ConfigureServices`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a>Wymagania routingu atrybutu

Routing atrybutu staje się wymagania. Na przykład:

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> w `Startup.Configure`.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a>Problem szczegóły odpowiedzi dla kodów stanu błędu

W programie ASP.NET Core 2.2 lub nowszej, MVC przekształca wynik błędu (wynik kod stanu 400 lub nowszej) do wyniku z <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. `ProblemDetails` jest:

* Na podstawie typu [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807).
* Standardowy format Określanie szczegółów błędu czytelny dla komputerów w odpowiedzi HTTP.

Poniższy kod w akcji kontrolera należy wziąć pod uwagę:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

Odpowiedź HTTP dotycząca elementu `NotFound` ma kod stanu 404 z `ProblemDetails` treści. Na przykład:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

Funkcja szczegółów problemu wymaga flagi zgodności, 2.2 lub nowszej. Domyślnym zachowaniem jest wyłączona po `SuppressMapClientErrors` właściwość jest ustawiona na `true`. Dodaj następujący kod w `Startup.ConfigureServices`:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

Użyj `ClientErrorMapping` właściwości, aby skonfigurować zawartość `ProblemDetails` odpowiedzi. Na przykład, poniższy kod aktualizacje `type` właściwość odpowiedzi 404:

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
