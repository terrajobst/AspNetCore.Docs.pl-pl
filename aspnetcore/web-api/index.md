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
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="d02ed-103">Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web</span><span class="sxs-lookup"><span data-stu-id="d02ed-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="d02ed-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="d02ed-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="d02ed-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d02ed-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d02ed-106">W tym dokumencie wyjaśniono, jak tworzenie internetowego interfejsu API w programie ASP.NET Core i jest najbardziej odpowiednie do użycia każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="d02ed-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="d02ed-107">Dziedziczyć klasy ControllerBase</span><span class="sxs-lookup"><span data-stu-id="d02ed-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="d02ed-108">Dziedzicz [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) klasy w kontrolerze, który ma pełnić rolę interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="d02ed-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="d02ed-109">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d02ed-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="d02ed-110">`ControllerBase` Klasa udostępnia kilka właściwości i metod.</span><span class="sxs-lookup"><span data-stu-id="d02ed-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="d02ed-111">W poprzednim kodzie przykłady [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) i [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="d02ed-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="d02ed-112">Te metody są wywoływane w ramach metod akcji do zwrócenia HTTP 400 i 201 kodów stanu, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="d02ed-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="d02ed-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) także podana przez właściwość `ControllerBase`, jest dostępny do obsługi żądania weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="d02ed-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="d02ed-114">Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="d02ed-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="d02ed-115">Platforma ASP.NET Core 2.1 wprowadzono [[klasy ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu do oznaczania klasa formantu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="d02ed-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="d02ed-116">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d02ed-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="d02ed-117">Zgodność wersji 2.1 lub nowszej, ustawić za pomocą [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), jest wymagana do używania tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d02ed-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="d02ed-118">Na przykład, wyróżniony kod w *Startup.ConfigureServices* ustawia 2.1 flagi zgodności:</span><span class="sxs-lookup"><span data-stu-id="d02ed-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="d02ed-119">`[ApiController]` Atrybutu często jest sprzężona z `ControllerBase` Aby włączyć zachowanie REST specyficzne dla kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="d02ed-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="d02ed-120">`ControllerBase` zapewnia dostęp do metod, takich jak [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) i [pliku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="d02ed-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="d02ed-121">Innym rozwiązaniem jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="d02ed-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="d02ed-122">W poniższych sekcjach opisano wygodnych funkcji dodane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="d02ed-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="d02ed-123">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="d02ed-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="d02ed-124">Błędy sprawdzania poprawności automatycznie wyzwoli odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="d02ed-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="d02ed-125">Poniższy kod staje się niepotrzebne w swoje działania:</span><span class="sxs-lookup"><span data-stu-id="d02ed-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="d02ed-126">Domyślnym zachowaniem jest wyłączona po [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="d02ed-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="d02ed-127">Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="d02ed-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="d02ed-128">Powiązanie źródła parametru wnioskowania</span><span class="sxs-lookup"><span data-stu-id="d02ed-128">Binding source parameter inference</span></span>

<span data-ttu-id="d02ed-129">Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="d02ed-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="d02ed-130">Istnieją następujące atrybuty źródło powiązania:</span><span class="sxs-lookup"><span data-stu-id="d02ed-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="d02ed-131">Atrybut</span><span class="sxs-lookup"><span data-stu-id="d02ed-131">Attribute</span></span>|<span data-ttu-id="d02ed-132">Źródło wiążące</span><span class="sxs-lookup"><span data-stu-id="d02ed-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="d02ed-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="d02ed-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="d02ed-134">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="d02ed-134">Request body</span></span> |
|<span data-ttu-id="d02ed-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="d02ed-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="d02ed-136">Dane formularza w treści żądania</span><span class="sxs-lookup"><span data-stu-id="d02ed-136">Form data in the request body</span></span> |
|<span data-ttu-id="d02ed-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="d02ed-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="d02ed-138">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="d02ed-138">Request header</span></span> |
|<span data-ttu-id="d02ed-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="d02ed-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="d02ed-140">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="d02ed-140">Request query string parameter</span></span> |
|<span data-ttu-id="d02ed-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="d02ed-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="d02ed-142">Dane trasy z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="d02ed-142">Route data from the current request</span></span> |
|<span data-ttu-id="d02ed-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="d02ed-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="d02ed-144">Usługa żądania wprowadzony jako parametru akcji</span><span class="sxs-lookup"><span data-stu-id="d02ed-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="d02ed-145">Nie używaj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`).</span><span class="sxs-lookup"><span data-stu-id="d02ed-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="d02ed-146">`%2f` nie będzie unescaped do `/`.</span><span class="sxs-lookup"><span data-stu-id="d02ed-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="d02ed-147">Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.</span><span class="sxs-lookup"><span data-stu-id="d02ed-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="d02ed-148">Bez `[ApiController]` atrybutu, powiązanie źródła jawnie zdefiniowanych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="d02ed-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="d02ed-149">W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:</span><span class="sxs-lookup"><span data-stu-id="d02ed-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="d02ed-150">Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="d02ed-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="d02ed-151">Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="d02ed-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="d02ed-152">Powiązania atrybutów źródłowych zachowują się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d02ed-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="d02ed-153">**[FromBody]**  jest wnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="d02ed-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="d02ed-154">Wyjątkiem od tej reguły jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) i [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="d02ed-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="d02ed-155">Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="d02ed-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="d02ed-156">Kiedy akcja ma więcej niż jeden parametr określony jawnie (za pośrednictwem `[FromBody]`) lub wywnioskowane jako powiązanej z treści żądania, zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d02ed-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="d02ed-157">Na przykład następujące podpisy akcji może spowodować wyjątek:</span><span class="sxs-lookup"><span data-stu-id="d02ed-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="d02ed-158">**[FromForm]**  jest wnioskowany dla parametrach akcji danego typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="d02ed-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="d02ed-159">Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d02ed-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="d02ed-160">**[FromRoute]**  jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="d02ed-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="d02ed-161">Gdy więcej niż jedna trasa jest zgodna z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="d02ed-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="d02ed-162">**[FromQuery]**  jest wnioskowany dla innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="d02ed-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="d02ed-163">Zasady wnioskowania domyślne są wyłączone podczas [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="d02ed-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="d02ed-164">Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="d02ed-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="d02ed-165">Wnioskowanie multipart/formularza data żądania</span><span class="sxs-lookup"><span data-stu-id="d02ed-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="d02ed-166">Gdy parametr akcji jest oznaczony za pomocą [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atrybutu `multipart/form-data` żądania jest wnioskowany typ zawartości.</span><span class="sxs-lookup"><span data-stu-id="d02ed-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="d02ed-167">Domyślnym zachowaniem jest wyłączona po [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="d02ed-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="d02ed-168">Dodaj następujący kod w *Startup.ConfigureServices* po `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="d02ed-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="d02ed-169">Wymagania routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="d02ed-169">Attribute routing requirement</span></span>

<span data-ttu-id="d02ed-170">Routing atrybutu staje się wymagania.</span><span class="sxs-lookup"><span data-stu-id="d02ed-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="d02ed-171">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d02ed-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="d02ed-172">Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) lub [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) w *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="d02ed-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d02ed-173">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d02ed-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
