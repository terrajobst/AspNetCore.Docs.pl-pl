---
title: Metody filtrowania dla stron Razor w programie ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak utworzyć metody filtrowania dla stron Razor w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: d9d4ea65a9357d19c283036e7ab9417e0deaeda2
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011721"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="47fc8-103">Metody filtrowania dla stron Razor w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47fc8-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="47fc8-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47fc8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47fc8-105">Strona razor filtry [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) i [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Zezwalaj stron Razor do uruchomienia kodu przed i po uruchomieniu programu obsługi stron Razor.</span><span class="sxs-lookup"><span data-stu-id="47fc8-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="47fc8-106">Filtry stron razor są podobne do [ASP.NET Core MVC filtry akcji](xref:mvc/controllers/filters#action-filters), z wyjątkiem nie można zastosować do metody obsługi poszczególnych stron.</span><span class="sxs-lookup"><span data-stu-id="47fc8-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span> 

<span data-ttu-id="47fc8-107">Strona razor filtrów:</span><span class="sxs-lookup"><span data-stu-id="47fc8-107">Razor Page filters:</span></span>

* <span data-ttu-id="47fc8-108">Uruchom kod, po wybraniu metody obsługi, ale przed wystąpieniem wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="47fc8-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="47fc8-109">Uruchom kod, przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="47fc8-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="47fc8-110">Uruchomienie kodu po wykonaniu metody programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="47fc8-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="47fc8-111">Można zaimplementować na stronie lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="47fc8-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="47fc8-112">Nie można zastosować do metody obsługi określonej strony.</span><span class="sxs-lookup"><span data-stu-id="47fc8-112">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="47fc8-113">Kod może działać, zanim metoda obsługi jest wykonywana przy użyciu konstruktora strony lub oprogramowanie pośredniczące, ale tylko filtry stron Razor mają dostęp do [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="47fc8-113">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="47fc8-114">Filtry mają [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) pochodne parametr, który zapewnia dostęp do `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="47fc8-114">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="47fc8-115">Na przykład [implementacji atrybutu filtru](#ifa) Przykładowa aplikacja dodaje nagłówek odpowiedzi, coś, co nie można wykonać za pomocą konstruktorów i oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="47fc8-115">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="47fc8-116">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47fc8-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="47fc8-117">Filtry stron razor zapewniają następujące metody, które mogą być stosowane globalnie lub na poziomie strony:</span><span class="sxs-lookup"><span data-stu-id="47fc8-117">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="47fc8-118">Synchroniczne metody:</span><span class="sxs-lookup"><span data-stu-id="47fc8-118">Synchronous methods:</span></span>

    * <span data-ttu-id="47fc8-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : metoda wywoływana po metody obsługi został wybrany, ale przed modelu występuje powiązania.</span><span class="sxs-lookup"><span data-stu-id="47fc8-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="47fc8-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : metoda wywoływana przed wykonaniem metody obsługi, po zakończeniu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="47fc8-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
    * <span data-ttu-id="47fc8-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : metoda wywoływana po wykonaniu metody obsługi, zanim wynik akcji.</span><span class="sxs-lookup"><span data-stu-id="47fc8-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="47fc8-122">Metody asynchroniczne:</span><span class="sxs-lookup"><span data-stu-id="47fc8-122">Asynchronous methods:</span></span>

    * <span data-ttu-id="47fc8-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : wywoływane asynchronicznie po metody obsługi został wybrany, ale przed modelu występuje powiązania.</span><span class="sxs-lookup"><span data-stu-id="47fc8-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
    * <span data-ttu-id="47fc8-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : wywoływane asynchronicznie przed wywołaniem metody obsługi po zakończeniu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="47fc8-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="47fc8-125">Implementowanie **albo** synchronicznej lub asynchronicznej wersję interfejsu filtru, nie obydwa.</span><span class="sxs-lookup"><span data-stu-id="47fc8-125">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="47fc8-126">Struktura najpierw sprawdza, czy filtr implementuje interfejs asynchroniczne, a jeśli tak jest, wywołuje metodę.</span><span class="sxs-lookup"><span data-stu-id="47fc8-126">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="47fc8-127">W przeciwnym razie wywołuje metody synchronicznej interfejsu.</span><span class="sxs-lookup"><span data-stu-id="47fc8-127">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="47fc8-128">Jeśli oba interfejsy są wdrożone, są nazywane tylko metody asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="47fc8-128">If both interfaces are implemented, only the async methods are be called.</span></span> <span data-ttu-id="47fc8-129">Ta sama zasada dotyczy przesłonięć na stronach, zaimplementuj synchronicznej lub asynchronicznej wersję override, nie obydwa.</span><span class="sxs-lookup"><span data-stu-id="47fc8-129">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="47fc8-130">Implementowanie globalnie filtrów stron Razor</span><span class="sxs-lookup"><span data-stu-id="47fc8-130">Implement Razor Page filters globally</span></span>

<span data-ttu-id="47fc8-131">Poniższy kod implementuje `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="47fc8-131">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="47fc8-132">W poprzednim kodzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="47fc8-132">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="47fc8-133">W przykładzie służy do zapewnienia informacji o śledzeniu dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47fc8-133">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="47fc8-134">Poniższy kod umożliwia `SampleAsyncPageFilter` w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="47fc8-134">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="47fc8-135">Poniższy kod przedstawia pełny `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="47fc8-135">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="47fc8-136">Poniższy kod wywoła `AddFolderApplicationModelConvention` do zastosowania `SampleAsyncPageFilter` tylko stron */subFolder*:</span><span class="sxs-lookup"><span data-stu-id="47fc8-136">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="47fc8-137">Poniższy kod implementuje synchronicznej `IPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="47fc8-137">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="47fc8-138">Poniższy kod umożliwia `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="47fc8-138">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="47fc8-139">Strona Razor Implementowanie filtry poprzez zastąpienie metody filtru</span><span class="sxs-lookup"><span data-stu-id="47fc8-139">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="47fc8-140">Poniższy kod zastępuje synchroniczne filtrów stron Razor:</span><span class="sxs-lookup"><span data-stu-id="47fc8-140">The following code overrides the synchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a><span data-ttu-id="47fc8-141">Implementowanie atrybutu filtru</span><span class="sxs-lookup"><span data-stu-id="47fc8-141">Implement a filter attribute</span></span>

<span data-ttu-id="47fc8-142">Wbudowany filtr opartych na atrybutach [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtru może być podklasą klasy.</span><span class="sxs-lookup"><span data-stu-id="47fc8-142">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="47fc8-143">Następujący filtr dodaje do odpowiedzi nagłówek:</span><span class="sxs-lookup"><span data-stu-id="47fc8-143">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="47fc8-144">Poniższy kod stosuje `AddHeader` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="47fc8-144">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="47fc8-145">Zobacz [zastąpienie domyślnej kolejności](xref:mvc/controllers/filters#overriding-the-default-order) instrukcje w przypadku przesłaniania kolejności.</span><span class="sxs-lookup"><span data-stu-id="47fc8-145">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="47fc8-146">Zobacz [anulowania i krótki circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) instrukcje dotyczące zwarcie potoku filtru z filtru.</span><span class="sxs-lookup"><span data-stu-id="47fc8-146">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a><span data-ttu-id="47fc8-147">Autoryzuj atrybutu filtru</span><span class="sxs-lookup"><span data-stu-id="47fc8-147">Authorize filter attribute</span></span>

<span data-ttu-id="47fc8-148">[Autoryzuj](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) atrybut można stosować do `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="47fc8-148">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
