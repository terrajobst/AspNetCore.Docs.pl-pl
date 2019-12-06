---
title: Tworzenie internetowych interfejsów API za pomocą ASP.NET Core
author: scottaddie
description: Poznaj podstawy tworzenia internetowego interfejsu API w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/22/2019
uid: web-api/index
ms.openlocfilehash: 5ef8b4d012f4ed90339ffea191612e4dc365d958
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880527"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="7f00c-103">Tworzenie internetowych interfejsów API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f00c-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="7f00c-104">Przez [Scott Addie](https://github.com/scottaddie) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7f00c-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7f00c-105">Platforma ASP.NET Core obsługuje tworzenie usług RESTful, znanych także jako internetowe interfejsy API, za pomocą języka C#.</span><span class="sxs-lookup"><span data-stu-id="7f00c-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="7f00c-106">Aby obsługiwać żądania, interfejs API sieci Web używa kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7f00c-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="7f00c-107">*Kontrolery* w INTERNETowym interfejsie API są klasami pochodnymi od `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="7f00c-108">W tym artykule pokazano, jak używać kontrolerów do obsługi żądań interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7f00c-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="7f00c-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="7f00c-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="7f00c-110">([Jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7f00c-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="7f00c-111">Klasa ControllerBase</span><span class="sxs-lookup"><span data-stu-id="7f00c-111">ControllerBase class</span></span>

