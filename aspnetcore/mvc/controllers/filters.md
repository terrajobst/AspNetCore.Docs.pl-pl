---
title: Filtry w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak działa filtr i sposobu ich używania w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: df6f144f23f36d8009a5638859846e3cfb768b37
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815444"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="4a417-103">Filtry w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a417-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="4a417-104">Przez [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4a417-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4a417-105">*Filtry* w programie ASP.NET Core umożliwia kodu do uruchomienia przed lub po określonych etapów w potoku przetwarzania żądań.</span><span class="sxs-lookup"><span data-stu-id="4a417-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="4a417-106">Wbudowanych filtrów obsługi zadań, takich jak:</span><span class="sxs-lookup"><span data-stu-id="4a417-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="4a417-107">Autoryzacja (blokowanie dostępu do zasobów, których użytkownik nie jest autoryzowany do).</span><span class="sxs-lookup"><span data-stu-id="4a417-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="4a417-108">Odpowiedź buforowania (zwarcie Potok żądań do zwracania odpowiedzi pamięci podręcznej).</span><span class="sxs-lookup"><span data-stu-id="4a417-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="4a417-109">Filtry niestandardowe mogą być tworzone do obsługi odciąż przekrojowe zagadnienia.</span><span class="sxs-lookup"><span data-stu-id="4a417-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="4a417-110">Przykładami odciąż przekrojowe zagadnienia obsługi błędów, buforowanie, konfiguracji, autoryzacji i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="4a417-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="4a417-111">Filtry uniknąć duplikowania kodu.</span><span class="sxs-lookup"><span data-stu-id="4a417-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="4a417-112">Na przykład błąd obsługi filtra wyjątku można skonsolidować obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="4a417-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="4a417-113">Ten dokument ma zastosowanie do stron Razor, kontrolery interfejsu API i kontrolery, za pomocą widoków.</span><span class="sxs-lookup"><span data-stu-id="4a417-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="4a417-114">[Wyświetlanie lub pobieranie przykładowych](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4a417-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="4a417-115">Jak działa filtr</span><span class="sxs-lookup"><span data-stu-id="4a417-115">How filters work</span></span>

<span data-ttu-id="4a417-116">Filtry są uruchamiane w ramach *potok wywołania akcji programu ASP.NET Core*nazywanych czasami *potoku filtru*.</span><span class="sxs-lookup"><span data-stu-id="4a417-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="4a417-117">Potok filtru jest uruchamiany po platformy ASP.NET Core wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="4a417-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, Routing oprogramowanie pośredniczące, wybór akcji i potoku wywołania akcji programu ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="4a417-120">Filtruj typy</span><span class="sxs-lookup"><span data-stu-id="4a417-120">Filter types</span></span>

