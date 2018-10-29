---
title: Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web
author: scottaddie
description: Informacje o funkcjach dostępnych podczas tworzenia internetowego interfejsu API w programie ASP.NET Core i moment jest właściwy użyć każdej funkcji.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: e4615e5d416ba2433d55309b25ee3643c6c636ac
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207007"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="32bd1-103">Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web</span><span class="sxs-lookup"><span data-stu-id="32bd1-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="32bd1-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="32bd1-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="32bd1-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32bd1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="32bd1-106">W tym dokumencie wyjaśniono, jak tworzenie internetowego interfejsu API w programie ASP.NET Core i jest najbardziej odpowiednie do użycia każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="32bd1-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="32bd1-107">Dziedziczyć klasy ControllerBase</span><span class="sxs-lookup"><span data-stu-id="32bd1-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="32bd1-108">Dziedzicz <xref:Microsoft.AspNetCore.Mvc.ControllerBase> klasy w kontrolerze, który ma pełnić rolę interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="32bd1-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="32bd1-109">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="32bd1-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="32bd1-110">`ControllerBase` Klasa udostępnia kilka właściwości i metod.</span><span class="sxs-lookup"><span data-stu-id="32bd1-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="32bd1-111">W poprzednim kodzie przykłady <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="32bd1-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="32bd1-112">Te metody są wywoływane w ramach metod akcji do zwrócenia HTTP 400 i 201 kodów stanu, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="32bd1-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="32bd1-113"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> Także podana przez właściwość `ControllerBase`, jest dostępny do obsługi żądania weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="32bd1-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="32bd1-114">Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="32bd1-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="32bd1-115">Platforma ASP.NET Core 2.1 wprowadzono [[klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atrybutu do oznaczania klasa formantu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="32bd1-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="32bd1-116">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="32bd1-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="32bd1-117">Zgodność wersji 2.1 lub nowszej, ustawić za pomocą <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, jest wymagana do używania tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="32bd1-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="32bd1-118">Na przykład, wyróżniony kod w *Startup.ConfigureServices* ustawia 2,2 flagi zgodności:</span><span class="sxs-lookup"><span data-stu-id="32bd1-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="32bd1-119">Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="32bd1-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="32bd1-120">`[ApiController]` Atrybutu często jest sprzężona z `ControllerBase` Aby włączyć zachowanie REST specyficzne dla kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="32bd1-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="32bd1-121">`ControllerBase` zapewnia dostęp do metod, takich jak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="32bd1-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="32bd1-122">Innym rozwiązaniem jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="32bd1-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="32bd1-123">W poniższych sekcjach opisano wygodnych funkcji dodane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="32bd1-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="32bd1-124">Problem szczegóły odpowiedzi dla kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="32bd1-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="32bd1-125">ASP.NET Core 2.1 i nowsze obejmuje [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), na podstawie typu [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="32bd1-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="32bd1-126">`ProblemDetails` Typu udostępnia standardowy format dla przekazywania maszyny można odczytać szczegóły błędów w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="32bd1-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="32bd1-127">W programie ASP.NET Core 2.2 i nowszych MVC przekształca wyniki kodu stanu błędu (kod stanu 400 lub nowszy) do wyniku za pomocą `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="32bd1-128">Rozważmy poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="32bd1-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="32bd1-129">Odpowiedź HTTP dotycząca elementu `NotFound` wynik zawiera kod stanu 404 z `ProblemDetails` treści podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="32bd1-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="32bd1-130">Funkcja szczegółów problemu wymaga flagi zgodności, 2.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="32bd1-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="32bd1-131">Domyślnym zachowaniem jest wyłączona po [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="32bd1-132">Następujący wyróżniony kod z `Startup.ConfigureServices` wyłącza szczegóły problemu:</span><span class="sxs-lookup"><span data-stu-id="32bd1-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="32bd1-133">Użyj [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> właściwości, aby skonfigurować zawartość `ProblemDetails` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="32bd1-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="32bd1-134">Na przykład, poniższy kod aktualizacje `type` właściwość odpowiedzi 404:</span><span class="sxs-lookup"><span data-stu-id="32bd1-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="32bd1-135">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="32bd1-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="32bd1-136">Błędy sprawdzania poprawności automatycznie wyzwoli odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="32bd1-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="32bd1-137">Poniższy kod staje się niepotrzebne w swoje działania:</span><span class="sxs-lookup"><span data-stu-id="32bd1-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="32bd1-138">Użyj <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> dostosować dane wyjściowe wynikowe odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="32bd1-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="32bd1-139">Domyślnym zachowaniem jest wyłączona po <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="32bd1-140">Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="32bd1-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="32bd1-141">Za pomocą flagi zgodności 2,2 lub nowszy, jest domyślny typ odpowiedzi zwracanych dla odpowiedzi 400 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="32bd1-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="32bd1-142">Użyj [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> właściwości, aby użyć formatu błędu platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="32bd1-142">Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="32bd1-143">Powiązanie źródła parametru wnioskowania</span><span class="sxs-lookup"><span data-stu-id="32bd1-143">Binding source parameter inference</span></span>

<span data-ttu-id="32bd1-144">Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="32bd1-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="32bd1-145">Istnieją następujące atrybuty źródło powiązania:</span><span class="sxs-lookup"><span data-stu-id="32bd1-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="32bd1-146">Atrybut</span><span class="sxs-lookup"><span data-stu-id="32bd1-146">Attribute</span></span>|<span data-ttu-id="32bd1-147">Źródło wiążące</span><span class="sxs-lookup"><span data-stu-id="32bd1-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="32bd1-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="32bd1-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="32bd1-149">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="32bd1-149">Request body</span></span> |
|<span data-ttu-id="32bd1-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="32bd1-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="32bd1-151">Dane formularza w treści żądania</span><span class="sxs-lookup"><span data-stu-id="32bd1-151">Form data in the request body</span></span> |
|<span data-ttu-id="32bd1-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="32bd1-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="32bd1-153">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="32bd1-153">Request header</span></span> |
|<span data-ttu-id="32bd1-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="32bd1-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="32bd1-155">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="32bd1-155">Request query string parameter</span></span> |
|<span data-ttu-id="32bd1-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="32bd1-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="32bd1-157">Dane trasy z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="32bd1-157">Route data from the current request</span></span> |
|<span data-ttu-id="32bd1-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="32bd1-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="32bd1-159">Usługa żądania wprowadzony jako parametru akcji</span><span class="sxs-lookup"><span data-stu-id="32bd1-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="32bd1-160">Nie używaj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`).</span><span class="sxs-lookup"><span data-stu-id="32bd1-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="32bd1-161">`%2f` nie będzie unescaped do `/`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="32bd1-162">Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="32bd1-163">Bez `[ApiController]` atrybutu, powiązanie źródła jawnie zdefiniowanych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="32bd1-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="32bd1-164">W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:</span><span class="sxs-lookup"><span data-stu-id="32bd1-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="32bd1-165">Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="32bd1-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="32bd1-166">Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="32bd1-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="32bd1-167">Powiązania atrybutów źródłowych zachowują się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="32bd1-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="32bd1-168">**[FromBody]**  jest wnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="32bd1-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="32bd1-169">Wyjątkiem od tej reguły jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="32bd1-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="32bd1-170">Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="32bd1-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="32bd1-171">`[FromBody]` nie jest wnioskowany dla typów prostych, takich jak `string` lub `int`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="32bd1-172">W związku z tym `[FromBody]` atrybut powinien być używany dla typów prostych, gdy te funkcje.</span><span class="sxs-lookup"><span data-stu-id="32bd1-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="32bd1-173">Kiedy akcja ma więcej niż jeden parametr określony jawnie (za pośrednictwem `[FromBody]`) lub wywnioskowane jako powiązanej z treści żądania, zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="32bd1-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="32bd1-174">Na przykład następujące podpisy akcji może spowodować wyjątek:</span><span class="sxs-lookup"><span data-stu-id="32bd1-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="32bd1-175">**[FromForm]**  jest wnioskowany dla parametrach akcji danego typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="32bd1-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="32bd1-176">Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="32bd1-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="32bd1-177">**[FromRoute]**  jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="32bd1-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="32bd1-178">Gdy więcej niż jedna trasa jest zgodna z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="32bd1-179">**[FromQuery]**  jest wnioskowany dla innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="32bd1-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="32bd1-180">Zasady wnioskowania domyślne są wyłączone podczas <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="32bd1-181">Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="32bd1-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="32bd1-182">Wnioskowanie multipart/formularza data żądania</span><span class="sxs-lookup"><span data-stu-id="32bd1-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="32bd1-183">Gdy parametr akcji jest oznaczony za pomocą [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atrybutu `multipart/form-data` żądania jest wnioskowany typ zawartości.</span><span class="sxs-lookup"><span data-stu-id="32bd1-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="32bd1-184">Domyślnym zachowaniem jest wyłączona po <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="32bd1-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="32bd1-185">Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="32bd1-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="32bd1-186">Wymagania routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="32bd1-186">Attribute routing requirement</span></span>

<span data-ttu-id="32bd1-187">Routing atrybutu staje się wymagania.</span><span class="sxs-lookup"><span data-stu-id="32bd1-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="32bd1-188">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="32bd1-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="32bd1-189">Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> w *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="32bd1-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="32bd1-190">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="32bd1-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
