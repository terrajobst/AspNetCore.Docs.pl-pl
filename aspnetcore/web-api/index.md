---
title: Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web
author: scottaddie
description: Informacje o funkcjach dostępnych podczas tworzenia internetowego interfejsu API w programie ASP.NET Core i moment jest właściwy użyć każdej funkcji.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="969ab-103">Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web</span><span class="sxs-lookup"><span data-stu-id="969ab-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="969ab-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="969ab-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="969ab-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="969ab-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="969ab-106">W tym dokumencie wyjaśniono, jak tworzenie internetowego interfejsu API w programie ASP.NET Core i jest najbardziej odpowiednie do użycia każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="969ab-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="969ab-107">Dziedziczyć klasy ControllerBase</span><span class="sxs-lookup"><span data-stu-id="969ab-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="969ab-108">Dziedzicz <xref:Microsoft.AspNetCore.Mvc.ControllerBase> klasy w kontrolerze, który ma pełnić rolę interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="969ab-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="969ab-109">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="969ab-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="969ab-110">`ControllerBase` Klasa udostępnia kilka właściwości i metod.</span><span class="sxs-lookup"><span data-stu-id="969ab-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="969ab-111">W poprzednim kodzie przykłady <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="969ab-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="969ab-112">Te metody są wywoływane w ramach metod akcji do zwrócenia HTTP 400 i 201 kodów stanu, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="969ab-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="969ab-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Także podana przez właściwość `ControllerBase`, jest dostępny do obsługi żądania weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="969ab-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a><span data-ttu-id="969ab-114">Adnotacja z atrybutem klasy ApiController</span><span class="sxs-lookup"><span data-stu-id="969ab-114">Annotation with ApiController attribute</span></span>

