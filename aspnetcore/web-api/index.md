---
title: Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web
author: scottaddie
description: Informacje o funkcjach dostępnych podczas tworzenia internetowego interfejsu API w programie ASP.NET Core i moment jest właściwy użyć każdej funkcji.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274969"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="4f704-103">Tworzenie interfejsów API za pomocą platformy ASP.NET Core w sieci web</span><span class="sxs-lookup"><span data-stu-id="4f704-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="4f704-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="4f704-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="4f704-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4f704-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4f704-106">W tym dokumencie wyjaśniono, jak tworzenie internetowego interfejsu API w programie ASP.NET Core i jest najbardziej odpowiednie do użycia każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="4f704-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="4f704-107">Dziedziczyć klasy ControllerBase</span><span class="sxs-lookup"><span data-stu-id="4f704-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="4f704-108">Dziedzicz [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) klasy w kontrolerze, który ma pełnić rolę interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="4f704-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="4f704-109">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4f704-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="4f704-110">`ControllerBase` Klasy zapewnia dostęp do wielu właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="4f704-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="4f704-111">W powyższym przykładzie obejmują niektóre z tych metod [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) i [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="4f704-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="4f704-112">Te metody są wywoływane w ramach metod akcji do zwrócenia HTTP 400 i 201 kodów stanu, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="4f704-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="4f704-113">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) także podana przez właściwość `ControllerBase`, jest dostępny do wykonania żądania weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="4f704-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="4f704-114">Dodawanie adnotacji do klasy za pomocą ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="4f704-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="4f704-115">Platforma ASP.NET Core 2.1 wprowadzono [[klasy ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) atrybutu do oznaczania klasa formantu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="4f704-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="4f704-116">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4f704-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="4f704-117">Ten atrybut często jest sprzężona z `ControllerBase` do uzyskania dostępu do użytecznych metod i właściwości.</span><span class="sxs-lookup"><span data-stu-id="4f704-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="4f704-118">`ControllerBase` zapewnia dostęp do metod, takich jak [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) i [pliku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="4f704-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="4f704-119">Innym rozwiązaniem jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="4f704-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="4f704-120">W poniższych sekcjach opisano wygodnych funkcji dodane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="4f704-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="4f704-121">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="4f704-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="4f704-122">Błędy sprawdzania poprawności automatycznie wyzwoli odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="4f704-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="4f704-123">Poniższy kod staje się niepotrzebne w swoje działania:</span><span class="sxs-lookup"><span data-stu-id="4f704-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="4f704-124">To zachowanie domyślne jest wyłączona w następującym kodem *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="4f704-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="4f704-125">Powiązanie źródła parametru wnioskowania</span><span class="sxs-lookup"><span data-stu-id="4f704-125">Binding source parameter inference</span></span>

<span data-ttu-id="4f704-126">Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="4f704-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="4f704-127">Istnieją następujące atrybuty źródło powiązania:</span><span class="sxs-lookup"><span data-stu-id="4f704-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="4f704-128">Atrybut</span><span class="sxs-lookup"><span data-stu-id="4f704-128">Attribute</span></span>|<span data-ttu-id="4f704-129">Źródło wiążące</span><span class="sxs-lookup"><span data-stu-id="4f704-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="4f704-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="4f704-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="4f704-131">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="4f704-131">Request body</span></span> |
|<span data-ttu-id="4f704-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="4f704-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="4f704-133">Dane formularza w treści żądania</span><span class="sxs-lookup"><span data-stu-id="4f704-133">Form data in the request body</span></span> |
|<span data-ttu-id="4f704-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="4f704-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="4f704-135">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="4f704-135">Request header</span></span> |
|<span data-ttu-id="4f704-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="4f704-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="4f704-137">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="4f704-137">Request query string parameter</span></span> |
|<span data-ttu-id="4f704-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="4f704-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="4f704-139">Dane trasy z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="4f704-139">Route data from the current request</span></span> |
|<span data-ttu-id="4f704-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="4f704-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="4f704-141">Usługa żądania wprowadzony jako parametru akcji</span><span class="sxs-lookup"><span data-stu-id="4f704-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="4f704-142">Czy **nie** użyj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`) ponieważ `%2f` nie unescaped do `/`.</span><span class="sxs-lookup"><span data-stu-id="4f704-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="4f704-143">Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.</span><span class="sxs-lookup"><span data-stu-id="4f704-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="4f704-144">Bez `[ApiController]` atrybutu, powiązanie źródła jawnie zdefiniowanych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4f704-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="4f704-145">W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:</span><span class="sxs-lookup"><span data-stu-id="4f704-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="4f704-146">Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="4f704-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="4f704-147">Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="4f704-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="4f704-148">Powiązania atrybutów źródłowych zachowują się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4f704-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="4f704-149">**[FromBody]**  jest wnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="4f704-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="4f704-150">Wyjątkiem od tej reguły jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) i [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="4f704-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="4f704-151">Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="4f704-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="4f704-152">Kiedy akcja ma więcej niż jeden parametr określony jawnie (za pośrednictwem `[FromBody]`) lub wywnioskowane jako powiązanej z treści żądania, zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4f704-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="4f704-153">Na przykład następujące podpisy akcji może spowodować wyjątek:</span><span class="sxs-lookup"><span data-stu-id="4f704-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="4f704-154">**[FromForm]**  jest wnioskowany dla parametrach akcji danego typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="4f704-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="4f704-155">Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4f704-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="4f704-156">**[FromRoute]**  jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="4f704-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="4f704-157">Gdy wiele tras zgodnych z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="4f704-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="4f704-158">**[FromQuery]**  jest wnioskowany dla innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="4f704-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="4f704-159">Reguły wnioskowania domyślne są wyłączone w następującym kodem *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="4f704-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="4f704-160">Wnioskowanie multipart/formularza data żądania</span><span class="sxs-lookup"><span data-stu-id="4f704-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="4f704-161">Gdy parametr akcji jest oznaczony za pomocą [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atrybutu `multipart/form-data` żądania jest wnioskowany typ zawartości.</span><span class="sxs-lookup"><span data-stu-id="4f704-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="4f704-162">Domyślnym zachowaniem jest wyłączona w następującym kodem *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="4f704-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="4f704-163">Wymagania routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="4f704-163">Attribute routing requirement</span></span>

<span data-ttu-id="4f704-164">Routing atrybutu staje się wymagania.</span><span class="sxs-lookup"><span data-stu-id="4f704-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="4f704-165">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4f704-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="4f704-166">Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) lub [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) w *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="4f704-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4f704-167">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4f704-167">Additional resources</span></span>

* [<span data-ttu-id="4f704-168">Zwracane typy akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="4f704-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="4f704-169">Niestandardowe elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="4f704-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="4f704-170">Formatowanie danych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="4f704-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="4f704-171">Strony pomocy korzystające z programu Swagger</span><span class="sxs-lookup"><span data-stu-id="4f704-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="4f704-172">Routing do akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="4f704-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
