---
title: Filtry w ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak działają filtry i jak korzystać z nich w ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 1/1/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 2300b14a6a89191d3d8c673311880fc144183da9
ms.sourcegitcommit: e7d4fe6727d423f905faaeaa312f6c25ef844047
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2020
ms.locfileid: "75608131"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="0d995-103">Filtry w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d995-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0d995-104">[Kirka Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra/)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0d995-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0d995-105">*Filtry* w ASP.NET Core umożliwiają uruchamianie kodu przed określonymi etapami w potoku przetwarzania żądań lub po nich.</span><span class="sxs-lookup"><span data-stu-id="0d995-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="0d995-106">Filtry wbudowane obsługują zadania takie jak:</span><span class="sxs-lookup"><span data-stu-id="0d995-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="0d995-107">Autoryzacja (zapobieganie dostępowi do zasobów, dla których użytkownik nie jest autoryzowany).</span><span class="sxs-lookup"><span data-stu-id="0d995-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="0d995-108">Buforowanie odpowiedzi (krótkie obwody potoku żądania w celu zwrócenia buforowanej odpowiedzi).</span><span class="sxs-lookup"><span data-stu-id="0d995-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="0d995-109">Filtry niestandardowe mogą być tworzone w celu obsłużenia krzyżowego rozcinania.</span><span class="sxs-lookup"><span data-stu-id="0d995-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="0d995-110">Przykłady zagadnień związanych z rozcinaniem obejmują obsługę błędów, buforowanie, konfigurację, autoryzację i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="0d995-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="0d995-111">Filtry unikają duplikowania kodu.</span><span class="sxs-lookup"><span data-stu-id="0d995-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="0d995-112">Na przykład filtr wyjątków obsługujący błędy może skonsolidować obsługę błędów.</span><span class="sxs-lookup"><span data-stu-id="0d995-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="0d995-113">Ten dokument ma zastosowanie do Razor Pages, kontrolerów interfejsu API i kontrolerów z widokami.</span><span class="sxs-lookup"><span data-stu-id="0d995-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="0d995-114">[Wyświetl lub Pobierz przykład](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0d995-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="0d995-115">Jak działają filtry</span><span class="sxs-lookup"><span data-stu-id="0d995-115">How filters work</span></span>

<span data-ttu-id="0d995-116">Filtry są uruchamiane w *potoku wywołania akcji ASP.NET Core*, czasami określane jako *potok filtru*.</span><span class="sxs-lookup"><span data-stu-id="0d995-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="0d995-117">Potok filtru jest uruchamiany po ASP.NET Core wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="0d995-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, kierowanie oprogramowania pośredniczącego, wybór akcji i potok akcji wywołania.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="0d995-120">Typy filtrów</span><span class="sxs-lookup"><span data-stu-id="0d995-120">Filter types</span></span>

