---
title: Tworzenie internetowych interfejsów API za pomocą ASP.NET Core
author: scottaddie
description: Poznaj podstawy tworzenia internetowego interfejsu API w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/22/2019
uid: web-api/index
ms.openlocfilehash: 3f52e4ce2d26902324ab30e0bda7ed8a4942daa0
ms.sourcegitcommit: ddc813f0f1fb293861a01597532919945b0e7fe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2019
ms.locfileid: "74412045"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="4237a-103">Tworzenie internetowych interfejsów API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4237a-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="4237a-104">Przez [Scott Addie](https://github.com/scottaddie) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4237a-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4237a-105">ASP.NET Core obsługuje tworzenie usług RESTful, znanych również jako interfejsy API sieci Web C#, przy użyciu programu.</span><span class="sxs-lookup"><span data-stu-id="4237a-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="4237a-106">Aby obsługiwać żądania, interfejs API sieci Web używa kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="4237a-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="4237a-107">*Kontrolery* w INTERNETowym interfejsie API są klasami pochodnymi od `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="4237a-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="4237a-108">W tym artykule pokazano, jak używać kontrolerów do obsługi żądań interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4237a-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="4237a-109">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="4237a-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="4237a-110">([Jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4237a-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="4237a-111">Klasa ControllerBase</span><span class="sxs-lookup"><span data-stu-id="4237a-111">ControllerBase class</span></span>

<span data-ttu-id="4237a-112">Internetowy interfejs API składa się z co najmniej jednej klasy kontrolera, która pochodzi od <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="4237a-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="4237a-113">Szablon projektu internetowego interfejsu API zawiera kontroler początkowy:</span><span class="sxs-lookup"><span data-stu-id="4237a-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="4237a-114">Nie twórz kontrolera interfejsu API sieci Web, pobierając z klasy <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="4237a-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="4237a-115">`Controller` pochodzi od `ControllerBase` i dodaje obsługę widoków, dlatego służy do obsługi stron sieci Web, a nie żądań interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4237a-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="4237a-116">Istnieje wyjątek od tej reguły: Jeśli planujesz używać tego samego kontrolera dla widoków i interfejsów API sieci Web, utwórz go z `Controller`.</span><span class="sxs-lookup"><span data-stu-id="4237a-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="4237a-117">Klasa `ControllerBase` dostarcza wiele właściwości i metod, które są przydatne do obsługi żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="4237a-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="4237a-118">Na przykład `ControllerBase.CreatedAtAction` zwraca kod stanu 201:</span><span class="sxs-lookup"><span data-stu-id="4237a-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="4237a-119">Poniżej przedstawiono kilka przykładów metod, które zapewnia `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="4237a-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="4237a-120">Metoda</span><span class="sxs-lookup"><span data-stu-id="4237a-120">Method</span></span>   |<span data-ttu-id="4237a-121">Uwagi</span><span class="sxs-lookup"><span data-stu-id="4237a-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest%2A>| <span data-ttu-id="4237a-122">Zwraca kod stanu 400.</span><span class="sxs-lookup"><span data-stu-id="4237a-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A>|<span data-ttu-id="4237a-123">Zwraca kod stanu 404.</span><span class="sxs-lookup"><span data-stu-id="4237a-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile%2A>|<span data-ttu-id="4237a-124">Zwraca plik.</span><span class="sxs-lookup"><span data-stu-id="4237a-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync%2A>|<span data-ttu-id="4237a-125">Wywołuje [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="4237a-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel%2A>|<span data-ttu-id="4237a-126">Wywołuje [walidację modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="4237a-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="4237a-127">Aby uzyskać listę wszystkich dostępnych metod i właściwości, zobacz <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="4237a-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="4237a-128">Atrybuty</span><span class="sxs-lookup"><span data-stu-id="4237a-128">Attributes</span></span>

<span data-ttu-id="4237a-129">Przestrzeń nazw <xref:Microsoft.AspNetCore.Mvc> zawiera atrybuty, których można użyć do skonfigurowania zachowania kontrolerów internetowego interfejsu API i metod akcji.</span><span class="sxs-lookup"><span data-stu-id="4237a-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="4237a-130">Poniższy przykład używa atrybutów, aby określić obsługiwane zlecenie akcji HTTP i wszystkie znane kody stanu HTTP, które mogą zostać zwrócone:</span><span class="sxs-lookup"><span data-stu-id="4237a-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="4237a-131">Poniżej przedstawiono kilka przykładów dostępnych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4237a-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="4237a-132">Atrybut</span><span class="sxs-lookup"><span data-stu-id="4237a-132">Attribute</span></span>|<span data-ttu-id="4237a-133">Uwagi</span><span class="sxs-lookup"><span data-stu-id="4237a-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="4237a-134">[Szlak](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="4237a-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="4237a-135">Określa wzorzec adresu URL dla kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="4237a-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="4237a-136">[Węglowodor](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="4237a-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="4237a-137">Określa prefiks i właściwości, które mają zostać dołączone do powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="4237a-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="4237a-138">[Narzędzia HttpGet](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="4237a-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="4237a-139">Identyfikuje akcję, która obsługuje czasownik HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="4237a-139">Identifies an action that supports the HTTP GET action verb.</span></span>|
|<span data-ttu-id="4237a-140">[Zużywa](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="4237a-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="4237a-141">Określa typy danych, które akcja akceptuje.</span><span class="sxs-lookup"><span data-stu-id="4237a-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="4237a-142">[Wyświetla](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="4237a-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="4237a-143">Określa typy danych, które zwraca akcja.</span><span class="sxs-lookup"><span data-stu-id="4237a-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="4237a-144">Aby zapoznać się z listą zawierającą dostępne atrybuty, zapoznaj się z przestrzenią nazw <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="4237a-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="4237a-145">ApiController — atrybut</span><span class="sxs-lookup"><span data-stu-id="4237a-145">ApiController attribute</span></span>

<span data-ttu-id="4237a-146">Atrybut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) można zastosować do klasy kontrolera, aby włączyć następujące zachowania specyficzne dla interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="4237a-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="4237a-147">Wymagania dotyczące routingu atrybutów</span><span class="sxs-lookup"><span data-stu-id="4237a-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="4237a-148">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="4237a-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="4237a-149">Wnioskowanie parametru źródła powiązania</span><span class="sxs-lookup"><span data-stu-id="4237a-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="4237a-150">Wieloczęściowe/formularz-wnioskowanie dotyczące danych</span><span class="sxs-lookup"><span data-stu-id="4237a-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="4237a-151">Szczegóły problemu dotyczące kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="4237a-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="4237a-152">Te funkcje wymagają [wersji](xref:mvc/compatibility-version) 2,1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="4237a-152">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="4237a-153">Atrybut na określonych kontrolerach</span><span class="sxs-lookup"><span data-stu-id="4237a-153">Attribute on specific controllers</span></span>

<span data-ttu-id="4237a-154">Atrybut `[ApiController]` można zastosować do określonych kontrolerów, jak w poniższym przykładzie z szablonu projektu:</span><span class="sxs-lookup"><span data-stu-id="4237a-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="4237a-155">Atrybut na wielu kontrolerach</span><span class="sxs-lookup"><span data-stu-id="4237a-155">Attribute on multiple controllers</span></span>

<span data-ttu-id="4237a-156">Jednym z metod używania atrybutu na więcej niż jednym kontrolerze jest utworzenie niestandardowej klasy kontrolera podstawowego z adnotacją z atrybutem `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="4237a-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="4237a-157">Poniższy przykład przedstawia niestandardową klasę bazową i kontroler, który pochodzi od niego:</span><span class="sxs-lookup"><span data-stu-id="4237a-157">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="4237a-158">Atrybut w zestawie</span><span class="sxs-lookup"><span data-stu-id="4237a-158">Attribute on an assembly</span></span>

<span data-ttu-id="4237a-159">Jeśli [wersja zgodności](xref:mvc/compatibility-version) jest ustawiona na 2,2 lub nowsza, atrybut `[ApiController]` może zostać zastosowany do zestawu.</span><span class="sxs-lookup"><span data-stu-id="4237a-159">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="4237a-160">Adnotacja w ten sposób stosuje zachowanie internetowego interfejsu API do wszystkich kontrolerów w zestawie.</span><span class="sxs-lookup"><span data-stu-id="4237a-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="4237a-161">Nie ma możliwości rezygnacji z poszczególnych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="4237a-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="4237a-162">Zastosuj atrybut poziomu zestawu do deklaracji przestrzeni nazw otaczającej klasę `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4237a-162">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="4237a-163">Wymagania dotyczące routingu atrybutów</span><span class="sxs-lookup"><span data-stu-id="4237a-163">Attribute routing requirement</span></span>

<span data-ttu-id="4237a-164">Atrybut `[ApiController]` powoduje, że atrybut routingu wymaga.</span><span class="sxs-lookup"><span data-stu-id="4237a-164">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="4237a-165">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4237a-165">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="4237a-166">Akcje są niedostępne za pośrednictwem [konwencjonalnych tras](xref:mvc/controllers/routing#conventional-routing) zdefiniowanych przez `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4237a-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="4237a-167">Akcje są niedostępne za pośrednictwem [konwencjonalnych tras](xref:mvc/controllers/routing#conventional-routing) zdefiniowanych przez <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> lub <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4237a-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc%2A> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute%2A> in `Startup.Configure`.</span></span>

::: moniker-end

## <a name="automatic-http-400-responses"></a><span data-ttu-id="4237a-168">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="4237a-168">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="4237a-169">`[ApiController]` atrybutu powoduje, że błędy walidacji modelu automatycznie wyzwalają odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="4237a-169">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="4237a-170">W związku z tym Poniższy kod jest zbędny w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="4237a-170">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="4237a-171">ASP.NET Core MVC używa filtru akcji <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter>, aby wykonać poprzednią kontrolę.</span><span class="sxs-lookup"><span data-stu-id="4237a-171">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="4237a-172">Domyślna odpowiedź nieprawidłowego żądania</span><span class="sxs-lookup"><span data-stu-id="4237a-172">Default BadRequest response</span></span>

<span data-ttu-id="4237a-173">W przypadku zgodności z wersją 2,1, domyślny typ odpowiedzi dla odpowiedzi HTTP 400 jest <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="4237a-173">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="4237a-174">Następująca treść żądania jest przykładem serializowanego typu:</span><span class="sxs-lookup"><span data-stu-id="4237a-174">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4237a-175">W przypadku zgodności z wersją 2,2 lub nowszą domyślnym typem odpowiedzi dla odpowiedzi HTTP 400 jest <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="4237a-175">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="4237a-176">Następująca treść żądania jest przykładem serializowanego typu:</span><span class="sxs-lookup"><span data-stu-id="4237a-176">The following request body is an example of the serialized type:</span></span>

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

<span data-ttu-id="4237a-177">Typ `ValidationProblemDetails`:</span><span class="sxs-lookup"><span data-stu-id="4237a-177">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="4237a-178">Zapewnia czytelny dla maszyn format służący do określania błędów w odpowiedziach interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4237a-178">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="4237a-179">Jest zgodna ze [specyfikacją RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="4237a-179">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="4237a-180">Rejestruj automatyczne odpowiedzi 400</span><span class="sxs-lookup"><span data-stu-id="4237a-180">Log automatic 400 responses</span></span>

<span data-ttu-id="4237a-181">Zobacz [, jak rejestrować 400 automatyczne odpowiedzi na błędy walidacji modelu (ASPNET/AspNetCore. Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="4237a-181">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="4237a-182">Wyłącz automatyczną odpowiedź 400</span><span class="sxs-lookup"><span data-stu-id="4237a-182">Disable automatic 400 response</span></span>

<span data-ttu-id="4237a-183">Aby wyłączyć zachowanie automatycznego 400, ustaw właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> na `true`.</span><span class="sxs-lookup"><span data-stu-id="4237a-183">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="4237a-184">Dodaj następujący wyróżniony kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4237a-184">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,5)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="4237a-185">Wnioskowanie parametru źródła powiązania</span><span class="sxs-lookup"><span data-stu-id="4237a-185">Binding source parameter inference</span></span>

<span data-ttu-id="4237a-186">Atrybut źródłowy powiązania definiuje lokalizację, w której zostanie znaleziona wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="4237a-186">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="4237a-187">Istnieją następujące atrybuty źródła powiązania:</span><span class="sxs-lookup"><span data-stu-id="4237a-187">The following binding source attributes exist:</span></span>

|<span data-ttu-id="4237a-188">Atrybut</span><span class="sxs-lookup"><span data-stu-id="4237a-188">Attribute</span></span>|<span data-ttu-id="4237a-189">Źródło powiązania</span><span class="sxs-lookup"><span data-stu-id="4237a-189">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="4237a-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="4237a-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="4237a-191">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="4237a-191">Request body</span></span> |
|<span data-ttu-id="4237a-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="4237a-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="4237a-193">Formularz danych w treści żądania</span><span class="sxs-lookup"><span data-stu-id="4237a-193">Form data in the request body</span></span> |
|<span data-ttu-id="4237a-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="4237a-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="4237a-195">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="4237a-195">Request header</span></span> |
|<span data-ttu-id="4237a-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="4237a-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="4237a-197">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="4237a-197">Request query string parameter</span></span> |
|<span data-ttu-id="4237a-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="4237a-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="4237a-199">Kierowanie danych z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="4237a-199">Route data from the current request</span></span> |
|<span data-ttu-id="4237a-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="4237a-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="4237a-201">Usługa żądania wstrzykiwana jako parametr akcji</span><span class="sxs-lookup"><span data-stu-id="4237a-201">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="4237a-202">Nie używaj `[FromRoute]`, gdy wartości mogą zawierać `%2f` (`/`).</span><span class="sxs-lookup"><span data-stu-id="4237a-202">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="4237a-203">`%2f` nie zostanie wystąpić do `/`.</span><span class="sxs-lookup"><span data-stu-id="4237a-203">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="4237a-204">Użyj `[FromQuery]`, jeśli wartość może zawierać `%2f`.</span><span class="sxs-lookup"><span data-stu-id="4237a-204">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="4237a-205">Bez atrybutów `[ApiController]` lub źródłowych powiązań, takich jak `[FromQuery]`, środowisko uruchomieniowe ASP.NET Core próbuje użyć spinacza modelu obiektów złożonych.</span><span class="sxs-lookup"><span data-stu-id="4237a-205">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="4237a-206">Segregator modelu obiektów złożonych pobiera dane od dostawców wartości w zdefiniowanej kolejności.</span><span class="sxs-lookup"><span data-stu-id="4237a-206">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="4237a-207">W poniższym przykładzie atrybut `[FromQuery]` wskazuje, że wartość parametru `discontinuedOnly` jest podana w ciągu zapytania w adresie URL żądania:</span><span class="sxs-lookup"><span data-stu-id="4237a-207">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="4237a-208">Atrybut `[ApiController]` stosuje reguły wnioskowania dla domyślnych źródeł danych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="4237a-208">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="4237a-209">Te reguły zapisują nie trzeba ręcznie identyfikować źródeł powiązań przez zastosowanie atrybutów do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="4237a-209">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="4237a-210">Reguły wnioskowania źródła powiązań zachowują się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4237a-210">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="4237a-211">`[FromBody]` jest wnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="4237a-211">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="4237a-212">Wyjątek dla reguły wnioskowania `[FromBody]` jest dowolnym złożonym, wbudowanym typem ze specjalnym znaczeniem, takim jak <xref:Microsoft.AspNetCore.Http.IFormCollection> i <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="4237a-212">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="4237a-213">Kod wnioskowania źródła powiązania ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="4237a-213">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="4237a-214">`[FromForm]` są wywnioskowane dla parametrów akcji typu <xref:Microsoft.AspNetCore.Http.IFormFile> i <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="4237a-214">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="4237a-215">Nie jest wywnioskowane dla żadnego prostego lub zdefiniowanego przez użytkownika typu.</span><span class="sxs-lookup"><span data-stu-id="4237a-215">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="4237a-216">`[FromRoute]` jest wywnioskowany dla każdej nazwy parametru akcji pasującej do parametru w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="4237a-216">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="4237a-217">Jeśli więcej niż jedna trasa pasuje do parametru akcji, dowolna wartość trasy jest uznawana za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="4237a-217">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="4237a-218">`[FromQuery]` jest wywnioskowany dla wszystkich innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="4237a-218">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="4237a-219">FromBody informacje o wnioskach</span><span class="sxs-lookup"><span data-stu-id="4237a-219">FromBody inference notes</span></span>

<span data-ttu-id="4237a-220">`[FromBody]` nie są wywnioskowane dla typów prostych, takich jak `string` lub `int`.</span><span class="sxs-lookup"><span data-stu-id="4237a-220">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="4237a-221">W związku z tym, atrybut `[FromBody]` powinien być używany dla typów prostych, gdy ta funkcja jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="4237a-221">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="4237a-222">Gdy akcja ma więcej niż jeden parametr powiązany z treścią żądania, zgłaszany jest wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4237a-222">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="4237a-223">Na przykład, wszystkie następujące sygnatury metody akcji powodują wyjątek:</span><span class="sxs-lookup"><span data-stu-id="4237a-223">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="4237a-224">`[FromBody]` wywnioskowane na obu, ponieważ są to typy złożone.</span><span class="sxs-lookup"><span data-stu-id="4237a-224">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="4237a-225">atrybut `[FromBody]` na jednym, wywnioskowany na drugim, ponieważ jest typem złożonym.</span><span class="sxs-lookup"><span data-stu-id="4237a-225">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="4237a-226">atrybut `[FromBody]` obu.</span><span class="sxs-lookup"><span data-stu-id="4237a-226">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="4237a-227">W ASP.NET Core 2,1 parametry typu kolekcji, takie jak listy i tablice, są nieprawidłowo wnioskowane jako `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="4237a-227">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="4237a-228">Atrybut `[FromBody]` powinien być używany dla tych parametrów, jeśli mają być powiązane z treścią żądania.</span><span class="sxs-lookup"><span data-stu-id="4237a-228">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="4237a-229">To zachowanie jest korygowane w ASP.NET Core 2,2 lub nowszych, gdzie parametry typu kolekcji są wyrzucane jako powiązane z treścią domyślnie.</span><span class="sxs-lookup"><span data-stu-id="4237a-229">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="4237a-230">Wyłącz reguły wnioskowania</span><span class="sxs-lookup"><span data-stu-id="4237a-230">Disable inference rules</span></span>

<span data-ttu-id="4237a-231">Aby wyłączyć wnioskowanie źródłowe powiązania, ustaw <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> na `true`.</span><span class="sxs-lookup"><span data-stu-id="4237a-231">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="4237a-232">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4237a-232">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](index/samples/2.x/2.1/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=1,4)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="4237a-233">Wieloczęściowe/formularz-wnioskowanie dotyczące danych</span><span class="sxs-lookup"><span data-stu-id="4237a-233">Multipart/form-data request inference</span></span>

<span data-ttu-id="4237a-234">Atrybut `[ApiController]` stosuje regułę wnioskowania, gdy parametr akcji ma adnotację z atrybutem [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) .</span><span class="sxs-lookup"><span data-stu-id="4237a-234">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="4237a-235">Typ zawartości żądania `multipart/form-data` jest wywnioskowany.</span><span class="sxs-lookup"><span data-stu-id="4237a-235">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="4237a-236">Aby wyłączyć domyślne zachowanie, ustaw właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> na `true` w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4237a-236">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

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

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="4237a-237">Szczegóły problemu dotyczące kodów stanu błędu</span><span class="sxs-lookup"><span data-stu-id="4237a-237">Problem details for error status codes</span></span>

<span data-ttu-id="4237a-238">Gdy wersja zgodności to 2,2 lub nowsza, MVC przeprowadzi wynik błędu (wynik z kodem stanu 400 lub nowszym) do wyniku z <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="4237a-238">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="4237a-239">Typ `ProblemDetails` jest oparty na [specyfikacji RFC 7807](https://tools.ietf.org/html/rfc7807) do udostępniania szczegółowych informacji o błędach w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="4237a-239">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="4237a-240">Rozważmy następujący kod w akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="4237a-240">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/2.2/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="4237a-241">Metoda `NotFound` generuje kod stanu HTTP 404 z treścią `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="4237a-241">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="4237a-242">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="4237a-242">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="4237a-243">Wyłącz odpowiedź ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="4237a-243">Disable ProblemDetails response</span></span>

<span data-ttu-id="4237a-244">Automatyczne tworzenie wystąpienia `ProblemDetails` jest wyłączone, gdy właściwość <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> jest ustawiona na `true`.</span><span class="sxs-lookup"><span data-stu-id="4237a-244">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors%2A> property is set to `true`.</span></span> <span data-ttu-id="4237a-245">Dodaj następujący kod w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4237a-245">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/2.2/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4237a-246">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4237a-246">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
