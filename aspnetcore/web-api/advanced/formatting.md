---
title: Formatowanie danych odpowiedzi w ASP.NET Core Web API
author: ardalis
description: Dowiedz się, jak sformatować dane odpowiedzi w ASP.NET Core Web API.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 7b19bf54ea9f8a87c26ba7567a7348fcbea997c8
ms.sourcegitcommit: e7dc89620fa02c2ff80bee1e3f77297f97616968
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71151175"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="fdcac-103">Formatowanie danych odpowiedzi w ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="fdcac-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="fdcac-104">Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="fdcac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fdcac-105">ASP.NET Core MVC obsługuje formatowanie danych odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fdcac-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="fdcac-106">Dane odpowiedzi można sformatować przy użyciu określonych formatów lub w odpowiedzi na żądany format klienta.</span><span class="sxs-lookup"><span data-stu-id="fdcac-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="fdcac-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fdcac-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="fdcac-108">Wyniki akcji specyficzne dla formatu</span><span class="sxs-lookup"><span data-stu-id="fdcac-108">Format-specific Action Results</span></span>

<span data-ttu-id="fdcac-109">Niektóre typy wyników akcji są specyficzne dla określonego formatu, takiego jak <xref:Microsoft.AspNetCore.Mvc.JsonResult> i. <xref:Microsoft.AspNetCore.Mvc.ContentResult></span><span class="sxs-lookup"><span data-stu-id="fdcac-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="fdcac-110">Akcje mogą zwracać wyniki sformatowane w określonym formacie, niezależnie od preferencji klienta.</span><span class="sxs-lookup"><span data-stu-id="fdcac-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="fdcac-111">Na przykład zwracanie `JsonResult` zwraca dane w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="fdcac-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="fdcac-112">Zwracanie `ContentResult` lub ciąg zwraca dane ciągu w formacie zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="fdcac-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="fdcac-113">Akcja nie jest wymagana do zwrócenia żadnego określonego typu.</span><span class="sxs-lookup"><span data-stu-id="fdcac-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="fdcac-114">ASP.NET Core obsługuje dowolną wartość zwracaną przez obiekt.</span><span class="sxs-lookup"><span data-stu-id="fdcac-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="fdcac-115">Wyniki akcji, które zwracają obiekty, które nie <xref:Microsoft.AspNetCore.Mvc.IActionResult> są typami, są serializowane <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> przy użyciu odpowiedniej implementacji.</span><span class="sxs-lookup"><span data-stu-id="fdcac-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="fdcac-116">Aby uzyskać więcej informacji, zobacz <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="fdcac-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="fdcac-117">Wbudowana Metoda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> pomocnika zwraca dane sformatowane w formacie JSON:[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="fdcac-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="fdcac-118">Pobieranie próbek zwraca listę autorów.</span><span class="sxs-lookup"><span data-stu-id="fdcac-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="fdcac-119">Za pomocą narzędzi deweloperskich przeglądarki F12 lub [po](https://www.getpostman.com/tools) powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="fdcac-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="fdcac-120">Zostanie wyświetlony nagłówek odpowiedzi zawierający **Typ zawartości:** `application/json; charset=utf-8` .</span><span class="sxs-lookup"><span data-stu-id="fdcac-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="fdcac-121">Wyświetlane są nagłówki żądań.</span><span class="sxs-lookup"><span data-stu-id="fdcac-121">The request headers are displayed.</span></span> <span data-ttu-id="fdcac-122">Na przykład `Accept` nagłówek.</span><span class="sxs-lookup"><span data-stu-id="fdcac-122">For example, the `Accept` header.</span></span> <span data-ttu-id="fdcac-123">`Accept` Nagłówek jest ignorowany przez poprzedni kod.</span><span class="sxs-lookup"><span data-stu-id="fdcac-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="fdcac-124">Aby zwrócić dane w formacie zwykłego tekstu <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> , użyj <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> i pomocnika:</span><span class="sxs-lookup"><span data-stu-id="fdcac-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="fdcac-125">W powyższym kodzie `text/plain`zwraca `Content-Type` wartość.</span><span class="sxs-lookup"><span data-stu-id="fdcac-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="fdcac-126">Zwracanie ciągu `Content-Type` z `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="fdcac-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="fdcac-127">W przypadku akcji z wieloma zwracanymi typami `IActionResult`zwracamy.</span><span class="sxs-lookup"><span data-stu-id="fdcac-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="fdcac-128">Na przykład zwrócenie różnych kodów stanu HTTP w oparciu o wynik wykonanych operacji.</span><span class="sxs-lookup"><span data-stu-id="fdcac-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="fdcac-129">Negocjowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="fdcac-129">Content negotiation</span></span>

<span data-ttu-id="fdcac-130">Negocjowanie zawartości odbywa się, gdy klient określi [nagłówek Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="fdcac-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="fdcac-131">Domyślny format używany przez ASP.NET Core to [JSON](https://json.org/).</span><span class="sxs-lookup"><span data-stu-id="fdcac-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="fdcac-132">Negocjowanie zawartości:</span><span class="sxs-lookup"><span data-stu-id="fdcac-132">Content negotiation is:</span></span>

* <span data-ttu-id="fdcac-133">Zaimplementowane przez <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span><span class="sxs-lookup"><span data-stu-id="fdcac-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="fdcac-134">Wbudowane wyniki akcji specyficzne dla kodu stanu zwracane z metod pomocnika.</span><span class="sxs-lookup"><span data-stu-id="fdcac-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="fdcac-135">Metody pomocnika wyników akcji są oparte na `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="fdcac-136">Po zwróceniu typu modelu zwracanym typem jest `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="fdcac-137">W poniższej metodzie działania są `Ok` stosowane `NotFound` metody pomocnika i:</span><span class="sxs-lookup"><span data-stu-id="fdcac-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="fdcac-138">Odpowiedź w formacie JSON jest zwracana, chyba że zażądano innego formatu i serwer może zwrócić żądany format.</span><span class="sxs-lookup"><span data-stu-id="fdcac-138">A JSON-formatted response is returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="fdcac-139">Narzędzia takie jak [programu Fiddler](https://www.telerik.com/fiddler) lub [Poster](https://www.getpostman.com/tools) `Accept` mogą ustawić nagłówek, aby określić format powrotu.</span><span class="sxs-lookup"><span data-stu-id="fdcac-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` header to specify the return format.</span></span> <span data-ttu-id="fdcac-140">`Accept` Gdy zawiera typ, który obsługuje serwer, zwracany jest ten typ.</span><span class="sxs-lookup"><span data-stu-id="fdcac-140">When the `Accept` contains a type the server supports, that type is returned.</span></span>

<span data-ttu-id="fdcac-141">Domyślnie ASP.NET Core obsługuje tylko kod JSON.</span><span class="sxs-lookup"><span data-stu-id="fdcac-141">By default, ASP.NET Core only supports JSON.</span></span> <span data-ttu-id="fdcac-142">W przypadku aplikacji, które nie zmieniają wartości domyślnych, odpowiedzi w formacie JSON są zawsze zwracane niezależnie od żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="fdcac-142">For apps not changing the default, JSON-formatted responses are alway returned regardless of the client request.</span></span> <span data-ttu-id="fdcac-143">W następnej sekcji pokazano, jak dodać dodatkowe elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="fdcac-143">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="fdcac-144">Akcje kontrolera mogą zwracać POCOs (zwykłe stare obiekty CLR).</span><span class="sxs-lookup"><span data-stu-id="fdcac-144">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="fdcac-145">Gdy zostanie zwrócona wartość poco, środowisko uruchomieniowe automatycznie `ObjectResult` tworzy, które zawija obiekt.</span><span class="sxs-lookup"><span data-stu-id="fdcac-145">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="fdcac-146">Klient pobiera sformatowany obiekt Zserializowany.</span><span class="sxs-lookup"><span data-stu-id="fdcac-146">The client gets the formatted serialized object.</span></span> <span data-ttu-id="fdcac-147">Jeśli zwracany obiekt jest `null`zwracany `204 No Content` , zwracana jest odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="fdcac-147">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="fdcac-148">Zwracanie typu obiektu:</span><span class="sxs-lookup"><span data-stu-id="fdcac-148">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="fdcac-149">W poprzednim kodzie żądanie poprawnego aliasu autora zwraca `200 OK` odpowiedź z danymi autora.</span><span class="sxs-lookup"><span data-stu-id="fdcac-149">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="fdcac-150">Żądanie dotyczące nieprawidłowego aliasu zwraca `204 No Content` odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="fdcac-150">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="fdcac-151">Nagłówek Accept</span><span class="sxs-lookup"><span data-stu-id="fdcac-151">The Accept header</span></span>

<span data-ttu-id="fdcac-152">*Negocjowanie* zawartości odbywa się po `Accept` pojawieniu się nagłówka w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="fdcac-152">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="fdcac-153">Gdy żądanie zawiera nagłówek Accept, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fdcac-153">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="fdcac-154">Wylicza typy nośników w nagłówku Accept w kolejności preferencji.</span><span class="sxs-lookup"><span data-stu-id="fdcac-154">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="fdcac-155">Próbuje znaleźć program formatujący, który może wygenerować odpowiedź w jednym z określonych formatów.</span><span class="sxs-lookup"><span data-stu-id="fdcac-155">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="fdcac-156">Jeśli nie zostanie znaleziony żaden program formatujący, który może spełnić żądanie klienta, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fdcac-156">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="fdcac-157">Zwraca `406 Not Acceptable` wartość <xref:Microsoft.AspNetCore.Mvc.MvcOptions> , jeśli została ustawiona, lub-</span><span class="sxs-lookup"><span data-stu-id="fdcac-157">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="fdcac-158">Próbuje znaleźć pierwszy program formatujący, który może wygenerować odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="fdcac-158">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="fdcac-159">Jeśli nie skonfigurowano programu formatującego dla żądanego formatu, jest używany pierwszy program formatujący, który może formatować obiekt.</span><span class="sxs-lookup"><span data-stu-id="fdcac-159">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="fdcac-160">Jeśli w `Accept` żądaniu nie zostanie wyświetlony nagłówek:</span><span class="sxs-lookup"><span data-stu-id="fdcac-160">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="fdcac-161">Pierwszy program formatujący, który może obsłużyć obiekt, jest używany do serializacji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fdcac-161">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="fdcac-162">Nie ma żadnej negocjacji.</span><span class="sxs-lookup"><span data-stu-id="fdcac-162">There isn't any negotiation taking place.</span></span> <span data-ttu-id="fdcac-163">Serwer określa format do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="fdcac-163">The server is determining what format to return.</span></span>

<span data-ttu-id="fdcac-164">Jeśli nagłówek Accept zawiera `*/*`, nagłówek jest ignorowany, chyba że `RespectBrowserAcceptHeader` <xref:Microsoft.AspNetCore.Mvc.MvcOptions>jest ustawiona na wartość true (prawda).</span><span class="sxs-lookup"><span data-stu-id="fdcac-164">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="fdcac-165">Przeglądarki i negocjacje zawartości</span><span class="sxs-lookup"><span data-stu-id="fdcac-165">Browsers and content negotiation</span></span>

<span data-ttu-id="fdcac-166">W przeciwieństwie do typowych klientów interfejsu API, `Accept` przeglądarki sieci Web dostarczają nagłówki.</span><span class="sxs-lookup"><span data-stu-id="fdcac-166">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="fdcac-167">Przeglądarka sieci Web określa wiele formatów, w tym symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="fdcac-167">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="fdcac-168">Domyślnie, gdy struktura wykryje, że żądanie pochodzi z przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="fdcac-168">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="fdcac-169">`Accept` Nagłówek jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="fdcac-169">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="fdcac-170">Zawartość jest zwracana w formacie JSON, o ile nie została skonfigurowana inaczej.</span><span class="sxs-lookup"><span data-stu-id="fdcac-170">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="fdcac-171">Zapewnia to bardziej spójne środowisko w przeglądarkach podczas używania interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="fdcac-171">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="fdcac-172">Aby skonfigurować aplikację do honorowania nagłówków akceptowanych przez przeglądarkę <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> , `true`ustaw wartość na:</span><span class="sxs-lookup"><span data-stu-id="fdcac-172">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="fdcac-173">Konfigurowanie elementów formatujących</span><span class="sxs-lookup"><span data-stu-id="fdcac-173">Configure formatters</span></span>

<span data-ttu-id="fdcac-174">Aplikacje, które muszą obsługiwać dodatkowe formaty, mogą dodać odpowiednie pakiety NuGet i skonfigurować obsługę.</span><span class="sxs-lookup"><span data-stu-id="fdcac-174">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="fdcac-175">Istnieją osobne elementy formatujące dla danych wejściowych i wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="fdcac-175">There are separate formatters for input and output.</span></span> <span data-ttu-id="fdcac-176">Wejściowe elementy formatującego są używane przez [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="fdcac-176">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="fdcac-177">Wyjściowe elementy formatujące są używane do formatowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fdcac-177">Output formatters are used to format responses.</span></span> <span data-ttu-id="fdcac-178">Aby uzyskać informacje na temat tworzenia niestandardowego programu formatującego, zobacz [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="fdcac-178">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="fdcac-179">Dodawanie obsługi formatu XML</span><span class="sxs-lookup"><span data-stu-id="fdcac-179">Add XML format support</span></span>

<span data-ttu-id="fdcac-180">Elementy formatujące XML zaimplementowane <xref:System.Xml.Serialization.XmlSerializer> przy użyciu są konfigurowane <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>przez wywołanie:</span><span class="sxs-lookup"><span data-stu-id="fdcac-180">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="fdcac-181">Poprzedni kod serializacji wyników przy użyciu `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-181">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="fdcac-182">W przypadku korzystania z powyższego kodu metody kontrolera powinny zwrócić odpowiedni format na podstawie `Accept` nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="fdcac-182">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="fdcac-183">Skonfiguruj elementy formatujące system. Text. JSON w oparciu o</span><span class="sxs-lookup"><span data-stu-id="fdcac-183">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="fdcac-184">Funkcje dla `System.Text.Json`elementów formatujących opartych na programie można skonfigurować `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`przy użyciu polecenia.</span><span class="sxs-lookup"><span data-stu-id="fdcac-184">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="fdcac-185">Opcje serializacji danych wyjściowych dla poszczególnych akcji można skonfigurować przy użyciu polecenia `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-185">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="fdcac-186">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fdcac-186">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="fdcac-187">Dodawanie obsługi formatu JSON opartego na Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="fdcac-187">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="fdcac-188">Przed ASP.NET Core 3,0 stosowane są domyślne elementy formatujące JSON zaimplementowane przy użyciu `Newtonsoft.Json` pakietu.</span><span class="sxs-lookup"><span data-stu-id="fdcac-188">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="fdcac-189">W ASP.NET Core 3,0 lub nowszych domyślne elementy formatujące JSON są oparte na `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-189">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="fdcac-190">Obsługa opartych na programie formatującegos i funkcji jest dostępna przez zainstalowanie pakietu NuGet [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) i skonfigurowanie go `Startup.ConfigureServices`w programie. `Newtonsoft.Json`</span><span class="sxs-lookup"><span data-stu-id="fdcac-190">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="fdcac-191">Niektóre funkcje mogą nie współdziałać `System.Text.Json`z modułami formatującego opartymi na języku i wymagają `Newtonsoft.Json`odwołania do elementów formatujących opartych na bazie.</span><span class="sxs-lookup"><span data-stu-id="fdcac-191">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="fdcac-192">Kontynuuj korzystanie z `Newtonsoft.Json`programu formatującego w oparciu o aplikacje:</span><span class="sxs-lookup"><span data-stu-id="fdcac-192">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="fdcac-193">Używa `Newtonsoft.Json` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="fdcac-193">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="fdcac-194">Na przykład `[JsonProperty]` lub `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-194">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="fdcac-195">Dostosowuje ustawienia serializacji.</span><span class="sxs-lookup"><span data-stu-id="fdcac-195">Customizes the serialization settings.</span></span>
* <span data-ttu-id="fdcac-196">Opiera się `Newtonsoft.Json` na udostępnianych funkcjach.</span><span class="sxs-lookup"><span data-stu-id="fdcac-196">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="fdcac-197">Konfiguruje `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-197">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="fdcac-198">Przed ASP.NET Core 3,0, `JsonResult.SerializerSettings` akceptuje `JsonSerializerSettings` wystąpienie, które jest specyficzne dla `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-198">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="fdcac-199">Generuje dokumentację [openapi](<xref:tutorials/web-api-help-pages-using-swagger>) .</span><span class="sxs-lookup"><span data-stu-id="fdcac-199">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="fdcac-200">Funkcje dla `Newtonsoft.Json`elementów formatujących opartych na programie można skonfigurować `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="fdcac-200">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefautlContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="fdcac-201">Opcje serializacji danych wyjściowych dla poszczególnych akcji można skonfigurować przy użyciu polecenia `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-201">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="fdcac-202">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fdcac-202">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="fdcac-203">Dodawanie obsługi formatu XML</span><span class="sxs-lookup"><span data-stu-id="fdcac-203">Add XML format support</span></span>

<span data-ttu-id="fdcac-204">Formatowanie XML wymaga pakietu NuGet [Microsoft. AspNetCore. MVC. formatującegos. XML](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) .</span><span class="sxs-lookup"><span data-stu-id="fdcac-204">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="fdcac-205">Elementy formatujące XML zaimplementowane <xref:System.Xml.Serialization.XmlSerializer> przy użyciu są konfigurowane <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>przez wywołanie:</span><span class="sxs-lookup"><span data-stu-id="fdcac-205">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="fdcac-206">Poprzedni kod serializacji wyników przy użyciu `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-206">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="fdcac-207">W przypadku korzystania z powyższego kodu metody kontrolera powinny zwrócić odpowiedni format na podstawie `Accept` nagłówka żądania.</span><span class="sxs-lookup"><span data-stu-id="fdcac-207">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="fdcac-208">Określ format</span><span class="sxs-lookup"><span data-stu-id="fdcac-208">Specify a format</span></span>

<span data-ttu-id="fdcac-209">Aby ograniczyć formaty odpowiedzi, Zastosuj [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtr.</span><span class="sxs-lookup"><span data-stu-id="fdcac-209">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="fdcac-210">Podobnie jak [](xref:mvc/controllers/filters)większość filtrów `[Produces]` , można zastosować do akcji, kontrolera lub zakresu globalnego:</span><span class="sxs-lookup"><span data-stu-id="fdcac-210">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="fdcac-211">Poprzedni [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtr:</span><span class="sxs-lookup"><span data-stu-id="fdcac-211">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="fdcac-212">Wymusza, aby wszystkie akcje w ramach kontrolera zwracały odpowiedzi w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="fdcac-212">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="fdcac-213">Jeśli inne elementy formatujące są skonfigurowane, a klient określi inny format, zwracany jest kod JSON.</span><span class="sxs-lookup"><span data-stu-id="fdcac-213">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="fdcac-214">Aby uzyskać więcej informacji, zobacz [filtry](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="fdcac-214">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="fdcac-215">Specjalne elementy formatujące Case</span><span class="sxs-lookup"><span data-stu-id="fdcac-215">Special case formatters</span></span>

<span data-ttu-id="fdcac-216">Niektóre specjalne przypadki są implementowane przy użyciu wbudowanych elementów formatujących.</span><span class="sxs-lookup"><span data-stu-id="fdcac-216">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="fdcac-217">Domyślnie `string` typy zwracane są formatowane jako *tekst/zwykły* (*text/html* , jeśli żąda się `Accept` za pośrednictwem nagłówka).</span><span class="sxs-lookup"><span data-stu-id="fdcac-217">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="fdcac-218">Takie zachowanie można usunąć, usuwając <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="fdcac-218">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>.</span></span> <span data-ttu-id="fdcac-219">Elementy formatujące są usuwane w `Configure` metodzie.</span><span class="sxs-lookup"><span data-stu-id="fdcac-219">Formatters are removed in the `Configure` method.</span></span> <span data-ttu-id="fdcac-220">Akcje, które mają typ zwracany obiektu modelu zwracają `204 No Content` , `null`gdy zwracają.</span><span class="sxs-lookup"><span data-stu-id="fdcac-220">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="fdcac-221">Takie zachowanie można usunąć, usuwając <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="fdcac-221">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="fdcac-222">Poniższy kod usuwa `TextOutputFormatter` i `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-222">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="fdcac-223">Bez zwracanych typów`406 Not Acceptable`zwracanych. `TextOutputFormatter` `string`</span><span class="sxs-lookup"><span data-stu-id="fdcac-223">Without the `TextOutputFormatter`, `string` return types return `406 Not Acceptable`.</span></span> <span data-ttu-id="fdcac-224">Jeśli element formatujący XML istnieje, formatuje `string` typy zwracane, `TextOutputFormatter` jeśli został usunięty.</span><span class="sxs-lookup"><span data-stu-id="fdcac-224">If an XML formatter exists, it formats `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="fdcac-225">Bez obiektów `HttpNoContentOutputFormatter`o wartości null są formatowane przy użyciu skonfigurowanego programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="fdcac-225">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="fdcac-226">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fdcac-226">For example:</span></span>

* <span data-ttu-id="fdcac-227">Program formatujący JSON zwraca odpowiedź z treścią `null`.</span><span class="sxs-lookup"><span data-stu-id="fdcac-227">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="fdcac-228">Program formatujący XML zwraca pusty element XML z zestawem `xsi:nil="true"` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="fdcac-228">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="fdcac-229">Mapowania adresów URL w formacie odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="fdcac-229">Response format URL mappings</span></span>

<span data-ttu-id="fdcac-230">Klienci mogą zażądać określonego formatu w ramach adresu URL, na przykład:</span><span class="sxs-lookup"><span data-stu-id="fdcac-230">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="fdcac-231">W ciągu zapytania lub części ścieżki.</span><span class="sxs-lookup"><span data-stu-id="fdcac-231">In the query string or part of the path.</span></span>
* <span data-ttu-id="fdcac-232">Przy użyciu rozszerzenia pliku specyficznego dla formatu, takiego jak. XML lub. JSON.</span><span class="sxs-lookup"><span data-stu-id="fdcac-232">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="fdcac-233">Mapowanie ze ścieżki żądania należy określić w marszrucie używanej przez interfejs API.</span><span class="sxs-lookup"><span data-stu-id="fdcac-233">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="fdcac-234">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fdcac-234">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="fdcac-235">Poprzednia trasa pozwala określić żądany format jako opcjonalne rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="fdcac-235">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="fdcac-236">Atrybut sprawdza obecność wartości format `RouteData` w i mapuje format odpowiedzi do odpowiedniego programu formatującego podczas tworzenia odpowiedzi. [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute)</span><span class="sxs-lookup"><span data-stu-id="fdcac-236">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="fdcac-237">Szlak</span><span class="sxs-lookup"><span data-stu-id="fdcac-237">Route</span></span>            |             <span data-ttu-id="fdcac-238">EQ</span><span class="sxs-lookup"><span data-stu-id="fdcac-238">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="fdcac-239">Domyślny program formatujący dane wyjściowe</span><span class="sxs-lookup"><span data-stu-id="fdcac-239">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="fdcac-240">Program formatujący JSON (jeśli jest skonfigurowany)</span><span class="sxs-lookup"><span data-stu-id="fdcac-240">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="fdcac-241">Program formatujący XML (jeśli jest skonfigurowany)</span><span class="sxs-lookup"><span data-stu-id="fdcac-241">The XML formatter (if configured)</span></span>  |