<span data-ttu-id="0d995-121">Każdy typ filtru jest wykonywany na innym etapie w potoku filtru:</span><span class="sxs-lookup"><span data-stu-id="0d995-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="0d995-122">[Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do określenia, czy użytkownik jest autoryzowany do żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="0d995-123">Filtry autoryzacji skracają obwód w przypadku, gdy żądanie nie jest autoryzowane.</span><span class="sxs-lookup"><span data-stu-id="0d995-123">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="0d995-124">[Filtry zasobów](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="0d995-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="0d995-125">Uruchom po autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-125">Run after authorization.</span></span>  
  * <span data-ttu-id="0d995-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> uruchamia kod przed resztą potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="0d995-127">Na przykład `OnResourceExecuting` uruchamia kod przed powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="0d995-127">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="0d995-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> uruchamia kod po zakończeniu reszty potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="0d995-129">[Filtry akcji](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="0d995-129">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="0d995-130">Uruchom kod bezpośrednio przed i po wywołaniu metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-130">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="0d995-131">Może zmienić argumenty przekazane do akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-131">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="0d995-132">Można zmienić wynik zwrócony z akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-132">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="0d995-133">**Nie** są obsługiwane w Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0d995-133">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="0d995-134">[Filtry wyjątków](#exception-filters) stosują zasady globalne do nieobsłużonych wyjątków, które występują przed zapisaniem treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0d995-134">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="0d995-135">[Filtry wyników](#result-filters) uruchamiają kod bezpośrednio przed i po wykonaniu wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-135">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="0d995-136">Są one uruchamiane tylko wtedy, gdy metoda akcji została wykonana pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="0d995-136">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="0d995-137">Są one przydatne dla logiki, która musi być w trakcie wyświetlania lub wykonywania w programie formatującego.</span><span class="sxs-lookup"><span data-stu-id="0d995-137">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="0d995-138">Na poniższym diagramie przedstawiono sposób, w jaki typy filtrów współdziałają w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-138">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Żądanie jest przetwarzane przez filtry autoryzacji, filtry zasobów, powiązania modelu, filtry akcji, wykonywanie akcji i konwersję wyników akcji, filtry wyjątków, filtry wynikowe i wykonywanie wyniku.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="0d995-141">Implementacja</span><span class="sxs-lookup"><span data-stu-id="0d995-141">Implementation</span></span>

<span data-ttu-id="0d995-142">Filtry obsługują implementacje synchroniczne i asynchroniczne za pomocą różnych definicji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="0d995-142">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="0d995-143">Filtry synchroniczne uruchamiają kod przed i po fazie potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-143">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="0d995-144">Na przykład, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> jest wywoływana przed wywołaniem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-144">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="0d995-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> jest wywoływana po powrocie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="0d995-146">Filtry asynchroniczne definiują metodę `On-Stage-ExecutionAsync`.</span><span class="sxs-lookup"><span data-stu-id="0d995-146">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="0d995-147">Na przykład <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="0d995-147">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="0d995-148">W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który wykonuje metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-148">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="0d995-149">Wiele etapów filtrowania</span><span class="sxs-lookup"><span data-stu-id="0d995-149">Multiple filter stages</span></span>

<span data-ttu-id="0d995-150">Interfejsy dla wielu etapów filtru można zaimplementować w pojedynczej klasie.</span><span class="sxs-lookup"><span data-stu-id="0d995-150">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="0d995-151">Na przykład Klasa <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementuje:</span><span class="sxs-lookup"><span data-stu-id="0d995-151">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="0d995-152">Synchroniczne: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="0d995-152">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="0d995-153">Asynchronicznie: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="0d995-153">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="0d995-154">Implementowanie synchronicznej lub asynchronicznej wersji interfejsu filtru **, a** **nie** obu.</span><span class="sxs-lookup"><span data-stu-id="0d995-154">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="0d995-155">Środowisko uruchomieniowe najpierw sprawdza, czy filtr implementuje interfejs asynchroniczny, a jeśli tak, wywołuje to.</span><span class="sxs-lookup"><span data-stu-id="0d995-155">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="0d995-156">Jeśli nie, wywołuje metody interfejsu synchronicznego.</span><span class="sxs-lookup"><span data-stu-id="0d995-156">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="0d995-157">Jeśli zarówno interfejsy asynchroniczne, jak i synchroniczne są zaimplementowane w jednej klasie, wywoływana jest tylko Metoda async.</span><span class="sxs-lookup"><span data-stu-id="0d995-157">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="0d995-158">W przypadku używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, Zastąp tylko metody synchroniczne lub metodę asynchroniczną dla każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-158">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="0d995-159">Wbudowane atrybuty filtru</span><span class="sxs-lookup"><span data-stu-id="0d995-159">Built-in filter attributes</span></span>

<span data-ttu-id="0d995-160">ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które można podklasować i dostosowywać.</span><span class="sxs-lookup"><span data-stu-id="0d995-160">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="0d995-161">Na przykład poniższy filtr wynikowy dodaje nagłówek do odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="0d995-161">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="0d995-162">Atrybuty umożliwiają filtrom akceptowanie argumentów, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="0d995-162">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="0d995-163">Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:</span><span class="sxs-lookup"><span data-stu-id="0d995-163">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="0d995-164">Użyj narzędzia, takiego jak [Narzędzia deweloperskie przeglądarki](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) , aby przeanalizować nagłówki.</span><span class="sxs-lookup"><span data-stu-id="0d995-164">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="0d995-165">W obszarze **nagłówki odpowiedzi**zostanie wyświetlona `author: Rick Anderson`.</span><span class="sxs-lookup"><span data-stu-id="0d995-165">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="0d995-166">Poniższy kod implementuje `ActionFilterAttribute`, który:</span><span class="sxs-lookup"><span data-stu-id="0d995-166">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="0d995-167">Odczytuje tytuł i nazwę z systemu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="0d995-167">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="0d995-168">W przeciwieństwie do poprzedniego przykładu, poniższy kod nie wymaga dodania parametrów filtru do kodu.</span><span class="sxs-lookup"><span data-stu-id="0d995-168">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="0d995-169">Dodaje tytuł i nazwę do nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0d995-169">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="0d995-170">Opcje konfiguracji są dostępne z [systemu konfiguracji](xref:fundamentals/configuration/index) przy użyciu [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="0d995-170">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="0d995-171">Na przykład, z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="0d995-171">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="0d995-172">W `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0d995-172">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="0d995-173">Klasa `PositionOptions` jest dodawana do kontenera usługi z obszarem konfiguracji `"Position"`.</span><span class="sxs-lookup"><span data-stu-id="0d995-173">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="0d995-174">`MyActionFilterAttribute` zostanie dodany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="0d995-174">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="0d995-175">Poniższy kod przedstawia klasę `PositionOptions`:</span><span class="sxs-lookup"><span data-stu-id="0d995-175">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="0d995-176">Poniższy kod stosuje `MyActionFilterAttribute` do metody `Index2`:</span><span class="sxs-lookup"><span data-stu-id="0d995-176">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="0d995-177">W obszarze **nagłówki odpowiedzi**, `author: Rick Anderson`i `Editor: Joe Smith` jest wyświetlana po wywołaniu punktu końcowego `Sample/Index2`.</span><span class="sxs-lookup"><span data-stu-id="0d995-177">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="0d995-178">Poniższy kod stosuje `MyActionFilterAttribute` i `AddHeaderAttribute` do strony Razor:</span><span class="sxs-lookup"><span data-stu-id="0d995-178">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="0d995-179">Nie można zastosować filtrów do metod obsługi stron Razor.</span><span class="sxs-lookup"><span data-stu-id="0d995-179">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="0d995-180">Mogą być stosowane do modelu strony Razor lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="0d995-180">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="0d995-181">Kilka interfejsów filtrów ma odpowiednie atrybuty, które mogą być używane jako klasy bazowe dla implementacji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="0d995-181">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="0d995-182">Atrybuty filtru:</span><span class="sxs-lookup"><span data-stu-id="0d995-182">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="0d995-183">Zakresy filtrów i kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="0d995-183">Filter scopes and order of execution</span></span>

<span data-ttu-id="0d995-184">Filtr można dodać do potoku w jednym z trzech *zakresów*:</span><span class="sxs-lookup"><span data-stu-id="0d995-184">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="0d995-185">Użycie atrybutu w akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d995-185">Using an attribute on a controller action.</span></span> <span data-ttu-id="0d995-186">Nie można zastosować atrybutów filtru do metod obsługi Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0d995-186">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="0d995-187">Użycie atrybutu na kontrolerze lub stronie Razor.</span><span class="sxs-lookup"><span data-stu-id="0d995-187">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="0d995-188">Globalnie dla wszystkich kontrolerów, akcji i Razor Pages, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="0d995-188">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="0d995-189">Domyślna kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="0d995-189">Default order of execution</span></span>

<span data-ttu-id="0d995-190">Jeśli istnieje wiele filtrów dla określonego etapu potoku, zakres określa domyślną kolejność wykonywania filtrowania.</span><span class="sxs-lookup"><span data-stu-id="0d995-190">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="0d995-191">Filtry globalne Otocz filtry klas, które z kolei filtrują metody przestrzenne.</span><span class="sxs-lookup"><span data-stu-id="0d995-191">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="0d995-192">W wyniku zagnieżdżania filtrów, *po* kodzie filtrów działa w odwrotnej kolejności *przed* kodem.</span><span class="sxs-lookup"><span data-stu-id="0d995-192">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="0d995-193">Sekwencja filtru:</span><span class="sxs-lookup"><span data-stu-id="0d995-193">The filter sequence:</span></span>

* <span data-ttu-id="0d995-194">*Przed* kodem filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="0d995-194">The *before* code of global filters.</span></span>
  * <span data-ttu-id="0d995-195">*Przed* kodem i filtrem strony Razor.</span><span class="sxs-lookup"><span data-stu-id="0d995-195">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="0d995-196">Filtr *przed* kodem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-196">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="0d995-197">*Po* kodzie metody akcji filtry.</span><span class="sxs-lookup"><span data-stu-id="0d995-197">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="0d995-198">Kod *po* stronie kontroler i filtr strony Razor.</span><span class="sxs-lookup"><span data-stu-id="0d995-198">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="0d995-199">Kod *po* filtrach globalnych.</span><span class="sxs-lookup"><span data-stu-id="0d995-199">The *after* code of global filters.</span></span>
  
<span data-ttu-id="0d995-200">Poniższy przykład ilustruje kolejność, w której metody filtrowania są wywoływane dla synchronicznych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-200">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="0d995-201">Sequence</span><span class="sxs-lookup"><span data-stu-id="0d995-201">Sequence</span></span> | <span data-ttu-id="0d995-202">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="0d995-202">Filter scope</span></span> | <span data-ttu-id="0d995-203">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-203">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="0d995-204">1</span><span class="sxs-lookup"><span data-stu-id="0d995-204">1</span></span> | <span data-ttu-id="0d995-205">Globalne</span><span class="sxs-lookup"><span data-stu-id="0d995-205">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="0d995-206">2</span><span class="sxs-lookup"><span data-stu-id="0d995-206">2</span></span> | <span data-ttu-id="0d995-207">Kontroler lub strona Razor</span><span class="sxs-lookup"><span data-stu-id="0d995-207">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="0d995-208">3</span><span class="sxs-lookup"><span data-stu-id="0d995-208">3</span></span> | <span data-ttu-id="0d995-209">Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-209">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="0d995-210">4</span><span class="sxs-lookup"><span data-stu-id="0d995-210">4</span></span> | <span data-ttu-id="0d995-211">Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-211">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="0d995-212">5</span><span class="sxs-lookup"><span data-stu-id="0d995-212">5</span></span> | <span data-ttu-id="0d995-213">Kontroler lub strona Razor</span><span class="sxs-lookup"><span data-stu-id="0d995-213">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="0d995-214">6</span><span class="sxs-lookup"><span data-stu-id="0d995-214">6</span></span> | <span data-ttu-id="0d995-215">Globalne</span><span class="sxs-lookup"><span data-stu-id="0d995-215">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="0d995-216">Filtry na poziomie kontrolera</span><span class="sxs-lookup"><span data-stu-id="0d995-216">Controller level filters</span></span>

<span data-ttu-id="0d995-217">Każdy kontroler, który dziedziczy z klasy bazowej <xref:Microsoft.AspNetCore.Mvc.Controller> obejmuje metody [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)i [Controller. OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="0d995-217">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="0d995-218">Te metody:</span><span class="sxs-lookup"><span data-stu-id="0d995-218">These methods:</span></span>

* <span data-ttu-id="0d995-219">Zawiń filtry, które są uruchamiane dla danego działania.</span><span class="sxs-lookup"><span data-stu-id="0d995-219">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="0d995-220">`OnActionExecuting` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-220">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="0d995-221">`OnActionExecuted` jest wywoływana po wszystkich filtrach akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-221">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="0d995-222">`OnActionExecutionAsync` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-222">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="0d995-223">Kod w filtrze po `next` jest uruchamiany po metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-223">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="0d995-224">Na przykład w przykładzie pobierania `MySampleActionFilter` jest stosowany globalnie podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0d995-224">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="0d995-225">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="0d995-225">The `TestController`:</span></span>

* <span data-ttu-id="0d995-226">Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do akcji `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="0d995-226">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="0d995-227">Zastępuje `OnActionExecuting` i `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="0d995-227">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="0d995-228">Przechodzenie do `https://localhost:5001/Test2/FilterTest2` uruchamia następujący kod:</span><span class="sxs-lookup"><span data-stu-id="0d995-228">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="0d995-229">Filtry na poziomie kontrolera ustawiają Właściwość [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) na `int.MinValue`.</span><span class="sxs-lookup"><span data-stu-id="0d995-229">Controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="0d995-230">Filtrów na poziomie kontrolera **nie** można ustawić do uruchomienia po zastosowaniu filtrów do metod.</span><span class="sxs-lookup"><span data-stu-id="0d995-230">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="0d995-231">Klauzula Order została omówiona w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-231">Order is explained in the next section.</span></span>

<span data-ttu-id="0d995-232">Aby uzyskać Razor Pages, zobacz [implementowanie filtrów stron Razor przez zastępowanie metod filtrowania](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="0d995-232">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="0d995-233">Zastępowanie kolejności domyślnej</span><span class="sxs-lookup"><span data-stu-id="0d995-233">Overriding the default order</span></span>

<span data-ttu-id="0d995-234">Domyślną sekwencję wykonywania można zastąpić, implementując <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-234">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="0d995-235">`IOrderedFilter` uwidacznia Właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, która ma pierwszeństwo przed zakresem w celu określenia kolejności wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0d995-235">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="0d995-236">Filtr z niższą `Order` wartością:</span><span class="sxs-lookup"><span data-stu-id="0d995-236">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="0d995-237">Uruchamia *przed* kodem przed filtrem o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="0d995-237">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="0d995-238">Uruchamia *po* kodzie po filtrze o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="0d995-238">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="0d995-239">Właściwość `Order` jest ustawiana za pomocą parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="0d995-239">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="0d995-240">Należy wziąć pod uwagę dwa filtry akcji na następującym kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="0d995-240">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="0d995-241">W `StartUp.ConfigureServices`zostanie dodany filtr globalny:</span><span class="sxs-lookup"><span data-stu-id="0d995-241">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="0d995-242">3 filtry działają w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="0d995-242">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="0d995-243">Właściwość `Order` przesłania zakres podczas określania kolejności, w której są uruchamiane filtry.</span><span class="sxs-lookup"><span data-stu-id="0d995-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="0d995-244">Filtry są sortowane najpierw według kolejności, a następnie zakres jest używany do przerwania powiązań.</span><span class="sxs-lookup"><span data-stu-id="0d995-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="0d995-245">Wszystkie wbudowane filtry implementują `IOrderedFilter` i ustawiają domyślną wartość `Order` równą 0.</span><span class="sxs-lookup"><span data-stu-id="0d995-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="0d995-246">Jak wspomniano wcześniej, filtry na poziomie kontrolera ustawiają Właściwość [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) na `int.MinValue` dla filtrów wbudowanych, zakres określa kolejność, chyba że `Order` jest ustawiona na wartość różną od zera.</span><span class="sxs-lookup"><span data-stu-id="0d995-246">As mentioned previously, controller level filters set the [Order](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="0d995-247">W poprzednim kodzie `MySampleActionFilter` ma zakres globalny, więc jest uruchamiany przed `MyAction2FilterAttribute`, który ma zakres kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d995-247">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="0d995-248">Aby najpierw wykonać `MyAction2FilterAttribute`, ustaw kolejność na `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="0d995-248">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="0d995-249">Aby najpierw uruchomić filtr globalny `MySampleActionFilter`, ustaw `Order` na `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="0d995-249">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="0d995-250">Anulowanie i krótkie obwody</span><span class="sxs-lookup"><span data-stu-id="0d995-250">Cancellation and short-circuiting</span></span>

<span data-ttu-id="0d995-251">Potok filtru może być skrócony przez ustawienie właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> na parametrze <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> dostarczonym do metody Filter.</span><span class="sxs-lookup"><span data-stu-id="0d995-251">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="0d995-252">Na przykład poniższy filtr zasobów uniemożliwia wykonanie pozostałej części potoku:</span><span class="sxs-lookup"><span data-stu-id="0d995-252">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="0d995-253">W poniższym kodzie, zarówno `ShortCircuitingResourceFilter`, jak i filtr `AddHeader`, dla metody akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="0d995-253">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="0d995-254">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="0d995-254">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="0d995-255">Uruchamia się najpierw, ponieważ jest to filtr zasobów, a `AddHeader` jest filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-255">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="0d995-256">Krótkie obwody pozostała część potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-256">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="0d995-257">W związku z tym filtr `AddHeader` nigdy nie jest uruchamiany dla akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="0d995-257">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="0d995-258">Takie zachowanie będzie takie samo, jeśli oba filtry zostały zastosowane na poziomie metody akcji, pod warunkiem że `ShortCircuitingResourceFilter` uruchamiane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="0d995-258">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="0d995-259">`ShortCircuitingResourceFilter` jest uruchamiany jako pierwszy ze względu na jego typ filtru lub jawne użycie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="0d995-259">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="0d995-260">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="0d995-260">Dependency injection</span></span>

<span data-ttu-id="0d995-261">Filtry można dodawać według typu lub według wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0d995-261">Filters can be added by type or by instance.</span></span> <span data-ttu-id="0d995-262">W przypadku dodania wystąpienia to wystąpienie jest używane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-262">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="0d995-263">Jeśli typ zostanie dodany, jego typ jest aktywowany.</span><span class="sxs-lookup"><span data-stu-id="0d995-263">If a type is added, it's type-activated.</span></span> <span data-ttu-id="0d995-264">Filtr aktywowany przez typ oznacza:</span><span class="sxs-lookup"><span data-stu-id="0d995-264">A type-activated filter means:</span></span>

* <span data-ttu-id="0d995-265">Wystąpienie jest tworzone dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-265">An instance is created for each request.</span></span>
* <span data-ttu-id="0d995-266">Wszystkie zależności konstruktora są wypełniane przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="0d995-266">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="0d995-267">Filtry zaimplementowane jako atrybuty i dodawane bezpośrednio do klas kontrolera lub metod akcji nie mogą mieć zależności konstruktorów udostępnianych przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="0d995-267">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="0d995-268">Zależności konstruktora nie mogą być dostarczone przez DI, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="0d995-268">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="0d995-269">Atrybuty muszą mieć podane parametry konstruktora, w których są stosowane.</span><span class="sxs-lookup"><span data-stu-id="0d995-269">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="0d995-270">Jest to ograniczenie działania atrybutów.</span><span class="sxs-lookup"><span data-stu-id="0d995-270">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="0d995-271">Następujące filtry obsługują zależności konstruktora dostarczone z elementów DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-271">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="0d995-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowany dla atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0d995-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="0d995-273">Powyższe filtry można zastosować do kontrolera lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-273">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="0d995-274">Rejestratory są dostępne z programu DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-274">Loggers are available from DI.</span></span> <span data-ttu-id="0d995-275">Należy jednak unikać tworzenia i używania filtrów wyłącznie w celach rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0d995-275">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="0d995-276">[Wbudowane rejestrowanie struktury](xref:fundamentals/logging/index) zazwyczaj zapewnia, co jest potrzebne do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0d995-276">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="0d995-277">Rejestrowanie dodane do filtrów:</span><span class="sxs-lookup"><span data-stu-id="0d995-277">Logging added to filters:</span></span>

* <span data-ttu-id="0d995-278">Należy skoncentrować się na problemach z domeną biznesową lub działaniu specyficznym dla filtra.</span><span class="sxs-lookup"><span data-stu-id="0d995-278">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="0d995-279">**Nie** należy rejestrować akcji ani innych zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="0d995-279">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="0d995-280">Wbudowane filtry rejestrują akcje i zdarzenia struktury.</span><span class="sxs-lookup"><span data-stu-id="0d995-280">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="0d995-281">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="0d995-281">ServiceFilterAttribute</span></span>

<span data-ttu-id="0d995-282">Typy implementacji filtru usługi są zarejestrowane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d995-282">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="0d995-283"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> Pobiera wystąpienie filtru z DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-283">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="0d995-284">Poniższy kod ilustruje `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="0d995-284">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="0d995-285">W poniższym kodzie, `AddHeaderResultServiceFilter` jest dodawane do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-285">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="0d995-286">W poniższym kodzie atrybut `ServiceFilter` Pobiera wystąpienie filtru `AddHeaderResultServiceFilter` z DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-286">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="0d995-287">Podczas używania `ServiceFilterAttribute`, należy ustawić [Servicefilterattribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="0d995-287">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="0d995-288">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="0d995-288">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="0d995-289">Środowisko uruchomieniowe ASP.NET Core nie gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="0d995-289">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="0d995-290">Zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-290">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="0d995-291">Filtr nie zostanie ponownie żądany z kontenera DI w pewnym momencie.</span><span class="sxs-lookup"><span data-stu-id="0d995-291">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="0d995-292">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="0d995-292">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="0d995-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="0d995-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="0d995-294">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="0d995-294">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="0d995-295">`CreateInstance` ładuje określony typ z DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-295">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="0d995-296">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="0d995-296">TypeFilterAttribute</span></span>

<span data-ttu-id="0d995-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> przypomina <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie jest rozpoznawany bezpośrednio w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="0d995-298">Tworzy wystąpienie tego typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="0d995-298">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="0d995-299">Ponieważ typy `TypeFilterAttribute` nie są rozpoznawane bezpośrednio w kontenerze DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-299">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="0d995-300">Typy, do których odwołuje się `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="0d995-300">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="0d995-301">Ich zależności są spełnione przez kontener DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-301">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="0d995-302">`TypeFilterAttribute` może opcjonalnie zaakceptować argumenty konstruktora dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="0d995-302">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="0d995-303">W przypadku używania `TypeFilterAttribute`ustawienie [TypeFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="0d995-303">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="0d995-304">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="0d995-304">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="0d995-305">Środowisko uruchomieniowe ASP.NET Core nie zapewnia żadnych gwarancji, że zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-305">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="0d995-306">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="0d995-306">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="0d995-307">Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="0d995-307">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="0d995-308">Filtry autoryzacji</span><span class="sxs-lookup"><span data-stu-id="0d995-308">Authorization filters</span></span>

<span data-ttu-id="0d995-309">Filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-309">Authorization filters:</span></span>

* <span data-ttu-id="0d995-310">Są pierwszymi filtrami uruchamianymi w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-310">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="0d995-311">Kontrola dostępu do metod akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-311">Control access to action methods.</span></span>
* <span data-ttu-id="0d995-312">Ma metodę Before, ale nie po metodzie.</span><span class="sxs-lookup"><span data-stu-id="0d995-312">Have a before method, but no after method.</span></span>

<span data-ttu-id="0d995-313">Niestandardowe filtry autoryzacji wymagają niestandardowej struktury autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-313">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="0d995-314">Preferuj skonfigurowanie zasad autoryzacji lub zapisanie niestandardowych zasad autoryzacji przez zapisanie filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="0d995-314">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="0d995-315">Wbudowany filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-315">The built-in authorization filter:</span></span>

* <span data-ttu-id="0d995-316">Wywołuje system autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-316">Calls the authorization system.</span></span>
* <span data-ttu-id="0d995-317">Nie zezwala na żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-317">Does not authorize requests.</span></span>

<span data-ttu-id="0d995-318">**Nie** zgłaszaj wyjątków w ramach filtrów autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-318">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="0d995-319">Wyjątek nie zostanie obsłużony.</span><span class="sxs-lookup"><span data-stu-id="0d995-319">The exception will not be handled.</span></span>
* <span data-ttu-id="0d995-320">Filtry wyjątków nie będą obsługiwać wyjątku.</span><span class="sxs-lookup"><span data-stu-id="0d995-320">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="0d995-321">Rozważ wygenerowanie wyzwania w przypadku wystąpienia wyjątku w filtrze autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-321">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="0d995-322">Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="0d995-322">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="0d995-323">Filtry zasobów</span><span class="sxs-lookup"><span data-stu-id="0d995-323">Resource filters</span></span>

<span data-ttu-id="0d995-324">Filtry zasobów:</span><span class="sxs-lookup"><span data-stu-id="0d995-324">Resource filters:</span></span>

* <span data-ttu-id="0d995-325">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-325">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="0d995-326">Wykonanie powoduje Zawijanie większości potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-326">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="0d995-327">Tylko [filtry autoryzacji](#authorization-filters) są uruchamiane przed filtrami zasobów.</span><span class="sxs-lookup"><span data-stu-id="0d995-327">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="0d995-328">Filtry zasobów są przydatne do krótkiego obwodu większości potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-328">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="0d995-329">Na przykład filtr buforowania może uniknąć pozostałej części potoku w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0d995-329">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="0d995-330">Przykłady filtru zasobów:</span><span class="sxs-lookup"><span data-stu-id="0d995-330">Resource filter examples:</span></span>

* <span data-ttu-id="0d995-331">[Filtr zasobów o krótkim obwodzie](#short-circuiting-resource-filter) pokazano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="0d995-331">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="0d995-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="0d995-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="0d995-333">Uniemożliwia powiązanie modelu z dostępem do danych formularza.</span><span class="sxs-lookup"><span data-stu-id="0d995-333">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="0d995-334">Służy do przekazywania dużych plików w celu uniemożliwienia odczytywania danych formularza do pamięci.</span><span class="sxs-lookup"><span data-stu-id="0d995-334">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="0d995-335">Filtry akcji</span><span class="sxs-lookup"><span data-stu-id="0d995-335">Action filters</span></span>

<span data-ttu-id="0d995-336">Filtry akcji **nie** mają zastosowania do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0d995-336">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="0d995-337">Razor Pages obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-337">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="0d995-338">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="0d995-338">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="0d995-339">Filtry akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-339">Action filters:</span></span>

* <span data-ttu-id="0d995-340">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-340">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="0d995-341">Ich wykonanie otacza wykonywanie metod akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-341">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="0d995-342">Poniższy kod przedstawia przykładowy filtr akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-342">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="0d995-343"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> udostępnia następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0d995-343">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="0d995-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — umożliwia odczytywanie danych wejściowych do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="0d995-345"><xref:Microsoft.AspNetCore.Mvc.Controller> — włącza manipulowanie wystąpieniem kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d995-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="0d995-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> — ustawienie `Result` wykonywanie krótkich obwodów metody akcji i kolejnych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="0d995-347">Zgłaszanie wyjątku w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-347">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="0d995-348">Zapobiega uruchamianiu kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="0d995-348">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="0d995-349">W przeciwieństwie do ustawienia `Result`, jest traktowany jako błąd, a nie pomyślny wynik.</span><span class="sxs-lookup"><span data-stu-id="0d995-349">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="0d995-350"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> zapewnia `Controller` i `Result` oraz następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0d995-350">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="0d995-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-true, jeśli wykonywanie akcji zostało skrócone przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="0d995-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="0d995-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-wartość null, jeśli filtr akcji lub poprzednio uruchomionego wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="0d995-353">Ustawienie tej właściwości na wartość null:</span><span class="sxs-lookup"><span data-stu-id="0d995-353">Setting this property to null:</span></span>

  * <span data-ttu-id="0d995-354">Skutecznie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-354">Effectively handles the exception.</span></span>
  * <span data-ttu-id="0d995-355">`Result` jest wykonywany tak, jakby został zwrócony z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-355">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="0d995-356">W przypadku `IAsyncActionFilter`, wywołanie do <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="0d995-356">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="0d995-357">Wykonuje wszystkie kolejne filtry akcji i metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-357">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="0d995-358">Zwraca wartość `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="0d995-358">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="0d995-359">Do krótkiego obwodu Przypisz <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> do wystąpienia wynikowego i nie wywołuj `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="0d995-359">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="0d995-360">Struktura zawiera abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="0d995-360">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="0d995-361">Filtr akcji `OnActionExecuting` może służyć do:</span><span class="sxs-lookup"><span data-stu-id="0d995-361">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="0d995-362">Zweryfikuj stan modelu.</span><span class="sxs-lookup"><span data-stu-id="0d995-362">Validate model state.</span></span>
* <span data-ttu-id="0d995-363">Zwróć błąd, jeśli stan jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="0d995-363">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="0d995-364">Metoda `OnActionExecuted` jest uruchamiana po metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-364">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="0d995-365">I mogą przeglądać wyniki akcji przy użyciu właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> i manipulować nimi.</span><span class="sxs-lookup"><span data-stu-id="0d995-365">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="0d995-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> jest ustawiona na wartość true, jeśli wykonywanie akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="0d995-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="0d995-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> jest ustawiona na wartość różną od null, jeśli akcja lub kolejny filtr akcji wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="0d995-368">Ustawienie `Exception` na wartość null:</span><span class="sxs-lookup"><span data-stu-id="0d995-368">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="0d995-369">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-369">Effectively handles an exception.</span></span>
  * <span data-ttu-id="0d995-370">`ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-370">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="0d995-371">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="0d995-371">Exception filters</span></span>

<span data-ttu-id="0d995-372">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="0d995-372">Exception filters:</span></span>

* <span data-ttu-id="0d995-373">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-373">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="0d995-374">Może służyć do implementowania typowych zasad obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="0d995-374">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="0d995-375">Następujący przykładowy filtr wyjątku używa niestandardowego widoku błędów, aby wyświetlić szczegóły dotyczące wyjątków występujących podczas opracowywania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-375">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="0d995-376">Następujący kod testuje filtr wyjątku:</span><span class="sxs-lookup"><span data-stu-id="0d995-376">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="0d995-377">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="0d995-377">Exception filters:</span></span>

* <span data-ttu-id="0d995-378">Nie mam zdarzeń przed i po.</span><span class="sxs-lookup"><span data-stu-id="0d995-378">Don't have before and after events.</span></span>
* <span data-ttu-id="0d995-379">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0d995-379">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="0d995-380">Obsłuż Nieobsłużone wyjątki występujące na stronie lub w tworzeniu kontrolera, [powiązania modelu](xref:mvc/models/model-binding), filtrach akcji lub metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-380">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="0d995-381">**Nie** należy przechwytywać wyjątków występujących w filtrach zasobów, filtrach wyników ani wykonywaniu wyniku MVC.</span><span class="sxs-lookup"><span data-stu-id="0d995-381">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="0d995-382">Aby obsłużyć wyjątek, ustaw właściwość <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> na `true` lub Zapisz odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="0d995-382">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="0d995-383">Spowoduje to zatrzymanie propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="0d995-383">This stops propagation of the exception.</span></span> <span data-ttu-id="0d995-384">Filtr wyjątku nie może przekształcić wyjątku w "powodzenie".</span><span class="sxs-lookup"><span data-stu-id="0d995-384">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="0d995-385">Można to zrobić tylko dla filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-385">Only an action filter can do that.</span></span>

<span data-ttu-id="0d995-386">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="0d995-386">Exception filters:</span></span>

* <span data-ttu-id="0d995-387">Są dobre dla wyjątków zalewkowania występujących w ramach akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-387">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="0d995-388">Nie są tak elastyczne jak błędy obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="0d995-388">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="0d995-389">Preferuj oprogramowanie pośredniczące dla obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="0d995-389">Prefer middleware for exception handling.</span></span> <span data-ttu-id="0d995-390">Użyj filtrów wyjątków tylko w przypadku, gdy obsługa błędów *różni* się w zależności od tego, która metoda akcji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0d995-390">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="0d995-391">Na przykład aplikacja może mieć metody akcji zarówno dla punktów końcowych interfejsu API, jak i dla widoków/HTML.</span><span class="sxs-lookup"><span data-stu-id="0d995-391">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="0d995-392">Punkty końcowe interfejsu API mogą zwracać informacje o błędach jako dane JSON, podczas gdy akcje oparte na widoku mogą zwrócić stronę błędu jako kod HTML.</span><span class="sxs-lookup"><span data-stu-id="0d995-392">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="0d995-393">Filtry wyników</span><span class="sxs-lookup"><span data-stu-id="0d995-393">Result filters</span></span>

<span data-ttu-id="0d995-394">Filtry wyników:</span><span class="sxs-lookup"><span data-stu-id="0d995-394">Result filters:</span></span>

* <span data-ttu-id="0d995-395">Zaimplementuj interfejs:</span><span class="sxs-lookup"><span data-stu-id="0d995-395">Implement an interface:</span></span>
  * <span data-ttu-id="0d995-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="0d995-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="0d995-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="0d995-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="0d995-398">Ich wykonanie otacza wykonywanie wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-398">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="0d995-399">IResultFilter i IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="0d995-399">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="0d995-400">Poniższy kod pokazuje filtr wynikowy, który dodaje nagłówek HTTP:</span><span class="sxs-lookup"><span data-stu-id="0d995-400">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="0d995-401">Rodzaj wykonywanego wyniku zależy od akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-401">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="0d995-402">Akcja zwracająca widok obejmuje wszystkie procesy przetwarzania Razor w ramach wykonywanej <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="0d995-402">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="0d995-403">Metoda interfejsu API może wykonywać pewne serializacji w ramach wykonywania wyniku.</span><span class="sxs-lookup"><span data-stu-id="0d995-403">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="0d995-404">Dowiedz się więcej o [wynikach akcji](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="0d995-404">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="0d995-405">Filtry wynikowe są wykonywane tylko wtedy, gdy akcja lub filtr akcji generuje wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-405">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="0d995-406">Filtry wynikowe nie są wykonywane, gdy:</span><span class="sxs-lookup"><span data-stu-id="0d995-406">Result filters are not executed when:</span></span>

* <span data-ttu-id="0d995-407">Filtr autoryzacji lub filtr zasobów jest krótki obwody potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-407">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="0d995-408">Filtr wyjątku obsługuje wyjątek przez wygenerowanie wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-408">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="0d995-409">Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> może skrócić wykonywanie wyniku akcji i kolejnych filtrów wyników przez ustawienie <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> do `true`.</span><span class="sxs-lookup"><span data-stu-id="0d995-409">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="0d995-410">Zapisuj w obiekcie Response podczas krótkiego obwodu, aby uniknąć generowania pustej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0d995-410">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="0d995-411">Zgłaszanie wyjątku w `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="0d995-411">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="0d995-412">Zapobiega wykonywaniu wyniku akcji i kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="0d995-412">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="0d995-413">Jest traktowany jako niepowodzenie, a nie wynikowy pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="0d995-413">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="0d995-414">Po uruchomieniu metody <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> odpowiedź prawdopodobnie została już wysłana do klienta.</span><span class="sxs-lookup"><span data-stu-id="0d995-414">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="0d995-415">Jeśli odpowiedź została już wysłana do klienta, nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="0d995-415">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="0d995-416">`ResultExecutedContext.Canceled` jest ustawiona na `true`, jeśli wykonywanie wyniku akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="0d995-416">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="0d995-417">`ResultExecutedContext.Exception` ma ustawioną wartość różną od null, jeśli wynik akcji lub kolejny filtr wynikowy wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-417">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="0d995-418">Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia ponowne zgłoszenie wyjątku w potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-418">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="0d995-419">Nie istnieje niezawodny sposób zapisu danych do odpowiedzi podczas obsługi wyjątku w filtrze wynikowym.</span><span class="sxs-lookup"><span data-stu-id="0d995-419">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="0d995-420">Jeśli nagłówki zostały opróżnione do klienta, gdy wynikiem akcji zgłasza wyjątek, nie istnieje niezawodny mechanizm wysyłania kodu błędu.</span><span class="sxs-lookup"><span data-stu-id="0d995-420">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="0d995-421">W przypadku <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>wywołanie `await next` na <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie kolejne filtry wynikowe i wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-421">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="0d995-422">Do krótkiego obwodu Ustaw [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) do `true` i nie wywołuj `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="0d995-422">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="0d995-423">Struktura zawiera abstrakcyjną `ResultFilterAttribute`, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="0d995-423">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="0d995-424">Przedstawiona wcześniej Klasa [Addheaderattribute](#add-header-attribute) jest przykładem atrybutu filtru wynikowego.</span><span class="sxs-lookup"><span data-stu-id="0d995-424">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="0d995-425">IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="0d995-425">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="0d995-426">Interfejsy <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> deklarują implementację <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, która jest uruchamiana dla wszystkich wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-426">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="0d995-427">Obejmuje to wyniki akcji generowane przez:</span><span class="sxs-lookup"><span data-stu-id="0d995-427">This includes action results produced by:</span></span>

* <span data-ttu-id="0d995-428">Filtry autoryzacji i filtry zasobów, które są skracane.</span><span class="sxs-lookup"><span data-stu-id="0d995-428">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="0d995-429">Filtry wyjątków.</span><span class="sxs-lookup"><span data-stu-id="0d995-429">Exception filters.</span></span>

<span data-ttu-id="0d995-430">Na przykład następujący filtr jest zawsze uruchamiany i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) z kodem stanu *jednostki nieprzetwarzanego 422* , gdy negocjowanie zawartości kończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="0d995-430">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="0d995-431">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="0d995-431">IFilterFactory</span></span>

<span data-ttu-id="0d995-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="0d995-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="0d995-433">W związku z tym wystąpienie `IFilterFactory` może być używane jako wystąpienie `IFilterMetadata` w dowolnym miejscu w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-433">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="0d995-434">Gdy środowisko uruchomieniowe przygotowuje się do wywołania filtru, próbuje rzutować go na `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="0d995-434">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="0d995-435">Jeśli rzutowanie powiedzie się, Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> zostanie wywołana w celu utworzenia wystąpienia `IFilterMetadata`, które jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="0d995-435">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="0d995-436">Zapewnia to elastyczny projekt, ponieważ dokładny potok filtru nie musi być ustawiony jawnie podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-436">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="0d995-437">`IFilterFactory` można zaimplementować przy użyciu niestandardowych implementacji atrybutów jako inne podejście do tworzenia filtrów:</span><span class="sxs-lookup"><span data-stu-id="0d995-437">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="0d995-438">Filtr jest stosowany w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="0d995-438">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="0d995-439">Przetestuj poprzedni kod, uruchamiając [przykład pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span><span class="sxs-lookup"><span data-stu-id="0d995-439">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="0d995-440">Wywołaj narzędzia deweloperskie F12.</span><span class="sxs-lookup"><span data-stu-id="0d995-440">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="0d995-441">Przejdź do adresu `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="0d995-441">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="0d995-442">Narzędzia programistyczne F12 wyświetlają następujące nagłówki odpowiedzi dodane przez przykładowy kod:</span><span class="sxs-lookup"><span data-stu-id="0d995-442">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="0d995-443">**autor:** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="0d995-443">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="0d995-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="0d995-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="0d995-445">**wewnętrzne:** `My header`</span><span class="sxs-lookup"><span data-stu-id="0d995-445">**internal:** `My header`</span></span>

<span data-ttu-id="0d995-446">Poprzedni kod tworzy nagłówek odpowiedzi **wewnętrznej:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="0d995-446">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="0d995-447">IFilterFactory zaimplementowane dla atrybutu</span><span class="sxs-lookup"><span data-stu-id="0d995-447">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="0d995-448">Filtry implementujące `IFilterFactory` są przydatne w przypadku filtrów, które:</span><span class="sxs-lookup"><span data-stu-id="0d995-448">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="0d995-449">Nie wymagaj parametrów przekazujących.</span><span class="sxs-lookup"><span data-stu-id="0d995-449">Don't require passing parameters.</span></span>
* <span data-ttu-id="0d995-450">Mają zależności konstruktora, które muszą być wypełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-450">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="0d995-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="0d995-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="0d995-452">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="0d995-452">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="0d995-453">`CreateInstance` ładuje określony typ z kontenera usług (DI).</span><span class="sxs-lookup"><span data-stu-id="0d995-453">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="0d995-454">Poniższy kod przedstawia trzy podejścia do stosowania `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="0d995-454">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="0d995-455">W poprzednim kodzie dekorowania nazwy metodę `[SampleActionFilter]` jest preferowanym podejściem do zastosowania `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="0d995-455">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="0d995-456">Używanie oprogramowania pośredniczącego w potoku filtru</span><span class="sxs-lookup"><span data-stu-id="0d995-456">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="0d995-457">Filtry zasobów działają podobnie jak [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) w celu obsłużenia wykonywania wszystkich elementów, które są późniejsze w potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-457">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="0d995-458">Ale filtry różnią się od oprogramowania pośredniczącego w tym, że są częścią środowiska uruchomieniowego, co oznacza, że ma dostęp do kontekstu i konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-458">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="0d995-459">Aby użyć oprogramowania pośredniczącego jako filtru, należy utworzyć typ z metodą `Configure`, która określa oprogramowanie pośredniczące, które ma zostać dodane do potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-459">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="0d995-460">W poniższym przykładzie jest wykorzystywane oprogramowanie pośredniczące lokalizacyjne do ustalenia bieżącej kultury dla żądania:</span><span class="sxs-lookup"><span data-stu-id="0d995-460">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="0d995-461">Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute>, aby uruchomić oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="0d995-461">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="0d995-462">Filtry oprogramowania pośredniczącego są uruchamiane na tym samym etapie potoku filtru jako filtry zasobów przed powiązaniem modelu i po pozostałej części potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-462">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="0d995-463">Następne akcje</span><span class="sxs-lookup"><span data-stu-id="0d995-463">Next actions</span></span>

* <span data-ttu-id="0d995-464">Zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="0d995-464">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="0d995-465">Aby eksperymentować z filtrami, należy [pobrać, przetestować i zmodyfikować przykład usługi GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="0d995-465">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0d995-466">[Kirka Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra/)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0d995-466">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0d995-467">*Filtry* w ASP.NET Core umożliwiają uruchamianie kodu przed określonymi etapami w potoku przetwarzania żądań lub po nich.</span><span class="sxs-lookup"><span data-stu-id="0d995-467">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="0d995-468">Filtry wbudowane obsługują zadania takie jak:</span><span class="sxs-lookup"><span data-stu-id="0d995-468">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="0d995-469">Autoryzacja (zapobieganie dostępowi do zasobów, dla których użytkownik nie jest autoryzowany).</span><span class="sxs-lookup"><span data-stu-id="0d995-469">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="0d995-470">Buforowanie odpowiedzi (krótkie obwody potoku żądania w celu zwrócenia buforowanej odpowiedzi).</span><span class="sxs-lookup"><span data-stu-id="0d995-470">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="0d995-471">Filtry niestandardowe mogą być tworzone w celu obsłużenia krzyżowego rozcinania.</span><span class="sxs-lookup"><span data-stu-id="0d995-471">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="0d995-472">Przykłady zagadnień związanych z rozcinaniem obejmują obsługę błędów, buforowanie, konfigurację, autoryzację i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="0d995-472">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="0d995-473">Filtry unikają duplikowania kodu.</span><span class="sxs-lookup"><span data-stu-id="0d995-473">Filters avoid duplicating code.</span></span> <span data-ttu-id="0d995-474">Na przykład filtr wyjątków obsługujący błędy może skonsolidować obsługę błędów.</span><span class="sxs-lookup"><span data-stu-id="0d995-474">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="0d995-475">Ten dokument ma zastosowanie do Razor Pages, kontrolerów interfejsu API i kontrolerów z widokami.</span><span class="sxs-lookup"><span data-stu-id="0d995-475">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="0d995-476">[Wyświetl lub Pobierz przykład](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0d995-476">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="0d995-477">Jak działają filtry</span><span class="sxs-lookup"><span data-stu-id="0d995-477">How filters work</span></span>

<span data-ttu-id="0d995-478">Filtry są uruchamiane w *potoku wywołania akcji ASP.NET Core*, czasami określane jako *potok filtru*.</span><span class="sxs-lookup"><span data-stu-id="0d995-478">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="0d995-479">Potok filtru jest uruchamiany po ASP.NET Core wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="0d995-479">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, kierowanie oprogramowania pośredniczącego, wybór akcji i potok wywołania akcji ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="0d995-482">Typy filtrów</span><span class="sxs-lookup"><span data-stu-id="0d995-482">Filter types</span></span>

<span data-ttu-id="0d995-483">Każdy typ filtru jest wykonywany na innym etapie w potoku filtru:</span><span class="sxs-lookup"><span data-stu-id="0d995-483">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="0d995-484">[Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do określenia, czy użytkownik jest autoryzowany do żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-484">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="0d995-485">Filtry autoryzacji skracają obwód w przypadku, gdy żądanie jest nieautoryzowane.</span><span class="sxs-lookup"><span data-stu-id="0d995-485">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="0d995-486">[Filtry zasobów](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="0d995-486">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="0d995-487">Uruchom po autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-487">Run after authorization.</span></span>  
  * <span data-ttu-id="0d995-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> może uruchomić kod przed resztą potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="0d995-489">Na przykład, `OnResourceExecuting` może uruchomić kod przed powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="0d995-489">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="0d995-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> można uruchomić kod po zakończeniu reszty potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="0d995-491">[Filtry akcji](#action-filters) mogą uruchomić kod bezpośrednio przed i po wywołaniu pojedynczej metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-491">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="0d995-492">Mogą one służyć do manipulowania argumentami przekazaną do akcji i wynik zwrócony z akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-492">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="0d995-493">Filtry akcji **nie** są obsługiwane w Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0d995-493">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="0d995-494">[Filtry wyjątków](#exception-filters) są używane do stosowania zasad globalnych do nieobsłużonych wyjątków, które wystąpiły przed zapisem w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0d995-494">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="0d995-495">[Filtry wynikowe](#result-filters) mogą uruchamiać kod bezpośrednio przed i po wykonaniu poszczególnych wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-495">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="0d995-496">Są one uruchamiane tylko wtedy, gdy metoda akcji została wykonana pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="0d995-496">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="0d995-497">Są one przydatne dla logiki, która musi być w trakcie wyświetlania lub wykonywania w programie formatującego.</span><span class="sxs-lookup"><span data-stu-id="0d995-497">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="0d995-498">Na poniższym diagramie przedstawiono sposób, w jaki typy filtrów współdziałają w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-498">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Żądanie jest przetwarzane przez filtry autoryzacji, filtry zasobów, powiązania modelu, filtry akcji, wykonywanie akcji i konwersję wyników akcji, filtry wyjątków, filtry wynikowe i wykonywanie wyniku.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="0d995-501">Implementacja</span><span class="sxs-lookup"><span data-stu-id="0d995-501">Implementation</span></span>

<span data-ttu-id="0d995-502">Filtry obsługują implementacje synchroniczne i asynchroniczne za pomocą różnych definicji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="0d995-502">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="0d995-503">Filtry synchroniczne mogą uruchamiać kod przed (`On-Stage-Executing`) i po nim (`On-Stage-Executed`) ich etap potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-503">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="0d995-504">Na przykład, `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-504">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="0d995-505">`OnActionExecuted` jest wywoływana po powrocie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-505">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="0d995-506">Filtry asynchroniczne definiują metodę `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="0d995-506">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="0d995-507">W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który wykonuje metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-507">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="0d995-508">Każda metoda `On-Stage-ExecutionAsync` przyjmuje `FilterType-ExecutionDelegate`, który wykonuje etap potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-508">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="0d995-509">Wiele etapów filtrowania</span><span class="sxs-lookup"><span data-stu-id="0d995-509">Multiple filter stages</span></span>

<span data-ttu-id="0d995-510">Interfejsy dla wielu etapów filtru można zaimplementować w pojedynczej klasie.</span><span class="sxs-lookup"><span data-stu-id="0d995-510">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="0d995-511">Na przykład Klasa <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementuje `IActionFilter`, `IResultFilter`i ich ekwiwalenty asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="0d995-511">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="0d995-512">Implementowanie synchronicznej lub asynchronicznej wersji interfejsu filtru **, a** **nie** obu.</span><span class="sxs-lookup"><span data-stu-id="0d995-512">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="0d995-513">Środowisko uruchomieniowe najpierw sprawdza, czy filtr implementuje interfejs asynchroniczny, a jeśli tak, wywołuje to.</span><span class="sxs-lookup"><span data-stu-id="0d995-513">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="0d995-514">Jeśli nie, wywołuje metody interfejsu synchronicznego.</span><span class="sxs-lookup"><span data-stu-id="0d995-514">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="0d995-515">Jeśli zarówno interfejsy asynchroniczne, jak i synchroniczne są zaimplementowane w jednej klasie, wywoływana jest tylko Metoda async.</span><span class="sxs-lookup"><span data-stu-id="0d995-515">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="0d995-516">W przypadku używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> przesłaniać tylko metody synchroniczne lub metodę asynchroniczną dla każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-516">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="0d995-517">Wbudowane atrybuty filtru</span><span class="sxs-lookup"><span data-stu-id="0d995-517">Built-in filter attributes</span></span>

<span data-ttu-id="0d995-518">ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które można podklasować i dostosowywać.</span><span class="sxs-lookup"><span data-stu-id="0d995-518">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="0d995-519">Na przykład poniższy filtr wynikowy dodaje nagłówek do odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="0d995-519">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="0d995-520">Atrybuty umożliwiają filtrom akceptowanie argumentów, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="0d995-520">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="0d995-521">Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:</span><span class="sxs-lookup"><span data-stu-id="0d995-521">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="0d995-522">Kilka interfejsów filtrów ma odpowiednie atrybuty, które mogą być używane jako klasy bazowe dla implementacji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="0d995-522">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="0d995-523">Atrybuty filtru:</span><span class="sxs-lookup"><span data-stu-id="0d995-523">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="0d995-524">Zakresy filtrów i kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="0d995-524">Filter scopes and order of execution</span></span>

<span data-ttu-id="0d995-525">Filtr można dodać do potoku w jednym z trzech *zakresów*:</span><span class="sxs-lookup"><span data-stu-id="0d995-525">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="0d995-526">Użycie atrybutu w akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-526">Using an attribute on an action.</span></span>
* <span data-ttu-id="0d995-527">Używanie atrybutu na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="0d995-527">Using an attribute on a controller.</span></span>
* <span data-ttu-id="0d995-528">Globalnie dla wszystkich kontrolerów i akcji, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="0d995-528">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="0d995-529">Poprzedni kod dodaje trzy filtry globalnie przy użyciu kolekcji [MvcOptions. filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) .</span><span class="sxs-lookup"><span data-stu-id="0d995-529">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="0d995-530">Domyślna kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="0d995-530">Default order of execution</span></span>

<span data-ttu-id="0d995-531">Jeśli istnieje wiele filtrów tego *samego typu*, zakres określa domyślną kolejność wykonywania filtrowania.</span><span class="sxs-lookup"><span data-stu-id="0d995-531">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="0d995-532">Filtry globalne Otocz filtry klas.</span><span class="sxs-lookup"><span data-stu-id="0d995-532">Global filters surround class filters.</span></span> <span data-ttu-id="0d995-533">Filtry klas Otocz filtry metod.</span><span class="sxs-lookup"><span data-stu-id="0d995-533">Class filters surround method filters.</span></span>

<span data-ttu-id="0d995-534">W wyniku zagnieżdżania filtrów, *po* kodzie filtrów działa w odwrotnej kolejności *przed* kodem.</span><span class="sxs-lookup"><span data-stu-id="0d995-534">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="0d995-535">Sekwencja filtru:</span><span class="sxs-lookup"><span data-stu-id="0d995-535">The filter sequence:</span></span>

* <span data-ttu-id="0d995-536">*Przed* kodem filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="0d995-536">The *before* code of global filters.</span></span>
  * <span data-ttu-id="0d995-537">*Przed* kodem filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d995-537">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="0d995-538">Filtr *przed* kodem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-538">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="0d995-539">*Po* kodzie metody akcji filtry.</span><span class="sxs-lookup"><span data-stu-id="0d995-539">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="0d995-540">*Po* kodzie filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d995-540">The *after* code of controller filters.</span></span>
* <span data-ttu-id="0d995-541">Kod *po* filtrach globalnych.</span><span class="sxs-lookup"><span data-stu-id="0d995-541">The *after* code of global filters.</span></span>
  
<span data-ttu-id="0d995-542">Poniższy przykład ilustruje kolejność, w której metody filtrowania są wywoływane dla synchronicznych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-542">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="0d995-543">Sequence</span><span class="sxs-lookup"><span data-stu-id="0d995-543">Sequence</span></span> | <span data-ttu-id="0d995-544">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="0d995-544">Filter scope</span></span> | <span data-ttu-id="0d995-545">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-545">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="0d995-546">1</span><span class="sxs-lookup"><span data-stu-id="0d995-546">1</span></span> | <span data-ttu-id="0d995-547">Globalne</span><span class="sxs-lookup"><span data-stu-id="0d995-547">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="0d995-548">2</span><span class="sxs-lookup"><span data-stu-id="0d995-548">2</span></span> | <span data-ttu-id="0d995-549">Kontroler</span><span class="sxs-lookup"><span data-stu-id="0d995-549">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="0d995-550">3</span><span class="sxs-lookup"><span data-stu-id="0d995-550">3</span></span> | <span data-ttu-id="0d995-551">Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-551">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="0d995-552">4</span><span class="sxs-lookup"><span data-stu-id="0d995-552">4</span></span> | <span data-ttu-id="0d995-553">Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-553">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="0d995-554">5</span><span class="sxs-lookup"><span data-stu-id="0d995-554">5</span></span> | <span data-ttu-id="0d995-555">Kontroler</span><span class="sxs-lookup"><span data-stu-id="0d995-555">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="0d995-556">6</span><span class="sxs-lookup"><span data-stu-id="0d995-556">6</span></span> | <span data-ttu-id="0d995-557">Globalne</span><span class="sxs-lookup"><span data-stu-id="0d995-557">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="0d995-558">Ta sekwencja pokazuje:</span><span class="sxs-lookup"><span data-stu-id="0d995-558">This sequence shows:</span></span>

* <span data-ttu-id="0d995-559">Filtr metody jest zagnieżdżony w obrębie filtru kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d995-559">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="0d995-560">Filtr kontrolera jest zagnieżdżony w obrębie filtru globalnego.</span><span class="sxs-lookup"><span data-stu-id="0d995-560">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="0d995-561">Kontrolery i filtry na poziomie strony Razor</span><span class="sxs-lookup"><span data-stu-id="0d995-561">Controller and Razor Page level filters</span></span>

<span data-ttu-id="0d995-562">Każdy kontroler, który dziedziczy z klasy bazowej <xref:Microsoft.AspNetCore.Mvc.Controller> obejmuje metody [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)i [Controller. OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="0d995-562">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="0d995-563">Te metody:</span><span class="sxs-lookup"><span data-stu-id="0d995-563">These methods:</span></span>

* <span data-ttu-id="0d995-564">Zawiń filtry, które są uruchamiane dla danego działania.</span><span class="sxs-lookup"><span data-stu-id="0d995-564">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="0d995-565">`OnActionExecuting` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-565">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="0d995-566">`OnActionExecuted` jest wywoływana po wszystkich filtrach akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-566">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="0d995-567">`OnActionExecutionAsync` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-567">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="0d995-568">Kod w filtrze po `next` jest uruchamiany po metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-568">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="0d995-569">Na przykład w przykładzie pobierania `MySampleActionFilter` jest stosowany globalnie podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="0d995-569">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="0d995-570">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="0d995-570">The `TestController`:</span></span>

* <span data-ttu-id="0d995-571">Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do akcji `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="0d995-571">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="0d995-572">Zastępuje `OnActionExecuting` i `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="0d995-572">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="0d995-573">Przechodzenie do `https://localhost:5001/Test/FilterTest2` uruchamia następujący kod:</span><span class="sxs-lookup"><span data-stu-id="0d995-573">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="0d995-574">Aby uzyskać Razor Pages, zobacz [implementowanie filtrów stron Razor przez zastępowanie metod filtrowania](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="0d995-574">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="0d995-575">Zastępowanie kolejności domyślnej</span><span class="sxs-lookup"><span data-stu-id="0d995-575">Overriding the default order</span></span>

<span data-ttu-id="0d995-576">Domyślną sekwencję wykonywania można zastąpić, implementując <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-576">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="0d995-577">`IOrderedFilter` uwidacznia Właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, która ma pierwszeństwo przed zakresem w celu określenia kolejności wykonywania.</span><span class="sxs-lookup"><span data-stu-id="0d995-577">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="0d995-578">Filtr z niższą `Order` wartością:</span><span class="sxs-lookup"><span data-stu-id="0d995-578">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="0d995-579">Uruchamia *przed* kodem przed filtrem o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="0d995-579">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="0d995-580">Uruchamia *po* kodzie po filtrze o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="0d995-580">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="0d995-581">Właściwość `Order` można ustawić przy użyciu parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="0d995-581">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="0d995-582">Należy wziąć pod uwagę te same 3 filtry akcji, które przedstawiono w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="0d995-582">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="0d995-583">Jeśli właściwość `Order` kontrolera i filtry globalne mają odpowiednio wartość 1 i 2, kolejność wykonywania zostanie odwrócona.</span><span class="sxs-lookup"><span data-stu-id="0d995-583">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="0d995-584">Sequence</span><span class="sxs-lookup"><span data-stu-id="0d995-584">Sequence</span></span> | <span data-ttu-id="0d995-585">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="0d995-585">Filter scope</span></span> | <span data-ttu-id="0d995-586">`Order` Właściwość</span><span class="sxs-lookup"><span data-stu-id="0d995-586">`Order` property</span></span> | <span data-ttu-id="0d995-587">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-587">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="0d995-588">1</span><span class="sxs-lookup"><span data-stu-id="0d995-588">1</span></span> | <span data-ttu-id="0d995-589">Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-589">Method</span></span> | <span data-ttu-id="0d995-590">0</span><span class="sxs-lookup"><span data-stu-id="0d995-590">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="0d995-591">2</span><span class="sxs-lookup"><span data-stu-id="0d995-591">2</span></span> | <span data-ttu-id="0d995-592">Kontroler</span><span class="sxs-lookup"><span data-stu-id="0d995-592">Controller</span></span> | <span data-ttu-id="0d995-593">1</span><span class="sxs-lookup"><span data-stu-id="0d995-593">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="0d995-594">3</span><span class="sxs-lookup"><span data-stu-id="0d995-594">3</span></span> | <span data-ttu-id="0d995-595">Globalne</span><span class="sxs-lookup"><span data-stu-id="0d995-595">Global</span></span> | <span data-ttu-id="0d995-596">2</span><span class="sxs-lookup"><span data-stu-id="0d995-596">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="0d995-597">4</span><span class="sxs-lookup"><span data-stu-id="0d995-597">4</span></span> | <span data-ttu-id="0d995-598">Globalne</span><span class="sxs-lookup"><span data-stu-id="0d995-598">Global</span></span> | <span data-ttu-id="0d995-599">2</span><span class="sxs-lookup"><span data-stu-id="0d995-599">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="0d995-600">5</span><span class="sxs-lookup"><span data-stu-id="0d995-600">5</span></span> | <span data-ttu-id="0d995-601">Kontroler</span><span class="sxs-lookup"><span data-stu-id="0d995-601">Controller</span></span> | <span data-ttu-id="0d995-602">1</span><span class="sxs-lookup"><span data-stu-id="0d995-602">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="0d995-603">6</span><span class="sxs-lookup"><span data-stu-id="0d995-603">6</span></span> | <span data-ttu-id="0d995-604">Metoda</span><span class="sxs-lookup"><span data-stu-id="0d995-604">Method</span></span> | <span data-ttu-id="0d995-605">0</span><span class="sxs-lookup"><span data-stu-id="0d995-605">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="0d995-606">Właściwość `Order` przesłania zakres podczas określania kolejności, w której są uruchamiane filtry.</span><span class="sxs-lookup"><span data-stu-id="0d995-606">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="0d995-607">Filtry są sortowane najpierw według kolejności, a następnie zakres jest używany do przerwania powiązań.</span><span class="sxs-lookup"><span data-stu-id="0d995-607">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="0d995-608">Wszystkie wbudowane filtry implementują `IOrderedFilter` i ustawiają domyślną wartość `Order` równą 0.</span><span class="sxs-lookup"><span data-stu-id="0d995-608">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="0d995-609">W przypadku filtrów wbudowanych zakres określa kolejność, chyba że `Order` jest ustawiona na wartość różną od zera.</span><span class="sxs-lookup"><span data-stu-id="0d995-609">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="0d995-610">Anulowanie i krótkie obwody</span><span class="sxs-lookup"><span data-stu-id="0d995-610">Cancellation and short-circuiting</span></span>

<span data-ttu-id="0d995-611">Potok filtru może być skrócony przez ustawienie właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> na parametrze <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> dostarczonym do metody Filter.</span><span class="sxs-lookup"><span data-stu-id="0d995-611">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="0d995-612">Na przykład poniższy filtr zasobów uniemożliwia wykonanie pozostałej części potoku:</span><span class="sxs-lookup"><span data-stu-id="0d995-612">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="0d995-613">W poniższym kodzie, zarówno `ShortCircuitingResourceFilter`, jak i filtr `AddHeader`, dla metody akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="0d995-613">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="0d995-614">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="0d995-614">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="0d995-615">Uruchamia się najpierw, ponieważ jest to filtr zasobów, a `AddHeader` jest filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-615">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="0d995-616">Krótkie obwody pozostała część potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-616">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="0d995-617">W związku z tym filtr `AddHeader` nigdy nie jest uruchamiany dla akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="0d995-617">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="0d995-618">Takie zachowanie będzie takie samo, jeśli oba filtry zostały zastosowane na poziomie metody akcji, pod warunkiem że `ShortCircuitingResourceFilter` uruchamiane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="0d995-618">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="0d995-619">`ShortCircuitingResourceFilter` jest uruchamiany jako pierwszy ze względu na jego typ filtru lub jawne użycie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="0d995-619">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="0d995-620">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="0d995-620">Dependency injection</span></span>

<span data-ttu-id="0d995-621">Filtry można dodawać według typu lub według wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="0d995-621">Filters can be added by type or by instance.</span></span> <span data-ttu-id="0d995-622">W przypadku dodania wystąpienia to wystąpienie jest używane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-622">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="0d995-623">Jeśli typ zostanie dodany, jego typ jest aktywowany.</span><span class="sxs-lookup"><span data-stu-id="0d995-623">If a type is added, it's type-activated.</span></span> <span data-ttu-id="0d995-624">Filtr aktywowany przez typ oznacza:</span><span class="sxs-lookup"><span data-stu-id="0d995-624">A type-activated filter means:</span></span>

* <span data-ttu-id="0d995-625">Wystąpienie jest tworzone dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-625">An instance is created for each request.</span></span>
* <span data-ttu-id="0d995-626">Wszystkie zależności konstruktora są wypełniane przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="0d995-626">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="0d995-627">Filtry zaimplementowane jako atrybuty i dodawane bezpośrednio do klas kontrolera lub metod akcji nie mogą mieć zależności konstruktorów udostępnianych przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="0d995-627">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="0d995-628">Zależności konstruktora nie mogą być dostarczone przez DI, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="0d995-628">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="0d995-629">Atrybuty muszą mieć podane parametry konstruktora, w których są stosowane.</span><span class="sxs-lookup"><span data-stu-id="0d995-629">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="0d995-630">Jest to ograniczenie działania atrybutów.</span><span class="sxs-lookup"><span data-stu-id="0d995-630">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="0d995-631">Następujące filtry obsługują zależności konstruktora dostarczone z elementów DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-631">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="0d995-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowany dla atrybutu.</span><span class="sxs-lookup"><span data-stu-id="0d995-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="0d995-633">Powyższe filtry można zastosować do kontrolera lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-633">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="0d995-634">Rejestratory są dostępne z programu DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-634">Loggers are available from DI.</span></span> <span data-ttu-id="0d995-635">Należy jednak unikać tworzenia i używania filtrów wyłącznie w celach rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0d995-635">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="0d995-636">[Wbudowane rejestrowanie struktury](xref:fundamentals/logging/index) zazwyczaj zapewnia, co jest potrzebne do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0d995-636">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="0d995-637">Rejestrowanie dodane do filtrów:</span><span class="sxs-lookup"><span data-stu-id="0d995-637">Logging added to filters:</span></span>

* <span data-ttu-id="0d995-638">Należy skoncentrować się na problemach z domeną biznesową lub działaniu specyficznym dla filtra.</span><span class="sxs-lookup"><span data-stu-id="0d995-638">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="0d995-639">**Nie** należy rejestrować akcji ani innych zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="0d995-639">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="0d995-640">Wbudowane filtry akcje dziennika i zdarzenia struktury.</span><span class="sxs-lookup"><span data-stu-id="0d995-640">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="0d995-641">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="0d995-641">ServiceFilterAttribute</span></span>

<span data-ttu-id="0d995-642">Typy implementacji filtru usługi są zarejestrowane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d995-642">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="0d995-643"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> Pobiera wystąpienie filtru z DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-643">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="0d995-644">Poniższy kod ilustruje `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="0d995-644">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="0d995-645">W poniższym kodzie, `AddHeaderResultServiceFilter` jest dodawane do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-645">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="0d995-646">W poniższym kodzie atrybut `ServiceFilter` Pobiera wystąpienie filtru `AddHeaderResultServiceFilter` z DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-646">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="0d995-647">Podczas używania `ServiceFilterAttribute`, należy ustawić [Servicefilterattribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="0d995-647">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="0d995-648">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="0d995-648">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="0d995-649">Środowisko uruchomieniowe ASP.NET Core nie gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="0d995-649">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="0d995-650">Zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-650">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="0d995-651">Filtr nie zostanie ponownie żądany z kontenera DI w pewnym momencie.</span><span class="sxs-lookup"><span data-stu-id="0d995-651">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="0d995-652">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="0d995-652">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="0d995-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="0d995-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="0d995-654">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="0d995-654">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="0d995-655">`CreateInstance` ładuje określony typ z DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-655">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="0d995-656">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="0d995-656">TypeFilterAttribute</span></span>

<span data-ttu-id="0d995-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> przypomina <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie jest rozpoznawany bezpośrednio w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="0d995-658">Tworzy wystąpienie tego typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="0d995-658">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="0d995-659">Ponieważ typy `TypeFilterAttribute` nie są rozpoznawane bezpośrednio w kontenerze DI:</span><span class="sxs-lookup"><span data-stu-id="0d995-659">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="0d995-660">Typy, do których odwołuje się `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="0d995-660">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="0d995-661">Ich zależności są spełnione przez kontener DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-661">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="0d995-662">`TypeFilterAttribute` może opcjonalnie zaakceptować argumenty konstruktora dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="0d995-662">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="0d995-663">W przypadku używania `TypeFilterAttribute`ustawienie [TypeFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="0d995-663">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="0d995-664">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="0d995-664">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="0d995-665">Środowisko uruchomieniowe ASP.NET Core nie zapewnia żadnych gwarancji, że zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-665">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="0d995-666">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="0d995-666">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="0d995-667">Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="0d995-667">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="0d995-668">Filtry autoryzacji</span><span class="sxs-lookup"><span data-stu-id="0d995-668">Authorization filters</span></span>

<span data-ttu-id="0d995-669">Filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-669">Authorization filters:</span></span>

* <span data-ttu-id="0d995-670">Są pierwszymi filtrami uruchamianymi w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-670">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="0d995-671">Kontrola dostępu do metod akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-671">Control access to action methods.</span></span>
* <span data-ttu-id="0d995-672">Ma metodę Before, ale nie po metodzie.</span><span class="sxs-lookup"><span data-stu-id="0d995-672">Have a before method, but no after method.</span></span>

<span data-ttu-id="0d995-673">Niestandardowe filtry autoryzacji wymagają niestandardowej struktury autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-673">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="0d995-674">Preferuj skonfigurowanie zasad autoryzacji lub zapisanie niestandardowych zasad autoryzacji przez zapisanie filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="0d995-674">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="0d995-675">Wbudowany filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-675">The built-in authorization filter:</span></span>

* <span data-ttu-id="0d995-676">Wywołuje system autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-676">Calls the authorization system.</span></span>
* <span data-ttu-id="0d995-677">Nie zezwala na żądania.</span><span class="sxs-lookup"><span data-stu-id="0d995-677">Does not authorize requests.</span></span>

<span data-ttu-id="0d995-678">**Nie** zgłaszaj wyjątków w ramach filtrów autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-678">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="0d995-679">Wyjątek nie zostanie obsłużony.</span><span class="sxs-lookup"><span data-stu-id="0d995-679">The exception will not be handled.</span></span>
* <span data-ttu-id="0d995-680">Filtry wyjątków nie będą obsługiwać wyjątku.</span><span class="sxs-lookup"><span data-stu-id="0d995-680">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="0d995-681">Rozważ wygenerowanie wyzwania w przypadku wystąpienia wyjątku w filtrze autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-681">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="0d995-682">Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="0d995-682">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="0d995-683">Filtry zasobów</span><span class="sxs-lookup"><span data-stu-id="0d995-683">Resource filters</span></span>

<span data-ttu-id="0d995-684">Filtry zasobów:</span><span class="sxs-lookup"><span data-stu-id="0d995-684">Resource filters:</span></span>

* <span data-ttu-id="0d995-685">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-685">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="0d995-686">Wykonanie powoduje Zawijanie większości potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-686">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="0d995-687">Tylko [filtry autoryzacji](#authorization-filters) są uruchamiane przed filtrami zasobów.</span><span class="sxs-lookup"><span data-stu-id="0d995-687">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="0d995-688">Filtry zasobów są przydatne do krótkiego obwodu większości potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-688">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="0d995-689">Na przykład filtr buforowania może uniknąć pozostałej części potoku w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="0d995-689">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="0d995-690">Przykłady filtru zasobów:</span><span class="sxs-lookup"><span data-stu-id="0d995-690">Resource filter examples:</span></span>

* <span data-ttu-id="0d995-691">[Filtr zasobów o krótkim obwodzie](#short-circuiting-resource-filter) pokazano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="0d995-691">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="0d995-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="0d995-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="0d995-693">Uniemożliwia powiązanie modelu z dostępem do danych formularza.</span><span class="sxs-lookup"><span data-stu-id="0d995-693">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="0d995-694">Służy do przekazywania dużych plików w celu uniemożliwienia odczytywania danych formularza do pamięci.</span><span class="sxs-lookup"><span data-stu-id="0d995-694">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="0d995-695">Filtry akcji</span><span class="sxs-lookup"><span data-stu-id="0d995-695">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0d995-696">Filtry akcji **nie** mają zastosowania do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0d995-696">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="0d995-697">Razor Pages obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-697">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="0d995-698">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="0d995-698">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="0d995-699">Filtry akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-699">Action filters:</span></span>

* <span data-ttu-id="0d995-700">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-700">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="0d995-701">Ich wykonanie otacza wykonywanie metod akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-701">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="0d995-702">Poniższy kod przedstawia przykładowy filtr akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-702">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="0d995-703"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> udostępnia następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0d995-703">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="0d995-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — umożliwia odczytywanie danych wejściowych metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="0d995-705"><xref:Microsoft.AspNetCore.Mvc.Controller> — włącza manipulowanie wystąpieniem kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0d995-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="0d995-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> — ustawienie `Result` wykonywanie krótkich obwodów metody akcji i kolejnych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="0d995-707">Zgłaszanie wyjątku w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-707">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="0d995-708">Zapobiega uruchamianiu kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="0d995-708">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="0d995-709">W przeciwieństwie do ustawienia `Result`, jest traktowany jako błąd, a nie pomyślny wynik.</span><span class="sxs-lookup"><span data-stu-id="0d995-709">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="0d995-710"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> zapewnia `Controller` i `Result` oraz następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0d995-710">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="0d995-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-true, jeśli wykonywanie akcji zostało skrócone przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="0d995-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="0d995-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-wartość null, jeśli filtr akcji lub poprzednio uruchomionego wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="0d995-713">Ustawienie tej właściwości na wartość null:</span><span class="sxs-lookup"><span data-stu-id="0d995-713">Setting this property to null:</span></span>

  * <span data-ttu-id="0d995-714">Skutecznie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-714">Effectively handles the exception.</span></span>
  * <span data-ttu-id="0d995-715">`Result` jest wykonywany tak, jakby został zwrócony z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-715">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="0d995-716">W przypadku `IAsyncActionFilter`, wywołanie do <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="0d995-716">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="0d995-717">Wykonuje wszystkie kolejne filtry akcji i metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-717">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="0d995-718">Zwraca wartość `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="0d995-718">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="0d995-719">Do krótkiego obwodu Przypisz <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> do wystąpienia wynikowego i nie wywołuj `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="0d995-719">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="0d995-720">Struktura zawiera abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="0d995-720">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="0d995-721">Filtr akcji `OnActionExecuting` może służyć do:</span><span class="sxs-lookup"><span data-stu-id="0d995-721">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="0d995-722">Zweryfikuj stan modelu.</span><span class="sxs-lookup"><span data-stu-id="0d995-722">Validate model state.</span></span>
* <span data-ttu-id="0d995-723">Zwróć błąd, jeśli stan jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="0d995-723">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="0d995-724">Metoda `OnActionExecuted` jest uruchamiana po metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="0d995-724">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="0d995-725">I mogą przeglądać wyniki akcji przy użyciu właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> i manipulować nimi.</span><span class="sxs-lookup"><span data-stu-id="0d995-725">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="0d995-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> jest ustawiona na wartość true, jeśli wykonywanie akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="0d995-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="0d995-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> jest ustawiona na wartość różną od null, jeśli akcja lub kolejny filtr akcji wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="0d995-728">Ustawienie `Exception` na wartość null:</span><span class="sxs-lookup"><span data-stu-id="0d995-728">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="0d995-729">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-729">Effectively handles an exception.</span></span>
  * <span data-ttu-id="0d995-730">`ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-730">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="0d995-731">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="0d995-731">Exception filters</span></span>

<span data-ttu-id="0d995-732">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="0d995-732">Exception filters:</span></span>

* <span data-ttu-id="0d995-733">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="0d995-733">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="0d995-734">Może służyć do implementowania typowych zasad obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="0d995-734">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="0d995-735">Następujący przykładowy filtr wyjątku używa niestandardowego widoku błędów, aby wyświetlić szczegóły dotyczące wyjątków występujących podczas opracowywania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0d995-735">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="0d995-736">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="0d995-736">Exception filters:</span></span>

* <span data-ttu-id="0d995-737">Nie mam zdarzeń przed i po.</span><span class="sxs-lookup"><span data-stu-id="0d995-737">Don't have before and after events.</span></span>
* <span data-ttu-id="0d995-738">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0d995-738">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="0d995-739">Obsłuż Nieobsłużone wyjątki występujące na stronie lub w tworzeniu kontrolera, [powiązania modelu](xref:mvc/models/model-binding), filtrach akcji lub metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-739">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="0d995-740">**Nie** należy przechwytywać wyjątków występujących w filtrach zasobów, filtrach wyników ani wykonywaniu wyniku MVC.</span><span class="sxs-lookup"><span data-stu-id="0d995-740">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="0d995-741">Aby obsłużyć wyjątek, ustaw właściwość <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> na `true` lub Zapisz odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="0d995-741">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="0d995-742">Spowoduje to zatrzymanie propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="0d995-742">This stops propagation of the exception.</span></span> <span data-ttu-id="0d995-743">Filtr wyjątku nie może przekształcić wyjątku w "powodzenie".</span><span class="sxs-lookup"><span data-stu-id="0d995-743">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="0d995-744">Można to zrobić tylko dla filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-744">Only an action filter can do that.</span></span>

<span data-ttu-id="0d995-745">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="0d995-745">Exception filters:</span></span>

* <span data-ttu-id="0d995-746">Są dobre dla wyjątków zalewkowania występujących w ramach akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-746">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="0d995-747">Nie są tak elastyczne jak błędy obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="0d995-747">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="0d995-748">Preferuj oprogramowanie pośredniczące dla obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="0d995-748">Prefer middleware for exception handling.</span></span> <span data-ttu-id="0d995-749">Użyj filtrów wyjątków tylko w przypadku, gdy obsługa błędów *różni* się w zależności od tego, która metoda akcji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="0d995-749">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="0d995-750">Na przykład aplikacja może mieć metody akcji zarówno dla punktów końcowych interfejsu API, jak i dla widoków/HTML.</span><span class="sxs-lookup"><span data-stu-id="0d995-750">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="0d995-751">Punkty końcowe interfejsu API mogą zwracać informacje o błędach jako dane JSON, podczas gdy akcje oparte na widoku mogą zwrócić stronę błędu jako kod HTML.</span><span class="sxs-lookup"><span data-stu-id="0d995-751">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="0d995-752">Filtry wyników</span><span class="sxs-lookup"><span data-stu-id="0d995-752">Result filters</span></span>

<span data-ttu-id="0d995-753">Filtry wyników:</span><span class="sxs-lookup"><span data-stu-id="0d995-753">Result filters:</span></span>

* <span data-ttu-id="0d995-754">Zaimplementuj interfejs:</span><span class="sxs-lookup"><span data-stu-id="0d995-754">Implement an interface:</span></span>
  * <span data-ttu-id="0d995-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="0d995-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="0d995-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="0d995-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="0d995-757">Ich wykonanie otacza wykonywanie wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-757">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="0d995-758">IResultFilter i IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="0d995-758">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="0d995-759">Poniższy kod pokazuje filtr wynikowy, który dodaje nagłówek HTTP:</span><span class="sxs-lookup"><span data-stu-id="0d995-759">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="0d995-760">Rodzaj wykonywanego wyniku zależy od akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-760">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="0d995-761">Akcja zwracająca widok obejmie wszystkie przetwarzanie Razor w ramach wykonywanej <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="0d995-761">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="0d995-762">Metoda interfejsu API może wykonywać pewne serializacji w ramach wykonywania wyniku.</span><span class="sxs-lookup"><span data-stu-id="0d995-762">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="0d995-763">Dowiedz się więcej o [wynikach akcji](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="0d995-763">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="0d995-764">Filtry wynikowe są wykonywane tylko wtedy, gdy akcja lub filtr akcji generuje wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-764">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="0d995-765">Filtry wynikowe nie są wykonywane, gdy:</span><span class="sxs-lookup"><span data-stu-id="0d995-765">Result filters are not executed when:</span></span>

* <span data-ttu-id="0d995-766">Filtr autoryzacji lub filtr zasobów jest krótki obwody potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-766">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="0d995-767">Filtr wyjątku obsługuje wyjątek przez wygenerowanie wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-767">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="0d995-768">Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> może skrócić wykonywanie wyniku akcji i kolejnych filtrów wyników przez ustawienie <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> do `true`.</span><span class="sxs-lookup"><span data-stu-id="0d995-768">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="0d995-769">Zapisuj w obiekcie Response podczas krótkiego obwodu, aby uniknąć generowania pustej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="0d995-769">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="0d995-770">Zgłaszanie wyjątku w `IResultFilter.OnResultExecuting` spowoduje:</span><span class="sxs-lookup"><span data-stu-id="0d995-770">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="0d995-771">Zapobiegaj wykonaniu wyniku akcji i kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="0d995-771">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="0d995-772">Być traktowany jako niepowodzenie, a nie wynikowy pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="0d995-772">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="0d995-773">Po uruchomieniu metody <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> odpowiedź została już wysłana do klienta.</span><span class="sxs-lookup"><span data-stu-id="0d995-773">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="0d995-774">Jeśli odpowiedź została już wysłana do klienta, nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="0d995-774">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="0d995-775">`ResultExecutedContext.Canceled` jest ustawiona na `true`, jeśli wykonywanie wyniku akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="0d995-775">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="0d995-776">`ResultExecutedContext.Exception` ma ustawioną wartość różną od null, jeśli wynik akcji lub kolejny filtr wynikowy wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0d995-776">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="0d995-777">Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia ponowne zgłoszenie wyjątku przez ASP.NET Core w dalszej części potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-777">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="0d995-778">Nie istnieje niezawodny sposób zapisu danych do odpowiedzi podczas obsługi wyjątku w filtrze wynikowym.</span><span class="sxs-lookup"><span data-stu-id="0d995-778">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="0d995-779">Jeśli nagłówki zostały opróżnione do klienta, gdy wynikiem akcji zgłasza wyjątek, nie istnieje niezawodny mechanizm wysyłania kodu błędu.</span><span class="sxs-lookup"><span data-stu-id="0d995-779">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="0d995-780">W przypadku <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>wywołanie `await next` na <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie kolejne filtry wynikowe i wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-780">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="0d995-781">Do krótkiego obwodu Ustaw [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) do `true` i nie wywołuj `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="0d995-781">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="0d995-782">Struktura zawiera abstrakcyjną `ResultFilterAttribute`, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="0d995-782">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="0d995-783">Przedstawiona wcześniej Klasa [Addheaderattribute](#add-header-attribute) jest przykładem atrybutu filtru wynikowego.</span><span class="sxs-lookup"><span data-stu-id="0d995-783">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="0d995-784">IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="0d995-784">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="0d995-785">Interfejsy <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> deklarują implementację <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, która jest uruchamiana dla wszystkich wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-785">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="0d995-786">Obejmuje to wyniki akcji generowane przez:</span><span class="sxs-lookup"><span data-stu-id="0d995-786">This includes action results produced by:</span></span>

* <span data-ttu-id="0d995-787">Filtry autoryzacji i filtry zasobów, które są skracane.</span><span class="sxs-lookup"><span data-stu-id="0d995-787">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="0d995-788">Filtry wyjątków.</span><span class="sxs-lookup"><span data-stu-id="0d995-788">Exception filters.</span></span>

<span data-ttu-id="0d995-789">Na przykład następujący filtr jest zawsze uruchamiany i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) z kodem stanu *jednostki nieprzetwarzanego 422* , gdy negocjowanie zawartości kończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="0d995-789">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="0d995-790">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="0d995-790">IFilterFactory</span></span>

<span data-ttu-id="0d995-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="0d995-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="0d995-792">W związku z tym wystąpienie `IFilterFactory` może być używane jako wystąpienie `IFilterMetadata` w dowolnym miejscu w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-792">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="0d995-793">Gdy środowisko uruchomieniowe przygotowuje się do wywołania filtru, próbuje rzutować go na `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="0d995-793">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="0d995-794">Jeśli rzutowanie powiedzie się, Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> zostanie wywołana w celu utworzenia wystąpienia `IFilterMetadata`, które jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="0d995-794">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="0d995-795">Zapewnia to elastyczny projekt, ponieważ dokładny potok filtru nie musi być ustawiony jawnie podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0d995-795">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="0d995-796">`IFilterFactory` można zaimplementować przy użyciu niestandardowych implementacji atrybutów jako inne podejście do tworzenia filtrów:</span><span class="sxs-lookup"><span data-stu-id="0d995-796">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="0d995-797">Poprzedni kod może być testowany przez uruchomienie [przykładu pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="0d995-797">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="0d995-798">Wywołaj narzędzia deweloperskie F12.</span><span class="sxs-lookup"><span data-stu-id="0d995-798">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="0d995-799">Przejdź do adresu `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="0d995-799">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="0d995-800">Narzędzia programistyczne F12 wyświetlają następujące nagłówki odpowiedzi dodane przez przykładowy kod:</span><span class="sxs-lookup"><span data-stu-id="0d995-800">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="0d995-801">**autor:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="0d995-801">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="0d995-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="0d995-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="0d995-803">**wewnętrzne:** `My header`</span><span class="sxs-lookup"><span data-stu-id="0d995-803">**internal:** `My header`</span></span>

<span data-ttu-id="0d995-804">Poprzedni kod tworzy nagłówek odpowiedzi **wewnętrznej:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="0d995-804">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="0d995-805">IFilterFactory zaimplementowane dla atrybutu</span><span class="sxs-lookup"><span data-stu-id="0d995-805">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="0d995-806">Filtry implementujące `IFilterFactory` są przydatne w przypadku filtrów, które:</span><span class="sxs-lookup"><span data-stu-id="0d995-806">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="0d995-807">Nie wymagaj parametrów przekazujących.</span><span class="sxs-lookup"><span data-stu-id="0d995-807">Don't require passing parameters.</span></span>
* <span data-ttu-id="0d995-808">Mają zależności konstruktora, które muszą być wypełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="0d995-808">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="0d995-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="0d995-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="0d995-810">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="0d995-810">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="0d995-811">`CreateInstance` ładuje określony typ z kontenera usług (DI).</span><span class="sxs-lookup"><span data-stu-id="0d995-811">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="0d995-812">Poniższy kod przedstawia trzy podejścia do stosowania `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="0d995-812">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="0d995-813">W poprzednim kodzie dekorowania nazwy metodę `[SampleActionFilter]` jest preferowanym podejściem do zastosowania `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="0d995-813">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="0d995-814">Używanie oprogramowania pośredniczącego w potoku filtru</span><span class="sxs-lookup"><span data-stu-id="0d995-814">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="0d995-815">Filtry zasobów działają podobnie jak [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) w celu obsłużenia wykonywania wszystkich elementów, które są późniejsze w potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-815">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="0d995-816">Ale filtry różnią się od oprogramowania pośredniczącego w tym, że są częścią środowiska uruchomieniowego ASP.NET Core, co oznacza, że ma dostęp do ASP.NET Core kontekstu i konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="0d995-816">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="0d995-817">Aby użyć oprogramowania pośredniczącego jako filtru, należy utworzyć typ z metodą `Configure`, która określa oprogramowanie pośredniczące, które ma zostać dodane do potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="0d995-817">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="0d995-818">W poniższym przykładzie jest wykorzystywane oprogramowanie pośredniczące lokalizacyjne do ustalenia bieżącej kultury dla żądania:</span><span class="sxs-lookup"><span data-stu-id="0d995-818">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="0d995-819">Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute>, aby uruchomić oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="0d995-819">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="0d995-820">Filtry oprogramowania pośredniczącego są uruchamiane na tym samym etapie potoku filtru jako filtry zasobów przed powiązaniem modelu i po pozostałej części potoku.</span><span class="sxs-lookup"><span data-stu-id="0d995-820">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="0d995-821">Następne akcje</span><span class="sxs-lookup"><span data-stu-id="0d995-821">Next actions</span></span>

* <span data-ttu-id="0d995-822">Zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="0d995-822">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="0d995-823">Aby eksperymentować z filtrami, należy [pobrać, przetestować i zmodyfikować przykład usługi GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="0d995-823">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