<span data-ttu-id="7f00c-112">Internetowy interfejs API składa się z co najmniej jednej klasy kontrolera, która pochodzi od <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="7f00c-113">Szablon projektu internetowego interfejsu API zawiera kontroler początkowy:</span><span class="sxs-lookup"><span data-stu-id="7f00c-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="7f00c-114">Nie twórz kontrolera interfejsu API sieci Web, pobierając z klasy <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="7f00c-115">`Controller` pochodzi od `ControllerBase` i dodaje obsługę widoków, dlatego służy do obsługi stron sieci Web, a nie żądań interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7f00c-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="7f00c-116">Istnieje wyjątek od tej reguły: Jeśli planujesz używać tego samego kontrolera dla widoków i interfejsów API sieci Web, utwórz go z `Controller`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="7f00c-117">Klasa `ControllerBase` dostarcza wiele właściwości i metod, które są przydatne do obsługi żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f00c-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="7f00c-118">Na przykład `ControllerBase.CreatedAtAction` zwraca kod stanu 201:</span><span class="sxs-lookup"><span data-stu-id="7f00c-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="7f00c-119">Poniżej przedstawiono kilka przykładów metod, które zapewnia `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="7f00c-120">Metoda</span><span class="sxs-lookup"><span data-stu-id="7f00c-120">Method</span></span>   |<span data-ttu-id="7f00c-121">Uwagi</span><span class="sxs-lookup"><span data-stu-id="7f00c-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| <span data-ttu-id="7f00c-122">Zwraca kod stanu 400.</span><span class="sxs-lookup"><span data-stu-id="7f00c-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|<span data-ttu-id="7f00c-123">Zwraca kod stanu 404.</span><span class="sxs-lookup"><span data-stu-id="7f00c-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|<span data-ttu-id="7f00c-124">Zwraca plik.</span><span class="sxs-lookup"><span data-stu-id="7f00c-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|<span data-ttu-id="7f00c-125">Wywołuje [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="7f00c-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|<span data-ttu-id="7f00c-126">Wywołuje [walidację modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="7f00c-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="7f00c-127">Aby uzyskać listę wszystkich dostępnych metod i właściwości, zobacz <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="7f00c-128">{1&gt;{2&gt;Atrybuty&lt;2}&lt;1}</span><span class="sxs-lookup"><span data-stu-id="7f00c-128">Attributes</span></span>

<span data-ttu-id="7f00c-129">Przestrzeń nazw <xref:Microsoft.AspNetCore.Mvc> zawiera atrybuty, których można użyć do skonfigurowania zachowania kontrolerów internetowego interfejsu API i metod akcji.</span><span class="sxs-lookup"><span data-stu-id="7f00c-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="7f00c-130">Poniższy przykład używa atrybutów, aby określić obsługiwane zlecenie akcji HTTP i wszystkie znane kody stanu HTTP, które mogą zostać zwrócone:</span><span class="sxs-lookup"><span data-stu-id="7f00c-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="7f00c-131">Poniżej przedstawiono kilka przykładów dostępnych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="7f00c-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="7f00c-132">Atrybut</span><span class="sxs-lookup"><span data-stu-id="7f00c-132">Attribute</span></span>|<span data-ttu-id="7f00c-133">Uwagi</span><span class="sxs-lookup"><span data-stu-id="7f00c-133">Notes</span></span>|
|---------|-----|
|[`[Route]`](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |<span data-ttu-id="7f00c-134">Określa wzorzec adresu URL dla kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="7f00c-134">Specifies URL pattern for a controller or action.</span></span>|
|[`[Bind]`](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |<span data-ttu-id="7f00c-135">Określa prefiks i właściwości, które mają zostać dołączone do powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="7f00c-135">Specifies prefix and properties to include for model binding.</span></span>|
|[`[HttpGet]`](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |<span data-ttu-id="7f00c-136">Identyfikuje akcję, która obsługuje czasownik HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="7f00c-136">Identifies an action that supports the HTTP GET action verb.</span></span>|
|[`[Consumes]`](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|<span data-ttu-id="7f00c-137">Określa typy danych, które akcja akceptuje.</span><span class="sxs-lookup"><span data-stu-id="7f00c-137">Specifies data types that an action accepts.</span></span>|
|[`[Produces]`](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|<span data-ttu-id="7f00c-138">Określa typy danych, które zwraca akcja.</span><span class="sxs-lookup"><span data-stu-id="7f00c-138">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="7f00c-139">Aby zapoznać się z listą zawierającą dostępne atrybuty, zapoznaj się z przestrzenią nazw <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-139">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="7f00c-140">ApiController — atrybut</span><span class="sxs-lookup"><span data-stu-id="7f00c-140">ApiController attribute</span></span>

<span data-ttu-id="7f00c-141">Atrybut [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) można zastosować do klasy kontrolera, aby włączyć następujące zachowania dotyczące interfejsów API ceniona:</span><span class="sxs-lookup"><span data-stu-id="7f00c-141">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="7f00c-142">Wymagania dotyczące routingu atrybutów</span><span class="sxs-lookup"><span data-stu-id="7f00c-142">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="7f00c-143">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="7f00c-143">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="7f00c-144">Wnioskowanie parametru źródła powiązania</span><span class="sxs-lookup"><span data-stu-id="7f00c-144">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="7f00c-145">Wieloczęściowe/formularz-wnioskowanie dotyczące danych</span><span class="sxs-lookup"><span data-stu-id="7f00c-145">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="7f00c-146">Szczegóły problemu dotyczące kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="7f00c-146">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="7f00c-147">Te funkcje wymagają [wersji](xref:mvc/compatibility-version) 2,1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="7f00c-147">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="7f00c-148">Atrybut na określonych kontrolerach</span><span class="sxs-lookup"><span data-stu-id="7f00c-148">Attribute on specific controllers</span></span>

<span data-ttu-id="7f00c-149">Atrybut `[ApiController]` można zastosować do określonych kontrolerów, jak w poniższym przykładzie z szablonu projektu:</span><span class="sxs-lookup"><span data-stu-id="7f00c-149">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="7f00c-150">Atrybut na wielu kontrolerach</span><span class="sxs-lookup"><span data-stu-id="7f00c-150">Attribute on multiple controllers</span></span>

<span data-ttu-id="7f00c-151">Jednym z metod używania atrybutu na więcej niż jednym kontrolerze jest utworzenie niestandardowej klasy kontrolera podstawowego z adnotacją z atrybutem `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-151">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="7f00c-152">Poniższy przykład przedstawia niestandardową klasę bazową i kontroler, który pochodzi od niego:</span><span class="sxs-lookup"><span data-stu-id="7f00c-152">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="7f00c-153">Atrybut w zestawie</span><span class="sxs-lookup"><span data-stu-id="7f00c-153">Attribute on an assembly</span></span>

