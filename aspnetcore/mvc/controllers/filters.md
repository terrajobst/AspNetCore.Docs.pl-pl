---
title: Filtry w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak działa filtr i sposobu ich używania w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 50b199744f32ad19335080da406db69665ec1ae9
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856163"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="96872-103">Filtry w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96872-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="96872-104">Przez [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="96872-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="96872-105">*Filtry* w programie ASP.NET Core umożliwia kodu do uruchomienia przed lub po określonych etapów w potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="96872-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="96872-106">Wbudowanych filtrów obsługi zadań, takich jak:</span><span class="sxs-lookup"><span data-stu-id="96872-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="96872-107">Autoryzacja (blokowanie dostępu do zasobów, których użytkownik nie jest autoryzowany do).</span><span class="sxs-lookup"><span data-stu-id="96872-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="96872-108">Odpowiedź buforowania (zwarcie Potok żądań do zwracania odpowiedzi pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="96872-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="96872-109">Filtry niestandardowe mogą być tworzone do obsługi odciąż przekrojowe zagadnienia.</span><span class="sxs-lookup"><span data-stu-id="96872-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="96872-110">Przykładami odciąż przekrojowe zagadnienia obsługi błędów, buforowanie, konfiguracji, autoryzacji i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="96872-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="96872-111">Filtry uniknąć duplikowania kodu.</span><span class="sxs-lookup"><span data-stu-id="96872-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="96872-112">Na przykład błąd obsługi filtra wyjątku można skonsolidować obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="96872-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="96872-113">Ten dokument ma zastosowanie do stron Razor, kontrolery interfejsu API i kontrolery, za pomocą widoków.</span><span class="sxs-lookup"><span data-stu-id="96872-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="96872-114">[Wyświetlanie lub pobieranie przykładowych](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="96872-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="96872-115">Jak działa filtr</span><span class="sxs-lookup"><span data-stu-id="96872-115">How filters work</span></span>

<span data-ttu-id="96872-116">Filtry są uruchamiane w ramach *potok wywołania akcji programu ASP.NET Core*nazywanych czasami *potoku filtru*.</span><span class="sxs-lookup"><span data-stu-id="96872-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="96872-117">Potok filtru jest uruchamiany po platformy ASP.NET Core wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="96872-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, Routing oprogramowanie pośredniczące, wybór akcji i potoku wywołania akcji programu ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="96872-120">Filtruj typy</span><span class="sxs-lookup"><span data-stu-id="96872-120">Filter types</span></span>