<span data-ttu-id="4a417-121">Każdy typ filtru jest wykonywane na różnych etapach potoku filtru:</span><span class="sxs-lookup"><span data-stu-id="4a417-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="4a417-122">[Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do ustalenia, czy użytkownik jest autoryzowany dla żądania.</span><span class="sxs-lookup"><span data-stu-id="4a417-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="4a417-123">Filtry autoryzacji zwarcie potoku, jeśli żądanie jest nieautoryzowane.</span><span class="sxs-lookup"><span data-stu-id="4a417-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="4a417-124">[Filtry zasobów](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="4a417-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="4a417-125">Uruchom po autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4a417-125">Run after authorization.</span></span>  
  * <span data-ttu-id="4a417-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> można uruchomić kod przed pozostałego potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="4a417-127">Na przykład `OnResourceExecuting` można uruchomić kod przed powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="4a417-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="4a417-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> można uruchomić kod po ukończeniu pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="4a417-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="4a417-129">[Filtry akcji](#action-filters) może uruchamiać kod bezpośrednio przed i po jest wywoływana z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="4a417-130">One może służyć do manipulowania Argumenty przekazane do akcji i wyniku zwracanego z akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="4a417-131">Filtry akcji są **nie** obsługiwane w przypadku stron Razor.</span><span class="sxs-lookup"><span data-stu-id="4a417-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="4a417-132">[Filtry wyjątków](#exception-filters) jest używana do stosowania zasad globalnych do nieobsługiwanych wyjątków występujące przed niczego zostały zapisane w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4a417-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="4a417-133">[Wynik filtry](#result-filters) może uruchamiać kod bezpośrednio przed i po wykonaniu wyniki poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="4a417-134">Działają one tylko wtedy, gdy metoda akcji zostało wykonane pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="4a417-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="4a417-135">Są one przydatne dla logiki, które należy otoczyć wykonywanie widoku lub elementu formatującego.</span><span class="sxs-lookup"><span data-stu-id="4a417-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="4a417-136">Na poniższym diagramie przedstawiono sposób interakcji typów filtrów w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Żądanie jest przetwarzane przez filtry autoryzacji, filtrów zasobów, powiązanie modelu, filtry akcji, wykonywanie akcji i konwersji wynik akcji, filtry wyjątków, filtry wyników i wynik wykonywania.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="4a417-139">Implementacja</span><span class="sxs-lookup"><span data-stu-id="4a417-139">Implementation</span></span>

<span data-ttu-id="4a417-140">Filtry obsługuje synchroniczne i asynchroniczne implementacji za pośrednictwem interfejsu w różnych definicjach.</span><span class="sxs-lookup"><span data-stu-id="4a417-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="4a417-141">Synchroniczne filtrów można uruchomić kod przed (`On-Stage-Executing`) oraz po (`On-Stage-Executed`) ich etap potoku.</span><span class="sxs-lookup"><span data-stu-id="4a417-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="4a417-142">Na przykład `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="4a417-143">`OnActionExecuted` jest wywoływana po powrocie z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="4a417-144">Zdefiniuj filtry asynchroniczne `On-Stage-ExecutionAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="4a417-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="4a417-145">W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który jest wykonywany metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="4a417-146">Każdy z `On-Stage-ExecutionAsync` metody przyjmują `FilterType-ExecutionDelegate` , który jest wykonywany etap potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="4a417-147">Wiele etapów filtru</span><span class="sxs-lookup"><span data-stu-id="4a417-147">Multiple filter stages</span></span>

<span data-ttu-id="4a417-148">Interfejsy na wiele etapów filtr można zaimplementować w jednej klasie.</span><span class="sxs-lookup"><span data-stu-id="4a417-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="4a417-149">Na przykład <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> klasy implementuje `IActionFilter`, `IResultFilter`i ich odpowiedniki async.</span><span class="sxs-lookup"><span data-stu-id="4a417-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="4a417-150">Implementowanie **albo** synchronicznej lub asynchronicznej wersję interfejsu filtru, **nie** jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="4a417-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="4a417-151">Środowisko uruchomieniowe sprawdza najpierw Jeśli filtr implementuje interfejs asynchroniczne, a jeśli tak jest, wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="4a417-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="4a417-152">W przeciwnym razie wywołuje metody synchronicznej interfejsu.</span><span class="sxs-lookup"><span data-stu-id="4a417-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="4a417-153">Jeśli synchronicznego i asynchronicznego interfejsy są implementowane w jedną klasę, jest wywoływana tylko metody asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="4a417-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="4a417-154">Podczas używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> przesłonić tylko synchroniczne metody lub metody asynchronicznej dla każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="4a417-155">Atrybuty filtru wbudowane</span><span class="sxs-lookup"><span data-stu-id="4a417-155">Built-in filter attributes</span></span>

<span data-ttu-id="4a417-156">ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które należy do podklasy i dostosowane.</span><span class="sxs-lookup"><span data-stu-id="4a417-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="4a417-157">Na przykład następujący filtr wyników dodaje do odpowiedzi nagłówek:</span><span class="sxs-lookup"><span data-stu-id="4a417-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="4a417-158">Atrybuty zezwalać na filtry przyjmowały argumenty, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4a417-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="4a417-159">Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:</span><span class="sxs-lookup"><span data-stu-id="4a417-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="4a417-160">Kilka interfejsów filtr ma odpowiednie atrybuty, które może służyć jako klay bazowe dla niestandardowych implementacji.</span><span class="sxs-lookup"><span data-stu-id="4a417-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="4a417-161">Atrybuty filtru:</span><span class="sxs-lookup"><span data-stu-id="4a417-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="4a417-162">Filtr zakresów i kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="4a417-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="4a417-163">Filtr można dodać do potoku w jednej z trzech *zakresy*:</span><span class="sxs-lookup"><span data-stu-id="4a417-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="4a417-164">Za pomocą atrybutu w akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="4a417-165">Za pomocą atrybutu na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="4a417-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="4a417-166">Globalnie dla wszystkich kontrolerów i akcji, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="4a417-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="4a417-167">Poprzedzający kod dodaje trzy filtry zasięg globalny dzięki [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) kolekcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="4a417-168">Domyślna kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="4a417-168">Default order of execution</span></span>

<span data-ttu-id="4a417-169">Gdy istnieje wiele filtrów dla danego etapu potoku, zakres Określa domyślną kolejność wykonywania filtrów.</span><span class="sxs-lookup"><span data-stu-id="4a417-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="4a417-170">Filtry globalne Otocz filtrów klasy, które z kolei Otocz metoda filtrów.</span><span class="sxs-lookup"><span data-stu-id="4a417-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="4a417-171">W wyniku zagnieżdżanie filtru *po* kod filtrów, który jest uruchamiany w odwrotnej kolejności *przed* kodu.</span><span class="sxs-lookup"><span data-stu-id="4a417-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="4a417-172">Sekwencja filtru:</span><span class="sxs-lookup"><span data-stu-id="4a417-172">The filter sequence:</span></span>

* <span data-ttu-id="4a417-173">*Przed* kodu filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="4a417-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="4a417-174">*Przed* kodu filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4a417-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="4a417-175">*Przed* kodu filtrów metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="4a417-176">*Po* kodu filtrów metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="4a417-177">*Po* kodu filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4a417-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="4a417-178">*Po* kodu filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="4a417-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="4a417-179">Poniższy przykład ilustruje zamówienia w filtrze, które metody są wywoływane dla filtrów akcji synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="4a417-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="4a417-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="4a417-180">Sequence</span></span> | <span data-ttu-id="4a417-181">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="4a417-181">Filter scope</span></span> | <span data-ttu-id="4a417-182">Filter — metoda</span><span class="sxs-lookup"><span data-stu-id="4a417-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="4a417-183">1</span><span class="sxs-lookup"><span data-stu-id="4a417-183">1</span></span> | <span data-ttu-id="4a417-184">Global</span><span class="sxs-lookup"><span data-stu-id="4a417-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="4a417-185">2</span><span class="sxs-lookup"><span data-stu-id="4a417-185">2</span></span> | <span data-ttu-id="4a417-186">Kontroler</span><span class="sxs-lookup"><span data-stu-id="4a417-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="4a417-187">3</span><span class="sxs-lookup"><span data-stu-id="4a417-187">3</span></span> | <span data-ttu-id="4a417-188">Metoda</span><span class="sxs-lookup"><span data-stu-id="4a417-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="4a417-189">4</span><span class="sxs-lookup"><span data-stu-id="4a417-189">4</span></span> | <span data-ttu-id="4a417-190">Metoda</span><span class="sxs-lookup"><span data-stu-id="4a417-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="4a417-191">5</span><span class="sxs-lookup"><span data-stu-id="4a417-191">5</span></span> | <span data-ttu-id="4a417-192">Kontroler</span><span class="sxs-lookup"><span data-stu-id="4a417-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="4a417-193">6</span><span class="sxs-lookup"><span data-stu-id="4a417-193">6</span></span> | <span data-ttu-id="4a417-194">Global</span><span class="sxs-lookup"><span data-stu-id="4a417-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="4a417-195">Ta sekwencja zawiera:</span><span class="sxs-lookup"><span data-stu-id="4a417-195">This sequence shows:</span></span>

* <span data-ttu-id="4a417-196">Filtr metody jest zagnieżdżona w filtrze kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4a417-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="4a417-197">Filtr kontrolera jest zagnieżdżony w filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="4a417-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="4a417-198">Filtry na poziomie kontrolera i strona Razor</span><span class="sxs-lookup"><span data-stu-id="4a417-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="4a417-199">Każdy kontroler, który dziedziczy z <xref:Microsoft.AspNetCore.Mvc.Controller> zawiera klasę bazową [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), i [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*) 
 `OnActionExecuted` metody.</span><span class="sxs-lookup"><span data-stu-id="4a417-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="4a417-200">Te metody:</span><span class="sxs-lookup"><span data-stu-id="4a417-200">These methods:</span></span>

* <span data-ttu-id="4a417-201">Otoczenie systemem filtry dla danej akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="4a417-202">`OnActionExecuting` jest wywoływana przed każdą filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="4a417-203">`OnActionExecuted` jest wywoływana po wszystkie filtry akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="4a417-204">`OnActionExecutionAsync` jest wywoływana przed każdą filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="4a417-205">Kod w filtrze po `next` jest uruchamiany po metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="4a417-206">Na przykład, w tym przykładzie pobierania `MySampleActionFilter` jest stosowane globalnie w uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="4a417-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="4a417-207">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="4a417-207">The `TestController`:</span></span>

* <span data-ttu-id="4a417-208">Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do `FilterTest2` akcji:</span><span class="sxs-lookup"><span data-stu-id="4a417-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="4a417-209">Zastępuje `OnActionExecuting` i `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="4a417-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="4a417-210">Przejdź do `https://localhost:5001/Test/FilterTest2` uruchomieniu następujący kod:</span><span class="sxs-lookup"><span data-stu-id="4a417-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="4a417-211">Dla stron Razor, zobacz [filtry stron Razor Implementowanie poprzez zastąpienie metody filtr](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="4a417-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="4a417-212">Zastępowanie domyślna kolejność</span><span class="sxs-lookup"><span data-stu-id="4a417-212">Overriding the default order</span></span>

<span data-ttu-id="4a417-213">Domyślną sekwencją wykonanie może być zastąpiona przez Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="4a417-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="4a417-214">`IOrderedFilter` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> właściwość, która ma pierwszeństwo przed zakresu, aby określić kolejność wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4a417-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="4a417-215">Filtr o niższych `Order` wartość:</span><span class="sxs-lookup"><span data-stu-id="4a417-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="4a417-216">Przebiegi *przed* kodu wcześniej filtr o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="4a417-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="4a417-217">Przebiegi *po* kodzie następnie filtr o wyższej `Order` wartość.</span><span class="sxs-lookup"><span data-stu-id="4a417-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="4a417-218">`Order` Właściwość można ustawić za pomocą parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="4a417-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="4a417-219">Należy wziąć pod uwagę tych samych filtrów akcji 3 pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4a417-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="4a417-220">Jeśli `Order` właściwość kontrolera i filtrów globalnych jest ustawiona na 1 i 2, odpowiednio, kolejność wykonywania została odwrócona.</span><span class="sxs-lookup"><span data-stu-id="4a417-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="4a417-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="4a417-221">Sequence</span></span> | <span data-ttu-id="4a417-222">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="4a417-222">Filter scope</span></span> | <span data-ttu-id="4a417-223">`Order` Właściwość</span><span class="sxs-lookup"><span data-stu-id="4a417-223">`Order` property</span></span> | <span data-ttu-id="4a417-224">Filter — metoda</span><span class="sxs-lookup"><span data-stu-id="4a417-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="4a417-225">1</span><span class="sxs-lookup"><span data-stu-id="4a417-225">1</span></span> | <span data-ttu-id="4a417-226">Metoda</span><span class="sxs-lookup"><span data-stu-id="4a417-226">Method</span></span> | <span data-ttu-id="4a417-227">0</span><span class="sxs-lookup"><span data-stu-id="4a417-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="4a417-228">2</span><span class="sxs-lookup"><span data-stu-id="4a417-228">2</span></span> | <span data-ttu-id="4a417-229">Kontroler</span><span class="sxs-lookup"><span data-stu-id="4a417-229">Controller</span></span> | <span data-ttu-id="4a417-230">1</span><span class="sxs-lookup"><span data-stu-id="4a417-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="4a417-231">3</span><span class="sxs-lookup"><span data-stu-id="4a417-231">3</span></span> | <span data-ttu-id="4a417-232">Global</span><span class="sxs-lookup"><span data-stu-id="4a417-232">Global</span></span> | <span data-ttu-id="4a417-233">2</span><span class="sxs-lookup"><span data-stu-id="4a417-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="4a417-234">4</span><span class="sxs-lookup"><span data-stu-id="4a417-234">4</span></span> | <span data-ttu-id="4a417-235">Global</span><span class="sxs-lookup"><span data-stu-id="4a417-235">Global</span></span> | <span data-ttu-id="4a417-236">2</span><span class="sxs-lookup"><span data-stu-id="4a417-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="4a417-237">5</span><span class="sxs-lookup"><span data-stu-id="4a417-237">5</span></span> | <span data-ttu-id="4a417-238">Kontroler</span><span class="sxs-lookup"><span data-stu-id="4a417-238">Controller</span></span> | <span data-ttu-id="4a417-239">1</span><span class="sxs-lookup"><span data-stu-id="4a417-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="4a417-240">6</span><span class="sxs-lookup"><span data-stu-id="4a417-240">6</span></span> | <span data-ttu-id="4a417-241">Metoda</span><span class="sxs-lookup"><span data-stu-id="4a417-241">Method</span></span> | <span data-ttu-id="4a417-242">0</span><span class="sxs-lookup"><span data-stu-id="4a417-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="4a417-243">`Order` Właściwość zastępuje zakres podczas ustalania kolejności, w której filtry są uruchamiane.</span><span class="sxs-lookup"><span data-stu-id="4a417-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="4a417-244">Filtry są najpierw posortowane według kolejności, a następnie do przerwania ties jest używany zakres.</span><span class="sxs-lookup"><span data-stu-id="4a417-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="4a417-245">Spośród filtrów wbudowanych implementują `IOrderedFilter` i Ustaw domyślną `Order` wartość 0.</span><span class="sxs-lookup"><span data-stu-id="4a417-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="4a417-246">Wbudowane filtry, zakres Określa kolejność, chyba że `Order` jest ustawiona na wartość inną niż zero.</span><span class="sxs-lookup"><span data-stu-id="4a417-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="4a417-247">Anulowanie i zwarcie</span><span class="sxs-lookup"><span data-stu-id="4a417-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="4a417-248">Może być short-circuited potoku filtru, ustawiając <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametr do metody filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="4a417-249">Na przykład następujący filtr zasobu uniemożliwia wykonywanie pozostałego potoku:</span><span class="sxs-lookup"><span data-stu-id="4a417-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="4a417-250">W poniższym kodzie zarówno `ShortCircuitingResourceFilter` i `AddHeader` filtr docelowy `SomeResource` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="4a417-251">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="4a417-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="4a417-252">Jest uruchamiany po pierwsze, ponieważ filtr zasobów i `AddHeader` jest filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="4a417-253">Short-Circuits pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="4a417-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="4a417-254">W związku z tym `AddHeader` filtr nigdy nie będzie uruchamiany dla `SomeResource` akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="4a417-255">To zachowanie będzie taki sam w przypadku obu filtrów były stosowane na poziomie metody akcji, podany `ShortCircuitingResourceFilter` uruchomiono pierwszy.</span><span class="sxs-lookup"><span data-stu-id="4a417-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="4a417-256">`ShortCircuitingResourceFilter` Uruchamia pierwszy ze względu na jej typ filtru lub przez jawne użycie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="4a417-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="4a417-257">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="4a417-257">Dependency injection</span></span>

<span data-ttu-id="4a417-258">Filtry można dodać według typu lub wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="4a417-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="4a417-259">Jeśli wystąpienie zostanie dodany, że wystąpienie jest używane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="4a417-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="4a417-260">Jeśli typ jest dodawany, jest typ aktywowany.</span><span class="sxs-lookup"><span data-stu-id="4a417-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="4a417-261">Oznacza typ aktywowany filtru:</span><span class="sxs-lookup"><span data-stu-id="4a417-261">A type-activated filter means:</span></span>

* <span data-ttu-id="4a417-262">Wystąpienie jest tworzone dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="4a417-262">An instance is created for each request.</span></span>
* <span data-ttu-id="4a417-263">Wszelkie zależności konstruktora są wypełniane przez [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="4a417-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="4a417-264">Filtry, które są zaimplementowane jako atrybuty i dodawane bezpośrednio do klasy kontrolera lub metody akcji nie może mieć konstruktora zależności, dostarczone przez [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="4a417-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="4a417-265">Nie można podać zależności Konstruktor przez DI, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="4a417-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="4a417-266">Atrybuty musi mieć ich parametry konstruktora dostarczane, gdy są one stosowane.</span><span class="sxs-lookup"><span data-stu-id="4a417-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="4a417-267">Jest to ograniczenie o współdziałaniu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="4a417-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="4a417-268">Poniższe filtry obsługują zależności Konstruktor udostępnionych w serwisie DI:</span><span class="sxs-lookup"><span data-stu-id="4a417-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="4a417-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowane w atrybucie.</span><span class="sxs-lookup"><span data-stu-id="4a417-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="4a417-270">Poprzedni filtrów można zastosować do kontrolera lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="4a417-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="4a417-271">Rejestratory są dostępne z DI.</span><span class="sxs-lookup"><span data-stu-id="4a417-271">Loggers are available from DI.</span></span> <span data-ttu-id="4a417-272">Jednak należy unikać tworzenia i używania filtrów służyć wyłącznie do celów rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="4a417-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="4a417-273">[Wbudowana struktura rejestrowania](xref:fundamentals/logging/index) zwykle zapewnia, co jest potrzebne do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="4a417-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="4a417-274">Rejestrowanie dodane do filtrów:</span><span class="sxs-lookup"><span data-stu-id="4a417-274">Logging added to filters:</span></span>

* <span data-ttu-id="4a417-275">Należy skoncentrować się na potencjalne problemy biznesowe domeny lub zachowania specyficzne dla filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="4a417-276">Należy **nie** dziennika akcji lub inne zdarzenia framework.</span><span class="sxs-lookup"><span data-stu-id="4a417-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="4a417-277">Wbudowane filtry dziennika akcji i zdarzenia framework.</span><span class="sxs-lookup"><span data-stu-id="4a417-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="4a417-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="4a417-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="4a417-279">Typy wdrożenia filtrów usługi są zarejestrowane w usłudze `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4a417-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="4a417-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> pobiera wystąpienia filtru z DI.</span><span class="sxs-lookup"><span data-stu-id="4a417-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="4a417-281">Poniższy kod przedstawia `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="4a417-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="4a417-282">W poniższym kodzie `AddHeaderResultServiceFilter` zostanie dodany do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="4a417-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="4a417-283">W poniższym kodzie `ServiceFilter` atrybut pobiera wystąpienia `AddHeaderResultServiceFilter` filtr z DI:</span><span class="sxs-lookup"><span data-stu-id="4a417-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="4a417-284">[ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="4a417-284">[ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="4a417-285">Stanowi wskazówkę wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu.</span><span class="sxs-lookup"><span data-stu-id="4a417-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="4a417-286">Środowisko uruchomieniowe programu ASP.NET Core nie gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="4a417-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="4a417-287">Czy zostaną utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="4a417-288">Filtr nie będzie ponowne żądanie z kontenera DI w pewnym momencie później.</span><span class="sxs-lookup"><span data-stu-id="4a417-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="4a417-289">Nie można używać z filtrem, który zależy od usługi z okresem istnienia innego niż pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="4a417-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="4a417-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="4a417-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="4a417-291">`IFilterFactory` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metodę tworzenia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="4a417-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="4a417-292">`CreateInstance` Ładuje określony typ z DI.</span><span class="sxs-lookup"><span data-stu-id="4a417-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="4a417-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="4a417-293">TypeFilterAttribute</span></span>

<span data-ttu-id="4a417-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> jest podobny do <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie zostanie rozwiązany bezpośrednio w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="4a417-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="4a417-295">Tworzy wystąpienie typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="4a417-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="4a417-296">Ponieważ `TypeFilterAttribute` typy nie są rozwiązane bezpośrednio w kontenerze DI:</span><span class="sxs-lookup"><span data-stu-id="4a417-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="4a417-297">Typy, które są wywoływane przy użyciu `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu kontenera DI.</span><span class="sxs-lookup"><span data-stu-id="4a417-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="4a417-298">Mają zależności są spełnione przez kontener DI.</span><span class="sxs-lookup"><span data-stu-id="4a417-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="4a417-299">`TypeFilterAttribute` Opcjonalnie można zaakceptować argumentów konstruktora dla typu.</span><span class="sxs-lookup"><span data-stu-id="4a417-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="4a417-300">Korzystając z `TypeFilterAttribute`, ustawiając `IsReusable` jest to wskazówka, wystąpienie filtru *może* być ponownie używane poza zakres żądania został utworzony w ciągu.</span><span class="sxs-lookup"><span data-stu-id="4a417-300">When using `TypeFilterAttribute`, setting `IsReusable` is a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="4a417-301">Środowisko uruchomieniowe programu ASP.NET Core zapewnia żadnej gwarancji, które zostaną utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-301">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span> <span data-ttu-id="4a417-302">`IsReusable` Nie można używać z filtrem, który zależy od usługi z okresem istnienia innego niż pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="4a417-302">`IsReusable` should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="4a417-303">Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="4a417-303">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="4a417-304">Filtry autoryzacji</span><span class="sxs-lookup"><span data-stu-id="4a417-304">Authorization filters</span></span>

<span data-ttu-id="4a417-305">Filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="4a417-305">Authorization filters:</span></span>

* <span data-ttu-id="4a417-306">Czy pierwszy filtry są uruchamiane w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-306">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="4a417-307">Kontrola dostępu do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-307">Control access to action methods.</span></span>
* <span data-ttu-id="4a417-308">Masz przed metodą, ale nie po metodzie.</span><span class="sxs-lookup"><span data-stu-id="4a417-308">Have a before method, but no after method.</span></span>

<span data-ttu-id="4a417-309">Filtry autoryzacji niestandardowe wymagają framework autoryzacja niestandardowa.</span><span class="sxs-lookup"><span data-stu-id="4a417-309">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="4a417-310">Preferuj Konfigurowanie zasad autoryzacji lub zapisu niestandardowych zasad autoryzacji za pośrednictwem pisania niestandardowego filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-310">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="4a417-311">Filtr autoryzacji wbudowane:</span><span class="sxs-lookup"><span data-stu-id="4a417-311">The built-in authorization filter:</span></span>

* <span data-ttu-id="4a417-312">Wywołuje system autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4a417-312">Calls the authorization system.</span></span>
* <span data-ttu-id="4a417-313">Nie zezwalają na żądania.</span><span class="sxs-lookup"><span data-stu-id="4a417-313">Does not authorize requests.</span></span>

<span data-ttu-id="4a417-314">Czy **nie** zgłaszają wyjątki w ramach filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="4a417-314">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="4a417-315">Wyjątek nie będzie obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="4a417-315">The exception will not be handled.</span></span>
* <span data-ttu-id="4a417-316">Filtry wyjątków nie będzie obsługiwać wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4a417-316">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="4a417-317">Należy wziąć pod uwagę, wydawanie wyzwanie w przypadku, gdy wystąpi wyjątek w filtru autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4a417-317">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="4a417-318">Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="4a417-318">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="4a417-319">Filtry zasobów</span><span class="sxs-lookup"><span data-stu-id="4a417-319">Resource filters</span></span>

<span data-ttu-id="4a417-320">Filtry zasobów:</span><span class="sxs-lookup"><span data-stu-id="4a417-320">Resource filters:</span></span>

* <span data-ttu-id="4a417-321">Implementowanie albo <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="4a417-321">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="4a417-322">Wykonanie opakowuje większość potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-322">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="4a417-323">Tylko [filtry autoryzacji](#authorization-filters) uruchamiane przed filtrami zasobów.</span><span class="sxs-lookup"><span data-stu-id="4a417-323">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="4a417-324">Filtry zasobów są przydatne do zwarcie większość potoku.</span><span class="sxs-lookup"><span data-stu-id="4a417-324">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="4a417-325">Na przykład filtr pamięci podręcznej można uniknąć pozostałego potoku na trafień pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="4a417-325">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="4a417-326">Przykłady filtrów zasobów:</span><span class="sxs-lookup"><span data-stu-id="4a417-326">Resource filter examples:</span></span>

* <span data-ttu-id="4a417-327">[Filtr zasobu short-circuiting](#short-circuiting-resource-filter) przedstawionego wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4a417-327">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="4a417-328">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="4a417-328">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="4a417-329">Zapobiega wiązania modelu dostępu do danych formularza.</span><span class="sxs-lookup"><span data-stu-id="4a417-329">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="4a417-330">Używane przekazywania plików o dużym rozmiarze, aby zapobiec wczytywana pamięci danych formularza.</span><span class="sxs-lookup"><span data-stu-id="4a417-330">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="4a417-331">Filtry akcji</span><span class="sxs-lookup"><span data-stu-id="4a417-331">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a417-332">Filtry akcji wykonaj **nie** dotyczą stron Razor.</span><span class="sxs-lookup"><span data-stu-id="4a417-332">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="4a417-333">Strony razor obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span><span class="sxs-lookup"><span data-stu-id="4a417-333">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="4a417-334">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="4a417-334">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="4a417-335">Filtry akcji:</span><span class="sxs-lookup"><span data-stu-id="4a417-335">Action filters:</span></span>

* <span data-ttu-id="4a417-336">Implementowanie albo <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interfejsu.</span><span class="sxs-lookup"><span data-stu-id="4a417-336">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="4a417-337">Ich wykonanie otacza wykonywania metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-337">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="4a417-338">Poniższy kod przedstawia przykładowy filtr akcji:</span><span class="sxs-lookup"><span data-stu-id="4a417-338">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="4a417-339"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> Zawiera następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="4a417-339">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="4a417-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — Umożliwia odczytanie danych wejściowych do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="4a417-341"><xref:Microsoft.AspNetCore.Mvc.Controller> — umożliwia manipulowanie wystąpienie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4a417-341"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="4a417-342"><xref:System.Web.Mvc.ActionExecutingContext.Result> -ustawienie `Result` short-circuits wykonywania metody akcji i filtry kolejnych działań.</span><span class="sxs-lookup"><span data-stu-id="4a417-342"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="4a417-343">Zostanie zgłoszony wyjątek w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="4a417-343">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="4a417-344">Uniemożliwia uruchamianie kolejne filtry.</span><span class="sxs-lookup"><span data-stu-id="4a417-344">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="4a417-345">W przeciwieństwie do ustawienia `Result`, jest traktowana jako błąd zamiast pomyślnego wyniku.</span><span class="sxs-lookup"><span data-stu-id="4a417-345">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="4a417-346"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> Zapewnia `Controller` i `Result` oraz następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="4a417-346">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="4a417-347"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> — Wartość true, jeśli zwartym został wykonanie akcji przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="4a417-347"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="4a417-348"><xref:System.Web.Mvc.ActionExecutedContext.Exception> Innych niż null w przypadku akcji lub filtru akcji wcześniej przebiegu zgłosiła wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4a417-348"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="4a417-349">Ustawienie tej właściwości na wartość null:</span><span class="sxs-lookup"><span data-stu-id="4a417-349">Setting this property to null:</span></span>

  * <span data-ttu-id="4a417-350">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4a417-350">Effectively handles the exception.</span></span>
  * <span data-ttu-id="4a417-351">`Result` jest wykonywane tak, jakby został zwrócony przez metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-351">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="4a417-352">Aby uzyskać `IAsyncActionFilter`, wywołanie <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="4a417-352">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="4a417-353">Wykonuje wszelkie filtry kolejnej akcji i metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-353">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="4a417-354">Zwraca `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="4a417-354">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="4a417-355">Aby zwarcie, należy przypisać <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> w wyniku wystąpienia i nie wywołuj `next` ( `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="4a417-355">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="4a417-356">Struktura dostarcza abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> , może być podklasą klasy.</span><span class="sxs-lookup"><span data-stu-id="4a417-356">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="4a417-357">`OnActionExecuting` Filtr akcji można używać do:</span><span class="sxs-lookup"><span data-stu-id="4a417-357">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="4a417-358">Sprawdź stan modelu.</span><span class="sxs-lookup"><span data-stu-id="4a417-358">Validate model state.</span></span>
* <span data-ttu-id="4a417-359">Zwraca błąd, jeśli stan jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="4a417-359">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="4a417-360">`OnActionExecuted` Metoda uruchamia się po metody akcji:</span><span class="sxs-lookup"><span data-stu-id="4a417-360">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="4a417-361">Można wyświetlić i manipulowania wynikami akcji za pomocą <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> właściwości.</span><span class="sxs-lookup"><span data-stu-id="4a417-361">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="4a417-362"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> ustawiono wartość true, jeśli jest to zwartym został wykonanie akcji przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="4a417-362"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="4a417-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> jest ustawiona na wartość inną niż null, jeśli akcji lub filtru akcji kolejnych zgłosiła wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4a417-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="4a417-364">Ustawienie `Exception` null:</span><span class="sxs-lookup"><span data-stu-id="4a417-364">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="4a417-365">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4a417-365">Effectively handles an exception.</span></span>
  * <span data-ttu-id="4a417-366">`ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie przez metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-366">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="4a417-367">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="4a417-367">Exception filters</span></span>

<span data-ttu-id="4a417-368">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="4a417-368">Exception filters:</span></span>

* <span data-ttu-id="4a417-369">Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="4a417-369">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="4a417-370">Może służyć do implementowania obsługi zasad typowych błędów.</span><span class="sxs-lookup"><span data-stu-id="4a417-370">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="4a417-371">Następujący filtr wyjątku przykładowych użyto widoku błędów niestandardowych, aby wyświetlić szczegóły dotyczące wyjątków, które występują, gdy aplikacja jest w trakcie opracowywania:</span><span class="sxs-lookup"><span data-stu-id="4a417-371">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="4a417-372">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="4a417-372">Exception filters:</span></span>

* <span data-ttu-id="4a417-373">Nie masz, przed i po nim zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="4a417-373">Don't have before and after events.</span></span>
* <span data-ttu-id="4a417-374">Implementowanie <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="4a417-374">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="4a417-375">Obsługa nieobsługiwanych wyjątków, które występują w stronę Razor lub tworzenia kontrolera [wiązanie modelu](xref:mvc/models/model-binding), filtry akcji lub metody akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-375">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="4a417-376">Czy **nie** przechwytywać wyjątków, które występują w filtrów zasobów, filtry wyników lub MVC wynik wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4a417-376">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="4a417-377">Aby obsłużyć wyjątek, należy ustawić <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> właściwości `true` lub zapisu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4a417-377">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="4a417-378">Spowoduje to zatrzymanie propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="4a417-378">This stops propagation of the exception.</span></span> <span data-ttu-id="4a417-379">Filtra wyjątku nie może włączyć wyjątek do "Powodzenie".</span><span class="sxs-lookup"><span data-stu-id="4a417-379">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="4a417-380">Filtr akcji można to zrobić.</span><span class="sxs-lookup"><span data-stu-id="4a417-380">Only an action filter can do that.</span></span>

<span data-ttu-id="4a417-381">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="4a417-381">Exception filters:</span></span>

* <span data-ttu-id="4a417-382">Dla zastosowań dobre są wyjątki wyłapywanie, które występują w ramach akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-382">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="4a417-383">Nie są tak elastyczne, oprogramowanie pośredniczące obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="4a417-383">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="4a417-384">Preferuj oprogramowanie pośredniczące do obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="4a417-384">Prefer middleware for exception handling.</span></span> <span data-ttu-id="4a417-385">Użyj wyjątek filtruje tylko wtedy, gdy obsługa błędów *różni się* oparte na jakiej metody akcji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="4a417-385">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="4a417-386">Na przykład aplikacja może mieć metody akcji dla obu punktów końcowych interfejsu API i widoki/HTML.</span><span class="sxs-lookup"><span data-stu-id="4a417-386">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="4a417-387">Punkty końcowe interfejsu API może zwrócić informacje o błędach w formacie JSON, natomiast akcje na podstawie widoku może zwrócić strony błędu w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="4a417-387">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="4a417-388">Filtry wyników</span><span class="sxs-lookup"><span data-stu-id="4a417-388">Result filters</span></span>

<span data-ttu-id="4a417-389">Filtry wyników:</span><span class="sxs-lookup"><span data-stu-id="4a417-389">Result filters:</span></span>

* <span data-ttu-id="4a417-390">Implementuj interfejs z:</span><span class="sxs-lookup"><span data-stu-id="4a417-390">Implement an interface:</span></span>
  * <span data-ttu-id="4a417-391"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="4a417-391"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="4a417-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="4a417-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="4a417-393">Ich wykonanie otacza wykonywania wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-393">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="4a417-394">IResultFilter i IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="4a417-394">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="4a417-395">Poniższy kod pokazuje filtr wynik, który dodaje nagłówek HTTP:</span><span class="sxs-lookup"><span data-stu-id="4a417-395">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="4a417-396">Typ wyniku wykonywania zależy od akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-396">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="4a417-397">Zwracanie Widok akcji obejmuje wszystkie razor przetwarzania w ramach <xref:Microsoft.AspNetCore.Mvc.ViewResult> wykonywana.</span><span class="sxs-lookup"><span data-stu-id="4a417-397">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="4a417-398">Metoda interfejsu API może wykonywać niektóre serializacji jako część wykonania wyniku.</span><span class="sxs-lookup"><span data-stu-id="4a417-398">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="4a417-399">Dowiedz się więcej o [wyników akcji](xref:mvc/controllers/actions)</span><span class="sxs-lookup"><span data-stu-id="4a417-399">Learn more about [action results](xref:mvc/controllers/actions)</span></span>

<span data-ttu-id="4a417-400">Filtry wyników są wykonywane tylko na pomyślne wyniki — gdy akcji lub filtry akcji dają wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-400">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="4a417-401">Filtry wyników nie są wykonywane, gdy filtry wyjątków obsługi wyjątku.</span><span class="sxs-lookup"><span data-stu-id="4a417-401">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="4a417-402"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> Metoda może zwarcie wykonywania wynik akcji i filtry kolejnych wyników, ustawiając <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> do `true`.</span><span class="sxs-lookup"><span data-stu-id="4a417-402">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="4a417-403">Zapisywana w obiekt odpowiedzi zwarcie, aby uniknąć generowania pustą odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="4a417-403">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="4a417-404">Zostanie zgłoszony wyjątek `IResultFilter.OnResultExecuting` będzie:</span><span class="sxs-lookup"><span data-stu-id="4a417-404">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="4a417-405">Zapobiec wykonaniu wyniku akcji i kolejne filtry.</span><span class="sxs-lookup"><span data-stu-id="4a417-405">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="4a417-406">Traktowane jako błąd zamiast pomyślnego wyniku.</span><span class="sxs-lookup"><span data-stu-id="4a417-406">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="4a417-407">Gdy <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> uruchamia metody:</span><span class="sxs-lookup"><span data-stu-id="4a417-407">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="4a417-408">Odpowiedź, prawdopodobnie została wysłana do klienta i nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="4a417-408">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="4a417-409">Jeśli wystąpił wyjątek, treść odpowiedzi nie są wysyłane.</span><span class="sxs-lookup"><span data-stu-id="4a417-409">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="4a417-410">`ResultExecutedContext.Canceled` ustawiono `true` Jeśli zwartym został wykonanie wynik akcji przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="4a417-410">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="4a417-411">`ResultExecutedContext.Exception` jest ustawiona na wartość inną niż null, jeśli wynik akcji lub filtru kolejnych wyników zgłosiła wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4a417-411">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="4a417-412">Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia wyjątek z jest zgłaszany ponownie przez platformy ASP.NET Core w dalszej części procesu.</span><span class="sxs-lookup"><span data-stu-id="4a417-412">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="4a417-413">Brak niezawodnego sposobu zapisywania danych do odpowiedzi podczas obsługi wyjątku w filtrze wynik.</span><span class="sxs-lookup"><span data-stu-id="4a417-413">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="4a417-414">Jeśli ma został opróżniony nagłówki do klienta, gdy wynik akcji zgłasza wyjątek, nie istnieje mechanizm niezawodne wysłać kod błędu.</span><span class="sxs-lookup"><span data-stu-id="4a417-414">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="4a417-415">Aby uzyskać <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, wywołanie `await next` na <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie filtry kolejnych wyników i wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-415">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="4a417-416">Aby zwarcie, ustaw [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) do `true` i nie wywołuj `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="4a417-416">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="4a417-417">Struktura dostarcza abstrakcyjną `ResultFilterAttribute` , może być podklasą klasy.</span><span class="sxs-lookup"><span data-stu-id="4a417-417">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="4a417-418">[AddHeaderAttribute](#add-header-attribute) klasy wyświetlane wcześniej jest przykładem atrybutów filtru wyniku.</span><span class="sxs-lookup"><span data-stu-id="4a417-418">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="4a417-419">IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="4a417-419">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="4a417-420"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> zadeklarować interfejsów <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementację, która jest uruchamiana dla wszystkich wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-420">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="4a417-421">Filtr jest stosowany do wszystkich wyników akcji, chyba że:</span><span class="sxs-lookup"><span data-stu-id="4a417-421">The filter is applied to all action results unless:</span></span>

* <span data-ttu-id="4a417-422"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> Lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> ma zastosowanie i short-circuits odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4a417-422">An <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>
* <span data-ttu-id="4a417-423">Filtra wyjątku obsługuje wyjątek, przedstawiając wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-423">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="4a417-424">Filtry w innych niż `IExceptionFilter` i `IAuthorizationFilter` nie powodują pominięcie `IAlwaysRunResultFilter` i `IAsyncAlwaysRunResultFilter`.</span><span class="sxs-lookup"><span data-stu-id="4a417-424">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short-circuit `IAlwaysRunResultFilter` and `IAsyncAlwaysRunResultFilter`.</span></span>

<span data-ttu-id="4a417-425">Na przykład następujący filtr zawsze uruchamia i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) za pomocą *422 brakuje jednostki* kod stanu niepowodzenia negocjacji zawartości:</span><span class="sxs-lookup"><span data-stu-id="4a417-425">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="4a417-426">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="4a417-426">IFilterFactory</span></span>

<span data-ttu-id="4a417-427"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="4a417-427"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="4a417-428">W związku z tym `IFilterFactory` wystąpienia mogą być używane jako `IFilterMetadata` wystąpienia w dowolnym miejscu w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-428">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="4a417-429">Gdy środowisko uruchomieniowe przygotowuje się do wywołania z filtrem, próbuje rzutować go na `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="4a417-429">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="4a417-430">Jeśli tego rzutowania zakończy się powodzeniem, <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metoda jest wywoływana, aby utworzyć `IFilterMetadata` wystąpienia, które jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="4a417-430">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="4a417-431">Dzięki temu elastycznością, ponieważ potoku filtru dokładne nie muszą być ustawiony w sposób jawny po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4a417-431">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="4a417-432">`IFilterFactory` można zaimplementować przy użyciu atrybutów niestandardowych implementacji jako innego podejścia do tworzenia filtrów:</span><span class="sxs-lookup"><span data-stu-id="4a417-432">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="4a417-433">Powyższy kod mogą być testowane przez uruchomienie [Pobierz przykładowe](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="4a417-433">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="4a417-434">Wywoływanie narzędzia programistyczne F12.</span><span class="sxs-lookup"><span data-stu-id="4a417-434">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="4a417-435">Przejdź do `https://localhost:5001/Sample/HeaderWithFactory`</span><span class="sxs-lookup"><span data-stu-id="4a417-435">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="4a417-436">Narzędzia programistyczne F12 wyświetlić następujące nagłówki odpowiedzi dodany przez kod przykładowy:</span><span class="sxs-lookup"><span data-stu-id="4a417-436">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="4a417-437">**Autor:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="4a417-437">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="4a417-438">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="4a417-438">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="4a417-439">**Wewnętrzny:** `My header`</span><span class="sxs-lookup"><span data-stu-id="4a417-439">**internal:** `My header`</span></span>

<span data-ttu-id="4a417-440">Powyższy kod tworzy **wewnętrzny:** `My header` nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4a417-440">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="4a417-441">IFilterFactory implementowane w atrybucie</span><span class="sxs-lookup"><span data-stu-id="4a417-441">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="4a417-442">Filtry, które implementują `IFilterFactory` są przydatne w przypadku filtrów, które:</span><span class="sxs-lookup"><span data-stu-id="4a417-442">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="4a417-443">Nie wymagają przekazywanie parametrów.</span><span class="sxs-lookup"><span data-stu-id="4a417-443">Don't require passing parameters.</span></span>
* <span data-ttu-id="4a417-444">Mają zależności konstruktora, które muszą zostać wypełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="4a417-444">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="4a417-445"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="4a417-445"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="4a417-446">`IFilterFactory` udostępnia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> metodę tworzenia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="4a417-446">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="4a417-447">`CreateInstance` Ładuje określony typ z kontenera usług (DI).</span><span class="sxs-lookup"><span data-stu-id="4a417-447">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="4a417-448">Poniższy kod pokazuje trzy sposoby stosowania `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="4a417-448">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="4a417-449">W poprzednim kodzie urządzanie metody `[SampleActionFilter]` jest preferowanym sposobem stosowania `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="4a417-449">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="4a417-450">Za pomocą oprogramowania pośredniczącego w potoku filtru</span><span class="sxs-lookup"><span data-stu-id="4a417-450">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="4a417-451">Filtry zasobów działają podobnie jak [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) w tym, że ujęty wykonanie wszystkich elementów, których można użyć później w potoku.</span><span class="sxs-lookup"><span data-stu-id="4a417-451">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="4a417-452">Jednak filtrów różnią się od oprogramowania pośredniczącego, w tym, że są one częścią środowiska uruchomieniowego platformy ASP.NET Core, co oznacza, że mają dostęp do kontekstu platformy ASP.NET Core i konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="4a417-452">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="4a417-453">Aby korzystać z oprogramowania pośredniczącego jako filtru, Utwórz typ z `Configure` metody, która określa oprogramowania pośredniczącego, aby wstawić do potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="4a417-453">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="4a417-454">W poniższym przykładzie użyto oprogramowania pośredniczącego lokalizacji ustalenie bieżącej kultury na żądanie:</span><span class="sxs-lookup"><span data-stu-id="4a417-454">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="4a417-455">Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> do uruchamiania oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="4a417-455">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="4a417-456">Oprogramowanie pośredniczące filtry są uruchamiane na tym samym etapie potoku filtru jako zasób filtry, przed powiązaniem modelu i po nim pozostałego potoku.</span><span class="sxs-lookup"><span data-stu-id="4a417-456">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="4a417-457">Następne akcje</span><span class="sxs-lookup"><span data-stu-id="4a417-457">Next actions</span></span>

* <span data-ttu-id="4a417-458">Zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter)</span><span class="sxs-lookup"><span data-stu-id="4a417-458">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="4a417-459">Aby poeksperymentować z filtrami, [pobierania, testowanie i zmodyfikować przykładowe GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="4a417-459">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
