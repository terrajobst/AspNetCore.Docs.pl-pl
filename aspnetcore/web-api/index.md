---
title: Tworzenie interfejsów API sieci web za pomocą platformy ASP.NET Core
author: scottaddie
description: Poznaj podstawy tworzenia internetowego interfejsu API w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/07/2019
uid: web-api/index
ms.openlocfilehash: 593fd33babc81cddfc4db2150a37e5ec3bc1a0be
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450836"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="d394c-103">Tworzenie interfejsów API sieci web za pomocą platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d394c-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="d394c-104">Przez [Scott Addie](https://github.com/scottaddie) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d394c-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d394c-105">Platforma ASP.NET Core obsługuje tworzenia usługi RESTful, nazywana również internetowych interfejsów API, za pomocą C#.</span><span class="sxs-lookup"><span data-stu-id="d394c-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="d394c-106">Do obsługi żądań, internetowy interfejs API korzysta z kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="d394c-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="d394c-107">*Kontrolery* w internetowym interfejsie API są klas, które wynikają z `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="d394c-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="d394c-108">W tym artykule pokazano, jak używać kontrolery do obsługi żądań interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d394c-108">This article shows how to use controllers for handling API requests.</span></span>

<span data-ttu-id="d394c-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="d394c-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="d394c-110">([Sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d394c-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="d394c-111">Klasa ControllerBase</span><span class="sxs-lookup"><span data-stu-id="d394c-111">ControllerBase class</span></span>

<span data-ttu-id="d394c-112">Interfejs API sieci web ma co najmniej jedną klasę kontrolera, które wynikają z <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="d394c-112">A web API has one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="d394c-113">Na przykład szablonu projektu interfejsu API sieci web tworzy kontroler wartości:</span><span class="sxs-lookup"><span data-stu-id="d394c-113">For example, the web API project template creates a Values controller:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

<span data-ttu-id="d394c-114">Nie należy tworzyć kontroler internetowego interfejsu API przez pochodząca od <xref:Microsoft.AspNetCore.Mvc.Controller> klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="d394c-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class.</span></span> <span data-ttu-id="d394c-115">`Controller` pochodzi od klasy `ControllerBase` i dodaje obsługę widoków, więc w przypadku stron sieci web obsługi żądań internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d394c-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span>  <span data-ttu-id="d394c-116">Wyjątkiem od tej reguły: Jeśli planujesz używać tego samego kontrolera zarówno dla widoków i interfejsów API, pochodzi z `Controller`.</span><span class="sxs-lookup"><span data-stu-id="d394c-116">There's an exception to this rule: if you plan to use the same controller for both views and APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="d394c-117">`ControllerBase` Klasy zawiera wiele właściwości i metod, które są przydatne do obsługi żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="d394c-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="d394c-118">Na przykład `ControllerBase.CreatedAtAction` zwraca kod stanu 201:</span><span class="sxs-lookup"><span data-stu-id="d394c-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 <span data-ttu-id="d394c-119">Poniżej przedstawiono kilka przykładów więcej metod, `ControllerBase` udostępnia.</span><span class="sxs-lookup"><span data-stu-id="d394c-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="d394c-120">Metoda</span><span class="sxs-lookup"><span data-stu-id="d394c-120">Method</span></span>  |<span data-ttu-id="d394c-121">Uwagi</span><span class="sxs-lookup"><span data-stu-id="d394c-121">Notes</span></span>  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="d394c-122">Zwraca kod stanu 400.</span><span class="sxs-lookup"><span data-stu-id="d394c-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |<span data-ttu-id="d394c-123">Zwraca kod stanu 404.</span><span class="sxs-lookup"><span data-stu-id="d394c-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="d394c-124">Zwraca plik.</span><span class="sxs-lookup"><span data-stu-id="d394c-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="d394c-125">Wywołuje [wiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="d394c-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="d394c-126">Wywołuje [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="d394c-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="d394c-127">Aby uzyskać listę wszystkich dostępnych metod i właściwości, zobacz <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="d394c-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="d394c-128">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="d394c-128">Attributes</span></span>

<span data-ttu-id="d394c-129"><xref:Microsoft.AspNetCore.Mvc> Przestrzeń nazw zawiera atrybuty, które mogą służyć do konfigurowania zachowania interfejsu API sieci web kontrolerów i metod akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="d394c-130">W poniższym przykładzie użyto atrybutów, aby określić metodę HTTP i zwrócony kodów stanu:</span><span class="sxs-lookup"><span data-stu-id="d394c-130">The following example uses attributes to specify the HTTP method accepted and the status codes returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="d394c-131">Poniżej przedstawiono kilka przykładów więcej atrybutów, które są dostępne.</span><span class="sxs-lookup"><span data-stu-id="d394c-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="d394c-132">Atrybut</span><span class="sxs-lookup"><span data-stu-id="d394c-132">Attribute</span></span>|<span data-ttu-id="d394c-133">Uwagi</span><span class="sxs-lookup"><span data-stu-id="d394c-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="d394c-134">[[Trasy]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="d394c-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="d394c-135">Określa adres URL wzorzec dla kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="d394c-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="d394c-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="d394c-137">Określa prefiks i właściwości do włączenia do wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="d394c-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="d394c-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="d394c-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="d394c-139">Określa akcję, która obsługuje metodę HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d394c-139">Identifies an action that supports the HTTP GET method.</span></span>|
|<span data-ttu-id="d394c-140">[[Zużywa]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="d394c-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="d394c-141">Określa typy danych, które akceptuje akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="d394c-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="d394c-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="d394c-143">Określa typy danych, które zwraca akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="d394c-144">Dla listy, która zawiera atrybuty, dostępności, zobacz <xref:Microsoft.AspNetCore.Mvc> przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="d394c-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="d394c-145">Atrybut klasy ApiController</span><span class="sxs-lookup"><span data-stu-id="d394c-145">ApiController attribute</span></span>

<span data-ttu-id="d394c-146">[[Klasy ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) można zastosować atrybutów do klasy kontrolera umożliwiają zachowania specyficzne dla interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="d394c-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable API-specific behaviors:</span></span>

* [<span data-ttu-id="d394c-147">Wymagania routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="d394c-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="d394c-148">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="d394c-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="d394c-149">Powiązanie źródła parametru wnioskowania</span><span class="sxs-lookup"><span data-stu-id="d394c-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="d394c-150">Wnioskowanie multipart/formularza data żądania</span><span class="sxs-lookup"><span data-stu-id="d394c-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="d394c-151">Szczegóły problemu kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="d394c-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="d394c-152">Te funkcje wymagają [zgodność wersji](<xref:mvc/compatibility-version>) 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="d394c-152">These features require a [compatibility version](<xref:mvc/compatibility-version>) of 2.1 or later.</span></span>

### <a name="apicontroller-on-specific-controllers"></a><span data-ttu-id="d394c-153">Klasy ApiController kontrolerach określonych</span><span class="sxs-lookup"><span data-stu-id="d394c-153">ApiController on specific controllers</span></span>

<span data-ttu-id="d394c-154">`[ApiController]` Atrybut można stosować do określonych kontrolerów, jak w poniższym przykładzie, za pomocą szablonu projektu:</span><span class="sxs-lookup"><span data-stu-id="d394c-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a><span data-ttu-id="d394c-155">Klasy ApiController na wielu kontrolerach</span><span class="sxs-lookup"><span data-stu-id="d394c-155">ApiController on multiple controllers</span></span>

<span data-ttu-id="d394c-156">Jedno z podejść do przy użyciu atrybutu na więcej niż jeden kontroler jest utworzenie klasy niestandardowej kontrolera podstawowego, oznaczony za pomocą `[ApiController]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="d394c-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="d394c-157">Oto przykład pokazujący niestandardowej klasy bazowej i na kontrolerze, który pochodzi od niego:</span><span class="sxs-lookup"><span data-stu-id="d394c-157">Here's an example showing a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a><span data-ttu-id="d394c-158">Klasy ApiController w zestawie</span><span class="sxs-lookup"><span data-stu-id="d394c-158">ApiController on an assembly</span></span>

<span data-ttu-id="d394c-159">Jeśli [zgodność wersji](<xref:mvc/compatibility-version>) jest ustawiony do wersji 2.2 lub nowszej, `[ApiController]` atrybut można stosować do zestawu.</span><span class="sxs-lookup"><span data-stu-id="d394c-159">If [compatibility version](<xref:mvc/compatibility-version>) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="d394c-160">Adnotacja w ten sposób dotyczy wszystkich kontrolerów w zestawie zachowanie interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="d394c-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="d394c-161">Nie ma możliwości można wycofać się dla poszczególnych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="d394c-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="d394c-162">Zastosuj atrybut poziomu zestawu do `Startup` klasy, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d394c-162">Apply the assembly-level attribute to the `Startup` class as shown in the following example:</span></span>

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a><span data-ttu-id="d394c-163">Wymagania routingu atrybutu</span><span class="sxs-lookup"><span data-stu-id="d394c-163">Attribute routing requirement</span></span>

<span data-ttu-id="d394c-164">`ApiController` Atrybutu sprawia, że atrybut routingu wymagania.</span><span class="sxs-lookup"><span data-stu-id="d394c-164">The `ApiController` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="d394c-165">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d394c-165">For example:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

<span data-ttu-id="d394c-166">Akcje są niedostępne za pośrednictwem [konwencjonalne trasy](xref:mvc/controllers/routing#conventional-routing) definicją <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d394c-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

## <a name="automatic-http-400-responses"></a><span data-ttu-id="d394c-167">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="d394c-167">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="d394c-168">`ApiController` Atrybutu sprawia, że błędy sprawdzania poprawności modelu automatycznie odpowiedzią wyzwalacza HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="d394c-168">The `ApiController` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="d394c-169">W związku z tym poniższy kod jest konieczny w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="d394c-169">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a><span data-ttu-id="d394c-170">Domyślny element BadRequest odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d394c-170">Default BadRequest response</span></span> 

<span data-ttu-id="d394c-171">Za pomocą wersji zgodności 2,2 lub nowszy, jest domyślny typ odpowiedzi na odpowiedzi HTTP 400 <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="d394c-171">With a compatibility version of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="d394c-172">`ValidationProblemDetails` Typ jest zgodny z [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="d394c-172">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="d394c-173">Aby zmienić domyślną odpowiedź do <xref:Microsoft.AspNetCore.Mvc.SerializableError>ustaw `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` właściwości `true` w `Startup.ConfigureServices`, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d394c-173">To change the default response to <xref:Microsoft.AspNetCore.Mvc.SerializableError>, set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a><span data-ttu-id="d394c-174">Dostosowywanie element BadRequest odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d394c-174">Customize BadRequest response</span></span>

<span data-ttu-id="d394c-175">Aby dostosować odpowiedzi, który jest wynikiem błędu sprawdzania poprawności, należy użyć <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="d394c-175">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="d394c-176">Dodaj następujący wyróżniony kod po `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="d394c-176">Add the following highlighted code after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a><span data-ttu-id="d394c-177">Zaloguj się automatyczne odpowiedzi 400</span><span class="sxs-lookup"><span data-stu-id="d394c-177">Log automatic 400 responses</span></span>

<span data-ttu-id="d394c-178">Zobacz [sposobu logowania się automatyczne 400 odpowiedzi na błędy sprawdzania poprawności modelu (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="d394c-178">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400"></a><span data-ttu-id="d394c-179">Wyłącz automatyczne 400</span><span class="sxs-lookup"><span data-stu-id="d394c-179">Disable automatic 400</span></span>

<span data-ttu-id="d394c-180">Aby wyłączyć automatyczne zachowanie 400, należy ustawić <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> właściwość `true`.</span><span class="sxs-lookup"><span data-stu-id="d394c-180">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="d394c-181">Dodaj następujący wyróżniony kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="d394c-181">Add the following highlighted code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="d394c-182">Powiązanie źródła parametru wnioskowania</span><span class="sxs-lookup"><span data-stu-id="d394c-182">Binding source parameter inference</span></span>

<span data-ttu-id="d394c-183">Atrybut źródłowy powiązania Określa lokalizację, w którym znajduje się wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-183">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="d394c-184">Istnieją następujące atrybuty źródło powiązania:</span><span class="sxs-lookup"><span data-stu-id="d394c-184">The following binding source attributes exist:</span></span>

|<span data-ttu-id="d394c-185">Atrybut</span><span class="sxs-lookup"><span data-stu-id="d394c-185">Attribute</span></span>|<span data-ttu-id="d394c-186">Źródło wiążące</span><span class="sxs-lookup"><span data-stu-id="d394c-186">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="d394c-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="d394c-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="d394c-188">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="d394c-188">Request body</span></span> |
|<span data-ttu-id="d394c-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="d394c-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="d394c-190">Dane formularza w treści żądania</span><span class="sxs-lookup"><span data-stu-id="d394c-190">Form data in the request body</span></span> |
|<span data-ttu-id="d394c-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="d394c-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="d394c-192">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="d394c-192">Request header</span></span> |
|<span data-ttu-id="d394c-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="d394c-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="d394c-194">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="d394c-194">Request query string parameter</span></span> |
|<span data-ttu-id="d394c-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="d394c-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="d394c-196">Dane trasy z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="d394c-196">Route data from the current request</span></span> |
|<span data-ttu-id="d394c-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="d394c-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="d394c-198">Usługa żądania wprowadzony jako parametru akcji</span><span class="sxs-lookup"><span data-stu-id="d394c-198">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="d394c-199">Nie używaj `[FromRoute]` po wartości mogą zawierać `%2f` (to znaczy `/`).</span><span class="sxs-lookup"><span data-stu-id="d394c-199">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="d394c-200">`%2f` nie będzie unescaped do `/`.</span><span class="sxs-lookup"><span data-stu-id="d394c-200">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="d394c-201">Użyj `[FromQuery]` Jeśli wartość może zawierać `%2f`.</span><span class="sxs-lookup"><span data-stu-id="d394c-201">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="d394c-202">Bez `[ApiController]` atrybutu lub powiązania atrybutów źródłowych, takich jak `[FromQuery]`, środowisko uruchomieniowe programu ASP.NET Core podejmują próbę użycia integratora modelu obiektu złożonego.</span><span class="sxs-lookup"><span data-stu-id="d394c-202">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="d394c-203">Integrator modelu obiektu złożonego ściąga dane z dostawców wartości w zdefiniowanej kolejności.</span><span class="sxs-lookup"><span data-stu-id="d394c-203">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="d394c-204">W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` podano wartość parametru ciągu zapytania adresu URL żądania:</span><span class="sxs-lookup"><span data-stu-id="d394c-204">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="d394c-205">`[ApiController]` Atrybutu stosuje reguły wnioskowania dla źródeł danych domyślne parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-205">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="d394c-206">Te reguły Zapisz z konieczności identyfikowanie źródeł powiązania, ręcznie przez zastosowanie atrybutów do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-206">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="d394c-207">Powiązanie źródła wnioskowania, dla których zasady mają następujące zachowanie:</span><span class="sxs-lookup"><span data-stu-id="d394c-207">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="d394c-208">`[FromBody]` jest wnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="d394c-208">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="d394c-209">Wyjątek dotyczący `[FromBody]` reguły wnioskowania jest dowolny typ złożony, wbudowane o specjalnym znaczeniu, takich jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="d394c-209">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="d394c-210">Wnioskowanie o kodzie źródłowym powiązania ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="d394c-210">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="d394c-211">`[FromForm]` jest wnioskowany dla parametrach akcji danego typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="d394c-211">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="d394c-212">Nie wynika dla wszystkich typów prostych lub zdefiniowanych przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d394c-212">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="d394c-213">`[FromRoute]` jest wnioskowany dla dowolnej nazwy parametru akcji parametrowi w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="d394c-213">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="d394c-214">Gdy więcej niż jedna trasa jest zgodna z parametrem akcji, wartości trasy jest uważany za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="d394c-214">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="d394c-215">`[FromQuery]` jest wnioskowany dla innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="d394c-215">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="d394c-216">Informacje o wnioskowania FromBody</span><span class="sxs-lookup"><span data-stu-id="d394c-216">FromBody inference notes</span></span>

<span data-ttu-id="d394c-217">`[FromBody]` nie jest wnioskowany dla typów prostych, takich jak `string` lub `int`.</span><span class="sxs-lookup"><span data-stu-id="d394c-217">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="d394c-218">W związku z tym `[FromBody]` atrybut powinien być używany dla typów prostych, gdy te funkcje są potrzebne.</span><span class="sxs-lookup"><span data-stu-id="d394c-218">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="d394c-219">Kiedy akcja ma więcej niż jeden parametr, powiązany z treści żądania, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d394c-219">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="d394c-220">Na przykład wszystkie poniższe podpisy metod akcji spowodować wyjątek:</span><span class="sxs-lookup"><span data-stu-id="d394c-220">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="d394c-221">`[FromBody]` wywnioskowane zarówno, ponieważ są one typy złożone.</span><span class="sxs-lookup"><span data-stu-id="d394c-221">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="d394c-222">`[FromBody]` atrybut na jednym, wywnioskowane z drugiej strony, ponieważ jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="d394c-222">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="d394c-223">`[FromBody]` atrybut na obu.</span><span class="sxs-lookup"><span data-stu-id="d394c-223">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> <span data-ttu-id="d394c-224">W programie ASP.NET Core 2.1 parametrów typu kolekcji, takie jak listy i tablice są niepoprawnie wnioskowane jako `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="d394c-224">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="d394c-225">`[FromBody]` Atrybut powinien być używany dla tych parametrów, jeśli mają być powiązane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="d394c-225">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="d394c-226">To zachowanie poprawia się w programie ASP.NET Core 2.2 lub nowszej, gdzie wywnioskowana, parametry typu kolekcji można powiązać z treści domyślnie.</span><span class="sxs-lookup"><span data-stu-id="d394c-226">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

### <a name="disable-inference-rules"></a><span data-ttu-id="d394c-227">Wyłącz zasady wnioskowania</span><span class="sxs-lookup"><span data-stu-id="d394c-227">Disable inference rules</span></span>

<span data-ttu-id="d394c-228">Aby wyłączyć wnioskowania źródło powiązania, ustaw <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> do `true`.</span><span class="sxs-lookup"><span data-stu-id="d394c-228">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="d394c-229">Dodaj następujący kod w `Startup.ConfigureServices` po `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="d394c-229">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="d394c-230">Wnioskowanie multipart/formularza data żądania</span><span class="sxs-lookup"><span data-stu-id="d394c-230">Multipart/form-data request inference</span></span>

<span data-ttu-id="d394c-231">`[ApiController]` Atrybut ma zastosowanie dana zasada wnioskowania, gdy parametr akcji jest oznaczony za pomocą [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) atrybut: `multipart/form-data` żądania jest wnioskowany typ zawartości.</span><span class="sxs-lookup"><span data-stu-id="d394c-231">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute: the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="d394c-232">Aby wyłączyć to zachowanie domyślne, należy ustawić <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> do `true` w `Startup.ConfigureServices`, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d394c-232">To disable the default behavior, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>  to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="d394c-233">Szczegóły problemu kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="d394c-233">Problem details for error status codes</span></span>

<span data-ttu-id="d394c-234">W przypadku 2,2 lub nowszej wersji platformy MVC przekształca wynik błędu (wynik kod stanu 400 lub nowszej) do wyniku z <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="d394c-234">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="d394c-235">`ProblemDetails` Typu opiera się na [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807) zapewniające szczegóły błędu czytelny dla komputerów w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d394c-235">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="d394c-236">Poniższy kod w akcji kontrolera należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="d394c-236">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="d394c-237">Odpowiedź HTTP dotycząca elementu `NotFound` ma kod stanu 404 z `ProblemDetails` treści.</span><span class="sxs-lookup"><span data-stu-id="d394c-237">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="d394c-238">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d394c-238">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="d394c-239">Dostosowywanie ProblemDetails odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d394c-239">Customize ProblemDetails response</span></span>

<span data-ttu-id="d394c-240">Użyj `ClientErrorMapping` właściwości, aby skonfigurować zawartość `ProblemDetails` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d394c-240">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="d394c-241">Na przykład, poniższy kod aktualizacje `type` właściwość odpowiedzi 404:</span><span class="sxs-lookup"><span data-stu-id="d394c-241">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a><span data-ttu-id="d394c-242">Wyłącz ProblemDetails odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d394c-242">Disable ProblemDetails response</span></span>

<span data-ttu-id="d394c-243">Automatyczne tworzenie `ProblemDetails` jest wyłączona, gdy `SuppressMapClientErrors` właściwość jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="d394c-243">The automatic creation of `ProblemDetails` is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="d394c-244">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d394c-244">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a><span data-ttu-id="d394c-245">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d394c-245">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