<span data-ttu-id="7f00c-154">Jeśli [wersja zgodności](xref:mvc/compatibility-version) jest ustawiona na 2,2 lub nowsza, atrybut `[ApiController]` może zostać zastosowany do zestawu.</span><span class="sxs-lookup"><span data-stu-id="7f00c-154">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="7f00c-155">Adnotacja w ten sposób stosuje zachowanie internetowego interfejsu API do wszystkich kontrolerów w zestawie.</span><span class="sxs-lookup"><span data-stu-id="7f00c-155">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="7f00c-156">Nie ma możliwości rezygnacji z poszczególnych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7f00c-156">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="7f00c-157">Zastosuj atrybut poziomu zestawu do deklaracji przestrzeni nazw otaczającej klasę `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7f00c-157">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

::: moniker-end

## <a name="attribute-routing-requirement"></a><span data-ttu-id="7f00c-158">Wymagania dotyczące routingu atrybutów</span><span class="sxs-lookup"><span data-stu-id="7f00c-158">Attribute routing requirement</span></span>

<span data-ttu-id="7f00c-159">Atrybut `[ApiController]` powoduje, że atrybut routingu wymaga.</span><span class="sxs-lookup"><span data-stu-id="7f00c-159">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="7f00c-160">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7f00c-160">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="7f00c-161">Akcje są niedostępne za pośrednictwem [konwencjonalnych tras](xref:mvc/controllers/routing#conventional-routing) zdefiniowanych przez `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-161">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="7f00c-162">Akcje są niedostępne za pośrednictwem [konwencjonalnych tras](xref:mvc/controllers/routing#conventional-routing) zdefiniowanych przez <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-162">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="7f00c-163">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="7f00c-163">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="7f00c-164">`[ApiController]` atrybutu powoduje, że błędy walidacji modelu automatycznie wyzwalają odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="7f00c-164">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="7f00c-165">W związku z tym Poniższy kod jest zbędny w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="7f00c-165">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="7f00c-166">ASP.NET Core MVC używa filtru akcji <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter>, aby wykonać poprzednią kontrolę.</span><span class="sxs-lookup"><span data-stu-id="7f00c-166">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="7f00c-167">Domyślna odpowiedź nieprawidłowego żądania</span><span class="sxs-lookup"><span data-stu-id="7f00c-167">Default BadRequest response</span></span>

<span data-ttu-id="7f00c-168">W przypadku zgodności z wersją 2,1, domyślny typ odpowiedzi dla odpowiedzi HTTP 400 jest <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-168">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="7f00c-169">Następująca treść żądania jest przykładem serializowanego typu:</span><span class="sxs-lookup"><span data-stu-id="7f00c-169">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7f00c-170">W przypadku zgodności z wersją 2,2 lub nowszą domyślnym typem odpowiedzi dla odpowiedzi HTTP 400 jest <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-170">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="7f00c-171">Następująca treść żądania jest przykładem serializowanego typu:</span><span class="sxs-lookup"><span data-stu-id="7f00c-171">The following request body is an example of the serialized type:</span></span>

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

<span data-ttu-id="7f00c-172">Typ `ValidationProblemDetails`:</span><span class="sxs-lookup"><span data-stu-id="7f00c-172">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="7f00c-173">Zapewnia czytelny dla maszyn format służący do określania błędów w odpowiedziach interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7f00c-173">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="7f00c-174">Jest zgodna ze [specyfikacją RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="7f00c-174">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="7f00c-175">Rejestruj automatyczne odpowiedzi 400</span><span class="sxs-lookup"><span data-stu-id="7f00c-175">Log automatic 400 responses</span></span>

<span data-ttu-id="7f00c-176">Zobacz [, jak rejestrować 400 automatyczne odpowiedzi na błędy walidacji modelu (ASPNET/AspNetCore. Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="7f00c-176">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="7f00c-177">Wyłącz automatyczną odpowiedź 400</span><span class="sxs-lookup"><span data-stu-id="7f00c-177">Disable automatic 400 response</span></span>

<span data-ttu-id="7f00c-178">Aby wyłączyć zachowanie automatycznego 400, ustaw właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> na `true`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-178">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="7f00c-179">Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7f00c-179">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="7f00c-180">Wnioskowanie parametru źródła powiązania</span><span class="sxs-lookup"><span data-stu-id="7f00c-180">Binding source parameter inference</span></span>

<span data-ttu-id="7f00c-181">Atrybut źródłowy powiązania definiuje lokalizację, w której zostanie znaleziona wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="7f00c-181">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="7f00c-182">Istnieją następujące atrybuty źródła powiązania:</span><span class="sxs-lookup"><span data-stu-id="7f00c-182">The following binding source attributes exist:</span></span>

|<span data-ttu-id="7f00c-183">Atrybut</span><span class="sxs-lookup"><span data-stu-id="7f00c-183">Attribute</span></span>|<span data-ttu-id="7f00c-184">Źródło powiązania</span><span class="sxs-lookup"><span data-stu-id="7f00c-184">Binding source</span></span> |
|---------|---------|
|[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | <span data-ttu-id="7f00c-185">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="7f00c-185">Request body</span></span> |
|[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | <span data-ttu-id="7f00c-186">Formularz danych w treści żądania</span><span class="sxs-lookup"><span data-stu-id="7f00c-186">Form data in the request body</span></span> |
|[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | <span data-ttu-id="7f00c-187">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="7f00c-187">Request header</span></span> |
|[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | <span data-ttu-id="7f00c-188">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="7f00c-188">Request query string parameter</span></span> |
|[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | <span data-ttu-id="7f00c-189">Kierowanie danych z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="7f00c-189">Route data from the current request</span></span> |
|[`[FromServices]`](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | <span data-ttu-id="7f00c-190">Usługa żądania wstrzykiwana jako parametr akcji</span><span class="sxs-lookup"><span data-stu-id="7f00c-190">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="7f00c-191">Nie używaj `[FromRoute]`, gdy wartości mogą zawierać `%2f` (`/`).</span><span class="sxs-lookup"><span data-stu-id="7f00c-191">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="7f00c-192">`%2f` nie zostanie wystąpić do `/`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-192">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="7f00c-193">Użyj `[FromQuery]`, jeśli wartość może zawierać `%2f`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-193">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="7f00c-194">Bez atrybutów `[ApiController]` lub źródłowych powiązań, takich jak `[FromQuery]`, środowisko uruchomieniowe ASP.NET Core próbuje użyć spinacza modelu obiektów złożonych.</span><span class="sxs-lookup"><span data-stu-id="7f00c-194">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="7f00c-195">Segregator modelu obiektów złożonych pobiera dane od dostawców wartości w zdefiniowanej kolejności.</span><span class="sxs-lookup"><span data-stu-id="7f00c-195">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="7f00c-196">W poniższym przykładzie atrybut `[FromQuery]` wskazuje, że wartość parametru `discontinuedOnly` jest podana w ciągu zapytania w adresie URL żądania:</span><span class="sxs-lookup"><span data-stu-id="7f00c-196">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="7f00c-197">Atrybut `[ApiController]` stosuje reguły wnioskowania dla domyślnych źródeł danych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="7f00c-197">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="7f00c-198">Te reguły zapisują nie trzeba ręcznie identyfikować źródeł powiązań przez zastosowanie atrybutów do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="7f00c-198">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="7f00c-199">Reguły wnioskowania źródła powiązań zachowują się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="7f00c-199">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="7f00c-200">`[FromBody]` jest wnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="7f00c-200">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="7f00c-201">Wyjątek dla reguły wnioskowania `[FromBody]` jest dowolnym złożonym, wbudowanym typem ze specjalnym znaczeniem, takim jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-201">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="7f00c-202">Kod wnioskowania źródła powiązania ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="7f00c-202">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="7f00c-203">`[FromForm]` są wywnioskowane dla parametrów akcji typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-203">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="7f00c-204">Nie jest wywnioskowane dla żadnego prostego lub zdefiniowanego przez użytkownika typu.</span><span class="sxs-lookup"><span data-stu-id="7f00c-204">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="7f00c-205">`[FromRoute]` jest wywnioskowany dla każdej nazwy parametru akcji pasującej do parametru w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="7f00c-205">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="7f00c-206">Jeśli więcej niż jedna trasa pasuje do parametru akcji, dowolna wartość trasy jest uznawana za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-206">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="7f00c-207">`[FromQuery]` jest wywnioskowany dla wszystkich innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="7f00c-207">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="7f00c-208">FromBody informacje o wnioskach</span><span class="sxs-lookup"><span data-stu-id="7f00c-208">FromBody inference notes</span></span>

<span data-ttu-id="7f00c-209">`[FromBody]` nie są wywnioskowane dla typów prostych, takich jak `string` lub `int`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-209">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="7f00c-210">W związku z tym, atrybut `[FromBody]` powinien być używany dla typów prostych, gdy ta funkcja jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="7f00c-210">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="7f00c-211">Gdy akcja ma więcej niż jeden parametr powiązany z treścią żądania, zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="7f00c-211">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="7f00c-212">Na przykład, wszystkie następujące sygnatury metody akcji powodują wyjątek:</span><span class="sxs-lookup"><span data-stu-id="7f00c-212">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="7f00c-213">`[FromBody]` wywnioskowane na obu, ponieważ są to typy złożone.</span><span class="sxs-lookup"><span data-stu-id="7f00c-213">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="7f00c-214">atrybut `[FromBody]` na jednym, wywnioskowany na drugim, ponieważ jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="7f00c-214">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="7f00c-215">atrybut `[FromBody]` obu.</span><span class="sxs-lookup"><span data-stu-id="7f00c-215">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="7f00c-216">W ASP.NET Core 2,1 parametry typu kolekcji, takie jak listy i tablice, są nieprawidłowo wnioskowane jako `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-216">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="7f00c-217">Atrybut `[FromBody]` powinien być używany dla tych parametrów, jeśli mają być powiązane z treścią żądania.</span><span class="sxs-lookup"><span data-stu-id="7f00c-217">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="7f00c-218">To zachowanie jest korygowane w ASP.NET Core 2,2 lub nowszych, gdzie parametry typu kolekcji są wyrzucane jako powiązane z treścią domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7f00c-218">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="7f00c-219">Wyłącz reguły wnioskowania</span><span class="sxs-lookup"><span data-stu-id="7f00c-219">Disable inference rules</span></span>

<span data-ttu-id="7f00c-220">Aby wyłączyć wnioskowanie źródłowe powiązania, ustaw <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> na `true`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-220">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="7f00c-221">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7f00c-221">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="7f00c-222">Wieloczęściowe/formularz-wnioskowanie dotyczące danych</span><span class="sxs-lookup"><span data-stu-id="7f00c-222">Multipart/form-data request inference</span></span>

<span data-ttu-id="7f00c-223">Atrybut `[ApiController]` stosuje regułę wnioskowania, gdy parametr akcji ma adnotację z atrybutem [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) .</span><span class="sxs-lookup"><span data-stu-id="7f00c-223">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="7f00c-224">Typ zawartości żądania `multipart/form-data` jest wywnioskowany.</span><span class="sxs-lookup"><span data-stu-id="7f00c-224">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="7f00c-225">Aby wyłączyć domyślne zachowanie, ustaw właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> na `true` w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7f00c-225">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="7f00c-226">Szczegóły problemu dotyczące kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="7f00c-226">Problem details for error status codes</span></span>

<span data-ttu-id="7f00c-227">Gdy wersja zgodności to 2,2 lub nowsza, MVC przeprowadzi wynik błędu (wynik z kodem stanu 400 lub nowszym) do wyniku z <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="7f00c-227">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="7f00c-228">Typ `ProblemDetails` jest oparty na [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807) do udostępniania szczegółowych informacji o błędach w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="7f00c-228">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="7f00c-229">Rozważmy następujący kod w akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="7f00c-229">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="7f00c-230">Metoda `NotFound` generuje kod stanu HTTP 404 z treścią `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-230">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="7f00c-231">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7f00c-231">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="7f00c-232">Wyłącz odpowiedź ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="7f00c-232">Disable ProblemDetails response</span></span>

<span data-ttu-id="7f00c-233">Automatyczne tworzenie wystąpienia `ProblemDetails` jest wyłączone, gdy właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="7f00c-233">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> property is set to `true`.</span></span> <span data-ttu-id="7f00c-234">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7f00c-234">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7f00c-235">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7f00c-235">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
