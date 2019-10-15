---
title: Filtry w ASP.NET Core
author: ardalis
description: Dowiedz się, jak działają filtry i jak korzystać z nich w ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 0c3597f24e02af40517e12a86127b140ed4fb550
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333931"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="069a6-103">Filtry w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="069a6-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="069a6-104">[Kirka Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra/)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="069a6-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="069a6-105">*Filtry* w ASP.NET Core umożliwiają uruchamianie kodu przed określonymi etapami w potoku przetwarzania żądań lub po nich.</span><span class="sxs-lookup"><span data-stu-id="069a6-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="069a6-106">Filtry wbudowane obsługują zadania takie jak:</span><span class="sxs-lookup"><span data-stu-id="069a6-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="069a6-107">Autoryzacja (zapobieganie dostępowi do zasobów, dla których użytkownik nie jest autoryzowany).</span><span class="sxs-lookup"><span data-stu-id="069a6-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="069a6-108">Buforowanie odpowiedzi (krótkie obwody potoku żądania w celu zwrócenia buforowanej odpowiedzi).</span><span class="sxs-lookup"><span data-stu-id="069a6-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="069a6-109">Filtry niestandardowe mogą być tworzone w celu obsłużenia krzyżowego rozcinania.</span><span class="sxs-lookup"><span data-stu-id="069a6-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="069a6-110">Przykłady zagadnień związanych z rozcinaniem obejmują obsługę błędów, buforowanie, konfigurację, autoryzację i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="069a6-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="069a6-111">Filtry unikają duplikowania kodu.</span><span class="sxs-lookup"><span data-stu-id="069a6-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="069a6-112">Na przykład filtr wyjątków obsługujący błędy może skonsolidować obsługę błędów.</span><span class="sxs-lookup"><span data-stu-id="069a6-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="069a6-113">Ten dokument ma zastosowanie do Razor Pages, kontrolerów interfejsu API i kontrolerów z widokami.</span><span class="sxs-lookup"><span data-stu-id="069a6-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="069a6-114">[Wyświetl lub Pobierz przykład](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="069a6-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="069a6-115">Jak działają filtry</span><span class="sxs-lookup"><span data-stu-id="069a6-115">How filters work</span></span>

<span data-ttu-id="069a6-116">Filtry są uruchamiane w *potoku wywołania akcji ASP.NET Core*, czasami określane jako *potok filtru*.</span><span class="sxs-lookup"><span data-stu-id="069a6-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="069a6-117">Potok filtru jest uruchamiany po ASP.NET Core wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="069a6-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, kierowanie oprogramowania pośredniczącego, wybór akcji i potok wywołania akcji ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="069a6-120">Typy filtrów</span><span class="sxs-lookup"><span data-stu-id="069a6-120">Filter types</span></span>

<span data-ttu-id="069a6-121">Każdy typ filtru jest wykonywany na innym etapie w potoku filtru:</span><span class="sxs-lookup"><span data-stu-id="069a6-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="069a6-122">[Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do określenia, czy użytkownik jest autoryzowany do żądania.</span><span class="sxs-lookup"><span data-stu-id="069a6-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="069a6-123">Filtry autoryzacji skracają obwód w przypadku, gdy żądanie jest nieautoryzowane.</span><span class="sxs-lookup"><span data-stu-id="069a6-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="069a6-124">[Filtry zasobów](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="069a6-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="069a6-125">Uruchom po autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="069a6-125">Run after authorization.</span></span>  
  * <span data-ttu-id="069a6-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> może uruchomić kod przed resztą potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="069a6-127">Na przykład, `OnResourceExecuting` może uruchomić kod przed powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="069a6-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="069a6-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> może uruchomić kod po zakończeniu reszty potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="069a6-129">[Filtry akcji](#action-filters) mogą uruchomić kod bezpośrednio przed i po wywołaniu pojedynczej metody akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="069a6-130">Mogą one służyć do manipulowania argumentami przekazaną do akcji i wynik zwrócony z akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="069a6-131">Filtry akcji **nie** są obsługiwane w Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="069a6-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="069a6-132">[Filtry wyjątków](#exception-filters) są używane do stosowania zasad globalnych do nieobsłużonych wyjątków, które wystąpiły przed zapisem w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="069a6-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="069a6-133">[Filtry wynikowe](#result-filters) mogą uruchamiać kod bezpośrednio przed i po wykonaniu poszczególnych wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="069a6-134">Są one uruchamiane tylko wtedy, gdy metoda akcji została wykonana pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="069a6-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="069a6-135">Są one przydatne dla logiki, która musi być w trakcie wyświetlania lub wykonywania w programie formatującego.</span><span class="sxs-lookup"><span data-stu-id="069a6-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="069a6-136">Na poniższym diagramie przedstawiono sposób, w jaki typy filtrów współdziałają w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Żądanie jest przetwarzane przez filtry autoryzacji, filtry zasobów, powiązania modelu, filtry akcji, wykonywanie akcji i konwersję wyników akcji, filtry wyjątków, filtry wynikowe i wykonywanie wyniku.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="069a6-139">Implementacja</span><span class="sxs-lookup"><span data-stu-id="069a6-139">Implementation</span></span>

<span data-ttu-id="069a6-140">Filtry obsługują implementacje synchroniczne i asynchroniczne za pomocą różnych definicji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="069a6-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="069a6-141">Filtry synchroniczne mogą uruchomić kod przed (`On-Stage-Executing`) i po nim (`On-Stage-Executed`) ich etap potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="069a6-142">Na przykład, `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="069a6-143">`OnActionExecuted` jest wywoływana po powrocie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="069a6-144">Filtry asynchroniczne definiują metodę `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="069a6-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="069a6-145">W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który wykonuje metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="069a6-146">Każda metoda `On-Stage-ExecutionAsync` przyjmuje `FilterType-ExecutionDelegate`, który wykonuje etap potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="069a6-147">Wiele etapów filtrowania</span><span class="sxs-lookup"><span data-stu-id="069a6-147">Multiple filter stages</span></span>

<span data-ttu-id="069a6-148">Interfejsy dla wielu etapów filtru można zaimplementować w pojedynczej klasie.</span><span class="sxs-lookup"><span data-stu-id="069a6-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="069a6-149">Na przykład Klasa <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementuje `IActionFilter`, `IResultFilter` i ich odpowiedniki asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="069a6-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="069a6-150">Implementowanie synchronicznej lub asynchronicznej wersji interfejsu filtru **, a** **nie** obu.</span><span class="sxs-lookup"><span data-stu-id="069a6-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="069a6-151">Środowisko uruchomieniowe najpierw sprawdza, czy filtr implementuje interfejs asynchroniczny, a jeśli tak, wywołuje to.</span><span class="sxs-lookup"><span data-stu-id="069a6-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="069a6-152">Jeśli nie, wywołuje metody interfejsu synchronicznego.</span><span class="sxs-lookup"><span data-stu-id="069a6-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="069a6-153">Jeśli zarówno interfejsy asynchroniczne, jak i synchroniczne są zaimplementowane w jednej klasie, wywoływana jest tylko Metoda async.</span><span class="sxs-lookup"><span data-stu-id="069a6-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="069a6-154">W przypadku używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, przesłaniają tylko metody synchroniczne lub metodę asynchroniczną dla każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="069a6-155">Wbudowane atrybuty filtru</span><span class="sxs-lookup"><span data-stu-id="069a6-155">Built-in filter attributes</span></span>

<span data-ttu-id="069a6-156">ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które można podklasować i dostosowywać.</span><span class="sxs-lookup"><span data-stu-id="069a6-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="069a6-157">Na przykład poniższy filtr wynikowy dodaje nagłówek do odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="069a6-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="069a6-158">Atrybuty umożliwiają filtrom akceptowanie argumentów, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="069a6-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="069a6-159">Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:</span><span class="sxs-lookup"><span data-stu-id="069a6-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="069a6-160">Kilka interfejsów filtrów ma odpowiednie atrybuty, które mogą być używane jako klasy bazowe dla implementacji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="069a6-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="069a6-161">Atrybuty filtru:</span><span class="sxs-lookup"><span data-stu-id="069a6-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="069a6-162">Zakresy filtrów i kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="069a6-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="069a6-163">Filtr można dodać do potoku w jednym z trzech *zakresów*:</span><span class="sxs-lookup"><span data-stu-id="069a6-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="069a6-164">Użycie atrybutu w akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="069a6-165">Używanie atrybutu na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="069a6-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="069a6-166">Globalnie dla wszystkich kontrolerów i akcji, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="069a6-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="069a6-167">Poprzedni kod dodaje trzy filtry globalnie przy użyciu kolekcji [MvcOptions. filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) .</span><span class="sxs-lookup"><span data-stu-id="069a6-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="069a6-168">Domyślna kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="069a6-168">Default order of execution</span></span>

<span data-ttu-id="069a6-169">Jeśli istnieje wiele filtrów dla określonego etapu potoku, zakres określa domyślną kolejność wykonywania filtrowania.</span><span class="sxs-lookup"><span data-stu-id="069a6-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="069a6-170">Filtry globalne Otocz filtry klas, które z kolei filtrują metody przestrzenne.</span><span class="sxs-lookup"><span data-stu-id="069a6-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="069a6-171">W wyniku zagnieżdżania filtrów, *po* kodzie filtrów działa w odwrotnej kolejności *przed* kodem.</span><span class="sxs-lookup"><span data-stu-id="069a6-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="069a6-172">Sekwencja filtru:</span><span class="sxs-lookup"><span data-stu-id="069a6-172">The filter sequence:</span></span>

* <span data-ttu-id="069a6-173">*Przed* kodem filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="069a6-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="069a6-174">*Przed* kodem filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="069a6-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="069a6-175">Filtr *przed* kodem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="069a6-176">*Po* kodzie metody akcji filtry.</span><span class="sxs-lookup"><span data-stu-id="069a6-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="069a6-177">*Po* kodzie filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="069a6-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="069a6-178">Kod *po* filtrach globalnych.</span><span class="sxs-lookup"><span data-stu-id="069a6-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="069a6-179">Poniższy przykład ilustruje kolejność, w której metody filtrowania są wywoływane dla synchronicznych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="069a6-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="069a6-180">Sequence</span></span> | <span data-ttu-id="069a6-181">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="069a6-181">Filter scope</span></span> | <span data-ttu-id="069a6-182">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="069a6-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="069a6-183">1</span><span class="sxs-lookup"><span data-stu-id="069a6-183">1</span></span> | <span data-ttu-id="069a6-184">Globalne</span><span class="sxs-lookup"><span data-stu-id="069a6-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="069a6-185">2</span><span class="sxs-lookup"><span data-stu-id="069a6-185">2</span></span> | <span data-ttu-id="069a6-186">Kontroler</span><span class="sxs-lookup"><span data-stu-id="069a6-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="069a6-187">3</span><span class="sxs-lookup"><span data-stu-id="069a6-187">3</span></span> | <span data-ttu-id="069a6-188">Metoda</span><span class="sxs-lookup"><span data-stu-id="069a6-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="069a6-189">4</span><span class="sxs-lookup"><span data-stu-id="069a6-189">4</span></span> | <span data-ttu-id="069a6-190">Metoda</span><span class="sxs-lookup"><span data-stu-id="069a6-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="069a6-191">5</span><span class="sxs-lookup"><span data-stu-id="069a6-191">5</span></span> | <span data-ttu-id="069a6-192">Kontroler</span><span class="sxs-lookup"><span data-stu-id="069a6-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="069a6-193">6</span><span class="sxs-lookup"><span data-stu-id="069a6-193">6</span></span> | <span data-ttu-id="069a6-194">Globalne</span><span class="sxs-lookup"><span data-stu-id="069a6-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="069a6-195">Ta sekwencja pokazuje:</span><span class="sxs-lookup"><span data-stu-id="069a6-195">This sequence shows:</span></span>

* <span data-ttu-id="069a6-196">Filtr metody jest zagnieżdżony w obrębie filtru kontrolera.</span><span class="sxs-lookup"><span data-stu-id="069a6-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="069a6-197">Filtr kontrolera jest zagnieżdżony w obrębie filtru globalnego.</span><span class="sxs-lookup"><span data-stu-id="069a6-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="069a6-198">Kontrolery i filtry na poziomie strony Razor</span><span class="sxs-lookup"><span data-stu-id="069a6-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="069a6-199">Każdy kontroler, który dziedziczy z klasy bazowej <xref:Microsoft.AspNetCore.Mvc.Controller> obejmuje metody [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)i [Controller. OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
 @ no__t-5 metod.</span><span class="sxs-lookup"><span data-stu-id="069a6-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="069a6-200">Te metody:</span><span class="sxs-lookup"><span data-stu-id="069a6-200">These methods:</span></span>

* <span data-ttu-id="069a6-201">Zawiń filtry, które są uruchamiane dla danego działania.</span><span class="sxs-lookup"><span data-stu-id="069a6-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="069a6-202">`OnActionExecuting` jest wywoływana przed dowolnym filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="069a6-203">`OnActionExecuted` jest wywoływana po wszystkich filtrach akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="069a6-204">`OnActionExecutionAsync` jest wywoływana przed dowolnym filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="069a6-205">Kod w filtrze po `next` jest uruchamiany po metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="069a6-206">Na przykład w przykładzie pobierania `MySampleActionFilter` jest zastosowany globalnie podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="069a6-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="069a6-207">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="069a6-207">The `TestController`:</span></span>

* <span data-ttu-id="069a6-208">Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do akcji `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="069a6-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="069a6-209">Zastępuje `OnActionExecuting` i `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="069a6-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="069a6-210">Przechodzenie do `https://localhost:5001/Test/FilterTest2` uruchamia następujący kod:</span><span class="sxs-lookup"><span data-stu-id="069a6-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="069a6-211">Aby uzyskać Razor Pages, zobacz [implementowanie filtrów stron Razor przez zastępowanie metod filtrowania](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="069a6-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="069a6-212">Zastępowanie kolejności domyślnej</span><span class="sxs-lookup"><span data-stu-id="069a6-212">Overriding the default order</span></span>

<span data-ttu-id="069a6-213">Domyślną sekwencję wykonywania można zastąpić, implementując <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="069a6-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="069a6-214">`IOrderedFilter` uwidacznia Właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, która ma pierwszeństwo przed zakresem w celu określenia kolejności wykonywania.</span><span class="sxs-lookup"><span data-stu-id="069a6-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="069a6-215">Filtr o niższej wartości `Order`:</span><span class="sxs-lookup"><span data-stu-id="069a6-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="069a6-216">Uruchamia *przed* kodem przed filtrem o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="069a6-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="069a6-217">Uruchamia *po* kodzie po filtrze o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="069a6-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="069a6-218">Właściwość `Order` można ustawić przy użyciu parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="069a6-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="069a6-219">Należy wziąć pod uwagę te same 3 filtry akcji, które przedstawiono w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="069a6-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="069a6-220">Jeśli właściwość `Order` kontrolera i filtry globalne mają odpowiednio wartość 1 i 2, kolejność wykonywania zostanie odwrócona.</span><span class="sxs-lookup"><span data-stu-id="069a6-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="069a6-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="069a6-221">Sequence</span></span> | <span data-ttu-id="069a6-222">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="069a6-222">Filter scope</span></span> | <span data-ttu-id="069a6-223">Właściwość `Order`</span><span class="sxs-lookup"><span data-stu-id="069a6-223">`Order` property</span></span> | <span data-ttu-id="069a6-224">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="069a6-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="069a6-225">1</span><span class="sxs-lookup"><span data-stu-id="069a6-225">1</span></span> | <span data-ttu-id="069a6-226">Metoda</span><span class="sxs-lookup"><span data-stu-id="069a6-226">Method</span></span> | <span data-ttu-id="069a6-227">0</span><span class="sxs-lookup"><span data-stu-id="069a6-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="069a6-228">2</span><span class="sxs-lookup"><span data-stu-id="069a6-228">2</span></span> | <span data-ttu-id="069a6-229">Kontroler</span><span class="sxs-lookup"><span data-stu-id="069a6-229">Controller</span></span> | <span data-ttu-id="069a6-230">1</span><span class="sxs-lookup"><span data-stu-id="069a6-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="069a6-231">3</span><span class="sxs-lookup"><span data-stu-id="069a6-231">3</span></span> | <span data-ttu-id="069a6-232">Globalne</span><span class="sxs-lookup"><span data-stu-id="069a6-232">Global</span></span> | <span data-ttu-id="069a6-233">2</span><span class="sxs-lookup"><span data-stu-id="069a6-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="069a6-234">4</span><span class="sxs-lookup"><span data-stu-id="069a6-234">4</span></span> | <span data-ttu-id="069a6-235">Globalne</span><span class="sxs-lookup"><span data-stu-id="069a6-235">Global</span></span> | <span data-ttu-id="069a6-236">2</span><span class="sxs-lookup"><span data-stu-id="069a6-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="069a6-237">5</span><span class="sxs-lookup"><span data-stu-id="069a6-237">5</span></span> | <span data-ttu-id="069a6-238">Kontroler</span><span class="sxs-lookup"><span data-stu-id="069a6-238">Controller</span></span> | <span data-ttu-id="069a6-239">1</span><span class="sxs-lookup"><span data-stu-id="069a6-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="069a6-240">6</span><span class="sxs-lookup"><span data-stu-id="069a6-240">6</span></span> | <span data-ttu-id="069a6-241">Metoda</span><span class="sxs-lookup"><span data-stu-id="069a6-241">Method</span></span> | <span data-ttu-id="069a6-242">0</span><span class="sxs-lookup"><span data-stu-id="069a6-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="069a6-243">Właściwość `Order` przesłania zakres podczas określania kolejności, w której są uruchamiane filtry.</span><span class="sxs-lookup"><span data-stu-id="069a6-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="069a6-244">Filtry są sortowane najpierw według kolejności, a następnie zakres jest używany do przerwania powiązań.</span><span class="sxs-lookup"><span data-stu-id="069a6-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="069a6-245">Wszystkie wbudowane filtry implementują `IOrderedFilter` i ustawiają domyślną `Order` wartość 0.</span><span class="sxs-lookup"><span data-stu-id="069a6-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="069a6-246">W przypadku filtrów wbudowanych zakres określa kolejność, chyba że `Order` ma ustawioną wartość różną od zera.</span><span class="sxs-lookup"><span data-stu-id="069a6-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="069a6-247">Anulowanie i krótkie obwody</span><span class="sxs-lookup"><span data-stu-id="069a6-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="069a6-248">Potok filtru może być skrócony przez ustawienie właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> w parametrze <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> dostarczonym do metody Filter.</span><span class="sxs-lookup"><span data-stu-id="069a6-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="069a6-249">Na przykład poniższy filtr zasobów uniemożliwia wykonanie pozostałej części potoku:</span><span class="sxs-lookup"><span data-stu-id="069a6-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="069a6-250">W poniższym kodzie filtr `ShortCircuitingResourceFilter` i `AddHeader` ma wartość docelową metody akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="069a6-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="069a6-251">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="069a6-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="069a6-252">Uruchamia się najpierw, ponieważ jest to filtr zasobów, a `AddHeader` to filtr akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="069a6-253">Krótkie obwody pozostała część potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="069a6-254">W związku z tym filtr `AddHeader` nigdy nie działa dla akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="069a6-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="069a6-255">Takie zachowanie będzie takie samo, jeśli oba filtry zostały zastosowane na poziomie metody akcji, pod warunkiem, że `ShortCircuitingResourceFilter` działały jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="069a6-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="069a6-256">@No__t-0 działa najpierw ze względu na jego typ filtru lub jawne użycie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="069a6-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="069a6-257">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="069a6-257">Dependency injection</span></span>

<span data-ttu-id="069a6-258">Filtry można dodawać według typu lub według wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="069a6-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="069a6-259">W przypadku dodania wystąpienia to wystąpienie jest używane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="069a6-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="069a6-260">Jeśli typ zostanie dodany, jego typ jest aktywowany.</span><span class="sxs-lookup"><span data-stu-id="069a6-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="069a6-261">Filtr aktywowany przez typ oznacza:</span><span class="sxs-lookup"><span data-stu-id="069a6-261">A type-activated filter means:</span></span>

* <span data-ttu-id="069a6-262">Wystąpienie jest tworzone dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="069a6-262">An instance is created for each request.</span></span>
* <span data-ttu-id="069a6-263">Wszystkie zależności konstruktora są wypełniane przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="069a6-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="069a6-264">Filtry zaimplementowane jako atrybuty i dodawane bezpośrednio do klas kontrolera lub metod akcji nie mogą mieć zależności konstruktorów udostępnianych przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="069a6-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="069a6-265">Zależności konstruktora nie mogą być dostarczone przez DI, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="069a6-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="069a6-266">Atrybuty muszą mieć podane parametry konstruktora, w których są stosowane.</span><span class="sxs-lookup"><span data-stu-id="069a6-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="069a6-267">Jest to ograniczenie działania atrybutów.</span><span class="sxs-lookup"><span data-stu-id="069a6-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="069a6-268">Następujące filtry obsługują zależności konstruktora dostarczone z elementów DI:</span><span class="sxs-lookup"><span data-stu-id="069a6-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="069a6-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowane dla atrybutu.</span><span class="sxs-lookup"><span data-stu-id="069a6-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="069a6-270">Powyższe filtry można zastosować do kontrolera lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="069a6-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="069a6-271">Rejestratory są dostępne z programu DI.</span><span class="sxs-lookup"><span data-stu-id="069a6-271">Loggers are available from DI.</span></span> <span data-ttu-id="069a6-272">Należy jednak unikać tworzenia i używania filtrów wyłącznie w celach rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="069a6-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="069a6-273">[Wbudowane rejestrowanie struktury](xref:fundamentals/logging/index) zazwyczaj zapewnia, co jest potrzebne do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="069a6-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="069a6-274">Rejestrowanie dodane do filtrów:</span><span class="sxs-lookup"><span data-stu-id="069a6-274">Logging added to filters:</span></span>

* <span data-ttu-id="069a6-275">Należy skoncentrować się na problemach z domeną biznesową lub działaniu specyficznym dla filtra.</span><span class="sxs-lookup"><span data-stu-id="069a6-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="069a6-276">**Nie** należy rejestrować akcji ani innych zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="069a6-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="069a6-277">Wbudowane filtry akcje dziennika i zdarzenia struktury.</span><span class="sxs-lookup"><span data-stu-id="069a6-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="069a6-278">Servicefilterattribute</span><span class="sxs-lookup"><span data-stu-id="069a6-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="069a6-279">Typy implementacji filtru usługi są zarejestrowane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="069a6-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="069a6-280">@No__t-0 Pobiera wystąpienie filtru z DI.</span><span class="sxs-lookup"><span data-stu-id="069a6-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="069a6-281">Poniższy kod ilustruje `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="069a6-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="069a6-282">W poniższym kodzie `AddHeaderResultServiceFilter` jest dodawany do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="069a6-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="069a6-283">W poniższym kodzie atrybut `ServiceFilter` Pobiera wystąpienie filtru `AddHeaderResultServiceFilter` z DI:</span><span class="sxs-lookup"><span data-stu-id="069a6-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="069a6-284">W przypadku używania `ServiceFilterAttribute` ustawienie [Servicefilterattribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="069a6-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="069a6-285">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="069a6-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="069a6-286">Środowisko uruchomieniowe ASP.NET Core nie gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="069a6-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="069a6-287">Zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="069a6-288">Filtr nie zostanie ponownie żądany z kontenera DI w pewnym momencie.</span><span class="sxs-lookup"><span data-stu-id="069a6-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="069a6-289">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="069a6-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="069a6-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="069a6-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="069a6-291">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> służącą do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="069a6-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="069a6-292">`CreateInstance` wczytuje określony typ z DI.</span><span class="sxs-lookup"><span data-stu-id="069a6-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="069a6-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="069a6-293">TypeFilterAttribute</span></span>

<span data-ttu-id="069a6-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> jest podobna do <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie jest rozpoznawany bezpośrednio w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="069a6-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="069a6-295">Tworzy wystąpienie tego typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="069a6-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="069a6-296">Ponieważ typy `TypeFilterAttribute` nie są rozpoznawane bezpośrednio w kontenerze DI:</span><span class="sxs-lookup"><span data-stu-id="069a6-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="069a6-297">Typy, do których odwołuje się `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="069a6-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="069a6-298">Ich zależności są spełnione przez kontener DI.</span><span class="sxs-lookup"><span data-stu-id="069a6-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="069a6-299">`TypeFilterAttribute` opcjonalnie może akceptować argumenty konstruktora dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="069a6-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="069a6-300">W przypadku używania `TypeFilterAttribute` ustawienie [TypeFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="069a6-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="069a6-301">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="069a6-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="069a6-302">Środowisko uruchomieniowe ASP.NET Core nie zapewnia żadnych gwarancji, że zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="069a6-303">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="069a6-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="069a6-304">Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="069a6-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="069a6-305">Filtry autoryzacji</span><span class="sxs-lookup"><span data-stu-id="069a6-305">Authorization filters</span></span>

<span data-ttu-id="069a6-306">Filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="069a6-306">Authorization filters:</span></span>

* <span data-ttu-id="069a6-307">Są pierwszymi filtrami uruchamianymi w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="069a6-308">Kontrola dostępu do metod akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-308">Control access to action methods.</span></span>
* <span data-ttu-id="069a6-309">Ma metodę Before, ale nie po metodzie.</span><span class="sxs-lookup"><span data-stu-id="069a6-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="069a6-310">Niestandardowe filtry autoryzacji wymagają niestandardowej struktury autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="069a6-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="069a6-311">Preferuj skonfigurowanie zasad autoryzacji lub zapisanie niestandardowych zasad autoryzacji przez zapisanie filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="069a6-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="069a6-312">Wbudowany filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="069a6-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="069a6-313">Wywołuje system autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="069a6-313">Calls the authorization system.</span></span>
* <span data-ttu-id="069a6-314">Nie zezwala na żądania.</span><span class="sxs-lookup"><span data-stu-id="069a6-314">Does not authorize requests.</span></span>

<span data-ttu-id="069a6-315">**Nie** zgłaszaj wyjątków w ramach filtrów autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="069a6-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="069a6-316">Wyjątek nie zostanie obsłużony.</span><span class="sxs-lookup"><span data-stu-id="069a6-316">The exception will not be handled.</span></span>
* <span data-ttu-id="069a6-317">Filtry wyjątków nie będą obsługiwać wyjątku.</span><span class="sxs-lookup"><span data-stu-id="069a6-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="069a6-318">Rozważ wygenerowanie wyzwania w przypadku wystąpienia wyjątku w filtrze autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="069a6-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="069a6-319">Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="069a6-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="069a6-320">Filtry zasobów</span><span class="sxs-lookup"><span data-stu-id="069a6-320">Resource filters</span></span>

<span data-ttu-id="069a6-321">Filtry zasobów:</span><span class="sxs-lookup"><span data-stu-id="069a6-321">Resource filters:</span></span>

* <span data-ttu-id="069a6-322">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="069a6-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="069a6-323">Wykonanie powoduje Zawijanie większości potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="069a6-324">Tylko [filtry autoryzacji](#authorization-filters) są uruchamiane przed filtrami zasobów.</span><span class="sxs-lookup"><span data-stu-id="069a6-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="069a6-325">Filtry zasobów są przydatne do krótkiego obwodu większości potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="069a6-326">Na przykład filtr buforowania może uniknąć pozostałej części potoku w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="069a6-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="069a6-327">Przykłady filtru zasobów:</span><span class="sxs-lookup"><span data-stu-id="069a6-327">Resource filter examples:</span></span>

* <span data-ttu-id="069a6-328">[Filtr zasobów o krótkim obwodzie](#short-circuiting-resource-filter) pokazano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="069a6-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="069a6-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="069a6-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="069a6-330">Uniemożliwia powiązanie modelu z dostępem do danych formularza.</span><span class="sxs-lookup"><span data-stu-id="069a6-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="069a6-331">Służy do przekazywania dużych plików w celu uniemożliwienia odczytywania danych formularza do pamięci.</span><span class="sxs-lookup"><span data-stu-id="069a6-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="069a6-332">Filtry akcji</span><span class="sxs-lookup"><span data-stu-id="069a6-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="069a6-333">Filtry akcji **nie** mają zastosowania do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="069a6-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="069a6-334">Razor Pages obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="069a6-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="069a6-335">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="069a6-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="069a6-336">Filtry akcji:</span><span class="sxs-lookup"><span data-stu-id="069a6-336">Action filters:</span></span>

* <span data-ttu-id="069a6-337">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="069a6-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="069a6-338">Ich wykonanie otacza wykonywanie metod akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="069a6-339">Poniższy kod przedstawia przykładowy filtr akcji:</span><span class="sxs-lookup"><span data-stu-id="069a6-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="069a6-340">@No__t-0 zawiera następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="069a6-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="069a6-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — umożliwia odczytywanie danych wejściowych metody akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="069a6-342"><xref:Microsoft.AspNetCore.Mvc.Controller> — umożliwia manipulowanie wystąpieniem kontrolera.</span><span class="sxs-lookup"><span data-stu-id="069a6-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="069a6-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> — ustawienie `Result` krótkie obwody wykonywanie metody akcji i kolejnych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="069a6-344">Zgłaszanie wyjątku w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="069a6-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="069a6-345">Zapobiega uruchamianiu kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="069a6-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="069a6-346">W przeciwieństwie do ustawienia `Result`, jest traktowany jako błąd, a nie pomyślny wynik.</span><span class="sxs-lookup"><span data-stu-id="069a6-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="069a6-347">@No__t-0 zapewnia `Controller` i `Result` oraz następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="069a6-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="069a6-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-true, jeśli wykonanie akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="069a6-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="069a6-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-wartość null, jeśli filtr akcji lub poprzednio uruchomionego elementu zgłosił wyjątek.</span><span class="sxs-lookup"><span data-stu-id="069a6-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="069a6-350">Ustawienie tej właściwości na wartość null:</span><span class="sxs-lookup"><span data-stu-id="069a6-350">Setting this property to null:</span></span>

  * <span data-ttu-id="069a6-351">Skutecznie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="069a6-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="069a6-352">`Result` jest wykonywany tak, jakby został zwrócony z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="069a6-353">Dla `IAsyncActionFilter` wywołania <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="069a6-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="069a6-354">Wykonuje wszystkie kolejne filtry akcji i metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="069a6-355">Zwraca `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="069a6-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="069a6-356">Do krótkiego obwodu Przypisz <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> do wystąpienia wynikowego i nie wywołuj `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="069a6-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="069a6-357">Struktura zawiera abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="069a6-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="069a6-358">Filtr akcji `OnActionExecuting` może służyć do:</span><span class="sxs-lookup"><span data-stu-id="069a6-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="069a6-359">Zweryfikuj stan modelu.</span><span class="sxs-lookup"><span data-stu-id="069a6-359">Validate model state.</span></span>
* <span data-ttu-id="069a6-360">Zwróć błąd, jeśli stan jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="069a6-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="069a6-361">Metoda `OnActionExecuted` jest uruchamiana po metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="069a6-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="069a6-362">I mogą przeglądać wyniki akcji przy użyciu właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> i manipulować nimi.</span><span class="sxs-lookup"><span data-stu-id="069a6-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="069a6-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> jest ustawiona na wartość true, jeśli wykonywanie akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="069a6-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="069a6-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> ma ustawioną wartość różną od null, jeśli akcja lub kolejny filtr akcji wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="069a6-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="069a6-365">Ustawienie `Exception` na null:</span><span class="sxs-lookup"><span data-stu-id="069a6-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="069a6-366">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="069a6-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="069a6-367">`ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="069a6-368">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="069a6-368">Exception filters</span></span>

<span data-ttu-id="069a6-369">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="069a6-369">Exception filters:</span></span>

* <span data-ttu-id="069a6-370">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="069a6-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="069a6-371">Może służyć do implementowania typowych zasad obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="069a6-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="069a6-372">Następujący przykładowy filtr wyjątku używa niestandardowego widoku błędów, aby wyświetlić szczegóły dotyczące wyjątków występujących podczas opracowywania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="069a6-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="069a6-373">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="069a6-373">Exception filters:</span></span>

* <span data-ttu-id="069a6-374">Nie mam zdarzeń przed i po.</span><span class="sxs-lookup"><span data-stu-id="069a6-374">Don't have before and after events.</span></span>
* <span data-ttu-id="069a6-375">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="069a6-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="069a6-376">Obsłuż Nieobsłużone wyjątki występujące na stronie lub w tworzeniu kontrolera, [powiązania modelu](xref:mvc/models/model-binding), filtrach akcji lub metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="069a6-377">**Nie** należy przechwytywać wyjątków występujących w filtrach zasobów, filtrach wyników ani wykonywaniu wyniku MVC.</span><span class="sxs-lookup"><span data-stu-id="069a6-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="069a6-378">Aby obsłużyć wyjątek, ustaw właściwość <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> na `true` lub Zapisz odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="069a6-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="069a6-379">Spowoduje to zatrzymanie propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="069a6-379">This stops propagation of the exception.</span></span> <span data-ttu-id="069a6-380">Filtr wyjątku nie może przekształcić wyjątku w "powodzenie".</span><span class="sxs-lookup"><span data-stu-id="069a6-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="069a6-381">Można to zrobić tylko dla filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-381">Only an action filter can do that.</span></span>

<span data-ttu-id="069a6-382">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="069a6-382">Exception filters:</span></span>

* <span data-ttu-id="069a6-383">Są dobre dla wyjątków zalewkowania występujących w ramach akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="069a6-384">Nie są tak elastyczne jak błędy obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="069a6-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="069a6-385">Preferuj oprogramowanie pośredniczące dla obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="069a6-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="069a6-386">Użyj filtrów wyjątków tylko w przypadku, gdy obsługa błędów *różni* się w zależności od tego, która metoda akcji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="069a6-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="069a6-387">Na przykład aplikacja może mieć metody akcji zarówno dla punktów końcowych interfejsu API, jak i dla widoków/HTML.</span><span class="sxs-lookup"><span data-stu-id="069a6-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="069a6-388">Punkty końcowe interfejsu API mogą zwracać informacje o błędach jako dane JSON, podczas gdy akcje oparte na widoku mogą zwrócić stronę błędu jako kod HTML.</span><span class="sxs-lookup"><span data-stu-id="069a6-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="069a6-389">Filtry wyników</span><span class="sxs-lookup"><span data-stu-id="069a6-389">Result filters</span></span>

<span data-ttu-id="069a6-390">Filtry wyników:</span><span class="sxs-lookup"><span data-stu-id="069a6-390">Result filters:</span></span>

* <span data-ttu-id="069a6-391">Zaimplementuj interfejs:</span><span class="sxs-lookup"><span data-stu-id="069a6-391">Implement an interface:</span></span>
  * <span data-ttu-id="069a6-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="069a6-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="069a6-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="069a6-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="069a6-394">Ich wykonanie otacza wykonywanie wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="069a6-395">IResultFilter i IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="069a6-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="069a6-396">Poniższy kod pokazuje filtr wynikowy, który dodaje nagłówek HTTP:</span><span class="sxs-lookup"><span data-stu-id="069a6-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="069a6-397">Rodzaj wykonywanego wyniku zależy od akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="069a6-398">Akcja zwracająca widok obejmie wszystkie przetwarzanie Razor w ramach <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="069a6-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="069a6-399">Metoda interfejsu API może wykonywać pewne serializacji w ramach wykonywania wyniku.</span><span class="sxs-lookup"><span data-stu-id="069a6-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="069a6-400">Dowiedz się więcej o [wynikach akcji](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="069a6-400">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="069a6-401">Filtry wynikowe są wykonywane tylko wtedy, gdy akcja lub filtr akcji generuje wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-401">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="069a6-402">Filtry wynikowe nie są wykonywane, gdy:</span><span class="sxs-lookup"><span data-stu-id="069a6-402">Result filters are not executed when:</span></span>

* <span data-ttu-id="069a6-403">Filtr autoryzacji lub filtr zasobów jest krótki obwody potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-403">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="069a6-404">Filtr wyjątku obsługuje wyjątek przez wygenerowanie wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-404">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="069a6-405">Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> może skrócić wykonywanie wyniku akcji i kolejnych filtrów wyników przez ustawienie <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> na `true`.</span><span class="sxs-lookup"><span data-stu-id="069a6-405">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="069a6-406">Zapisuj w obiekcie Response podczas krótkiego obwodu, aby uniknąć generowania pustej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="069a6-406">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="069a6-407">Zgłaszanie wyjątku w `IResultFilter.OnResultExecuting` spowoduje:</span><span class="sxs-lookup"><span data-stu-id="069a6-407">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="069a6-408">Zapobiegaj wykonaniu wyniku akcji i kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="069a6-408">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="069a6-409">Być traktowany jako niepowodzenie, a nie wynikowy pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="069a6-409">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="069a6-410">Po uruchomieniu metody <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> odpowiedź została już wysłana do klienta.</span><span class="sxs-lookup"><span data-stu-id="069a6-410">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="069a6-411">Jeśli odpowiedź została już wysłana do klienta, nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="069a6-411">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="069a6-412">`ResultExecutedContext.Canceled` jest ustawiona na `true`, jeśli wykonywanie wyniku akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="069a6-412">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="069a6-413">`ResultExecutedContext.Exception` ma ustawioną wartość różną od null, jeśli wynik akcji lub kolejny filtr wynikowy wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="069a6-413">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="069a6-414">Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia ponowne zgłoszenie wyjątku przez ASP.NET Core w dalszej części potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-414">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="069a6-415">Nie istnieje niezawodny sposób zapisu danych do odpowiedzi podczas obsługi wyjątku w filtrze wynikowym.</span><span class="sxs-lookup"><span data-stu-id="069a6-415">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="069a6-416">Jeśli nagłówki zostały opróżnione do klienta, gdy wynikiem akcji zgłasza wyjątek, nie istnieje niezawodny mechanizm wysyłania kodu błędu.</span><span class="sxs-lookup"><span data-stu-id="069a6-416">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="069a6-417">W przypadku <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter> wywołanie do `await next` w <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie kolejne filtry wynikowe i wynik działania.</span><span class="sxs-lookup"><span data-stu-id="069a6-417">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="069a6-418">Do krótkiego obwodu Ustaw [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) na `true` i nie wywołuj `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="069a6-418">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="069a6-419">Struktura zawiera abstrakcyjną `ResultFilterAttribute`, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="069a6-419">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="069a6-420">Przedstawiona wcześniej Klasa [Addheaderattribute](#add-header-attribute) jest przykładem atrybutu filtru wynikowego.</span><span class="sxs-lookup"><span data-stu-id="069a6-420">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="069a6-421">IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="069a6-421">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="069a6-422">Interfejsy <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> deklarują implementację <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, która jest uruchamiana dla wszystkich wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-422">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="069a6-423">Obejmuje to wyniki akcji generowane przez:</span><span class="sxs-lookup"><span data-stu-id="069a6-423">This includes action results produced by:</span></span>

* <span data-ttu-id="069a6-424">Filtry autoryzacji i filtry zasobów, które są skracane.</span><span class="sxs-lookup"><span data-stu-id="069a6-424">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="069a6-425">Filtry wyjątków.</span><span class="sxs-lookup"><span data-stu-id="069a6-425">Exception filters.</span></span>

<span data-ttu-id="069a6-426">Na przykład następujący filtr zawsze jest uruchamiany i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) z kodem stanu *jednostki nieprzetwarzanych 422* , gdy negocjowanie zawartości kończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="069a6-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="069a6-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="069a6-427">IFilterFactory</span></span>

<span data-ttu-id="069a6-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="069a6-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="069a6-429">W związku z tym wystąpienie `IFilterFactory` może być używane jako wystąpienie `IFilterMetadata` w dowolnym miejscu w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="069a6-430">Gdy środowisko uruchomieniowe przygotowuje się do wywołania filtru, próbuje rzutować go na `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="069a6-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="069a6-431">Jeśli rzutowanie powiedzie się, Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> zostanie wywołana w celu utworzenia wystąpienia `IFilterMetadata`, które jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="069a6-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="069a6-432">Zapewnia to elastyczny projekt, ponieważ dokładny potok filtru nie musi być ustawiony jawnie podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="069a6-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="069a6-433">`IFilterFactory` można zaimplementować przy użyciu niestandardowych implementacji atrybutów jako inne podejście do tworzenia filtrów:</span><span class="sxs-lookup"><span data-stu-id="069a6-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="069a6-434">Poprzedni kod może być testowany przez uruchomienie [przykładu pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="069a6-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="069a6-435">Wywołaj narzędzia deweloperskie F12.</span><span class="sxs-lookup"><span data-stu-id="069a6-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="069a6-436">Przejdź do `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="069a6-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="069a6-437">Narzędzia programistyczne F12 wyświetlają następujące nagłówki odpowiedzi dodane przez przykładowy kod:</span><span class="sxs-lookup"><span data-stu-id="069a6-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="069a6-438">**autor:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="069a6-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="069a6-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="069a6-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="069a6-440">**wewnętrzne:** `My header`</span><span class="sxs-lookup"><span data-stu-id="069a6-440">**internal:** `My header`</span></span>

<span data-ttu-id="069a6-441">Poprzedni kod tworzy nagłówek odpowiedzi **wewnętrznej:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="069a6-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="069a6-442">IFilterFactory zaimplementowane dla atrybutu</span><span class="sxs-lookup"><span data-stu-id="069a6-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="069a6-443">Filtry implementujące `IFilterFactory` są przydatne w przypadku filtrów, które:</span><span class="sxs-lookup"><span data-stu-id="069a6-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="069a6-444">Nie wymagaj parametrów przekazujących.</span><span class="sxs-lookup"><span data-stu-id="069a6-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="069a6-445">Mają zależności konstruktora, które muszą być wypełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="069a6-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="069a6-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="069a6-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="069a6-447">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> służącą do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="069a6-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="069a6-448">`CreateInstance` ładuje określony typ z kontenera usług (DI).</span><span class="sxs-lookup"><span data-stu-id="069a6-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="069a6-449">Poniższy kod przedstawia trzy podejścia do stosowania `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="069a6-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="069a6-450">W poprzednim kodzie dekorowania nazwy metodę z `[SampleActionFilter]` jest preferowanym podejściem do zastosowania `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="069a6-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="069a6-451">Używanie oprogramowania pośredniczącego w potoku filtru</span><span class="sxs-lookup"><span data-stu-id="069a6-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="069a6-452">Filtry zasobów działają podobnie jak [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) w celu obsłużenia wykonywania wszystkich elementów, które są późniejsze w potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="069a6-453">Ale filtry różnią się od oprogramowania pośredniczącego w tym, że są częścią środowiska uruchomieniowego ASP.NET Core, co oznacza, że ma dostęp do ASP.NET Core kontekstu i konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="069a6-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="069a6-454">Aby użyć oprogramowania pośredniczącego jako filtru, należy utworzyć typ z metodą `Configure`, która określa oprogramowanie pośredniczące, które ma zostać dodane do potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="069a6-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="069a6-455">W poniższym przykładzie jest wykorzystywane oprogramowanie pośredniczące lokalizacyjne do ustalenia bieżącej kultury dla żądania:</span><span class="sxs-lookup"><span data-stu-id="069a6-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="069a6-456">Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute>, aby uruchomić oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="069a6-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="069a6-457">Filtry oprogramowania pośredniczącego są uruchamiane na tym samym etapie potoku filtru jako filtry zasobów przed powiązaniem modelu i po pozostałej części potoku.</span><span class="sxs-lookup"><span data-stu-id="069a6-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="069a6-458">Następne akcje</span><span class="sxs-lookup"><span data-stu-id="069a6-458">Next actions</span></span>

* <span data-ttu-id="069a6-459">Zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="069a6-459">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="069a6-460">Aby eksperymentować z filtrami, należy [pobrać, przetestować i zmodyfikować przykład usługi GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="069a6-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
