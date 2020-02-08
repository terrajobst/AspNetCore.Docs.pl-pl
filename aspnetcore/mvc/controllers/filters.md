---
title: Filtry w ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak działają filtry i jak korzystać z nich w ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
uid: mvc/controllers/filters
ms.openlocfilehash: c4bb9d5746e494106ead6ad5bbf972bbcc5a39f1
ms.sourcegitcommit: 0e21d4f8111743bcb205a2ae0f8e57910c3e8c25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/05/2020
ms.locfileid: "77034068"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="475f3-103">Filtry w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="475f3-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="475f3-104">[Kirka Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra/)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="475f3-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="475f3-105">*Filtry* w ASP.NET Core umożliwiają uruchamianie kodu przed określonymi etapami w potoku przetwarzania żądań lub po nich.</span><span class="sxs-lookup"><span data-stu-id="475f3-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="475f3-106">Filtry wbudowane obsługują zadania takie jak:</span><span class="sxs-lookup"><span data-stu-id="475f3-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="475f3-107">Autoryzacja (zapobieganie dostępowi do zasobów, dla których użytkownik nie jest autoryzowany).</span><span class="sxs-lookup"><span data-stu-id="475f3-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="475f3-108">Buforowanie odpowiedzi (krótkie obwody potoku żądania w celu zwrócenia buforowanej odpowiedzi).</span><span class="sxs-lookup"><span data-stu-id="475f3-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="475f3-109">Filtry niestandardowe mogą być tworzone w celu obsłużenia krzyżowego rozcinania.</span><span class="sxs-lookup"><span data-stu-id="475f3-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="475f3-110">Przykłady zagadnień związanych z rozcinaniem obejmują obsługę błędów, buforowanie, konfigurację, autoryzację i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="475f3-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="475f3-111">Filtry unikają duplikowania kodu.</span><span class="sxs-lookup"><span data-stu-id="475f3-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="475f3-112">Na przykład filtr wyjątków obsługujący błędy może skonsolidować obsługę błędów.</span><span class="sxs-lookup"><span data-stu-id="475f3-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="475f3-113">Ten dokument ma zastosowanie do Razor Pages, kontrolerów interfejsu API i kontrolerów z widokami.</span><span class="sxs-lookup"><span data-stu-id="475f3-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span> <span data-ttu-id="475f3-114">Filtry nie działają bezpośrednio ze [składnikami Razor](xref:blazor/components).</span><span class="sxs-lookup"><span data-stu-id="475f3-114">Filters don't work directly with [Razor components](xref:blazor/components).</span></span> <span data-ttu-id="475f3-115">Filtr może mieć tylko pośredni wpływ na składnik, gdy:</span><span class="sxs-lookup"><span data-stu-id="475f3-115">A filter can only indirectly affect a component when:</span></span>

* <span data-ttu-id="475f3-116">Składnik jest osadzony na stronie lub widoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-116">The component is embedded in a page or view.</span></span>
* <span data-ttu-id="475f3-117">Strona lub kontroler/widok używają filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-117">The page or controller/view uses the filter.</span></span>

