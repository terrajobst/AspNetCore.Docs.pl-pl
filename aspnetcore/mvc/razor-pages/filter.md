---
title: Metody filtru dla elementu Razor strony platformy ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak utworzyć filtr metody dla stron Razor w ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/5/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/filter
ms.openlocfilehash: b04253b9240cb88c4f0d3824a4b9fda947d6da08
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/19/2018
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="c7022-103">Metody filtru dla elementu Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7022-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c7022-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7022-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c7022-105">Filtry stron razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Zezwalaj stron Razor do uruchomienia kodu przed i po uruchomieniu programu obsługi stron Razor.</span><span class="sxs-lookup"><span data-stu-id="c7022-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="c7022-106">Są podobne do filtrów stron razor [platformy ASP.NET Core MVC filtry akcji](xref:mvc/controllers/filters#action-filters), z wyjątkiem nie można zastosować do metody obsługi poszczególnych stron.</span><span class="sxs-lookup"><span data-stu-id="c7022-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span> 

<span data-ttu-id="c7022-107">Filtry stron razor:</span><span class="sxs-lookup"><span data-stu-id="c7022-107">Razor Page filters:</span></span>

* <span data-ttu-id="c7022-108">Uruchomić kod po wybraniu metody obsługi, ale przed wystąpieniem wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c7022-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="c7022-109">Uruchomić kod przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c7022-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="c7022-110">Uruchomienia kodu po wykonaniu metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="c7022-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="c7022-111">Można zaimplementować na stronie lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="c7022-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="c7022-112">Nie można zastosować do metody obsługi określonej strony.</span><span class="sxs-lookup"><span data-stu-id="c7022-112">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="c7022-113">Można uruchomić kod przed metoda obsługi jest wykonywana przy użyciu konstruktora strony lub oprogramowanie pośredniczące, ale tylko filtry Razor strony mają dostęp do [element HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="c7022-113">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="c7022-114">Filtry mają [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) pochodnych parametr, który zapewnia dostęp do `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c7022-114">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="c7022-115">Na przykład [zaimplementować atrybutu filtru](#ifa) próbki dodaje nagłówek odpowiedzi, coś, co nie można przeprowadzić z konstruktorów lub oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="c7022-115">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="c7022-116">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7022-116">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c7022-117">Filtry stron razor zawierają następujące metody, które mogą być stosowane globalnie lub na poziomie strony:</span><span class="sxs-lookup"><span data-stu-id="c7022-117">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="c7022-118">Metod synchronicznych:</span><span class="sxs-lookup"><span data-stu-id="c7022-118">Synchronous methods:</span></span>

    * <span data-ttu-id="c7022-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : wywoływana po wybrana została metoda obsługi, ale przed modelu występuje powiązania.</span><span class="sxs-lookup"><span data-stu-id="c7022-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="c7022-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : metoda wywoływana przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c7022-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
    * <span data-ttu-id="c7022-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : metoda wywoływana po wykonaniu metody obsługi, zanim wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="c7022-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="c7022-122">Metod asynchronicznych:</span><span class="sxs-lookup"><span data-stu-id="c7022-122">Asynchronous methods:</span></span>

    * <span data-ttu-id="c7022-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : wywołana asynchronicznie po metoda obsługi została wybrana, ale przed modelu występuje powiązania.</span><span class="sxs-lookup"><span data-stu-id="c7022-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="c7022-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : wywołana asynchronicznie przed wywołaniem metody obsługi po zakończeniu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c7022-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="c7022-125">Implementowanie **albo** synchronicznego lub wersji asynchronicznego interfejsu filtru, nie oba.</span><span class="sxs-lookup"><span data-stu-id="c7022-125">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="c7022-126">Platformę najpierw sprawdza, czy filtr implementuje interfejs asynchronicznych, a jeśli tak, który wywołuje.</span><span class="sxs-lookup"><span data-stu-id="c7022-126">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="c7022-127">Jeśli nie wywołuje metody interfejsu synchronicznego.</span><span class="sxs-lookup"><span data-stu-id="c7022-127">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="c7022-128">Jeśli zostaną zaimplementowane dotyczą obu interfejsów, są nazywane tylko metody asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="c7022-128">If both interfaces are implemented, only the async methods are be called.</span></span> <span data-ttu-id="c7022-129">Tę samą zasadę stosuje się do zastąpienia na stronach, wdrożenie synchronicznego lub wersja async zastąpienie, nie oba.</span><span class="sxs-lookup"><span data-stu-id="c7022-129">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="c7022-130">Implementowanie globalnie filtrów stron Razor</span><span class="sxs-lookup"><span data-stu-id="c7022-130">Implement Razor Page filters globally</span></span>

<span data-ttu-id="c7022-131">Poniższy kod implementuje `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="c7022-131">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="c7022-132">W powyższym kodzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="c7022-132">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="c7022-133">Jest używany w próbce o podanie informacji śledzenia dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7022-133">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="c7022-134">Poniższy kod umożliwia `SampleAsyncPageFilter` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="c7022-134">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="c7022-135">Poniższy kod przedstawia pełną `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="c7022-135">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="c7022-136">Poniższy kod wywołania `AddFolderApplicationModelConvention` dotyczyć `SampleAsyncPageFilter` tylko stron */subFolder*:</span><span class="sxs-lookup"><span data-stu-id="c7022-136">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="c7022-137">Poniższy kod implementuje synchroniczne `IPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="c7022-137">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="c7022-138">Poniższy kod umożliwia `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="c7022-138">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"
## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="c7022-139">Implementowanie filtrów stron Razor przez zastąpienie metody filtru</span><span class="sxs-lookup"><span data-stu-id="c7022-139">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="c7022-140">Poniższy kod zastępuje synchroniczne filtry Razor strony:</span><span class="sxs-lookup"><span data-stu-id="c7022-140">The following code overrides the synchronous Razor Page filters:</span></span>

<span data-ttu-id="c7022-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="c7022-141">[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]</span></span>

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a><span data-ttu-id="c7022-142">Implementowanie atrybutu filtru</span><span class="sxs-lookup"><span data-stu-id="c7022-142">Implement a filter attribute</span></span>

<span data-ttu-id="c7022-143">Wbudowany filtr na podstawie atrybutów [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtru może być podklasą klasy.</span><span class="sxs-lookup"><span data-stu-id="c7022-143">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="c7022-144">Następujący filtr dodaje do odpowiedzi nagłówek:</span><span class="sxs-lookup"><span data-stu-id="c7022-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="c7022-145">Poniższy kod ma zastosowanie `AddHeader` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="c7022-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="c7022-146">Zobacz [zastępowanie domyślna kolejność](xref:mvc/controllers/filters#overriding-the-default-order) instrukcje dotyczące Zastępowanie kolejności.</span><span class="sxs-lookup"><span data-stu-id="c7022-146">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="c7022-147">Zobacz [anulowania i krótki circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) instrukcje zwarcia potoku filtru z filtru.</span><span class="sxs-lookup"><span data-stu-id="c7022-147">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a><span data-ttu-id="c7022-148">Atrybut Filtr autoryzacji</span><span class="sxs-lookup"><span data-stu-id="c7022-148">Authorize filter attribute</span></span>

<span data-ttu-id="c7022-149">[Autoryzacji](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) atrybut można stosować do `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="c7022-149">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