<span data-ttu-id="969ab-115">Platforma ASP.NET Core 2.1 wprowadzono [[klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu do oznaczania klasa formantu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="969ab-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="969ab-116">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="969ab-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="969ab-117">Zgodność wersji 2.1 lub nowszej, ustawić za pomocą <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, jest wymagana do używania tego atrybutu na poziomie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="969ab-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="969ab-118">Na przykład, wyróżniony kod w `Startup.ConfigureServices` ustawia 2.1 flagi zgodności:</span><span class="sxs-lookup"><span data-stu-id="969ab-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="969ab-119">Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="969ab-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="969ab-120">W programie ASP.NET Core 2.2 lub nowszej `[ApiController]` atrybut można stosować do zestawu.</span><span class="sxs-lookup"><span data-stu-id="969ab-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="969ab-121">Adnotacja w ten sposób dotyczy wszystkich kontrolerów w zestawie zachowanie interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="969ab-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="969ab-122">Należy pamiętać, że nie ma możliwości można wycofać się dla poszczególnych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="969ab-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="969ab-123">Jako zalecenie atrybuty poziomu zestawu powinny być stosowane do `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="969ab-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="969ab-124">Zgodność wersję 2,2 lub nowszego, ustawić za pomocą <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, jest wymagana do używania tego atrybutu na poziomie zestawu.</span><span class="sxs-lookup"><span data-stu-id="969ab-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="969ab-125">`[ApiController]` Atrybutu często jest sprzężona z `ControllerBase` Aby włączyć zachowanie REST specyficzne dla kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="969ab-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="969ab-126">`ControllerBase` zapewnia dostęp do metod, takich jak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="969ab-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="969ab-127">Innym rozwiązaniem jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="969ab-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="969ab-128">W poniższych sekcjach opisano wygodnych funkcji dodane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="969ab-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="969ab-129">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="969ab-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="969ab-130">Błędy sprawdzania poprawności modelu automatycznie wyzwoli odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="969ab-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="969ab-131">W związku z tym poniższy kod staje się niepotrzebne w swoje działania:</span><span class="sxs-lookup"><span data-stu-id="969ab-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="969ab-132">Użyj <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> na wyniki wynikowe odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="969ab-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="969ab-133">Wyłączanie zachowanie domyślne jest przydatne w przypadku, gdy wybór może odzyskać sprawność po błędzie sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="969ab-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="969ab-134">Domyślnym zachowaniem jest wyłączona po <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="969ab-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="969ab-135">Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="969ab-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="969ab-136">Za pomocą flagi zgodności 2,2 lub nowszy, jest domyślny typ odpowiedzi na odpowiedzi HTTP 400 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="969ab-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="969ab-137">`ValidationProblemDetails` Typ jest zgodny z [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="969ab-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="969ab-138">Ustaw `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` właściwości `true` zamiast zwracać platformy ASP.NET Core 2.1 format błąd <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="969ab-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="969ab-139">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="969ab-139">Add the following code in `Startup.ConfigureServices`:</span></span>

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

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="969ab-140">Powiązanie źródła parametru wnioskowania</span><span class="sxs-lookup"><span data-stu-id="969ab-140">Binding source parameter inference</span></span>

<span data-ttu-id="969ab-141">Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="969ab-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="969ab-142">Istnieją następujące atrybuty źródło powiązania:</span><span class="sxs-lookup"><span data-stu-id="969ab-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="969ab-143">Atrybut</span><span class="sxs-lookup"><span data-stu-id="969ab-143">Attribute</span></span>|<span data-ttu-id="969ab-144">Źródło wiążące</span><span class="sxs-lookup"><span data-stu-id="969ab-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="969ab-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="969ab-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="969ab-146">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="969ab-146">Request body</span></span> |
|<span data-ttu-id="969ab-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="969ab-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="969ab-148">Dane formularza w treści żądania</span><span class="sxs-lookup"><span data-stu-id="969ab-148">Form data in the request body</span></span> |
|<span data-ttu-id="969ab-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="969ab-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="969ab-150">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="969ab-150">Request header</span></span> |
|<span data-ttu-id="969ab-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="969ab-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="969ab-152">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="969ab-152">Request query string parameter</span></span> |
|<span data-ttu-id="969ab-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="969ab-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="969ab-154">Dane trasy z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="969ab-154">Route data from the current request</span></span> |
|<span data-ttu-id="969ab-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="969ab-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="969ab-156">Usługa żądania wprowadzony jako parametru akcji</span><span class="sxs-lookup"><span data-stu-id="969ab-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="969ab-157">Nie używaj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`).</span><span class="sxs-lookup"><span data-stu-id="969ab-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="969ab-158">`%2f` nie będzie unescaped do `/`.</span><span class="sxs-lookup"><span data-stu-id="969ab-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="969ab-159">Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.</span><span class="sxs-lookup"><span data-stu-id="969ab-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="969ab-160">Bez `[ApiController]` atrybutu, powiązanie źródła jawnie zdefiniowanych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="969ab-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="969ab-161">Bez `[ApiController]` lub innych atrybutów źródło powiązania, takich jak `[FromQuery]`, środowisko uruchomieniowe programu ASP.NET Core podejmują próbę użycia integratora modelu obiektu złożonego.</span><span class="sxs-lookup"><span data-stu-id="969ab-161">Without `[ApiController]` or other binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="969ab-162">Integrator modelu obiektu złożonego ściąga dane z dostawców wartości, (które mają zdefiniowaną kolejność).</span><span class="sxs-lookup"><span data-stu-id="969ab-162">The complex object model binder pulls data from value providers (which have a defined order).</span></span> <span data-ttu-id="969ab-163">Na przykład "body integratora modelu" jest zawsze zgadzaj.</span><span class="sxs-lookup"><span data-stu-id="969ab-163">For instance, the 'body model binder' is always opt in.</span></span>

<span data-ttu-id="969ab-164">W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:</span><span class="sxs-lookup"><span data-stu-id="969ab-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="969ab-165">Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="969ab-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="969ab-166">Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="969ab-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="969ab-167">Powiązania atrybutów źródłowych zachowują się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="969ab-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="969ab-168">**[FromBody]**  jest wnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="969ab-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="969ab-169">Wyjątkiem od tej reguły jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="969ab-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="969ab-170">Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="969ab-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="969ab-171">`[FromBody]` nie jest wnioskowany dla typów prostych, takich jak `string` lub `int`.</span><span class="sxs-lookup"><span data-stu-id="969ab-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="969ab-172">W związku z tym `[FromBody]` atrybut powinien być używany dla typów prostych, gdy te funkcje są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="969ab-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="969ab-173">Kiedy akcja ma więcej niż jeden parametr określony jawnie (za pośrednictwem `[FromBody]`) lub wywnioskowane jako powiązanej z treści żądania, zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="969ab-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="969ab-174">Na przykład następujące podpisy akcji może spowodować wyjątek:</span><span class="sxs-lookup"><span data-stu-id="969ab-174">For example, the following action signatures cause an exception:</span></span>

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > <span data-ttu-id="969ab-175">W programie ASP.NET Core 2.1 parametrów typu kolekcji, takie jak listy i tablice są niepoprawnie wnioskowane jako [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span><span class="sxs-lookup"><span data-stu-id="969ab-175">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute).</span></span> <span data-ttu-id="969ab-176">[[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) powinny być używane do tych parametrów, jeśli mają być powiązane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="969ab-176">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="969ab-177">To zachowanie jest stała, w programie ASP.NET Core 2.2 lub nowszej, gdzie wywnioskowana, parametry typu kolekcji można powiązać z treści domyślnie.</span><span class="sxs-lookup"><span data-stu-id="969ab-177">This behavior is fixed in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

* <span data-ttu-id="969ab-178">**[FromForm]**  jest wnioskowany dla parametrach akcji danego typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="969ab-178">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="969ab-179">Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="969ab-179">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="969ab-180">**[FromRoute]**  jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="969ab-180">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="969ab-181">Gdy więcej niż jedna trasa jest zgodna z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="969ab-181">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="969ab-182">**[FromQuery]**  jest wnioskowany dla innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="969ab-182">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="969ab-183">Zasady wnioskowania domyślne są wyłączone podczas <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="969ab-183">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="969ab-184">Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="969ab-184">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="969ab-185">Wnioskowanie multipart/formularza data żądania</span><span class="sxs-lookup"><span data-stu-id="969ab-185">Multipart/form-data request inference</span></span>

<span data-ttu-id="969ab-186">Gdy parametr akcji jest oznaczony za pomocą [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atrybutu `multipart/form-data` żądania jest wnioskowany typ zawartości.</span><span class="sxs-lookup"><span data-stu-id="969ab-186">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="969ab-187">Domyślnym zachowaniem jest wyłączona po <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="969ab-187">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="969ab-188">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="969ab-188">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="969ab-189">Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="969ab-189">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="969ab-190">Wymagania routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="969ab-190">Attribute routing requirement</span></span>

<span data-ttu-id="969ab-191">Routing atrybutu staje się wymagania.</span><span class="sxs-lookup"><span data-stu-id="969ab-191">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="969ab-192">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="969ab-192">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="969ab-193">Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="969ab-193">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="969ab-194">Problem szczegóły odpowiedzi dla kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="969ab-194">Problem details responses for error status codes</span></span>

<span data-ttu-id="969ab-195">W programie ASP.NET Core 2.2 lub nowszej, MVC przekształca wynik błędu (wynik kod stanu 400 lub nowszej) do wyniku z <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="969ab-195">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="969ab-196">`ProblemDetails` jest:</span><span class="sxs-lookup"><span data-stu-id="969ab-196">`ProblemDetails` is:</span></span>

* <span data-ttu-id="969ab-197">Na podstawie typu [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="969ab-197">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="969ab-198">Standardowy format Określanie szczegółów błędu czytelny dla komputerów w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="969ab-198">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="969ab-199">Poniższy kod w akcji kontrolera należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="969ab-199">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="969ab-200">Odpowiedź HTTP dotycząca elementu `NotFound` ma kod stanu 404 z `ProblemDetails` treści.</span><span class="sxs-lookup"><span data-stu-id="969ab-200">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="969ab-201">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="969ab-201">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="969ab-202">Funkcja szczegółów problemu wymaga flagi zgodności, 2.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="969ab-202">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="969ab-203">Domyślnym zachowaniem jest wyłączona po `SuppressMapClientErrors` właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="969ab-203">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="969ab-204">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="969ab-204">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="969ab-205">Użyj `ClientErrorMapping` właściwości, aby skonfigurować zawartość `ProblemDetails` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="969ab-205">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="969ab-206">Na przykład, poniższy kod aktualizacje `type` właściwość odpowiedzi 404:</span><span class="sxs-lookup"><span data-stu-id="969ab-206">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="969ab-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="969ab-207">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