<span data-ttu-id="96872-121">Każdy typ filtru jest wykonywane na różnych etapach potoku filtru:</span><span class="sxs-lookup"><span data-stu-id="96872-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="96872-122">[Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do ustalenia, czy użytkownik jest autoryzowany dla żądania.</span><span class="sxs-lookup"><span data-stu-id="96872-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="96872-123">Filtry autoryzacji zwarcie potoku, jeśli żądanie jest nieautoryzowane.</span><span class="sxs-lookup"><span data-stu-id="96872-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="96872-124">[Filtry zasobów](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="96872-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="96872-125">Uruchom po autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="96872-125">Run after authorization.</span></span>  
  * <span data-ttu-id="96872-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> można uruchomić kod przed pozostałego potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="96872-127">Na przykład `OnResourceExecuting` można uruchomić kod przed powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="96872-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="96872-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> można uruchomić kod po ukończeniu pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="96872-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="96872-129">[Filtry akcji](#action-filters) może uruchamiać kod bezpośrednio przed i po jest wywoływana z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="96872-130">One może służyć do manipulowania Argumenty przekazane do akcji i wyniku zwracanego z akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="96872-131">Filtry akcji są **nie** obsługiwane w przypadku stron Razor.</span><span class="sxs-lookup"><span data-stu-id="96872-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="96872-132">[Filtry wyjątków](#exception-filters) jest używana do stosowania zasad globalnych do nieobsługiwanych wyjątków występujące przed niczego zostały zapisane w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="96872-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="96872-133">[Wynik filtry](#result-filters) może uruchamiać kod bezpośrednio przed i po wykonaniu wyniki poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="96872-134">Działają one tylko wtedy, gdy metoda akcji zostało wykonane pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="96872-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="96872-135">Są one przydatne dla logiki, które należy otoczyć wykonywanie widoku lub elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="96872-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="96872-136">Na poniższym diagramie przedstawiono sposób interakcji typów filtrów w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Żądanie jest przetwarzane przez filtry autoryzacji, filtrów zasobów, powiązanie modelu, filtry akcji, wykonywanie akcji i konwersji wynik akcji, filtry wyjątków, filtry wyników i wynik wykonywania.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="96872-139">Implementacja</span><span class="sxs-lookup"><span data-stu-id="96872-139">Implementation</span></span>

<span data-ttu-id="96872-140">Filtry obsługuje synchroniczne i asynchroniczne implementacji za pośrednictwem interfejsu w różnych definicjach.</span><span class="sxs-lookup"><span data-stu-id="96872-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="96872-141">Synchroniczne filtrów można uruchomić kod przed (`On-Stage-Executing`) oraz po (`On-Stage-Executed`) ich etap potoku.</span><span class="sxs-lookup"><span data-stu-id="96872-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="96872-142">Na przykład `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="96872-143">`OnActionExecuted` jest wywoływana po powrocie z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="96872-144">Zdefiniuj filtry asynchroniczne `On-Stage-ExecutionAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="96872-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="96872-145">W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który jest wykonywany metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="96872-146">Każdy z `On-Stage-ExecutionAsync` metody przyjmują `FilterType-ExecutionDelegate` , który jest wykonywany etap potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="96872-147">Wiele etapów filtru</span><span class="sxs-lookup"><span data-stu-id="96872-147">Multiple filter stages</span></span>

<span data-ttu-id="96872-148">Interfejsy na wiele etapów filtr można zaimplementować w jednej klasie.</span><span class="sxs-lookup"><span data-stu-id="96872-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="96872-149">Na przykład <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> klasy implementuje `IActionFilter`, `IResultFilter`i ich odpowiedniki async.</span><span class="sxs-lookup"><span data-stu-id="96872-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="96872-150">Implementowanie **albo** synchronicznej lub asynchronicznej wersję interfejsu filtru, **nie** jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="96872-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="96872-151">Środowisko uruchomieniowe sprawdza najpierw Jeśli filtr implementuje interfejs asynchroniczne, a jeśli tak jest, wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="96872-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="96872-152">W przeciwnym razie wywołuje metody synchronicznej interfejsu.</span><span class="sxs-lookup"><span data-stu-id="96872-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="96872-153">Jeśli synchronicznego i asynchronicznego interfejsy są implementowane w jedną klasę, jest wywoływana tylko metody asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="96872-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="96872-154">Podczas używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> przesłonić tylko synchroniczne metody lub metody asynchronicznej dla każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="96872-155">Atrybuty filtru wbudowane</span><span class="sxs-lookup"><span data-stu-id="96872-155">Built-in filter attributes</span></span>

<span data-ttu-id="96872-156">ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które należy do podklasy i dostosowane.</span><span class="sxs-lookup"><span data-stu-id="96872-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="96872-157">Na przykład następujący filtr wyników dodaje do odpowiedzi nagłówek:</span><span class="sxs-lookup"><span data-stu-id="96872-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="96872-158">Atrybuty zezwalać na filtry przyjmowały argumenty, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="96872-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="96872-159">Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:</span><span class="sxs-lookup"><span data-stu-id="96872-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="96872-160">Kilka interfejsów filtr ma odpowiednie atrybuty, które może służyć jako klay bazowe dla niestandardowych implementacji.</span><span class="sxs-lookup"><span data-stu-id="96872-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="96872-161">Atrybuty filtru:</span><span class="sxs-lookup"><span data-stu-id="96872-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="96872-162">Filtr zakresów i kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="96872-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="96872-163">Filtr można dodać do potoku w jednej z trzech *zakresy*:</span><span class="sxs-lookup"><span data-stu-id="96872-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="96872-164">Za pomocą atrybutu w akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="96872-165">Za pomocą atrybutu na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="96872-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="96872-166">Globalnie dla wszystkich kontrolerów i akcji, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="96872-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="96872-167">Poprzedzający kod dodaje trzy filtry zasięg globalny dzięki [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) kolekcji.</span><span class="sxs-lookup"><span data-stu-id="96872-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="96872-168">Domyślna kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="96872-168">Default order of execution</span></span>

<span data-ttu-id="96872-169">Gdy istnieje wiele filtrów dla danego etapu potoku, zakres Określa domyślną kolejność wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="96872-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="96872-170">Filtry globalne Otocz filtrów klasy, które z kolei Otocz metoda filtrów.</span><span class="sxs-lookup"><span data-stu-id="96872-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="96872-171">W wyniku zagnieżdżanie filtru *po* kod filtrów, który jest uruchamiany w odwrotnej kolejności *przed* kodu.</span><span class="sxs-lookup"><span data-stu-id="96872-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="96872-172">Sekwencja filtru:</span><span class="sxs-lookup"><span data-stu-id="96872-172">The filter sequence:</span></span>

* <span data-ttu-id="96872-173">*Przed* kodu filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="96872-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="96872-174">*Przed* kodu filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="96872-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="96872-175">*Przed* kodu filtrów metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="96872-176">*Po* kodu filtrów metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="96872-177">*Po* kodu filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="96872-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="96872-178">*Po* kodu filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="96872-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="96872-179">Poniższy przykład ilustruje zamówienia w filtrze, które metody są wywoływane dla filtrów akcji synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="96872-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="96872-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="96872-180">Sequence</span></span> | <span data-ttu-id="96872-181">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="96872-181">Filter scope</span></span> | <span data-ttu-id="96872-182">Filter — metoda</span><span class="sxs-lookup"><span data-stu-id="96872-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="96872-183">1</span><span class="sxs-lookup"><span data-stu-id="96872-183">1</span></span> | <span data-ttu-id="96872-184">Global</span><span class="sxs-lookup"><span data-stu-id="96872-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="96872-185">2</span><span class="sxs-lookup"><span data-stu-id="96872-185">2</span></span> | <span data-ttu-id="96872-186">Kontroler</span><span class="sxs-lookup"><span data-stu-id="96872-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="96872-187">3</span><span class="sxs-lookup"><span data-stu-id="96872-187">3</span></span> | <span data-ttu-id="96872-188">Metoda</span><span class="sxs-lookup"><span data-stu-id="96872-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="96872-189">4</span><span class="sxs-lookup"><span data-stu-id="96872-189">4</span></span> | <span data-ttu-id="96872-190">Metoda</span><span class="sxs-lookup"><span data-stu-id="96872-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="96872-191">5</span><span class="sxs-lookup"><span data-stu-id="96872-191">5</span></span> | <span data-ttu-id="96872-192">Kontroler</span><span class="sxs-lookup"><span data-stu-id="96872-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="96872-193">6</span><span class="sxs-lookup"><span data-stu-id="96872-193">6</span></span> | <span data-ttu-id="96872-194">Global</span><span class="sxs-lookup"><span data-stu-id="96872-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="96872-195">Ta sekwencja zawiera:</span><span class="sxs-lookup"><span data-stu-id="96872-195">This sequence shows:</span></span>

* <span data-ttu-id="96872-196">Filtr metody jest zagnieżdżona w filtrze kontrolera.</span><span class="sxs-lookup"><span data-stu-id="96872-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="96872-197">Filtr kontrolera jest zagnieżdżony w filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="96872-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="96872-198">Filtry na poziomie kontrolera i strona Razor</span><span class="sxs-lookup"><span data-stu-id="96872-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="96872-199">Każdy kontroler, który dziedziczy z <xref:Microsoft.AspNetCore.Mvc.Controller> zawiera klasę bazową [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), i [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*) 
 `OnActionExecuted` metody.</span><span class="sxs-lookup"><span data-stu-id="96872-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="96872-200">Te metody:</span><span class="sxs-lookup"><span data-stu-id="96872-200">These methods:</span></span>

* <span data-ttu-id="96872-201">Otoczenie systemem filtry dla danej akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="96872-202">`OnActionExecuting` jest wywoływana przed każdą filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="96872-203">`OnActionExecuted` jest wywoływana po wszystkie filtry akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="96872-204">`OnActionExecutionAsync` jest wywoływana przed każdą filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="96872-205">Kod w filtrze po `next` jest uruchamiany po metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="96872-206">Na przykład, w tym przykładzie pobierania `MySampleActionFilter` jest stosowane globalnie w uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="96872-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="96872-207">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="96872-207">The `TestController`:</span></span>

* <span data-ttu-id="96872-208">Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do `FilterTest2` akcji:</span><span class="sxs-lookup"><span data-stu-id="96872-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="96872-209">Zastępuje `OnActionExecuting` i `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="96872-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="96872-210">Przejdź do `https://localhost:5001/Test/FilterTest2` uruchomieniu następujący kod:</span><span class="sxs-lookup"><span data-stu-id="96872-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="96872-211">Dla stron Razor, zobacz [filtry stron Razor Implementowanie poprzez zastąpienie metody filtr](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="96872-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="96872-212">Zastępowanie domyślna kolejność</span><span class="sxs-lookup"><span data-stu-id="96872-212">Overriding the default order</span></span>

<span data-ttu-id="96872-213">Domyślną sekwencją wykonanie może być zastąpiona przez Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="96872-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="96872-214">`IOrderedFilter` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> właściwość, która ma pierwszeństwo przed zakresu, aby określić kolejność wykonywania.</span><span class="sxs-lookup"><span data-stu-id="96872-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="96872-215">Filtr o niższych `Order` wartość:</span><span class="sxs-lookup"><span data-stu-id="96872-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="96872-216">Przebiegi *przed* kodu wcześniej filtr o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="96872-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="96872-217">Przebiegi *po* kodzie następnie filtr o wyższej `Order` wartość.</span><span class="sxs-lookup"><span data-stu-id="96872-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="96872-218">`Order` Właściwość można ustawić za pomocą parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="96872-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="96872-219">Należy wziąć pod uwagę tych samych filtrów akcji 3 pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="96872-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="96872-220">Jeśli `Order` właściwość kontrolera i filtrów globalnych jest ustawiona na 1 i 2, odpowiednio, kolejność wykonywania została odwrócona.</span><span class="sxs-lookup"><span data-stu-id="96872-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="96872-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="96872-221">Sequence</span></span> | <span data-ttu-id="96872-222">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="96872-222">Filter scope</span></span> | <span data-ttu-id="96872-223">`Order` Właściwość</span><span class="sxs-lookup"><span data-stu-id="96872-223">`Order` property</span></span> | <span data-ttu-id="96872-224">Filter — metoda</span><span class="sxs-lookup"><span data-stu-id="96872-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="96872-225">1</span><span class="sxs-lookup"><span data-stu-id="96872-225">1</span></span> | <span data-ttu-id="96872-226">Metoda</span><span class="sxs-lookup"><span data-stu-id="96872-226">Method</span></span> | <span data-ttu-id="96872-227">0</span><span class="sxs-lookup"><span data-stu-id="96872-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="96872-228">2</span><span class="sxs-lookup"><span data-stu-id="96872-228">2</span></span> | <span data-ttu-id="96872-229">Kontroler</span><span class="sxs-lookup"><span data-stu-id="96872-229">Controller</span></span> | <span data-ttu-id="96872-230">1</span><span class="sxs-lookup"><span data-stu-id="96872-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="96872-231">3</span><span class="sxs-lookup"><span data-stu-id="96872-231">3</span></span> | <span data-ttu-id="96872-232">Global</span><span class="sxs-lookup"><span data-stu-id="96872-232">Global</span></span> | <span data-ttu-id="96872-233">2</span><span class="sxs-lookup"><span data-stu-id="96872-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="96872-234">4</span><span class="sxs-lookup"><span data-stu-id="96872-234">4</span></span> | <span data-ttu-id="96872-235">Global</span><span class="sxs-lookup"><span data-stu-id="96872-235">Global</span></span> | <span data-ttu-id="96872-236">2</span><span class="sxs-lookup"><span data-stu-id="96872-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="96872-237">5</span><span class="sxs-lookup"><span data-stu-id="96872-237">5</span></span> | <span data-ttu-id="96872-238">Kontroler</span><span class="sxs-lookup"><span data-stu-id="96872-238">Controller</span></span> | <span data-ttu-id="96872-239">1</span><span class="sxs-lookup"><span data-stu-id="96872-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="96872-240">6</span><span class="sxs-lookup"><span data-stu-id="96872-240">6</span></span> | <span data-ttu-id="96872-241">Metoda</span><span class="sxs-lookup"><span data-stu-id="96872-241">Method</span></span> | <span data-ttu-id="96872-242">0</span><span class="sxs-lookup"><span data-stu-id="96872-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="96872-243">`Order` Właściwość zastępuje zakres podczas ustalania kolejności, w której filtry są uruchamiane.</span><span class="sxs-lookup"><span data-stu-id="96872-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="96872-244">Filtry są najpierw posortowane według kolejności, a następnie do przerwania ties jest używany zakres.</span><span class="sxs-lookup"><span data-stu-id="96872-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="96872-245">Spośród filtrów wbudowanych implementują `IOrderedFilter` i Ustaw domyślną `Order` wartość 0.</span><span class="sxs-lookup"><span data-stu-id="96872-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="96872-246">Wbudowane filtry, zakres Określa kolejność, chyba że `Order` jest ustawiona na wartość inną niż zero.</span><span class="sxs-lookup"><span data-stu-id="96872-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="96872-247">Anulowanie i zwarcie</span><span class="sxs-lookup"><span data-stu-id="96872-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="96872-248">Może być short-circuited potoku filtru, ustawiając <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametr do metody filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="96872-249">Na przykład następujący filtr zasobu uniemożliwia wykonywanie pozostałego potoku:</span><span class="sxs-lookup"><span data-stu-id="96872-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="96872-250">W poniższym kodzie zarówno `ShortCircuitingResourceFilter` i `AddHeader` filtr docelowy `SomeResource` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="96872-251">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="96872-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="96872-252">Jest uruchamiany po pierwsze, ponieważ filtr zasobów i `AddHeader` jest filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="96872-253">Short-Circuits pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="96872-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="96872-254">W związku z tym `AddHeader` filtr nigdy nie będzie uruchamiany dla `SomeResource` akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="96872-255">To zachowanie będzie taki sam w przypadku obu filtrów były stosowane na poziomie metody akcji, podany `ShortCircuitingResourceFilter` uruchomiono pierwszy.</span><span class="sxs-lookup"><span data-stu-id="96872-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="96872-256">`ShortCircuitingResourceFilter` Uruchamia pierwszy ze względu na jej typ filtru lub przez jawne użycie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="96872-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="96872-257">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="96872-257">Dependency injection</span></span>

<span data-ttu-id="96872-258">Filtry można dodać według typu lub wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="96872-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="96872-259">Jeśli wystąpienie zostanie dodany, że wystąpienie jest używane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="96872-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="96872-260">Jeśli typ jest dodawany, jest typ aktywowany.</span><span class="sxs-lookup"><span data-stu-id="96872-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="96872-261">Oznacza typ aktywowany filtru:</span><span class="sxs-lookup"><span data-stu-id="96872-261">A type-activated filter means:</span></span>

* <span data-ttu-id="96872-262">Wystąpienie jest tworzone dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="96872-262">An instance is created for each request.</span></span>
* <span data-ttu-id="96872-263">Wszelkie zależności konstruktora są wypełniane przez [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="96872-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="96872-264">Filtry, które są zaimplementowane jako atrybuty i dodawane bezpośrednio do klasy kontrolera lub metody akcji nie może mieć konstruktora zależności, dostarczone przez [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="96872-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="96872-265">Nie można podać zależności Konstruktor przez DI, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="96872-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="96872-266">Atrybuty musi mieć ich parametry konstruktora dostarczane, gdy są one stosowane.</span><span class="sxs-lookup"><span data-stu-id="96872-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="96872-267">Jest to ograniczenie o współdziałaniu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="96872-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="96872-268">Poniższe filtry obsługują zależności Konstruktor udostępnionych w serwisie DI:</span><span class="sxs-lookup"><span data-stu-id="96872-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="96872-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowane w atrybucie.</span><span class="sxs-lookup"><span data-stu-id="96872-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="96872-270">Poprzedni filtrów można zastosować do kontrolera lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="96872-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="96872-271">Rejestratory są dostępne z DI.</span><span class="sxs-lookup"><span data-stu-id="96872-271">Loggers are available from DI.</span></span> <span data-ttu-id="96872-272">Jednak należy unikać tworzenia i używania filtrów służyć wyłącznie do celów rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="96872-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="96872-273">[Wbudowana struktura rejestrowania](xref:fundamentals/logging/index) zwykle zapewnia, co jest potrzebne do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="96872-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="96872-274">Rejestrowanie dodane do filtrów:</span><span class="sxs-lookup"><span data-stu-id="96872-274">Logging added to filters:</span></span>

* <span data-ttu-id="96872-275">Należy skoncentrować się na potencjalne problemy biznesowe domeny lub zachowania specyficzne dla filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="96872-276">Należy **nie** dziennika akcji lub inne zdarzenia framework.</span><span class="sxs-lookup"><span data-stu-id="96872-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="96872-277">Wbudowane filtry dziennika akcji i zdarzenia framework.</span><span class="sxs-lookup"><span data-stu-id="96872-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="96872-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="96872-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="96872-279">Typy wdrożenia filtrów usługi są zarejestrowane w usłudze `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="96872-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="96872-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> pobiera wystąpienia filtru z DI.</span><span class="sxs-lookup"><span data-stu-id="96872-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="96872-281">Poniższy kod przedstawia `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="96872-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="96872-282">W poniższym kodzie `AddHeaderResultServiceFilter` zostanie dodany do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="96872-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="96872-283">W poniższym kodzie `ServiceFilter` atrybut pobiera wystąpienia `AddHeaderResultServiceFilter` filtr z DI:</span><span class="sxs-lookup"><span data-stu-id="96872-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="96872-284">Korzystając z `ServiceFilterAttribute`, ustawiając [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="96872-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="96872-285">Stanowi wskazówkę wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu.</span><span class="sxs-lookup"><span data-stu-id="96872-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="96872-286">Środowisko uruchomieniowe programu ASP.NET Core nie gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="96872-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="96872-287">Czy zostaną utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="96872-288">Filtr nie będzie ponowne żądanie z kontenera DI w pewnym momencie później.</span><span class="sxs-lookup"><span data-stu-id="96872-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="96872-289">Nie można używać z filtrem, który zależy od usługi z okresem istnienia innego niż pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="96872-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="96872-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="96872-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="96872-291">`IFilterFactory` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metodę tworzenia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="96872-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="96872-292">`CreateInstance` Ładuje określony typ z DI.</span><span class="sxs-lookup"><span data-stu-id="96872-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="96872-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="96872-293">TypeFilterAttribute</span></span>

<span data-ttu-id="96872-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> jest podobny do <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie zostanie rozwiązany bezpośrednio w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="96872-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="96872-295">Tworzy wystąpienie typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="96872-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="96872-296">Ponieważ `TypeFilterAttribute` typy nie są rozwiązane bezpośrednio w kontenerze DI:</span><span class="sxs-lookup"><span data-stu-id="96872-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="96872-297">Typy, które są wywoływane przy użyciu `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="96872-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="96872-298">Mają zależności są spełnione przez kontener DI.</span><span class="sxs-lookup"><span data-stu-id="96872-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="96872-299">`TypeFilterAttribute` Opcjonalnie można zaakceptować argumentów konstruktora dla typu.</span><span class="sxs-lookup"><span data-stu-id="96872-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="96872-300">Korzystając z `TypeFilterAttribute`, ustawiając [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="96872-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="96872-301">Zawiera wskazówki, które wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu.</span><span class="sxs-lookup"><span data-stu-id="96872-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="96872-302">Środowisko uruchomieniowe programu ASP.NET Core zapewnia żadnej gwarancji, które zostaną utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="96872-303">Nie można używać z filtrem, który zależy od usługi z okresem istnienia innego niż pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="96872-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="96872-304">Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="96872-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="96872-305">Filtry autoryzacji</span><span class="sxs-lookup"><span data-stu-id="96872-305">Authorization filters</span></span>

<span data-ttu-id="96872-306">Filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="96872-306">Authorization filters:</span></span>

* <span data-ttu-id="96872-307">Czy pierwszy filtry są uruchamiane w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="96872-308">Kontrola dostępu do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-308">Control access to action methods.</span></span>
* <span data-ttu-id="96872-309">Masz przed metodą, ale nie po metodzie.</span><span class="sxs-lookup"><span data-stu-id="96872-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="96872-310">Filtry autoryzacji niestandardowe wymagają framework autoryzacja niestandardowa.</span><span class="sxs-lookup"><span data-stu-id="96872-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="96872-311">Preferuj Konfigurowanie zasad autoryzacji lub zapisu niestandardowych zasad autoryzacji za pośrednictwem pisania niestandardowego filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="96872-312">Filtr autoryzacji wbudowane:</span><span class="sxs-lookup"><span data-stu-id="96872-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="96872-313">Wywołuje system autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="96872-313">Calls the authorization system.</span></span>
* <span data-ttu-id="96872-314">Nie zezwalają na żądania.</span><span class="sxs-lookup"><span data-stu-id="96872-314">Does not authorize requests.</span></span>

<span data-ttu-id="96872-315">Czy **nie** zgłaszają wyjątki w ramach filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="96872-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="96872-316">Wyjątek nie będzie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="96872-316">The exception will not be handled.</span></span>
* <span data-ttu-id="96872-317">Filtry wyjątków nie będzie obsługiwać wyjątek.</span><span class="sxs-lookup"><span data-stu-id="96872-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="96872-318">Należy wziąć pod uwagę, wydawanie wyzwanie w przypadku, gdy wystąpi wyjątek w filtru autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="96872-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="96872-319">Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="96872-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="96872-320">Filtry zasobów</span><span class="sxs-lookup"><span data-stu-id="96872-320">Resource filters</span></span>

<span data-ttu-id="96872-321">Filtry zasobów:</span><span class="sxs-lookup"><span data-stu-id="96872-321">Resource filters:</span></span>

* <span data-ttu-id="96872-322">Implementowanie albo <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="96872-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="96872-323">Wykonanie opakowuje większość potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="96872-324">Tylko [filtry autoryzacji](#authorization-filters) uruchamiane przed filtrami zasobów.</span><span class="sxs-lookup"><span data-stu-id="96872-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="96872-325">Filtry zasobów są przydatne do zwarcie większość potoku.</span><span class="sxs-lookup"><span data-stu-id="96872-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="96872-326">Na przykład filtr pamięci podręcznej można uniknąć pozostałego potoku na trafień pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="96872-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="96872-327">Przykłady filtrów zasobów:</span><span class="sxs-lookup"><span data-stu-id="96872-327">Resource filter examples:</span></span>

* <span data-ttu-id="96872-328">[Filtr zasobu short-circuiting](#short-circuiting-resource-filter) przedstawionego wcześniej.</span><span class="sxs-lookup"><span data-stu-id="96872-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="96872-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="96872-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="96872-330">Zapobiega wiązania modelu dostępu do danych formularza.</span><span class="sxs-lookup"><span data-stu-id="96872-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="96872-331">Używane przekazywania plików o dużym rozmiarze, aby zapobiec wczytywana pamięci danych formularza.</span><span class="sxs-lookup"><span data-stu-id="96872-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="96872-332">Filtry akcji</span><span class="sxs-lookup"><span data-stu-id="96872-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96872-333">Filtry akcji wykonaj **nie** dotyczą stron Razor.</span><span class="sxs-lookup"><span data-stu-id="96872-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="96872-334">Strony razor obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span><span class="sxs-lookup"><span data-stu-id="96872-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="96872-335">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="96872-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="96872-336">Filtry akcji:</span><span class="sxs-lookup"><span data-stu-id="96872-336">Action filters:</span></span>

* <span data-ttu-id="96872-337">Implementowanie albo <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="96872-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="96872-338">Ich wykonanie otacza wykonywania metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="96872-339">Poniższy kod przedstawia przykładowy filtr akcji:</span><span class="sxs-lookup"><span data-stu-id="96872-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="96872-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> Zawiera następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="96872-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="96872-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — Umożliwia odczytanie danych wejściowych do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="96872-342"><xref:Microsoft.AspNetCore.Mvc.Controller> — umożliwia manipulowanie wystąpienie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="96872-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="96872-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> -ustawienie `Result` short-circuits wykonywania metody akcji i filtry kolejnych działań.</span><span class="sxs-lookup"><span data-stu-id="96872-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="96872-344">Zostanie zgłoszony wyjątek w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="96872-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="96872-345">Uniemożliwia uruchamianie kolejne filtry.</span><span class="sxs-lookup"><span data-stu-id="96872-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="96872-346">W przeciwieństwie do ustawienia `Result`, jest traktowana jako błąd zamiast pomyślnego wyniku.</span><span class="sxs-lookup"><span data-stu-id="96872-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="96872-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> Zapewnia `Controller` i `Result` oraz następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="96872-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="96872-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> — Wartość true, jeśli zwartym został wykonanie akcji przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="96872-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="96872-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> Innych niż null w przypadku akcji lub filtru akcji wcześniej przebiegu zgłosiła wyjątek.</span><span class="sxs-lookup"><span data-stu-id="96872-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="96872-350">Ustawienie tej właściwości na wartość null:</span><span class="sxs-lookup"><span data-stu-id="96872-350">Setting this property to null:</span></span>

  * <span data-ttu-id="96872-351">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="96872-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="96872-352">`Result` jest wykonywane tak, jakby został zwrócony przez metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="96872-353">Aby uzyskać `IAsyncActionFilter`, wywołanie <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="96872-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="96872-354">Wykonuje wszelkie filtry kolejnej akcji i metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="96872-355">Zwraca `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="96872-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="96872-356">Aby zwarcie, należy przypisać <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> w wyniku wystąpienia i nie wywołuj `next` ( `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="96872-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="96872-357">Struktura dostarcza abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> , może być podklasą klasy.</span><span class="sxs-lookup"><span data-stu-id="96872-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="96872-358">`OnActionExecuting` Filtr akcji można używać do:</span><span class="sxs-lookup"><span data-stu-id="96872-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="96872-359">Sprawdź stan modelu.</span><span class="sxs-lookup"><span data-stu-id="96872-359">Validate model state.</span></span>
* <span data-ttu-id="96872-360">Zwraca błąd, jeśli stan jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="96872-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="96872-361">`OnActionExecuted` Metoda uruchamia się po metody akcji:</span><span class="sxs-lookup"><span data-stu-id="96872-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="96872-362">Można wyświetlić i manipulowania wynikami akcji za pomocą <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> właściwości.</span><span class="sxs-lookup"><span data-stu-id="96872-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="96872-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> ustawiono wartość true, jeśli jest to zwartym został wykonanie akcji przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="96872-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="96872-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> jest ustawiona na wartość inną niż null, jeśli akcji lub filtru akcji kolejnych zgłosiła wyjątek.</span><span class="sxs-lookup"><span data-stu-id="96872-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="96872-365">Ustawienie `Exception` null:</span><span class="sxs-lookup"><span data-stu-id="96872-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="96872-366">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="96872-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="96872-367">`ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie przez metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="96872-368">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="96872-368">Exception filters</span></span>

<span data-ttu-id="96872-369">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="96872-369">Exception filters:</span></span>

* <span data-ttu-id="96872-370">Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="96872-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="96872-371">Może służyć do implementowania obsługi zasad typowych błędów.</span><span class="sxs-lookup"><span data-stu-id="96872-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="96872-372">Następujący filtr wyjątku przykładowych użyto widoku błędów niestandardowych, aby wyświetlić szczegóły dotyczące wyjątków, które występują, gdy aplikacja jest w trakcie opracowywania:</span><span class="sxs-lookup"><span data-stu-id="96872-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="96872-373">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="96872-373">Exception filters:</span></span>

* <span data-ttu-id="96872-374">Nie masz, przed i po nim zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="96872-374">Don't have before and after events.</span></span>
* <span data-ttu-id="96872-375">Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="96872-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="96872-376">Obsługa nieobsługiwanych wyjątków, które występują w stronę Razor lub tworzenia kontrolera [wiązanie modelu](xref:mvc/models/model-binding), filtry akcji lub metody akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="96872-377">Czy **nie** przechwytywać wyjątków, które występują w filtrów zasobów, filtry wyników lub MVC wynik wykonywania.</span><span class="sxs-lookup"><span data-stu-id="96872-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="96872-378">Aby obsłużyć wyjątek, należy ustawić <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> właściwości `true` lub zapisu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="96872-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="96872-379">Spowoduje to zatrzymanie propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="96872-379">This stops propagation of the exception.</span></span> <span data-ttu-id="96872-380">Filtra wyjątku nie może włączyć wyjątek do "Powodzenie".</span><span class="sxs-lookup"><span data-stu-id="96872-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="96872-381">Filtr akcji można to zrobić.</span><span class="sxs-lookup"><span data-stu-id="96872-381">Only an action filter can do that.</span></span>

<span data-ttu-id="96872-382">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="96872-382">Exception filters:</span></span>

* <span data-ttu-id="96872-383">Dla zastosowań dobre są wyjątki wyłapywanie, które występują w ramach akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="96872-384">Nie są tak elastyczne, oprogramowanie pośredniczące obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="96872-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="96872-385">Preferuj oprogramowanie pośredniczące do obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="96872-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="96872-386">Użyj wyjątek filtruje tylko wtedy, gdy obsługa błędów *różni się* oparte na jakiej metody akcji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="96872-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="96872-387">Na przykład aplikacja może mieć metody akcji dla obu punktów końcowych interfejsu API i widoki/HTML.</span><span class="sxs-lookup"><span data-stu-id="96872-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="96872-388">Punkty końcowe interfejsu API może zwrócić informacje o błędach w formacie JSON, natomiast akcje na podstawie widoku może zwrócić strony błędu w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="96872-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="96872-389">Filtry wyników</span><span class="sxs-lookup"><span data-stu-id="96872-389">Result filters</span></span>

<span data-ttu-id="96872-390">Filtry wyników:</span><span class="sxs-lookup"><span data-stu-id="96872-390">Result filters:</span></span>

* <span data-ttu-id="96872-391">Implementuj interfejs z:</span><span class="sxs-lookup"><span data-stu-id="96872-391">Implement an interface:</span></span>
  * <span data-ttu-id="96872-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="96872-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="96872-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="96872-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="96872-394">Ich wykonanie otacza wykonywania wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="96872-395">IResultFilter i IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="96872-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="96872-396">Poniższy kod pokazuje filtr wynik, który dodaje nagłówek HTTP:</span><span class="sxs-lookup"><span data-stu-id="96872-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="96872-397">Typ wyniku wykonywania zależy od akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="96872-398">Zwracanie Widok akcji obejmuje wszystkie razor przetwarzania w ramach <xref:Microsoft.AspNetCore.Mvc.ViewResult> wykonywana.</span><span class="sxs-lookup"><span data-stu-id="96872-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="96872-399">Metoda interfejsu API może wykonywać niektóre serializacji jako część wykonania wyniku.</span><span class="sxs-lookup"><span data-stu-id="96872-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="96872-400">Dowiedz się więcej o [wyników akcji](xref:mvc/controllers/actions)</span><span class="sxs-lookup"><span data-stu-id="96872-400">Learn more about [action results](xref:mvc/controllers/actions)</span></span>

<span data-ttu-id="96872-401">Filtry wyników są wykonywane tylko na pomyślne wyniki — gdy akcji lub filtry akcji dają wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-401">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="96872-402">Filtry wyników nie są wykonywane, gdy filtry wyjątków obsługi wyjątku.</span><span class="sxs-lookup"><span data-stu-id="96872-402">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="96872-403"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> Metoda może zwarcie wykonywania wynik akcji i filtry kolejnych wyników, ustawiając <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> do `true`.</span><span class="sxs-lookup"><span data-stu-id="96872-403">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="96872-404">Zapisywana w obiekt odpowiedzi zwarcie, aby uniknąć generowania pustą odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="96872-404">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="96872-405">Zostanie zgłoszony wyjątek `IResultFilter.OnResultExecuting` będzie:</span><span class="sxs-lookup"><span data-stu-id="96872-405">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="96872-406">Zapobiec wykonaniu wyniku akcji i kolejne filtry.</span><span class="sxs-lookup"><span data-stu-id="96872-406">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="96872-407">Traktowane jako błąd zamiast pomyślnego wyniku.</span><span class="sxs-lookup"><span data-stu-id="96872-407">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="96872-408">Gdy <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> uruchamia metody:</span><span class="sxs-lookup"><span data-stu-id="96872-408">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="96872-409">Odpowiedź, prawdopodobnie została wysłana do klienta i nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="96872-409">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="96872-410">Jeśli wystąpił wyjątek, treść odpowiedzi nie są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="96872-410">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="96872-411">`ResultExecutedContext.Canceled` ustawiono `true` Jeśli zwartym został wykonanie wynik akcji przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="96872-411">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="96872-412">`ResultExecutedContext.Exception` jest ustawiona na wartość inną niż null, jeśli wynik akcji lub filtru kolejnych wyników zgłosiła wyjątek.</span><span class="sxs-lookup"><span data-stu-id="96872-412">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="96872-413">Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia wyjątek z jest zgłaszany ponownie przez platformy ASP.NET Core w dalszej części procesu.</span><span class="sxs-lookup"><span data-stu-id="96872-413">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="96872-414">Brak niezawodnego sposobu zapisywania danych do odpowiedzi podczas obsługi wyjątku w filtrze wynik.</span><span class="sxs-lookup"><span data-stu-id="96872-414">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="96872-415">Jeśli ma został opróżniony nagłówki do klienta, gdy wynik akcji zgłasza wyjątek, nie istnieje mechanizm niezawodne wysłać kod błędu.</span><span class="sxs-lookup"><span data-stu-id="96872-415">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="96872-416">Aby uzyskać <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, wywołanie `await next` na <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie filtry kolejnych wyników i wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-416">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="96872-417">Aby zwarcie, ustaw [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) do `true` i nie wywołuj `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="96872-417">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="96872-418">Struktura dostarcza abstrakcyjną `ResultFilterAttribute` , może być podklasą klasy.</span><span class="sxs-lookup"><span data-stu-id="96872-418">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="96872-419">[AddHeaderAttribute](#add-header-attribute) klasy wyświetlane wcześniej jest przykładem atrybutów filtru wyniku.</span><span class="sxs-lookup"><span data-stu-id="96872-419">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="96872-420">IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="96872-420">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="96872-421"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> zadeklarować interfejsów <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementację, która jest uruchamiana dla wszystkich wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-421">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="96872-422">Filtr jest stosowany do wszystkich wyników akcji, chyba że:</span><span class="sxs-lookup"><span data-stu-id="96872-422">The filter is applied to all action results unless:</span></span>

* <span data-ttu-id="96872-423"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> Lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> ma zastosowanie i short-circuits odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="96872-423">An <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>
* <span data-ttu-id="96872-424">Filtra wyjątku obsługuje wyjątek, przedstawiając wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="96872-424">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="96872-425">Filtry w innych niż `IExceptionFilter` i `IAuthorizationFilter` nie powodują pominięcie `IAlwaysRunResultFilter` i `IAsyncAlwaysRunResultFilter`.</span><span class="sxs-lookup"><span data-stu-id="96872-425">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short-circuit `IAlwaysRunResultFilter` and `IAsyncAlwaysRunResultFilter`.</span></span>

<span data-ttu-id="96872-426">Na przykład następujący filtr zawsze uruchamia i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) za pomocą *422 brakuje jednostki* kod stanu niepowodzenia negocjacji zawartości:</span><span class="sxs-lookup"><span data-stu-id="96872-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="96872-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="96872-427">IFilterFactory</span></span>

<span data-ttu-id="96872-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="96872-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="96872-429">W związku z tym `IFilterFactory` wystąpienia mogą być używane jako `IFilterMetadata` wystąpienia w dowolnym miejscu w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="96872-430">Gdy środowisko uruchomieniowe przygotowuje się do wywołania z filtrem, próbuje rzutować go na `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="96872-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="96872-431">Jeśli tego rzutowania zakończy się powodzeniem, <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metoda jest wywoływana, aby utworzyć `IFilterMetadata` wystąpienia, które jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="96872-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="96872-432">Dzięki temu elastycznością, ponieważ potoku filtru dokładne nie muszą być ustawiony w sposób jawny po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96872-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="96872-433">`IFilterFactory` można zaimplementować przy użyciu atrybutów niestandardowych implementacji jako innego podejścia do tworzenia filtrów:</span><span class="sxs-lookup"><span data-stu-id="96872-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="96872-434">Powyższy kod mogą być testowane przez uruchomienie [Pobierz przykładowe](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="96872-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="96872-435">Wywoływanie narzędzia programistyczne F12.</span><span class="sxs-lookup"><span data-stu-id="96872-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="96872-436">Przejdź do `https://localhost:5001/Sample/HeaderWithFactory`</span><span class="sxs-lookup"><span data-stu-id="96872-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="96872-437">Narzędzia programistyczne F12 wyświetlić następujące nagłówki odpowiedzi dodany przez kod przykładowy:</span><span class="sxs-lookup"><span data-stu-id="96872-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="96872-438">**Autor:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="96872-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="96872-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="96872-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="96872-440">**Wewnętrzny:** `My header`</span><span class="sxs-lookup"><span data-stu-id="96872-440">**internal:** `My header`</span></span>

<span data-ttu-id="96872-441">Powyższy kod tworzy **wewnętrzny:** `My header` nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="96872-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="96872-442">IFilterFactory implementowane w atrybucie</span><span class="sxs-lookup"><span data-stu-id="96872-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="96872-443">Filtry, które implementują `IFilterFactory` są przydatne w przypadku filtrów, które:</span><span class="sxs-lookup"><span data-stu-id="96872-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="96872-444">Nie wymagają przekazywanie parametrów.</span><span class="sxs-lookup"><span data-stu-id="96872-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="96872-445">Mają zależności konstruktora, które muszą zostać wypełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="96872-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="96872-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="96872-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="96872-447">`IFilterFactory` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metodę tworzenia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="96872-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="96872-448">`CreateInstance` Ładuje określony typ z kontenera usług (DI).</span><span class="sxs-lookup"><span data-stu-id="96872-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="96872-449">Poniższy kod pokazuje trzy sposoby stosowania `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="96872-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="96872-450">W poprzednim kodzie urządzanie metody `[SampleActionFilter]` jest preferowanym sposobem stosowania `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="96872-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="96872-451">Za pomocą oprogramowania pośredniczącego w potoku filtru</span><span class="sxs-lookup"><span data-stu-id="96872-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="96872-452">Filtry zasobów działają podobnie jak [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) w tym, że ujęty wykonanie wszystkich elementów, których można użyć później w potoku.</span><span class="sxs-lookup"><span data-stu-id="96872-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="96872-453">Jednak filtrów różnią się od oprogramowania pośredniczącego, w tym, że są one częścią środowiska uruchomieniowego platformy ASP.NET Core, co oznacza, że mają dostęp do kontekstu platformy ASP.NET Core i konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="96872-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="96872-454">Aby korzystać z oprogramowania pośredniczącego jako filtru, Utwórz typ z `Configure` metody, która określa oprogramowania pośredniczącego, aby wstawić do potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="96872-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="96872-455">W poniższym przykładzie użyto oprogramowania pośredniczącego lokalizacji ustalenie bieżącej kultury na żądanie:</span><span class="sxs-lookup"><span data-stu-id="96872-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="96872-456">Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> do uruchamiania oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="96872-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="96872-457">Oprogramowanie pośredniczące filtry są uruchamiane na tym samym etapie potoku filtru jako zasób filtry, przed powiązaniem modelu i po nim pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="96872-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="96872-458">Następne akcje</span><span class="sxs-lookup"><span data-stu-id="96872-458">Next actions</span></span>

* <span data-ttu-id="96872-459">Zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter)</span><span class="sxs-lookup"><span data-stu-id="96872-459">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="96872-460">Aby poeksperymentować z filtrami, [pobierania, testowanie i zmodyfikować przykładowe GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="96872-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