<span data-ttu-id="475f3-118">[Wyświetl lub Pobierz przykład](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="475f3-118">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="475f3-119">Jak działają filtry</span><span class="sxs-lookup"><span data-stu-id="475f3-119">How filters work</span></span>

<span data-ttu-id="475f3-120">Filtry są uruchamiane w *potoku wywołania akcji ASP.NET Core*, czasami określane jako *potok filtru*.</span><span class="sxs-lookup"><span data-stu-id="475f3-120">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span> <span data-ttu-id="475f3-121">Potok filtru jest uruchamiany po ASP.NET Core wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="475f3-121">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, kierowanie oprogramowania pośredniczącego, wybór akcji i potok akcji wywołania.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="475f3-124">Typy filtrów</span><span class="sxs-lookup"><span data-stu-id="475f3-124">Filter types</span></span>

<span data-ttu-id="475f3-125">Każdy typ filtru jest wykonywany na innym etapie w potoku filtru:</span><span class="sxs-lookup"><span data-stu-id="475f3-125">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="475f3-126">[Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do określenia, czy użytkownik jest autoryzowany do żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-126">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="475f3-127">Filtry autoryzacji skracają obwód w przypadku, gdy żądanie nie jest autoryzowane.</span><span class="sxs-lookup"><span data-stu-id="475f3-127">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="475f3-128">[Filtry zasobów](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="475f3-128">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="475f3-129">Uruchom po autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-129">Run after authorization.</span></span>  
  * <span data-ttu-id="475f3-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> uruchamia kod przed resztą potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="475f3-131">Na przykład `OnResourceExecuting` uruchamia kod przed powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="475f3-131">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="475f3-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> uruchamia kod po zakończeniu reszty potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="475f3-133">[Filtry akcji](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="475f3-133">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="475f3-134">Uruchom kod bezpośrednio przed i po wywołaniu metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-134">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="475f3-135">Może zmienić argumenty przekazane do akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-135">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="475f3-136">Można zmienić wynik zwrócony z akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-136">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="475f3-137">**Nie** są obsługiwane w Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475f3-137">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="475f3-138">[Filtry wyjątków](#exception-filters) stosują zasady globalne do nieobsłużonych wyjątków, które występują przed zapisaniem treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="475f3-138">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="475f3-139">[Filtry wyników](#result-filters) uruchamiają kod bezpośrednio przed i po wykonaniu wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-139">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="475f3-140">Są one uruchamiane tylko wtedy, gdy metoda akcji została wykonana pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="475f3-140">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="475f3-141">Są one przydatne dla logiki, która musi być w trakcie wyświetlania lub wykonywania w programie formatującego.</span><span class="sxs-lookup"><span data-stu-id="475f3-141">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="475f3-142">Na poniższym diagramie przedstawiono sposób, w jaki typy filtrów współdziałają w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-142">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Żądanie jest przetwarzane przez filtry autoryzacji, filtry zasobów, powiązania modelu, filtry akcji, wykonywanie akcji i konwersję wyników akcji, filtry wyjątków, filtry wynikowe i wykonywanie wyniku.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="475f3-145">Wdrażanie</span><span class="sxs-lookup"><span data-stu-id="475f3-145">Implementation</span></span>

<span data-ttu-id="475f3-146">Filtry obsługują implementacje synchroniczne i asynchroniczne za pomocą różnych definicji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="475f3-146">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="475f3-147">Filtry synchroniczne uruchamiają kod przed i po fazie potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-147">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="475f3-148">Na przykład, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> jest wywoływana przed wywołaniem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-148">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="475f3-149"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> jest wywoływana po powrocie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-149"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="475f3-150">Filtry asynchroniczne definiują metodę `On-Stage-ExecutionAsync`.</span><span class="sxs-lookup"><span data-stu-id="475f3-150">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="475f3-151">Na przykład <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="475f3-151">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="475f3-152">W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który wykonuje metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-152">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="475f3-153">Wiele etapów filtrowania</span><span class="sxs-lookup"><span data-stu-id="475f3-153">Multiple filter stages</span></span>

<span data-ttu-id="475f3-154">Interfejsy dla wielu etapów filtru można zaimplementować w pojedynczej klasie.</span><span class="sxs-lookup"><span data-stu-id="475f3-154">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="475f3-155">Na przykład Klasa <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementuje:</span><span class="sxs-lookup"><span data-stu-id="475f3-155">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="475f3-156">Synchroniczne: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="475f3-156">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="475f3-157">Asynchronicznie: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="475f3-157">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="475f3-158">Implementowanie synchronicznej lub asynchronicznej wersji interfejsu filtru **, a** **nie** obu.</span><span class="sxs-lookup"><span data-stu-id="475f3-158">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="475f3-159">Środowisko uruchomieniowe najpierw sprawdza, czy filtr implementuje interfejs asynchroniczny, a jeśli tak, wywołuje to.</span><span class="sxs-lookup"><span data-stu-id="475f3-159">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="475f3-160">Jeśli nie, wywołuje metody interfejsu synchronicznego.</span><span class="sxs-lookup"><span data-stu-id="475f3-160">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="475f3-161">Jeśli zarówno interfejsy asynchroniczne, jak i synchroniczne są zaimplementowane w jednej klasie, wywoływana jest tylko Metoda async.</span><span class="sxs-lookup"><span data-stu-id="475f3-161">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="475f3-162">W przypadku używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, Zastąp tylko metody synchroniczne lub metodę asynchroniczną dla każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-162">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="475f3-163">Wbudowane atrybuty filtru</span><span class="sxs-lookup"><span data-stu-id="475f3-163">Built-in filter attributes</span></span>

<span data-ttu-id="475f3-164">ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które można podklasować i dostosowywać.</span><span class="sxs-lookup"><span data-stu-id="475f3-164">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="475f3-165">Na przykład poniższy filtr wynikowy dodaje nagłówek do odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="475f3-165">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="475f3-166">Atrybuty umożliwiają filtrom akceptowanie argumentów, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="475f3-166">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="475f3-167">Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:</span><span class="sxs-lookup"><span data-stu-id="475f3-167">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="475f3-168">Użyj narzędzia, takiego jak [Narzędzia deweloperskie przeglądarki](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) , aby przeanalizować nagłówki.</span><span class="sxs-lookup"><span data-stu-id="475f3-168">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="475f3-169">W obszarze **nagłówki odpowiedzi**zostanie wyświetlona `author: Rick Anderson`.</span><span class="sxs-lookup"><span data-stu-id="475f3-169">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="475f3-170">Poniższy kod implementuje `ActionFilterAttribute`, który:</span><span class="sxs-lookup"><span data-stu-id="475f3-170">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="475f3-171">Odczytuje tytuł i nazwę z systemu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="475f3-171">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="475f3-172">W przeciwieństwie do poprzedniego przykładu, poniższy kod nie wymaga dodania parametrów filtru do kodu.</span><span class="sxs-lookup"><span data-stu-id="475f3-172">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="475f3-173">Dodaje tytuł i nazwę do nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="475f3-173">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="475f3-174">Opcje konfiguracji są dostępne z [systemu konfiguracji](xref:fundamentals/configuration/index) przy użyciu [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="475f3-174">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="475f3-175">Na przykład, z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="475f3-175">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="475f3-176">W `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="475f3-176">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="475f3-177">Klasa `PositionOptions` jest dodawana do kontenera usługi z obszarem konfiguracji `"Position"`.</span><span class="sxs-lookup"><span data-stu-id="475f3-177">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="475f3-178">`MyActionFilterAttribute` zostanie dodany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="475f3-178">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="475f3-179">Poniższy kod przedstawia klasę `PositionOptions`:</span><span class="sxs-lookup"><span data-stu-id="475f3-179">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="475f3-180">Poniższy kod stosuje `MyActionFilterAttribute` do metody `Index2`:</span><span class="sxs-lookup"><span data-stu-id="475f3-180">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="475f3-181">W obszarze **nagłówki odpowiedzi**, `author: Rick Anderson`i `Editor: Joe Smith` jest wyświetlana po wywołaniu punktu końcowego `Sample/Index2`.</span><span class="sxs-lookup"><span data-stu-id="475f3-181">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="475f3-182">Poniższy kod stosuje `MyActionFilterAttribute` i `AddHeaderAttribute` do strony Razor:</span><span class="sxs-lookup"><span data-stu-id="475f3-182">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="475f3-183">Nie można zastosować filtrów do metod obsługi stron Razor.</span><span class="sxs-lookup"><span data-stu-id="475f3-183">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="475f3-184">Mogą być stosowane do modelu strony Razor lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="475f3-184">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="475f3-185">Kilka interfejsów filtrów ma odpowiednie atrybuty, które mogą być używane jako klasy bazowe dla implementacji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="475f3-185">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="475f3-186">Atrybuty filtru:</span><span class="sxs-lookup"><span data-stu-id="475f3-186">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="475f3-187">Zakresy filtrów i kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="475f3-187">Filter scopes and order of execution</span></span>

<span data-ttu-id="475f3-188">Filtr można dodać do potoku w jednym z trzech *zakresów*:</span><span class="sxs-lookup"><span data-stu-id="475f3-188">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="475f3-189">Użycie atrybutu w akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="475f3-189">Using an attribute on a controller action.</span></span> <span data-ttu-id="475f3-190">Nie można zastosować atrybutów filtru do metod obsługi Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475f3-190">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="475f3-191">Użycie atrybutu na kontrolerze lub stronie Razor.</span><span class="sxs-lookup"><span data-stu-id="475f3-191">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="475f3-192">Globalnie dla wszystkich kontrolerów, akcji i Razor Pages, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="475f3-192">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="475f3-193">Domyślna kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="475f3-193">Default order of execution</span></span>

<span data-ttu-id="475f3-194">Jeśli istnieje wiele filtrów dla określonego etapu potoku, zakres określa domyślną kolejność wykonywania filtrowania.</span><span class="sxs-lookup"><span data-stu-id="475f3-194">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="475f3-195">Filtry globalne Otocz filtry klas, które z kolei filtrują metody przestrzenne.</span><span class="sxs-lookup"><span data-stu-id="475f3-195">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="475f3-196">W wyniku zagnieżdżania filtrów, *po* kodzie filtrów działa w odwrotnej kolejności *przed* kodem.</span><span class="sxs-lookup"><span data-stu-id="475f3-196">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="475f3-197">Sekwencja filtru:</span><span class="sxs-lookup"><span data-stu-id="475f3-197">The filter sequence:</span></span>

* <span data-ttu-id="475f3-198">*Przed* kodem filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="475f3-198">The *before* code of global filters.</span></span>
  * <span data-ttu-id="475f3-199">*Przed* kodem i filtrem strony Razor.</span><span class="sxs-lookup"><span data-stu-id="475f3-199">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="475f3-200">Filtr *przed* kodem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-200">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="475f3-201">*Po* kodzie metody akcji filtry.</span><span class="sxs-lookup"><span data-stu-id="475f3-201">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="475f3-202">Kod *po* stronie kontroler i filtr strony Razor.</span><span class="sxs-lookup"><span data-stu-id="475f3-202">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="475f3-203">Kod *po* filtrach globalnych.</span><span class="sxs-lookup"><span data-stu-id="475f3-203">The *after* code of global filters.</span></span>
  
<span data-ttu-id="475f3-204">Poniższy przykład ilustruje kolejność, w której metody filtrowania są wywoływane dla synchronicznych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-204">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="475f3-205">Sequence</span><span class="sxs-lookup"><span data-stu-id="475f3-205">Sequence</span></span> | <span data-ttu-id="475f3-206">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="475f3-206">Filter scope</span></span> | <span data-ttu-id="475f3-207">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-207">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="475f3-208">1</span><span class="sxs-lookup"><span data-stu-id="475f3-208">1</span></span> | <span data-ttu-id="475f3-209">Globalny</span><span class="sxs-lookup"><span data-stu-id="475f3-209">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="475f3-210">2</span><span class="sxs-lookup"><span data-stu-id="475f3-210">2</span></span> | <span data-ttu-id="475f3-211">Kontroler lub strona Razor</span><span class="sxs-lookup"><span data-stu-id="475f3-211">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="475f3-212">3</span><span class="sxs-lookup"><span data-stu-id="475f3-212">3</span></span> | <span data-ttu-id="475f3-213">Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-213">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="475f3-214">4</span><span class="sxs-lookup"><span data-stu-id="475f3-214">4</span></span> | <span data-ttu-id="475f3-215">Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-215">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="475f3-216">5</span><span class="sxs-lookup"><span data-stu-id="475f3-216">5</span></span> | <span data-ttu-id="475f3-217">Kontroler lub strona Razor</span><span class="sxs-lookup"><span data-stu-id="475f3-217">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="475f3-218">6</span><span class="sxs-lookup"><span data-stu-id="475f3-218">6</span></span> | <span data-ttu-id="475f3-219">Globalny</span><span class="sxs-lookup"><span data-stu-id="475f3-219">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="475f3-220">Filtry na poziomie kontrolera</span><span class="sxs-lookup"><span data-stu-id="475f3-220">Controller level filters</span></span>

<span data-ttu-id="475f3-221">Każdy kontroler, który dziedziczy z klasy bazowej <xref:Microsoft.AspNetCore.Mvc.Controller> obejmuje metody [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)i [Controller. OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="475f3-221">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="475f3-222">Te metody:</span><span class="sxs-lookup"><span data-stu-id="475f3-222">These methods:</span></span>

* <span data-ttu-id="475f3-223">Zawiń filtry, które są uruchamiane dla danego działania.</span><span class="sxs-lookup"><span data-stu-id="475f3-223">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="475f3-224">`OnActionExecuting` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-224">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="475f3-225">`OnActionExecuted` jest wywoływana po wszystkich filtrach akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-225">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="475f3-226">`OnActionExecutionAsync` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-226">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="475f3-227">Kod w filtrze po `next` jest uruchamiany po metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-227">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="475f3-228">Na przykład w przykładzie pobierania `MySampleActionFilter` jest stosowany globalnie podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="475f3-228">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="475f3-229">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="475f3-229">The `TestController`:</span></span>

* <span data-ttu-id="475f3-230">Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do akcji `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="475f3-230">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="475f3-231">Zastępuje `OnActionExecuting` i `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="475f3-231">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="475f3-232">Przechodzenie do `https://localhost:5001/Test2/FilterTest2` uruchamia następujący kod:</span><span class="sxs-lookup"><span data-stu-id="475f3-232">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="475f3-233">Filtry na poziomie kontrolera ustawiają Właściwość [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) na `int.MinValue`.</span><span class="sxs-lookup"><span data-stu-id="475f3-233">Controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="475f3-234">Filtrów na poziomie kontrolera **nie** można ustawić do uruchomienia po zastosowaniu filtrów do metod.</span><span class="sxs-lookup"><span data-stu-id="475f3-234">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="475f3-235">Klauzula Order została omówiona w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-235">Order is explained in the next section.</span></span>

<span data-ttu-id="475f3-236">Aby uzyskać Razor Pages, zobacz [implementowanie filtrów stron Razor przez zastępowanie metod filtrowania](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="475f3-236">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="475f3-237">Zastępowanie kolejności domyślnej</span><span class="sxs-lookup"><span data-stu-id="475f3-237">Overriding the default order</span></span>

<span data-ttu-id="475f3-238">Domyślną sekwencję wykonywania można zastąpić, implementując <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-238">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="475f3-239">`IOrderedFilter` uwidacznia Właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, która ma pierwszeństwo przed zakresem w celu określenia kolejności wykonywania.</span><span class="sxs-lookup"><span data-stu-id="475f3-239">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="475f3-240">Filtr z niższą `Order` wartością:</span><span class="sxs-lookup"><span data-stu-id="475f3-240">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="475f3-241">Uruchamia *przed* kodem przed filtrem o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="475f3-241">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="475f3-242">Uruchamia *po* kodzie po filtrze o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="475f3-242">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="475f3-243">Właściwość `Order` jest ustawiana za pomocą parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="475f3-243">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="475f3-244">Należy wziąć pod uwagę dwa filtry akcji na następującym kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="475f3-244">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="475f3-245">W `StartUp.ConfigureServices`zostanie dodany filtr globalny:</span><span class="sxs-lookup"><span data-stu-id="475f3-245">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="475f3-246">3 filtry działają w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="475f3-246">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="475f3-247">Właściwość `Order` przesłania zakres podczas określania kolejności, w której są uruchamiane filtry.</span><span class="sxs-lookup"><span data-stu-id="475f3-247">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="475f3-248">Filtry są sortowane najpierw według kolejności, a następnie zakres jest używany do przerwania powiązań.</span><span class="sxs-lookup"><span data-stu-id="475f3-248">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="475f3-249">Wszystkie wbudowane filtry implementują `IOrderedFilter` i ustawiają domyślną wartość `Order` równą 0.</span><span class="sxs-lookup"><span data-stu-id="475f3-249">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="475f3-250">Jak wspomniano wcześniej, filtry na poziomie kontrolera ustawiają Właściwość [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) na `int.MinValue` dla filtrów wbudowanych, zakres określa kolejność, chyba że `Order` jest ustawiona na wartość różną od zera.</span><span class="sxs-lookup"><span data-stu-id="475f3-250">As mentioned previously, controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="475f3-251">W poprzednim kodzie `MySampleActionFilter` ma zakres globalny, więc jest uruchamiany przed `MyAction2FilterAttribute`, który ma zakres kontrolera.</span><span class="sxs-lookup"><span data-stu-id="475f3-251">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="475f3-252">Aby najpierw wykonać `MyAction2FilterAttribute`, ustaw kolejność na `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="475f3-252">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="475f3-253">Aby najpierw uruchomić filtr globalny `MySampleActionFilter`, ustaw `Order` na `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="475f3-253">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="475f3-254">Anulowanie i krótkie obwody</span><span class="sxs-lookup"><span data-stu-id="475f3-254">Cancellation and short-circuiting</span></span>

<span data-ttu-id="475f3-255">Potok filtru może być skrócony przez ustawienie właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> na parametrze <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> dostarczonym do metody Filter.</span><span class="sxs-lookup"><span data-stu-id="475f3-255">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="475f3-256">Na przykład poniższy filtr zasobów uniemożliwia wykonanie pozostałej części potoku:</span><span class="sxs-lookup"><span data-stu-id="475f3-256">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="475f3-257">W poniższym kodzie, zarówno `ShortCircuitingResourceFilter`, jak i filtr `AddHeader`, dla metody akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="475f3-257">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="475f3-258">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="475f3-258">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="475f3-259">Uruchamia się najpierw, ponieważ jest to filtr zasobów, a `AddHeader` jest filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-259">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="475f3-260">Krótkie obwody pozostała część potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-260">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="475f3-261">W związku z tym filtr `AddHeader` nigdy nie jest uruchamiany dla akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="475f3-261">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="475f3-262">Takie zachowanie będzie takie samo, jeśli oba filtry zostały zastosowane na poziomie metody akcji, pod warunkiem że `ShortCircuitingResourceFilter` uruchamiane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="475f3-262">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="475f3-263">`ShortCircuitingResourceFilter` jest uruchamiany jako pierwszy ze względu na jego typ filtru lub jawne użycie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="475f3-263">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="475f3-264">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="475f3-264">Dependency injection</span></span>

<span data-ttu-id="475f3-265">Filtry można dodawać według typu lub według wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="475f3-265">Filters can be added by type or by instance.</span></span> <span data-ttu-id="475f3-266">W przypadku dodania wystąpienia to wystąpienie jest używane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-266">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="475f3-267">Jeśli typ zostanie dodany, jego typ jest aktywowany.</span><span class="sxs-lookup"><span data-stu-id="475f3-267">If a type is added, it's type-activated.</span></span> <span data-ttu-id="475f3-268">Filtr aktywowany przez typ oznacza:</span><span class="sxs-lookup"><span data-stu-id="475f3-268">A type-activated filter means:</span></span>

* <span data-ttu-id="475f3-269">Wystąpienie jest tworzone dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-269">An instance is created for each request.</span></span>
* <span data-ttu-id="475f3-270">Wszystkie zależności konstruktora są wypełniane przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="475f3-270">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="475f3-271">Filtry zaimplementowane jako atrybuty i dodawane bezpośrednio do klas kontrolera lub metod akcji nie mogą mieć zależności konstruktorów udostępnianych przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="475f3-271">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="475f3-272">Zależności konstruktora nie mogą być dostarczone przez DI, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="475f3-272">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="475f3-273">Atrybuty muszą mieć podane parametry konstruktora, w których są stosowane.</span><span class="sxs-lookup"><span data-stu-id="475f3-273">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="475f3-274">Jest to ograniczenie działania atrybutów.</span><span class="sxs-lookup"><span data-stu-id="475f3-274">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="475f3-275">Następujące filtry obsługują zależności konstruktora dostarczone z elementów DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-275">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="475f3-276"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowany dla atrybutu.</span><span class="sxs-lookup"><span data-stu-id="475f3-276"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="475f3-277">Powyższe filtry można zastosować do kontrolera lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-277">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="475f3-278">Rejestratory są dostępne z programu DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-278">Loggers are available from DI.</span></span> <span data-ttu-id="475f3-279">Należy jednak unikać tworzenia i używania filtrów wyłącznie w celach rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="475f3-279">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="475f3-280">[Wbudowane rejestrowanie struktury](xref:fundamentals/logging/index) zazwyczaj zapewnia, co jest potrzebne do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="475f3-280">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="475f3-281">Rejestrowanie dodane do filtrów:</span><span class="sxs-lookup"><span data-stu-id="475f3-281">Logging added to filters:</span></span>

* <span data-ttu-id="475f3-282">Należy skoncentrować się na problemach z domeną biznesową lub działaniu specyficznym dla filtra.</span><span class="sxs-lookup"><span data-stu-id="475f3-282">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="475f3-283">**Nie** należy rejestrować akcji ani innych zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="475f3-283">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="475f3-284">Wbudowane filtry rejestrują akcje i zdarzenia struktury.</span><span class="sxs-lookup"><span data-stu-id="475f3-284">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="475f3-285">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="475f3-285">ServiceFilterAttribute</span></span>

<span data-ttu-id="475f3-286">Typy implementacji filtru usługi są zarejestrowane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="475f3-286">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="475f3-287"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> Pobiera wystąpienie filtru z DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-287">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="475f3-288">Poniższy kod ilustruje `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="475f3-288">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="475f3-289">W poniższym kodzie, `AddHeaderResultServiceFilter` jest dodawane do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-289">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="475f3-290">W poniższym kodzie atrybut `ServiceFilter` Pobiera wystąpienie filtru `AddHeaderResultServiceFilter` z DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-290">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="475f3-291">Podczas używania `ServiceFilterAttribute`, należy ustawić [Servicefilterattribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="475f3-291">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="475f3-292">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="475f3-292">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="475f3-293">Środowisko uruchomieniowe ASP.NET Core nie gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="475f3-293">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="475f3-294">Zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-294">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="475f3-295">Filtr nie zostanie ponownie żądany z kontenera DI w pewnym momencie.</span><span class="sxs-lookup"><span data-stu-id="475f3-295">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="475f3-296">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="475f3-296">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="475f3-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="475f3-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="475f3-298">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="475f3-298">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="475f3-299">`CreateInstance` ładuje określony typ z DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-299">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="475f3-300">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="475f3-300">TypeFilterAttribute</span></span>

<span data-ttu-id="475f3-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> przypomina <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie jest rozpoznawany bezpośrednio w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="475f3-302">Tworzy wystąpienie tego typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="475f3-302">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="475f3-303">Ponieważ typy `TypeFilterAttribute` nie są rozpoznawane bezpośrednio w kontenerze DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-303">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="475f3-304">Typy, do których odwołuje się `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="475f3-304">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="475f3-305">Ich zależności są spełnione przez kontener DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-305">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="475f3-306">`TypeFilterAttribute` może opcjonalnie zaakceptować argumenty konstruktora dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="475f3-306">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="475f3-307">W przypadku używania `TypeFilterAttribute`ustawienie [TypeFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="475f3-307">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="475f3-308">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="475f3-308">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="475f3-309">Środowisko uruchomieniowe ASP.NET Core nie zapewnia żadnych gwarancji, że zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-309">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="475f3-310">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="475f3-310">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="475f3-311">Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="475f3-311">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="475f3-312">Filtry autoryzacji</span><span class="sxs-lookup"><span data-stu-id="475f3-312">Authorization filters</span></span>

<span data-ttu-id="475f3-313">Filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-313">Authorization filters:</span></span>

* <span data-ttu-id="475f3-314">Są pierwszymi filtrami uruchamianymi w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-314">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="475f3-315">Kontrola dostępu do metod akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-315">Control access to action methods.</span></span>
* <span data-ttu-id="475f3-316">Ma metodę Before, ale nie po metodzie.</span><span class="sxs-lookup"><span data-stu-id="475f3-316">Have a before method, but no after method.</span></span>

<span data-ttu-id="475f3-317">Niestandardowe filtry autoryzacji wymagają niestandardowej struktury autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-317">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="475f3-318">Preferuj skonfigurowanie zasad autoryzacji lub zapisanie niestandardowych zasad autoryzacji przez zapisanie filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="475f3-318">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="475f3-319">Wbudowany filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-319">The built-in authorization filter:</span></span>

* <span data-ttu-id="475f3-320">Wywołuje system autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-320">Calls the authorization system.</span></span>
* <span data-ttu-id="475f3-321">Nie zezwala na żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-321">Does not authorize requests.</span></span>

<span data-ttu-id="475f3-322">**Nie** zgłaszaj wyjątków w ramach filtrów autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-322">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="475f3-323">Wyjątek nie zostanie obsłużony.</span><span class="sxs-lookup"><span data-stu-id="475f3-323">The exception will not be handled.</span></span>
* <span data-ttu-id="475f3-324">Filtry wyjątków nie będą obsługiwać wyjątku.</span><span class="sxs-lookup"><span data-stu-id="475f3-324">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="475f3-325">Rozważ wygenerowanie wyzwania w przypadku wystąpienia wyjątku w filtrze autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-325">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="475f3-326">Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="475f3-326">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="475f3-327">Filtry zasobów</span><span class="sxs-lookup"><span data-stu-id="475f3-327">Resource filters</span></span>

<span data-ttu-id="475f3-328">Filtry zasobów:</span><span class="sxs-lookup"><span data-stu-id="475f3-328">Resource filters:</span></span>

* <span data-ttu-id="475f3-329">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-329">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="475f3-330">Wykonanie powoduje Zawijanie większości potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-330">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="475f3-331">Tylko [filtry autoryzacji](#authorization-filters) są uruchamiane przed filtrami zasobów.</span><span class="sxs-lookup"><span data-stu-id="475f3-331">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="475f3-332">Filtry zasobów są przydatne do krótkiego obwodu większości potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-332">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="475f3-333">Na przykład filtr buforowania może uniknąć pozostałej części potoku w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="475f3-333">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="475f3-334">Przykłady filtru zasobów:</span><span class="sxs-lookup"><span data-stu-id="475f3-334">Resource filter examples:</span></span>

* <span data-ttu-id="475f3-335">[Filtr zasobów o krótkim obwodzie](#short-circuiting-resource-filter) pokazano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="475f3-335">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="475f3-336">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="475f3-336">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="475f3-337">Uniemożliwia powiązanie modelu z dostępem do danych formularza.</span><span class="sxs-lookup"><span data-stu-id="475f3-337">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="475f3-338">Służy do przekazywania dużych plików w celu uniemożliwienia odczytywania danych formularza do pamięci.</span><span class="sxs-lookup"><span data-stu-id="475f3-338">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="475f3-339">Filtry akcji</span><span class="sxs-lookup"><span data-stu-id="475f3-339">Action filters</span></span>

<span data-ttu-id="475f3-340">Filtry akcji **nie** mają zastosowania do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475f3-340">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="475f3-341">Razor Pages obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-341">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="475f3-342">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="475f3-342">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="475f3-343">Filtry akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-343">Action filters:</span></span>

* <span data-ttu-id="475f3-344">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-344">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="475f3-345">Ich wykonanie otacza wykonywanie metod akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-345">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="475f3-346">Poniższy kod przedstawia przykładowy filtr akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-346">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="475f3-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> udostępnia następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="475f3-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="475f3-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — umożliwia odczytywanie danych wejściowych do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="475f3-349"><xref:Microsoft.AspNetCore.Mvc.Controller> — włącza manipulowanie wystąpieniem kontrolera.</span><span class="sxs-lookup"><span data-stu-id="475f3-349"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="475f3-350"><xref:System.Web.Mvc.ActionExecutingContext.Result> — ustawienie `Result` wykonywanie krótkich obwodów metody akcji i kolejnych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-350"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="475f3-351">Zgłaszanie wyjątku w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-351">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="475f3-352">Zapobiega uruchamianiu kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="475f3-352">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="475f3-353">W przeciwieństwie do ustawienia `Result`, jest traktowany jako błąd, a nie pomyślny wynik.</span><span class="sxs-lookup"><span data-stu-id="475f3-353">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="475f3-354"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> zapewnia `Controller` i `Result` oraz następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="475f3-354">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="475f3-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-true, jeśli wykonywanie akcji zostało skrócone przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="475f3-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="475f3-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-wartość null, jeśli filtr akcji lub poprzednio uruchomionego wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="475f3-357">Ustawienie tej właściwości na wartość null:</span><span class="sxs-lookup"><span data-stu-id="475f3-357">Setting this property to null:</span></span>

  * <span data-ttu-id="475f3-358">Skutecznie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-358">Effectively handles the exception.</span></span>
  * <span data-ttu-id="475f3-359">`Result` jest wykonywany tak, jakby został zwrócony z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-359">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="475f3-360">W przypadku `IAsyncActionFilter`, wywołanie do <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="475f3-360">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="475f3-361">Wykonuje wszystkie kolejne filtry akcji i metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-361">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="475f3-362">Zwraca wartość `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="475f3-362">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="475f3-363">Do krótkiego obwodu Przypisz <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> do wystąpienia wynikowego i nie wywołuj `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="475f3-363">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="475f3-364">Struktura zawiera abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="475f3-364">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="475f3-365">Filtr akcji `OnActionExecuting` może służyć do:</span><span class="sxs-lookup"><span data-stu-id="475f3-365">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="475f3-366">Zweryfikuj stan modelu.</span><span class="sxs-lookup"><span data-stu-id="475f3-366">Validate model state.</span></span>
* <span data-ttu-id="475f3-367">Zwróć błąd, jeśli stan jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="475f3-367">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="475f3-368">Metoda `OnActionExecuted` jest uruchamiana po metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-368">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="475f3-369">I mogą przeglądać wyniki akcji przy użyciu właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> i manipulować nimi.</span><span class="sxs-lookup"><span data-stu-id="475f3-369">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="475f3-370"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> jest ustawiona na wartość true, jeśli wykonywanie akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="475f3-370"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="475f3-371"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> jest ustawiona na wartość różną od null, jeśli akcja lub kolejny filtr akcji wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-371"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="475f3-372">Ustawienie `Exception` na wartość null:</span><span class="sxs-lookup"><span data-stu-id="475f3-372">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="475f3-373">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-373">Effectively handles an exception.</span></span>
  * <span data-ttu-id="475f3-374">`ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-374">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="475f3-375">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="475f3-375">Exception filters</span></span>

<span data-ttu-id="475f3-376">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="475f3-376">Exception filters:</span></span>

* <span data-ttu-id="475f3-377">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-377">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="475f3-378">Może służyć do implementowania typowych zasad obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="475f3-378">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="475f3-379">Następujący przykładowy filtr wyjątku używa niestandardowego widoku błędów, aby wyświetlić szczegóły dotyczące wyjątków występujących podczas opracowywania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-379">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="475f3-380">Następujący kod testuje filtr wyjątku:</span><span class="sxs-lookup"><span data-stu-id="475f3-380">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="475f3-381">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="475f3-381">Exception filters:</span></span>

* <span data-ttu-id="475f3-382">Nie mam zdarzeń przed i po.</span><span class="sxs-lookup"><span data-stu-id="475f3-382">Don't have before and after events.</span></span>
* <span data-ttu-id="475f3-383">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="475f3-383">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="475f3-384">Obsłuż Nieobsłużone wyjątki występujące na stronie lub w tworzeniu kontrolera, [powiązania modelu](xref:mvc/models/model-binding), filtrach akcji lub metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-384">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="475f3-385">**Nie** należy przechwytywać wyjątków występujących w filtrach zasobów, filtrach wyników ani wykonywaniu wyniku MVC.</span><span class="sxs-lookup"><span data-stu-id="475f3-385">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="475f3-386">Aby obsłużyć wyjątek, ustaw właściwość <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> na `true` lub Zapisz odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="475f3-386">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="475f3-387">Spowoduje to zatrzymanie propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="475f3-387">This stops propagation of the exception.</span></span> <span data-ttu-id="475f3-388">Filtr wyjątku nie może przekształcić wyjątku w "powodzenie".</span><span class="sxs-lookup"><span data-stu-id="475f3-388">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="475f3-389">Można to zrobić tylko dla filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-389">Only an action filter can do that.</span></span>

<span data-ttu-id="475f3-390">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="475f3-390">Exception filters:</span></span>

* <span data-ttu-id="475f3-391">Są dobre dla wyjątków zalewkowania występujących w ramach akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-391">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="475f3-392">Nie są tak elastyczne jak błędy obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="475f3-392">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="475f3-393">Preferuj oprogramowanie pośredniczące dla obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="475f3-393">Prefer middleware for exception handling.</span></span> <span data-ttu-id="475f3-394">Użyj filtrów wyjątków tylko w przypadku, gdy obsługa błędów *różni* się w zależności od tego, która metoda akcji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="475f3-394">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="475f3-395">Na przykład aplikacja może mieć metody akcji zarówno dla punktów końcowych interfejsu API, jak i dla widoków/HTML.</span><span class="sxs-lookup"><span data-stu-id="475f3-395">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="475f3-396">Punkty końcowe interfejsu API mogą zwracać informacje o błędach jako dane JSON, podczas gdy akcje oparte na widoku mogą zwrócić stronę błędu jako kod HTML.</span><span class="sxs-lookup"><span data-stu-id="475f3-396">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="475f3-397">Filtry wyników</span><span class="sxs-lookup"><span data-stu-id="475f3-397">Result filters</span></span>

<span data-ttu-id="475f3-398">Filtry wyników:</span><span class="sxs-lookup"><span data-stu-id="475f3-398">Result filters:</span></span>

* <span data-ttu-id="475f3-399">Zaimplementuj interfejs:</span><span class="sxs-lookup"><span data-stu-id="475f3-399">Implement an interface:</span></span>
  * <span data-ttu-id="475f3-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="475f3-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="475f3-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="475f3-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="475f3-402">Ich wykonanie otacza wykonywanie wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-402">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="475f3-403">IResultFilter i IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="475f3-403">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="475f3-404">Poniższy kod pokazuje filtr wynikowy, który dodaje nagłówek HTTP:</span><span class="sxs-lookup"><span data-stu-id="475f3-404">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="475f3-405">Rodzaj wykonywanego wyniku zależy od akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-405">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="475f3-406">Akcja zwracająca widok obejmuje wszystkie procesy przetwarzania Razor w ramach wykonywanej <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="475f3-406">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="475f3-407">Metoda interfejsu API może wykonywać pewne serializacji w ramach wykonywania wyniku.</span><span class="sxs-lookup"><span data-stu-id="475f3-407">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="475f3-408">Dowiedz się więcej o [wynikach akcji](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="475f3-408">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="475f3-409">Filtry wynikowe są wykonywane tylko wtedy, gdy akcja lub filtr akcji generuje wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-409">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="475f3-410">Filtry wynikowe nie są wykonywane, gdy:</span><span class="sxs-lookup"><span data-stu-id="475f3-410">Result filters are not executed when:</span></span>

* <span data-ttu-id="475f3-411">Filtr autoryzacji lub filtr zasobów jest krótki obwody potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-411">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="475f3-412">Filtr wyjątku obsługuje wyjątek przez wygenerowanie wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-412">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="475f3-413">Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> może skrócić wykonywanie wyniku akcji i kolejnych filtrów wyników przez ustawienie <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> do `true`.</span><span class="sxs-lookup"><span data-stu-id="475f3-413">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="475f3-414">Zapisuj w obiekcie Response podczas krótkiego obwodu, aby uniknąć generowania pustej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="475f3-414">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="475f3-415">Zgłaszanie wyjątku w `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="475f3-415">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="475f3-416">Zapobiega wykonywaniu wyniku akcji i kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="475f3-416">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="475f3-417">Jest traktowany jako niepowodzenie, a nie wynikowy pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="475f3-417">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="475f3-418">Po uruchomieniu metody <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> odpowiedź prawdopodobnie została już wysłana do klienta.</span><span class="sxs-lookup"><span data-stu-id="475f3-418">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="475f3-419">Jeśli odpowiedź została już wysłana do klienta, nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="475f3-419">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="475f3-420">`ResultExecutedContext.Canceled` jest ustawiona na `true`, jeśli wykonywanie wyniku akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="475f3-420">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="475f3-421">`ResultExecutedContext.Exception` ma ustawioną wartość różną od null, jeśli wynik akcji lub kolejny filtr wynikowy wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-421">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="475f3-422">Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia ponowne zgłoszenie wyjątku w potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-422">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="475f3-423">Nie istnieje niezawodny sposób zapisu danych do odpowiedzi podczas obsługi wyjątku w filtrze wynikowym.</span><span class="sxs-lookup"><span data-stu-id="475f3-423">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="475f3-424">Jeśli nagłówki zostały opróżnione do klienta, gdy wynikiem akcji zgłasza wyjątek, nie istnieje niezawodny mechanizm wysyłania kodu błędu.</span><span class="sxs-lookup"><span data-stu-id="475f3-424">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="475f3-425">W przypadku <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>wywołanie `await next` na <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie kolejne filtry wynikowe i wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-425">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="475f3-426">Do krótkiego obwodu Ustaw [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) do `true` i nie wywołuj `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="475f3-426">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="475f3-427">Struktura zawiera abstrakcyjną `ResultFilterAttribute`, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="475f3-427">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="475f3-428">Przedstawiona wcześniej Klasa [Addheaderattribute](#add-header-attribute) jest przykładem atrybutu filtru wynikowego.</span><span class="sxs-lookup"><span data-stu-id="475f3-428">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="475f3-429">IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="475f3-429">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="475f3-430">Interfejsy <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> deklarują implementację <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, która jest uruchamiana dla wszystkich wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-430">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="475f3-431">Obejmuje to wyniki akcji generowane przez:</span><span class="sxs-lookup"><span data-stu-id="475f3-431">This includes action results produced by:</span></span>

* <span data-ttu-id="475f3-432">Filtry autoryzacji i filtry zasobów, które są skracane.</span><span class="sxs-lookup"><span data-stu-id="475f3-432">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="475f3-433">Filtry wyjątków.</span><span class="sxs-lookup"><span data-stu-id="475f3-433">Exception filters.</span></span>

<span data-ttu-id="475f3-434">Na przykład następujący filtr jest zawsze uruchamiany i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) z kodem stanu *jednostki nieprzetwarzanego 422* , gdy negocjowanie zawartości kończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="475f3-434">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="475f3-435">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="475f3-435">IFilterFactory</span></span>

<span data-ttu-id="475f3-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="475f3-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="475f3-437">W związku z tym wystąpienie `IFilterFactory` może być używane jako wystąpienie `IFilterMetadata` w dowolnym miejscu w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-437">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="475f3-438">Gdy środowisko uruchomieniowe przygotowuje się do wywołania filtru, próbuje rzutować go na `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="475f3-438">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="475f3-439">Jeśli rzutowanie powiedzie się, Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> zostanie wywołana w celu utworzenia wystąpienia `IFilterMetadata`, które jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="475f3-439">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="475f3-440">Zapewnia to elastyczny projekt, ponieważ dokładny potok filtru nie musi być ustawiony jawnie podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-440">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="475f3-441">`IFilterFactory` można zaimplementować przy użyciu niestandardowych implementacji atrybutów jako inne podejście do tworzenia filtrów:</span><span class="sxs-lookup"><span data-stu-id="475f3-441">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="475f3-442">Filtr jest stosowany w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="475f3-442">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="475f3-443">Przetestuj poprzedni kod, uruchamiając [przykład pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span><span class="sxs-lookup"><span data-stu-id="475f3-443">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="475f3-444">Wywołaj narzędzia deweloperskie F12.</span><span class="sxs-lookup"><span data-stu-id="475f3-444">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="475f3-445">Przejdź do adresu `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="475f3-445">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="475f3-446">Narzędzia programistyczne F12 wyświetlają następujące nagłówki odpowiedzi dodane przez przykładowy kod:</span><span class="sxs-lookup"><span data-stu-id="475f3-446">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="475f3-447">**autor:** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="475f3-447">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="475f3-448">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="475f3-448">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="475f3-449">**wewnętrzne:** `My header`</span><span class="sxs-lookup"><span data-stu-id="475f3-449">**internal:** `My header`</span></span>

<span data-ttu-id="475f3-450">Poprzedni kod tworzy nagłówek odpowiedzi **wewnętrznej:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="475f3-450">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="475f3-451">IFilterFactory zaimplementowane dla atrybutu</span><span class="sxs-lookup"><span data-stu-id="475f3-451">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="475f3-452">Filtry implementujące `IFilterFactory` są przydatne w przypadku filtrów, które:</span><span class="sxs-lookup"><span data-stu-id="475f3-452">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="475f3-453">Nie wymagaj parametrów przekazujących.</span><span class="sxs-lookup"><span data-stu-id="475f3-453">Don't require passing parameters.</span></span>
* <span data-ttu-id="475f3-454">Mają zależności konstruktora, które muszą być wypełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-454">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="475f3-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="475f3-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="475f3-456">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="475f3-456">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="475f3-457">`CreateInstance` ładuje określony typ z kontenera usług (DI).</span><span class="sxs-lookup"><span data-stu-id="475f3-457">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="475f3-458">Poniższy kod przedstawia trzy podejścia do stosowania `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="475f3-458">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="475f3-459">W poprzednim kodzie dekorowania nazwy metodę `[SampleActionFilter]` jest preferowanym podejściem do zastosowania `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="475f3-459">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="475f3-460">Używanie oprogramowania pośredniczącego w potoku filtru</span><span class="sxs-lookup"><span data-stu-id="475f3-460">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="475f3-461">Filtry zasobów działają podobnie jak [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) w celu obsłużenia wykonywania wszystkich elementów, które są późniejsze w potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-461">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="475f3-462">Ale filtry różnią się od oprogramowania pośredniczącego w tym, że są częścią środowiska uruchomieniowego, co oznacza, że ma dostęp do kontekstu i konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-462">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="475f3-463">Aby użyć oprogramowania pośredniczącego jako filtru, należy utworzyć typ z metodą `Configure`, która określa oprogramowanie pośredniczące, które ma zostać dodane do potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-463">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="475f3-464">W poniższym przykładzie jest wykorzystywane oprogramowanie pośredniczące lokalizacyjne do ustalenia bieżącej kultury dla żądania:</span><span class="sxs-lookup"><span data-stu-id="475f3-464">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="475f3-465">Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute>, aby uruchomić oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="475f3-465">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="475f3-466">Filtry oprogramowania pośredniczącego są uruchamiane na tym samym etapie potoku filtru jako filtry zasobów przed powiązaniem modelu i po pozostałej części potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-466">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="475f3-467">Następne akcje</span><span class="sxs-lookup"><span data-stu-id="475f3-467">Next actions</span></span>

* <span data-ttu-id="475f3-468">Zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="475f3-468">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="475f3-469">Aby eksperymentować z filtrami, należy [pobrać, przetestować i zmodyfikować przykład usługi GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="475f3-469">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="475f3-470">[Kirka Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra/)i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="475f3-470">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="475f3-471">*Filtry* w ASP.NET Core umożliwiają uruchamianie kodu przed określonymi etapami w potoku przetwarzania żądań lub po nich.</span><span class="sxs-lookup"><span data-stu-id="475f3-471">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="475f3-472">Filtry wbudowane obsługują zadania takie jak:</span><span class="sxs-lookup"><span data-stu-id="475f3-472">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="475f3-473">Autoryzacja (zapobieganie dostępowi do zasobów, dla których użytkownik nie jest autoryzowany).</span><span class="sxs-lookup"><span data-stu-id="475f3-473">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="475f3-474">Buforowanie odpowiedzi (krótkie obwody potoku żądania w celu zwrócenia buforowanej odpowiedzi).</span><span class="sxs-lookup"><span data-stu-id="475f3-474">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="475f3-475">Filtry niestandardowe mogą być tworzone w celu obsłużenia krzyżowego rozcinania.</span><span class="sxs-lookup"><span data-stu-id="475f3-475">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="475f3-476">Przykłady zagadnień związanych z rozcinaniem obejmują obsługę błędów, buforowanie, konfigurację, autoryzację i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="475f3-476">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="475f3-477">Filtry unikają duplikowania kodu.</span><span class="sxs-lookup"><span data-stu-id="475f3-477">Filters avoid duplicating code.</span></span> <span data-ttu-id="475f3-478">Na przykład filtr wyjątków obsługujący błędy może skonsolidować obsługę błędów.</span><span class="sxs-lookup"><span data-stu-id="475f3-478">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="475f3-479">Ten dokument ma zastosowanie do Razor Pages, kontrolerów interfejsu API i kontrolerów z widokami.</span><span class="sxs-lookup"><span data-stu-id="475f3-479">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="475f3-480">[Wyświetl lub Pobierz przykład](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="475f3-480">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="475f3-481">Jak działają filtry</span><span class="sxs-lookup"><span data-stu-id="475f3-481">How filters work</span></span>

<span data-ttu-id="475f3-482">Filtry są uruchamiane w *potoku wywołania akcji ASP.NET Core*, czasami określane jako *potok filtru*.</span><span class="sxs-lookup"><span data-stu-id="475f3-482">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="475f3-483">Potok filtru jest uruchamiany po ASP.NET Core wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="475f3-483">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![Żądanie jest przetwarzane przez inne oprogramowanie pośredniczące, kierowanie oprogramowania pośredniczącego, wybór akcji i potok wywołania akcji ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="475f3-486">Typy filtrów</span><span class="sxs-lookup"><span data-stu-id="475f3-486">Filter types</span></span>

<span data-ttu-id="475f3-487">Każdy typ filtru jest wykonywany na innym etapie w potoku filtru:</span><span class="sxs-lookup"><span data-stu-id="475f3-487">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="475f3-488">[Filtry autoryzacji](#authorization-filters) są uruchamiane jako pierwsze i są używane do określenia, czy użytkownik jest autoryzowany do żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-488">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="475f3-489">Filtry autoryzacji skracają obwód w przypadku, gdy żądanie jest nieautoryzowane.</span><span class="sxs-lookup"><span data-stu-id="475f3-489">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="475f3-490">[Filtry zasobów](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="475f3-490">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="475f3-491">Uruchom po autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-491">Run after authorization.</span></span>  
  * <span data-ttu-id="475f3-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> może uruchomić kod przed resztą potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="475f3-493">Na przykład, `OnResourceExecuting` może uruchomić kod przed powiązaniem modelu.</span><span class="sxs-lookup"><span data-stu-id="475f3-493">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="475f3-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> można uruchomić kod po zakończeniu reszty potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="475f3-495">[Filtry akcji](#action-filters) mogą uruchomić kod bezpośrednio przed i po wywołaniu pojedynczej metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-495">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="475f3-496">Mogą one służyć do manipulowania argumentami przekazaną do akcji i wynik zwrócony z akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-496">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="475f3-497">Filtry akcji **nie** są obsługiwane w Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475f3-497">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="475f3-498">[Filtry wyjątków](#exception-filters) są używane do stosowania zasad globalnych do nieobsłużonych wyjątków, które wystąpiły przed zapisem w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="475f3-498">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="475f3-499">[Filtry wynikowe](#result-filters) mogą uruchamiać kod bezpośrednio przed i po wykonaniu poszczególnych wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-499">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="475f3-500">Są one uruchamiane tylko wtedy, gdy metoda akcji została wykonana pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="475f3-500">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="475f3-501">Są one przydatne dla logiki, która musi być w trakcie wyświetlania lub wykonywania w programie formatującego.</span><span class="sxs-lookup"><span data-stu-id="475f3-501">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="475f3-502">Na poniższym diagramie przedstawiono sposób, w jaki typy filtrów współdziałają w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-502">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![Żądanie jest przetwarzane przez filtry autoryzacji, filtry zasobów, powiązania modelu, filtry akcji, wykonywanie akcji i konwersję wyników akcji, filtry wyjątków, filtry wynikowe i wykonywanie wyniku.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="475f3-505">Wdrażanie</span><span class="sxs-lookup"><span data-stu-id="475f3-505">Implementation</span></span>

<span data-ttu-id="475f3-506">Filtry obsługują implementacje synchroniczne i asynchroniczne za pomocą różnych definicji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="475f3-506">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="475f3-507">Filtry synchroniczne mogą uruchamiać kod przed (`On-Stage-Executing`) i po nim (`On-Stage-Executed`) ich etap potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-507">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="475f3-508">Na przykład, `OnActionExecuting` jest wywoływana przed wywołaniem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-508">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="475f3-509">`OnActionExecuted` jest wywoływana po powrocie metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-509">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="475f3-510">Filtry asynchroniczne definiują metodę `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="475f3-510">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="475f3-511">W poprzednim kodzie `SampleAsyncActionFilter` ma <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), który wykonuje metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-511">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="475f3-512">Każda metoda `On-Stage-ExecutionAsync` przyjmuje `FilterType-ExecutionDelegate`, który wykonuje etap potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-512">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="475f3-513">Wiele etapów filtrowania</span><span class="sxs-lookup"><span data-stu-id="475f3-513">Multiple filter stages</span></span>

<span data-ttu-id="475f3-514">Interfejsy dla wielu etapów filtru można zaimplementować w pojedynczej klasie.</span><span class="sxs-lookup"><span data-stu-id="475f3-514">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="475f3-515">Na przykład Klasa <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementuje `IActionFilter`, `IResultFilter`i ich ekwiwalenty asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="475f3-515">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="475f3-516">Implementowanie synchronicznej lub asynchronicznej wersji interfejsu filtru **, a** **nie** obu.</span><span class="sxs-lookup"><span data-stu-id="475f3-516">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="475f3-517">Środowisko uruchomieniowe najpierw sprawdza, czy filtr implementuje interfejs asynchroniczny, a jeśli tak, wywołuje to.</span><span class="sxs-lookup"><span data-stu-id="475f3-517">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="475f3-518">Jeśli nie, wywołuje metody interfejsu synchronicznego.</span><span class="sxs-lookup"><span data-stu-id="475f3-518">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="475f3-519">Jeśli zarówno interfejsy asynchroniczne, jak i synchroniczne są zaimplementowane w jednej klasie, wywoływana jest tylko Metoda async.</span><span class="sxs-lookup"><span data-stu-id="475f3-519">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="475f3-520">W przypadku używania klas abstrakcyjnych, takich jak <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> przesłaniać tylko metody synchroniczne lub metodę asynchroniczną dla każdego typu filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-520">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="475f3-521">Wbudowane atrybuty filtru</span><span class="sxs-lookup"><span data-stu-id="475f3-521">Built-in filter attributes</span></span>

<span data-ttu-id="475f3-522">ASP.NET Core obejmuje wbudowane filtry oparte na atrybutach, które można podklasować i dostosowywać.</span><span class="sxs-lookup"><span data-stu-id="475f3-522">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="475f3-523">Na przykład poniższy filtr wynikowy dodaje nagłówek do odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="475f3-523">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="475f3-524">Atrybuty umożliwiają filtrom akceptowanie argumentów, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="475f3-524">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="475f3-525">Zastosuj `AddHeaderAttribute` do kontrolera lub metody akcji i określ nazwę i wartość nagłówka HTTP:</span><span class="sxs-lookup"><span data-stu-id="475f3-525">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="475f3-526">Kilka interfejsów filtrów ma odpowiednie atrybuty, które mogą być używane jako klasy bazowe dla implementacji niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="475f3-526">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="475f3-527">Atrybuty filtru:</span><span class="sxs-lookup"><span data-stu-id="475f3-527">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="475f3-528">Zakresy filtrów i kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="475f3-528">Filter scopes and order of execution</span></span>

<span data-ttu-id="475f3-529">Filtr można dodać do potoku w jednym z trzech *zakresów*:</span><span class="sxs-lookup"><span data-stu-id="475f3-529">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="475f3-530">Użycie atrybutu w akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-530">Using an attribute on an action.</span></span>
* <span data-ttu-id="475f3-531">Używanie atrybutu na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="475f3-531">Using an attribute on a controller.</span></span>
* <span data-ttu-id="475f3-532">Globalnie dla wszystkich kontrolerów i akcji, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="475f3-532">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="475f3-533">Poprzedni kod dodaje trzy filtry globalnie przy użyciu kolekcji [MvcOptions. filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) .</span><span class="sxs-lookup"><span data-stu-id="475f3-533">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="475f3-534">Domyślna kolejność wykonywania</span><span class="sxs-lookup"><span data-stu-id="475f3-534">Default order of execution</span></span>

<span data-ttu-id="475f3-535">Jeśli istnieje wiele filtrów tego *samego typu*, zakres określa domyślną kolejność wykonywania filtrowania.</span><span class="sxs-lookup"><span data-stu-id="475f3-535">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="475f3-536">Filtry globalne Otocz filtry klas.</span><span class="sxs-lookup"><span data-stu-id="475f3-536">Global filters surround class filters.</span></span> <span data-ttu-id="475f3-537">Filtry klas Otocz filtry metod.</span><span class="sxs-lookup"><span data-stu-id="475f3-537">Class filters surround method filters.</span></span>

<span data-ttu-id="475f3-538">W wyniku zagnieżdżania filtrów, *po* kodzie filtrów działa w odwrotnej kolejności *przed* kodem.</span><span class="sxs-lookup"><span data-stu-id="475f3-538">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="475f3-539">Sekwencja filtru:</span><span class="sxs-lookup"><span data-stu-id="475f3-539">The filter sequence:</span></span>

* <span data-ttu-id="475f3-540">*Przed* kodem filtrów globalnych.</span><span class="sxs-lookup"><span data-stu-id="475f3-540">The *before* code of global filters.</span></span>
  * <span data-ttu-id="475f3-541">*Przed* kodem filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="475f3-541">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="475f3-542">Filtr *przed* kodem metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-542">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="475f3-543">*Po* kodzie metody akcji filtry.</span><span class="sxs-lookup"><span data-stu-id="475f3-543">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="475f3-544">*Po* kodzie filtrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="475f3-544">The *after* code of controller filters.</span></span>
* <span data-ttu-id="475f3-545">Kod *po* filtrach globalnych.</span><span class="sxs-lookup"><span data-stu-id="475f3-545">The *after* code of global filters.</span></span>
  
<span data-ttu-id="475f3-546">Poniższy przykład ilustruje kolejność, w której metody filtrowania są wywoływane dla synchronicznych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-546">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="475f3-547">Sequence</span><span class="sxs-lookup"><span data-stu-id="475f3-547">Sequence</span></span> | <span data-ttu-id="475f3-548">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="475f3-548">Filter scope</span></span> | <span data-ttu-id="475f3-549">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-549">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="475f3-550">1</span><span class="sxs-lookup"><span data-stu-id="475f3-550">1</span></span> | <span data-ttu-id="475f3-551">Globalny</span><span class="sxs-lookup"><span data-stu-id="475f3-551">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="475f3-552">2</span><span class="sxs-lookup"><span data-stu-id="475f3-552">2</span></span> | <span data-ttu-id="475f3-553">Kontroler</span><span class="sxs-lookup"><span data-stu-id="475f3-553">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="475f3-554">3</span><span class="sxs-lookup"><span data-stu-id="475f3-554">3</span></span> | <span data-ttu-id="475f3-555">Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-555">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="475f3-556">4</span><span class="sxs-lookup"><span data-stu-id="475f3-556">4</span></span> | <span data-ttu-id="475f3-557">Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-557">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="475f3-558">5</span><span class="sxs-lookup"><span data-stu-id="475f3-558">5</span></span> | <span data-ttu-id="475f3-559">Kontroler</span><span class="sxs-lookup"><span data-stu-id="475f3-559">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="475f3-560">6</span><span class="sxs-lookup"><span data-stu-id="475f3-560">6</span></span> | <span data-ttu-id="475f3-561">Globalny</span><span class="sxs-lookup"><span data-stu-id="475f3-561">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="475f3-562">Ta sekwencja pokazuje:</span><span class="sxs-lookup"><span data-stu-id="475f3-562">This sequence shows:</span></span>

* <span data-ttu-id="475f3-563">Filtr metody jest zagnieżdżony w obrębie filtru kontrolera.</span><span class="sxs-lookup"><span data-stu-id="475f3-563">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="475f3-564">Filtr kontrolera jest zagnieżdżony w obrębie filtru globalnego.</span><span class="sxs-lookup"><span data-stu-id="475f3-564">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="475f3-565">Kontrolery i filtry na poziomie strony Razor</span><span class="sxs-lookup"><span data-stu-id="475f3-565">Controller and Razor Page level filters</span></span>

<span data-ttu-id="475f3-566">Każdy kontroler, który dziedziczy z klasy bazowej <xref:Microsoft.AspNetCore.Mvc.Controller> obejmuje metody [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)i [Controller. OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="475f3-566">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="475f3-567">Te metody:</span><span class="sxs-lookup"><span data-stu-id="475f3-567">These methods:</span></span>

* <span data-ttu-id="475f3-568">Zawiń filtry, które są uruchamiane dla danego działania.</span><span class="sxs-lookup"><span data-stu-id="475f3-568">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="475f3-569">`OnActionExecuting` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-569">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="475f3-570">`OnActionExecuted` jest wywoływana po wszystkich filtrach akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-570">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="475f3-571">`OnActionExecutionAsync` jest wywoływana przed jakimkolwiek filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-571">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="475f3-572">Kod w filtrze po `next` jest uruchamiany po metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-572">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="475f3-573">Na przykład w przykładzie pobierania `MySampleActionFilter` jest stosowany globalnie podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="475f3-573">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="475f3-574">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="475f3-574">The `TestController`:</span></span>

* <span data-ttu-id="475f3-575">Stosuje `SampleActionFilterAttribute` (`[SampleActionFilter]`) do akcji `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="475f3-575">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="475f3-576">Zastępuje `OnActionExecuting` i `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="475f3-576">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="475f3-577">Przechodzenie do `https://localhost:5001/Test/FilterTest2` uruchamia następujący kod:</span><span class="sxs-lookup"><span data-stu-id="475f3-577">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="475f3-578">Aby uzyskać Razor Pages, zobacz [implementowanie filtrów stron Razor przez zastępowanie metod filtrowania](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="475f3-578">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="475f3-579">Zastępowanie kolejności domyślnej</span><span class="sxs-lookup"><span data-stu-id="475f3-579">Overriding the default order</span></span>

<span data-ttu-id="475f3-580">Domyślną sekwencję wykonywania można zastąpić, implementując <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-580">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="475f3-581">`IOrderedFilter` uwidacznia Właściwość <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order>, która ma pierwszeństwo przed zakresem w celu określenia kolejności wykonywania.</span><span class="sxs-lookup"><span data-stu-id="475f3-581">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="475f3-582">Filtr z niższą `Order` wartością:</span><span class="sxs-lookup"><span data-stu-id="475f3-582">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="475f3-583">Uruchamia *przed* kodem przed filtrem o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="475f3-583">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="475f3-584">Uruchamia *po* kodzie po filtrze o wyższej wartości `Order`.</span><span class="sxs-lookup"><span data-stu-id="475f3-584">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="475f3-585">Właściwość `Order` można ustawić przy użyciu parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="475f3-585">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="475f3-586">Należy wziąć pod uwagę te same 3 filtry akcji, które przedstawiono w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="475f3-586">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="475f3-587">Jeśli właściwość `Order` kontrolera i filtry globalne mają odpowiednio wartość 1 i 2, kolejność wykonywania zostanie odwrócona.</span><span class="sxs-lookup"><span data-stu-id="475f3-587">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="475f3-588">Sequence</span><span class="sxs-lookup"><span data-stu-id="475f3-588">Sequence</span></span> | <span data-ttu-id="475f3-589">Zakres filtru</span><span class="sxs-lookup"><span data-stu-id="475f3-589">Filter scope</span></span> | <span data-ttu-id="475f3-590">`Order` Właściwość</span><span class="sxs-lookup"><span data-stu-id="475f3-590">`Order` property</span></span> | <span data-ttu-id="475f3-591">Filter — Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-591">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="475f3-592">1</span><span class="sxs-lookup"><span data-stu-id="475f3-592">1</span></span> | <span data-ttu-id="475f3-593">Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-593">Method</span></span> | <span data-ttu-id="475f3-594">0</span><span class="sxs-lookup"><span data-stu-id="475f3-594">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="475f3-595">2</span><span class="sxs-lookup"><span data-stu-id="475f3-595">2</span></span> | <span data-ttu-id="475f3-596">Kontroler</span><span class="sxs-lookup"><span data-stu-id="475f3-596">Controller</span></span> | <span data-ttu-id="475f3-597">1</span><span class="sxs-lookup"><span data-stu-id="475f3-597">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="475f3-598">3</span><span class="sxs-lookup"><span data-stu-id="475f3-598">3</span></span> | <span data-ttu-id="475f3-599">Globalny</span><span class="sxs-lookup"><span data-stu-id="475f3-599">Global</span></span> | <span data-ttu-id="475f3-600">2</span><span class="sxs-lookup"><span data-stu-id="475f3-600">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="475f3-601">4</span><span class="sxs-lookup"><span data-stu-id="475f3-601">4</span></span> | <span data-ttu-id="475f3-602">Globalny</span><span class="sxs-lookup"><span data-stu-id="475f3-602">Global</span></span> | <span data-ttu-id="475f3-603">2</span><span class="sxs-lookup"><span data-stu-id="475f3-603">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="475f3-604">5</span><span class="sxs-lookup"><span data-stu-id="475f3-604">5</span></span> | <span data-ttu-id="475f3-605">Kontroler</span><span class="sxs-lookup"><span data-stu-id="475f3-605">Controller</span></span> | <span data-ttu-id="475f3-606">1</span><span class="sxs-lookup"><span data-stu-id="475f3-606">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="475f3-607">6</span><span class="sxs-lookup"><span data-stu-id="475f3-607">6</span></span> | <span data-ttu-id="475f3-608">Metoda</span><span class="sxs-lookup"><span data-stu-id="475f3-608">Method</span></span> | <span data-ttu-id="475f3-609">0</span><span class="sxs-lookup"><span data-stu-id="475f3-609">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="475f3-610">Właściwość `Order` przesłania zakres podczas określania kolejności, w której są uruchamiane filtry.</span><span class="sxs-lookup"><span data-stu-id="475f3-610">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="475f3-611">Filtry są sortowane najpierw według kolejności, a następnie zakres jest używany do przerwania powiązań.</span><span class="sxs-lookup"><span data-stu-id="475f3-611">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="475f3-612">Wszystkie wbudowane filtry implementują `IOrderedFilter` i ustawiają domyślną wartość `Order` równą 0.</span><span class="sxs-lookup"><span data-stu-id="475f3-612">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="475f3-613">W przypadku filtrów wbudowanych zakres określa kolejność, chyba że `Order` jest ustawiona na wartość różną od zera.</span><span class="sxs-lookup"><span data-stu-id="475f3-613">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="475f3-614">Anulowanie i krótkie obwody</span><span class="sxs-lookup"><span data-stu-id="475f3-614">Cancellation and short-circuiting</span></span>

<span data-ttu-id="475f3-615">Potok filtru może być skrócony przez ustawienie właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> na parametrze <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> dostarczonym do metody Filter.</span><span class="sxs-lookup"><span data-stu-id="475f3-615">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="475f3-616">Na przykład poniższy filtr zasobów uniemożliwia wykonanie pozostałej części potoku:</span><span class="sxs-lookup"><span data-stu-id="475f3-616">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="475f3-617">W poniższym kodzie, zarówno `ShortCircuitingResourceFilter`, jak i filtr `AddHeader`, dla metody akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="475f3-617">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="475f3-618">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="475f3-618">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="475f3-619">Uruchamia się najpierw, ponieważ jest to filtr zasobów, a `AddHeader` jest filtrem akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-619">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="475f3-620">Krótkie obwody pozostała część potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-620">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="475f3-621">W związku z tym filtr `AddHeader` nigdy nie jest uruchamiany dla akcji `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="475f3-621">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="475f3-622">Takie zachowanie będzie takie samo, jeśli oba filtry zostały zastosowane na poziomie metody akcji, pod warunkiem że `ShortCircuitingResourceFilter` uruchamiane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="475f3-622">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="475f3-623">`ShortCircuitingResourceFilter` jest uruchamiany jako pierwszy ze względu na jego typ filtru lub jawne użycie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="475f3-623">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="475f3-624">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="475f3-624">Dependency injection</span></span>

<span data-ttu-id="475f3-625">Filtry można dodawać według typu lub według wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="475f3-625">Filters can be added by type or by instance.</span></span> <span data-ttu-id="475f3-626">W przypadku dodania wystąpienia to wystąpienie jest używane dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-626">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="475f3-627">Jeśli typ zostanie dodany, jego typ jest aktywowany.</span><span class="sxs-lookup"><span data-stu-id="475f3-627">If a type is added, it's type-activated.</span></span> <span data-ttu-id="475f3-628">Filtr aktywowany przez typ oznacza:</span><span class="sxs-lookup"><span data-stu-id="475f3-628">A type-activated filter means:</span></span>

* <span data-ttu-id="475f3-629">Wystąpienie jest tworzone dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-629">An instance is created for each request.</span></span>
* <span data-ttu-id="475f3-630">Wszystkie zależności konstruktora są wypełniane przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="475f3-630">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="475f3-631">Filtry zaimplementowane jako atrybuty i dodawane bezpośrednio do klas kontrolera lub metod akcji nie mogą mieć zależności konstruktorów udostępnianych przez [iniekcję zależności](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="475f3-631">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="475f3-632">Zależności konstruktora nie mogą być dostarczone przez DI, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="475f3-632">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="475f3-633">Atrybuty muszą mieć podane parametry konstruktora, w których są stosowane.</span><span class="sxs-lookup"><span data-stu-id="475f3-633">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="475f3-634">Jest to ograniczenie działania atrybutów.</span><span class="sxs-lookup"><span data-stu-id="475f3-634">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="475f3-635">Następujące filtry obsługują zależności konstruktora dostarczone z elementów DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-635">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="475f3-636"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> zaimplementowany dla atrybutu.</span><span class="sxs-lookup"><span data-stu-id="475f3-636"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="475f3-637">Powyższe filtry można zastosować do kontrolera lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-637">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="475f3-638">Rejestratory są dostępne z programu DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-638">Loggers are available from DI.</span></span> <span data-ttu-id="475f3-639">Należy jednak unikać tworzenia i używania filtrów wyłącznie w celach rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="475f3-639">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="475f3-640">[Wbudowane rejestrowanie struktury](xref:fundamentals/logging/index) zazwyczaj zapewnia, co jest potrzebne do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="475f3-640">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="475f3-641">Rejestrowanie dodane do filtrów:</span><span class="sxs-lookup"><span data-stu-id="475f3-641">Logging added to filters:</span></span>

* <span data-ttu-id="475f3-642">Należy skoncentrować się na problemach z domeną biznesową lub działaniu specyficznym dla filtra.</span><span class="sxs-lookup"><span data-stu-id="475f3-642">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="475f3-643">**Nie** należy rejestrować akcji ani innych zdarzeń struktury.</span><span class="sxs-lookup"><span data-stu-id="475f3-643">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="475f3-644">Wbudowane filtry akcje dziennika i zdarzenia struktury.</span><span class="sxs-lookup"><span data-stu-id="475f3-644">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="475f3-645">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="475f3-645">ServiceFilterAttribute</span></span>

<span data-ttu-id="475f3-646">Typy implementacji filtru usługi są zarejestrowane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="475f3-646">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="475f3-647"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> Pobiera wystąpienie filtru z DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-647">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="475f3-648">Poniższy kod ilustruje `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="475f3-648">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="475f3-649">W poniższym kodzie, `AddHeaderResultServiceFilter` jest dodawane do kontenera DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-649">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="475f3-650">W poniższym kodzie atrybut `ServiceFilter` Pobiera wystąpienie filtru `AddHeaderResultServiceFilter` z DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-650">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="475f3-651">Podczas używania `ServiceFilterAttribute`, należy ustawić [Servicefilterattribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="475f3-651">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="475f3-652">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="475f3-652">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="475f3-653">Środowisko uruchomieniowe ASP.NET Core nie gwarantuje:</span><span class="sxs-lookup"><span data-stu-id="475f3-653">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="475f3-654">Zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-654">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="475f3-655">Filtr nie zostanie ponownie żądany z kontenera DI w pewnym momencie.</span><span class="sxs-lookup"><span data-stu-id="475f3-655">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="475f3-656">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="475f3-656">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="475f3-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="475f3-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="475f3-658">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="475f3-658">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="475f3-659">`CreateInstance` ładuje określony typ z DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-659">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="475f3-660">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="475f3-660">TypeFilterAttribute</span></span>

<span data-ttu-id="475f3-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> przypomina <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ale jego typ nie jest rozpoznawany bezpośrednio w kontenerze DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="475f3-662">Tworzy wystąpienie tego typu przy użyciu <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="475f3-662">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="475f3-663">Ponieważ typy `TypeFilterAttribute` nie są rozpoznawane bezpośrednio w kontenerze DI:</span><span class="sxs-lookup"><span data-stu-id="475f3-663">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="475f3-664">Typy, do których odwołuje się `TypeFilterAttribute` nie muszą być zarejestrowane przy użyciu DI kontenera.</span><span class="sxs-lookup"><span data-stu-id="475f3-664">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="475f3-665">Ich zależności są spełnione przez kontener DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-665">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="475f3-666">`TypeFilterAttribute` może opcjonalnie zaakceptować argumenty konstruktora dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="475f3-666">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="475f3-667">W przypadku używania `TypeFilterAttribute`ustawienie [TypeFilterAttribute. IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="475f3-667">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="475f3-668">Zapewnia wskazówkę, że wystąpienie filtru *może* być ponownie używane poza zakresem żądania, który został utworzony w ramach.</span><span class="sxs-lookup"><span data-stu-id="475f3-668">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="475f3-669">Środowisko uruchomieniowe ASP.NET Core nie zapewnia żadnych gwarancji, że zostanie utworzone pojedyncze wystąpienie filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-669">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="475f3-670">Nie powinien być używany z filtrem, który zależy od usług z okresem istnienia innym niż pojedynczy.</span><span class="sxs-lookup"><span data-stu-id="475f3-670">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="475f3-671">Poniższy przykład pokazuje, jak przekazywać argumenty do typu przy użyciu `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="475f3-671">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="475f3-672">Filtry autoryzacji</span><span class="sxs-lookup"><span data-stu-id="475f3-672">Authorization filters</span></span>

<span data-ttu-id="475f3-673">Filtry autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-673">Authorization filters:</span></span>

* <span data-ttu-id="475f3-674">Są pierwszymi filtrami uruchamianymi w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-674">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="475f3-675">Kontrola dostępu do metod akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-675">Control access to action methods.</span></span>
* <span data-ttu-id="475f3-676">Ma metodę Before, ale nie po metodzie.</span><span class="sxs-lookup"><span data-stu-id="475f3-676">Have a before method, but no after method.</span></span>

<span data-ttu-id="475f3-677">Niestandardowe filtry autoryzacji wymagają niestandardowej struktury autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-677">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="475f3-678">Preferuj skonfigurowanie zasad autoryzacji lub zapisanie niestandardowych zasad autoryzacji przez zapisanie filtru niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="475f3-678">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="475f3-679">Wbudowany filtr autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-679">The built-in authorization filter:</span></span>

* <span data-ttu-id="475f3-680">Wywołuje system autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-680">Calls the authorization system.</span></span>
* <span data-ttu-id="475f3-681">Nie zezwala na żądania.</span><span class="sxs-lookup"><span data-stu-id="475f3-681">Does not authorize requests.</span></span>

<span data-ttu-id="475f3-682">**Nie** zgłaszaj wyjątków w ramach filtrów autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-682">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="475f3-683">Wyjątek nie zostanie obsłużony.</span><span class="sxs-lookup"><span data-stu-id="475f3-683">The exception will not be handled.</span></span>
* <span data-ttu-id="475f3-684">Filtry wyjątków nie będą obsługiwać wyjątku.</span><span class="sxs-lookup"><span data-stu-id="475f3-684">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="475f3-685">Rozważ wygenerowanie wyzwania w przypadku wystąpienia wyjątku w filtrze autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-685">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="475f3-686">Dowiedz się więcej o [autoryzacji](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="475f3-686">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="475f3-687">Filtry zasobów</span><span class="sxs-lookup"><span data-stu-id="475f3-687">Resource filters</span></span>

<span data-ttu-id="475f3-688">Filtry zasobów:</span><span class="sxs-lookup"><span data-stu-id="475f3-688">Resource filters:</span></span>

* <span data-ttu-id="475f3-689">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-689">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="475f3-690">Wykonanie powoduje Zawijanie większości potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-690">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="475f3-691">Tylko [filtry autoryzacji](#authorization-filters) są uruchamiane przed filtrami zasobów.</span><span class="sxs-lookup"><span data-stu-id="475f3-691">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="475f3-692">Filtry zasobów są przydatne do krótkiego obwodu większości potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-692">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="475f3-693">Na przykład filtr buforowania może uniknąć pozostałej części potoku w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="475f3-693">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="475f3-694">Przykłady filtru zasobów:</span><span class="sxs-lookup"><span data-stu-id="475f3-694">Resource filter examples:</span></span>

* <span data-ttu-id="475f3-695">[Filtr zasobów o krótkim obwodzie](#short-circuiting-resource-filter) pokazano wcześniej.</span><span class="sxs-lookup"><span data-stu-id="475f3-695">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="475f3-696">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="475f3-696">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="475f3-697">Uniemożliwia powiązanie modelu z dostępem do danych formularza.</span><span class="sxs-lookup"><span data-stu-id="475f3-697">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="475f3-698">Służy do przekazywania dużych plików w celu uniemożliwienia odczytywania danych formularza do pamięci.</span><span class="sxs-lookup"><span data-stu-id="475f3-698">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="475f3-699">Filtry akcji</span><span class="sxs-lookup"><span data-stu-id="475f3-699">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="475f3-700">Filtry akcji **nie** mają zastosowania do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475f3-700">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="475f3-701">Razor Pages obsługuje <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-701">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="475f3-702">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="475f3-702">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="475f3-703">Filtry akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-703">Action filters:</span></span>

* <span data-ttu-id="475f3-704">Zaimplementuj interfejs <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-704">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="475f3-705">Ich wykonanie otacza wykonywanie metod akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-705">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="475f3-706">Poniższy kod przedstawia przykładowy filtr akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-706">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="475f3-707"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> udostępnia następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="475f3-707">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="475f3-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> — umożliwia odczytywanie danych wejściowych metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="475f3-709"><xref:Microsoft.AspNetCore.Mvc.Controller> — włącza manipulowanie wystąpieniem kontrolera.</span><span class="sxs-lookup"><span data-stu-id="475f3-709"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="475f3-710"><xref:System.Web.Mvc.ActionExecutingContext.Result> — ustawienie `Result` wykonywanie krótkich obwodów metody akcji i kolejnych filtrów akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-710"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="475f3-711">Zgłaszanie wyjątku w metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-711">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="475f3-712">Zapobiega uruchamianiu kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="475f3-712">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="475f3-713">W przeciwieństwie do ustawienia `Result`, jest traktowany jako błąd, a nie pomyślny wynik.</span><span class="sxs-lookup"><span data-stu-id="475f3-713">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="475f3-714"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> zapewnia `Controller` i `Result` oraz następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="475f3-714">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="475f3-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-true, jeśli wykonywanie akcji zostało skrócone przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="475f3-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="475f3-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-wartość null, jeśli filtr akcji lub poprzednio uruchomionego wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="475f3-717">Ustawienie tej właściwości na wartość null:</span><span class="sxs-lookup"><span data-stu-id="475f3-717">Setting this property to null:</span></span>

  * <span data-ttu-id="475f3-718">Skutecznie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-718">Effectively handles the exception.</span></span>
  * <span data-ttu-id="475f3-719">`Result` jest wykonywany tak, jakby został zwrócony z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-719">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="475f3-720">W przypadku `IAsyncActionFilter`, wywołanie do <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="475f3-720">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="475f3-721">Wykonuje wszystkie kolejne filtry akcji i metodę akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-721">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="475f3-722">Zwraca wartość `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="475f3-722">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="475f3-723">Do krótkiego obwodu Przypisz <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> do wystąpienia wynikowego i nie wywołuj `next` (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="475f3-723">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="475f3-724">Struktura zawiera abstrakcyjną <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="475f3-724">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="475f3-725">Filtr akcji `OnActionExecuting` może służyć do:</span><span class="sxs-lookup"><span data-stu-id="475f3-725">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="475f3-726">Zweryfikuj stan modelu.</span><span class="sxs-lookup"><span data-stu-id="475f3-726">Validate model state.</span></span>
* <span data-ttu-id="475f3-727">Zwróć błąd, jeśli stan jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="475f3-727">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="475f3-728">Metoda `OnActionExecuted` jest uruchamiana po metodzie akcji:</span><span class="sxs-lookup"><span data-stu-id="475f3-728">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="475f3-729">I mogą przeglądać wyniki akcji przy użyciu właściwości <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> i manipulować nimi.</span><span class="sxs-lookup"><span data-stu-id="475f3-729">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="475f3-730"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> jest ustawiona na wartość true, jeśli wykonywanie akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="475f3-730"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="475f3-731"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> jest ustawiona na wartość różną od null, jeśli akcja lub kolejny filtr akcji wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-731"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="475f3-732">Ustawienie `Exception` na wartość null:</span><span class="sxs-lookup"><span data-stu-id="475f3-732">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="475f3-733">Efektywnie obsługuje wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-733">Effectively handles an exception.</span></span>
  * <span data-ttu-id="475f3-734">`ActionExecutedContext.Result` jest wykonywane tak, jakby były zwracane normalnie z metody akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-734">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="475f3-735">Filtry wyjątków</span><span class="sxs-lookup"><span data-stu-id="475f3-735">Exception filters</span></span>

<span data-ttu-id="475f3-736">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="475f3-736">Exception filters:</span></span>

* <span data-ttu-id="475f3-737">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="475f3-737">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="475f3-738">Może służyć do implementowania typowych zasad obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="475f3-738">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="475f3-739">Następujący przykładowy filtr wyjątku używa niestandardowego widoku błędów, aby wyświetlić szczegóły dotyczące wyjątków występujących podczas opracowywania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="475f3-739">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="475f3-740">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="475f3-740">Exception filters:</span></span>

* <span data-ttu-id="475f3-741">Nie mam zdarzeń przed i po.</span><span class="sxs-lookup"><span data-stu-id="475f3-741">Don't have before and after events.</span></span>
* <span data-ttu-id="475f3-742">Zaimplementuj <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="475f3-742">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="475f3-743">Obsłuż Nieobsłużone wyjątki występujące na stronie lub w tworzeniu kontrolera, [powiązania modelu](xref:mvc/models/model-binding), filtrach akcji lub metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-743">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="475f3-744">**Nie** należy przechwytywać wyjątków występujących w filtrach zasobów, filtrach wyników ani wykonywaniu wyniku MVC.</span><span class="sxs-lookup"><span data-stu-id="475f3-744">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="475f3-745">Aby obsłużyć wyjątek, ustaw właściwość <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> na `true` lub Zapisz odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="475f3-745">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="475f3-746">Spowoduje to zatrzymanie propagacji wyjątku.</span><span class="sxs-lookup"><span data-stu-id="475f3-746">This stops propagation of the exception.</span></span> <span data-ttu-id="475f3-747">Filtr wyjątku nie może przekształcić wyjątku w "powodzenie".</span><span class="sxs-lookup"><span data-stu-id="475f3-747">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="475f3-748">Można to zrobić tylko dla filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-748">Only an action filter can do that.</span></span>

<span data-ttu-id="475f3-749">Filtry wyjątków:</span><span class="sxs-lookup"><span data-stu-id="475f3-749">Exception filters:</span></span>

* <span data-ttu-id="475f3-750">Są dobre dla wyjątków zalewkowania występujących w ramach akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-750">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="475f3-751">Nie są tak elastyczne jak błędy obsługi oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="475f3-751">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="475f3-752">Preferuj oprogramowanie pośredniczące dla obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="475f3-752">Prefer middleware for exception handling.</span></span> <span data-ttu-id="475f3-753">Użyj filtrów wyjątków tylko w przypadku, gdy obsługa błędów *różni* się w zależności od tego, która metoda akcji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="475f3-753">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="475f3-754">Na przykład aplikacja może mieć metody akcji zarówno dla punktów końcowych interfejsu API, jak i dla widoków/HTML.</span><span class="sxs-lookup"><span data-stu-id="475f3-754">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="475f3-755">Punkty końcowe interfejsu API mogą zwracać informacje o błędach jako dane JSON, podczas gdy akcje oparte na widoku mogą zwrócić stronę błędu jako kod HTML.</span><span class="sxs-lookup"><span data-stu-id="475f3-755">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="475f3-756">Filtry wyników</span><span class="sxs-lookup"><span data-stu-id="475f3-756">Result filters</span></span>

<span data-ttu-id="475f3-757">Filtry wyników:</span><span class="sxs-lookup"><span data-stu-id="475f3-757">Result filters:</span></span>

* <span data-ttu-id="475f3-758">Zaimplementuj interfejs:</span><span class="sxs-lookup"><span data-stu-id="475f3-758">Implement an interface:</span></span>
  * <span data-ttu-id="475f3-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="475f3-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="475f3-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> lub <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="475f3-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="475f3-761">Ich wykonanie otacza wykonywanie wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-761">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="475f3-762">IResultFilter i IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="475f3-762">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="475f3-763">Poniższy kod pokazuje filtr wynikowy, który dodaje nagłówek HTTP:</span><span class="sxs-lookup"><span data-stu-id="475f3-763">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="475f3-764">Rodzaj wykonywanego wyniku zależy od akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-764">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="475f3-765">Akcja zwracająca widok obejmie wszystkie przetwarzanie Razor w ramach wykonywanej <xref:Microsoft.AspNetCore.Mvc.ViewResult>.</span><span class="sxs-lookup"><span data-stu-id="475f3-765">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="475f3-766">Metoda interfejsu API może wykonywać pewne serializacji w ramach wykonywania wyniku.</span><span class="sxs-lookup"><span data-stu-id="475f3-766">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="475f3-767">Dowiedz się więcej o [wynikach akcji](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="475f3-767">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="475f3-768">Filtry wynikowe są wykonywane tylko wtedy, gdy akcja lub filtr akcji generuje wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-768">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="475f3-769">Filtry wynikowe nie są wykonywane, gdy:</span><span class="sxs-lookup"><span data-stu-id="475f3-769">Result filters are not executed when:</span></span>

* <span data-ttu-id="475f3-770">Filtr autoryzacji lub filtr zasobów jest krótki obwody potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-770">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="475f3-771">Filtr wyjątku obsługuje wyjątek przez wygenerowanie wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-771">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="475f3-772">Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> może skrócić wykonywanie wyniku akcji i kolejnych filtrów wyników przez ustawienie <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> do `true`.</span><span class="sxs-lookup"><span data-stu-id="475f3-772">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="475f3-773">Zapisuj w obiekcie Response podczas krótkiego obwodu, aby uniknąć generowania pustej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="475f3-773">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="475f3-774">Zgłaszanie wyjątku w `IResultFilter.OnResultExecuting` spowoduje:</span><span class="sxs-lookup"><span data-stu-id="475f3-774">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="475f3-775">Zapobiegaj wykonaniu wyniku akcji i kolejnych filtrów.</span><span class="sxs-lookup"><span data-stu-id="475f3-775">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="475f3-776">Być traktowany jako niepowodzenie, a nie wynikowy pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="475f3-776">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="475f3-777">Po uruchomieniu metody <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> odpowiedź została już wysłana do klienta.</span><span class="sxs-lookup"><span data-stu-id="475f3-777">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="475f3-778">Jeśli odpowiedź została już wysłana do klienta, nie można jej zmienić.</span><span class="sxs-lookup"><span data-stu-id="475f3-778">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="475f3-779">`ResultExecutedContext.Canceled` jest ustawiona na `true`, jeśli wykonywanie wyniku akcji było krótkie przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="475f3-779">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="475f3-780">`ResultExecutedContext.Exception` ma ustawioną wartość różną od null, jeśli wynik akcji lub kolejny filtr wynikowy wywołał wyjątek.</span><span class="sxs-lookup"><span data-stu-id="475f3-780">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="475f3-781">Ustawienie `Exception` do wartości null skutecznie obsługuje wyjątek i uniemożliwia ponowne zgłoszenie wyjątku przez ASP.NET Core w dalszej części potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-781">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="475f3-782">Nie istnieje niezawodny sposób zapisu danych do odpowiedzi podczas obsługi wyjątku w filtrze wynikowym.</span><span class="sxs-lookup"><span data-stu-id="475f3-782">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="475f3-783">Jeśli nagłówki zostały opróżnione do klienta, gdy wynikiem akcji zgłasza wyjątek, nie istnieje niezawodny mechanizm wysyłania kodu błędu.</span><span class="sxs-lookup"><span data-stu-id="475f3-783">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="475f3-784">W przypadku <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>wywołanie `await next` na <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> wykonuje wszystkie kolejne filtry wynikowe i wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-784">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="475f3-785">Do krótkiego obwodu Ustaw [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) do `true` i nie wywołuj `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="475f3-785">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="475f3-786">Struktura zawiera abstrakcyjną `ResultFilterAttribute`, która może być podklasą.</span><span class="sxs-lookup"><span data-stu-id="475f3-786">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="475f3-787">Przedstawiona wcześniej Klasa [Addheaderattribute](#add-header-attribute) jest przykładem atrybutu filtru wynikowego.</span><span class="sxs-lookup"><span data-stu-id="475f3-787">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="475f3-788">IAlwaysRunResultFilter i IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="475f3-788">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="475f3-789">Interfejsy <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> i <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> deklarują implementację <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter>, która jest uruchamiana dla wszystkich wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-789">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="475f3-790">Obejmuje to wyniki akcji generowane przez:</span><span class="sxs-lookup"><span data-stu-id="475f3-790">This includes action results produced by:</span></span>

* <span data-ttu-id="475f3-791">Filtry autoryzacji i filtry zasobów, które są skracane.</span><span class="sxs-lookup"><span data-stu-id="475f3-791">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="475f3-792">Filtry wyjątków.</span><span class="sxs-lookup"><span data-stu-id="475f3-792">Exception filters.</span></span>

<span data-ttu-id="475f3-793">Na przykład następujący filtr jest zawsze uruchamiany i ustawia wynik akcji (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) z kodem stanu *jednostki nieprzetwarzanego 422* , gdy negocjowanie zawartości kończy się niepowodzeniem:</span><span class="sxs-lookup"><span data-stu-id="475f3-793">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="475f3-794">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="475f3-794">IFilterFactory</span></span>

<span data-ttu-id="475f3-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="475f3-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="475f3-796">W związku z tym wystąpienie `IFilterFactory` może być używane jako wystąpienie `IFilterMetadata` w dowolnym miejscu w potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-796">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="475f3-797">Gdy środowisko uruchomieniowe przygotowuje się do wywołania filtru, próbuje rzutować go na `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="475f3-797">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="475f3-798">Jeśli rzutowanie powiedzie się, Metoda <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> zostanie wywołana w celu utworzenia wystąpienia `IFilterMetadata`, które jest wywoływane.</span><span class="sxs-lookup"><span data-stu-id="475f3-798">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="475f3-799">Zapewnia to elastyczny projekt, ponieważ dokładny potok filtru nie musi być ustawiony jawnie podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="475f3-799">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="475f3-800">`IFilterFactory` można zaimplementować przy użyciu niestandardowych implementacji atrybutów jako inne podejście do tworzenia filtrów:</span><span class="sxs-lookup"><span data-stu-id="475f3-800">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="475f3-801">Poprzedni kod może być testowany przez uruchomienie [przykładu pobierania](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="475f3-801">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="475f3-802">Wywołaj narzędzia deweloperskie F12.</span><span class="sxs-lookup"><span data-stu-id="475f3-802">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="475f3-803">Przejdź do adresu `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="475f3-803">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="475f3-804">Narzędzia programistyczne F12 wyświetlają następujące nagłówki odpowiedzi dodane przez przykładowy kod:</span><span class="sxs-lookup"><span data-stu-id="475f3-804">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="475f3-805">**autor:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="475f3-805">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="475f3-806">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="475f3-806">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="475f3-807">**wewnętrzne:** `My header`</span><span class="sxs-lookup"><span data-stu-id="475f3-807">**internal:** `My header`</span></span>

<span data-ttu-id="475f3-808">Poprzedni kod tworzy nagłówek odpowiedzi **wewnętrznej:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="475f3-808">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="475f3-809">IFilterFactory zaimplementowane dla atrybutu</span><span class="sxs-lookup"><span data-stu-id="475f3-809">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="475f3-810">Filtry implementujące `IFilterFactory` są przydatne w przypadku filtrów, które:</span><span class="sxs-lookup"><span data-stu-id="475f3-810">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="475f3-811">Nie wymagaj parametrów przekazujących.</span><span class="sxs-lookup"><span data-stu-id="475f3-811">Don't require passing parameters.</span></span>
* <span data-ttu-id="475f3-812">Mają zależności konstruktora, które muszą być wypełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="475f3-812">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="475f3-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementuje <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="475f3-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="475f3-814">`IFilterFactory` uwidacznia metodę <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> do tworzenia wystąpienia <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="475f3-814">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="475f3-815">`CreateInstance` ładuje określony typ z kontenera usług (DI).</span><span class="sxs-lookup"><span data-stu-id="475f3-815">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="475f3-816">Poniższy kod przedstawia trzy podejścia do stosowania `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="475f3-816">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="475f3-817">W poprzednim kodzie dekorowania nazwy metodę `[SampleActionFilter]` jest preferowanym podejściem do zastosowania `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="475f3-817">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="475f3-818">Używanie oprogramowania pośredniczącego w potoku filtru</span><span class="sxs-lookup"><span data-stu-id="475f3-818">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="475f3-819">Filtry zasobów działają podobnie jak [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) w celu obsłużenia wykonywania wszystkich elementów, które są późniejsze w potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-819">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="475f3-820">Ale filtry różnią się od oprogramowania pośredniczącego w tym, że są częścią środowiska uruchomieniowego ASP.NET Core, co oznacza, że ma dostęp do ASP.NET Core kontekstu i konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="475f3-820">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="475f3-821">Aby użyć oprogramowania pośredniczącego jako filtru, należy utworzyć typ z metodą `Configure`, która określa oprogramowanie pośredniczące, które ma zostać dodane do potoku filtru.</span><span class="sxs-lookup"><span data-stu-id="475f3-821">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="475f3-822">W poniższym przykładzie jest wykorzystywane oprogramowanie pośredniczące lokalizacyjne do ustalenia bieżącej kultury dla żądania:</span><span class="sxs-lookup"><span data-stu-id="475f3-822">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="475f3-823">Użyj <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute>, aby uruchomić oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="475f3-823">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="475f3-824">Filtry oprogramowania pośredniczącego są uruchamiane na tym samym etapie potoku filtru jako filtry zasobów przed powiązaniem modelu i po pozostałej części potoku.</span><span class="sxs-lookup"><span data-stu-id="475f3-824">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="475f3-825">Następne akcje</span><span class="sxs-lookup"><span data-stu-id="475f3-825">Next actions</span></span>

* <span data-ttu-id="475f3-826">Zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="475f3-826">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="475f3-827">Aby eksperymentować z filtrami, należy [pobrać, przetestować i zmodyfikować przykład usługi GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="475f3-827">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
