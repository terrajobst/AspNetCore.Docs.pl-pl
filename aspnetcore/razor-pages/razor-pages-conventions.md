---
title: Razor Pages Konwencji tras i aplikacji w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób Konwencja i konwencje dostawcy modelu aplikacji ułatwiają kontrolowanie routingu, odnajdywania i przetwarzania stron.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/22/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: a0a1eda69da34d1865fd11ef464c3697bcd01eff
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779214"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="47e78-103">Razor Pages Konwencji tras i aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47e78-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="47e78-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="47e78-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="47e78-105">Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="47e78-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="47e78-106">Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="47e78-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="47e78-107">Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony.</span><span class="sxs-lookup"><span data-stu-id="47e78-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="47e78-108">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="47e78-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="47e78-109">Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="47e78-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="47e78-110">Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="47e78-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="47e78-111">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47e78-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="47e78-112">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="47e78-112">Scenario</span></span> | <span data-ttu-id="47e78-113">Przykład ilustruje...</span><span class="sxs-lookup"><span data-stu-id="47e78-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="47e78-114">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="47e78-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="47e78-115">Konwencje. Add</span><span class="sxs-lookup"><span data-stu-id="47e78-115">Conventions.Add</span></span><ul><li><span data-ttu-id="47e78-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="47e78-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="47e78-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="47e78-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="47e78-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="47e78-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="47e78-119">Dodaj szablon trasy i nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47e78-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="47e78-120">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="47e78-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="47e78-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="47e78-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="47e78-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="47e78-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="47e78-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="47e78-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="47e78-124">Dodaj szablon trasy do stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="47e78-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="47e78-125">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="47e78-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="47e78-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="47e78-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="47e78-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="47e78-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="47e78-128">ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</span><span class="sxs-lookup"><span data-stu-id="47e78-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="47e78-129">Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47e78-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="47e78-130">Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="47e78-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="47e78-131">Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="47e78-131">The following convention examples are explained later in this topic:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add( ... );
            options.Conventions.AddFolderRouteModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageRouteModelConvention(
                "/About", model => { ... });
            options.Conventions.AddPageRoute(
                "/Contact", "TheContactPage/{text?}");
            options.Conventions.AddFolderApplicationModelConvention(
                "/OtherPages", model => { ... });
            options.Conventions.AddPageApplicationModelConvention(
                "/About", model => { ... });
            options.Conventions.ConfigureFilter(model => { ... });
            options.Conventions.ConfigureFilter( ... );
        });
}
```

::: moniker-end

## <a name="route-order"></a><span data-ttu-id="47e78-132">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="47e78-132">Route order</span></span>

<span data-ttu-id="47e78-133">Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="47e78-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="47e78-134">Porządek</span><span class="sxs-lookup"><span data-stu-id="47e78-134">Order</span></span>            | <span data-ttu-id="47e78-135">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="47e78-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="47e78-136">-1</span><span class="sxs-lookup"><span data-stu-id="47e78-136">-1</span></span>               | <span data-ttu-id="47e78-137">Trasa jest przetwarzana przed przetworzeniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="47e78-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="47e78-138">0</span><span class="sxs-lookup"><span data-stu-id="47e78-138">0</span></span>                | <span data-ttu-id="47e78-139">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="47e78-139">Order isn't specified (default value).</span></span> <span data-ttu-id="47e78-140">Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="47e78-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="47e78-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="47e78-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="47e78-142">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="47e78-143">Przetwarzanie trasy zostało ustanowione według Konwencji:</span><span class="sxs-lookup"><span data-stu-id="47e78-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="47e78-144">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="47e78-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="47e78-145">Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.</span><span class="sxs-lookup"><span data-stu-id="47e78-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="47e78-146">Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="47e78-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="47e78-147">Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="47e78-148">Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="47e78-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="47e78-149">Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania.</span><span class="sxs-lookup"><span data-stu-id="47e78-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="47e78-150">Postaraj się, aby uprościć schemat routingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47e78-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="47e78-151">Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47e78-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="47e78-152">Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="47e78-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="47e78-153">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="47e78-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="47e78-154">Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="47e78-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="47e78-155">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="47e78-155">Model conventions</span></span>

<span data-ttu-id="47e78-156">Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="47e78-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="47e78-157">Dodawanie Konwencji modelu trasy do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="47e78-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="47e78-158">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.</span><span class="sxs-lookup"><span data-stu-id="47e78-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="47e78-159">Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="47e78-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="47e78-160">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`.</span><span class="sxs-lookup"><span data-stu-id="47e78-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="47e78-161">Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="47e78-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="47e78-162">Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="47e78-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="47e78-163">Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="47e78-164">Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="47e78-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="47e78-165">Szablon `{aboutTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="47e78-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="47e78-166">Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="47e78-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="47e78-167">Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="47e78-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="47e78-168">Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="47e78-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="47e78-169">Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="47e78-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="47e78-170">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="47e78-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="47e78-171">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="47e78-172">Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="47e78-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="47e78-173">Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="47e78-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="47e78-174">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="47e78-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="47e78-177">Dodawanie Konwencji modelu aplikacji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="47e78-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="47e78-178">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47e78-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="47e78-179">Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="47e78-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="47e78-180">Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów.</span><span class="sxs-lookup"><span data-stu-id="47e78-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="47e78-181">Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="47e78-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="47e78-182">Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="47e78-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="47e78-183">Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader` do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="47e78-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="47e78-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="47e78-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="47e78-185">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="47e78-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="47e78-187">Dodawanie Konwencji modelu programu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="47e78-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="47e78-188">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.</span><span class="sxs-lookup"><span data-stu-id="47e78-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="47e78-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="47e78-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="47e78-190">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="47e78-190">Page route action conventions</span></span>

<span data-ttu-id="47e78-191">Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.</span><span class="sxs-lookup"><span data-stu-id="47e78-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="47e78-192">Konwencja modelu trasy folderu</span><span class="sxs-lookup"><span data-stu-id="47e78-192">Folder route model convention</span></span>

<span data-ttu-id="47e78-193">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="47e78-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="47e78-194">Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="47e78-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="47e78-195">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="47e78-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="47e78-196">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="47e78-197">Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="47e78-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="47e78-198">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="47e78-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="47e78-199">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="47e78-200">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="47e78-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="47e78-203">Konwencja modelu trasy strony</span><span class="sxs-lookup"><span data-stu-id="47e78-203">Page route model convention</span></span>

<span data-ttu-id="47e78-204">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="47e78-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="47e78-205">Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="47e78-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="47e78-206">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="47e78-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="47e78-207">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="47e78-208">Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="47e78-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="47e78-209">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="47e78-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="47e78-210">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="47e78-211">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="47e78-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="47e78-214">Dostosowywanie tras stron przy użyciu transformatora parametrów</span><span class="sxs-lookup"><span data-stu-id="47e78-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="47e78-215">Trasy stron generowane przez ASP.NET Core mogą być dostosowywane przy użyciu transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="47e78-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="47e78-216">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="47e78-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="47e78-217">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="47e78-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="47e78-218">Konwencja modelu trasy strony `PageRouteTransformerConvention` stosuje transformator parametrów do segmentów nazw folderów i plików w przypadku automatycznie generowanych tras stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47e78-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="47e78-219">Na przykład plik Razor Pages w */Pages/SubscriptionManagement/ViewAll.cshtml* będzie miał swoją trasę ponownie zapisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="47e78-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="47e78-220">`PageRouteTransformerConvention` przekształca tylko automatycznie generowane segmenty trasy strony, która pochodzi z folderu Razor Pages i nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="47e78-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="47e78-221">Nie przekształca segmentów tras dodanych z dyrektywą `@page`.</span><span class="sxs-lookup"><span data-stu-id="47e78-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="47e78-222">Konwencja nie przetwarza również tras dodanych przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="47e78-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="47e78-223">@No__t_0 jest zarejestrowana jako opcja w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="47e78-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(
                new PageRouteTransformerConvention(
                    new SlugifyParameterTransformer()));
        });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a><span data-ttu-id="47e78-224">Konfigurowanie trasy strony</span><span class="sxs-lookup"><span data-stu-id="47e78-224">Configure a page route</span></span>

<span data-ttu-id="47e78-225">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony.</span><span class="sxs-lookup"><span data-stu-id="47e78-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="47e78-226">Wygenerowane linki do strony używają określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="47e78-227">`AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.</span><span class="sxs-lookup"><span data-stu-id="47e78-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="47e78-228">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="47e78-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="47e78-229">Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="47e78-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="47e78-230">Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="47e78-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="47e78-231">Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="47e78-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="47e78-232">Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:</span><span class="sxs-lookup"><span data-stu-id="47e78-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="47e78-235">Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact` lub trasy niestandardowej, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="47e78-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="47e78-236">Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="47e78-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykładowa przeglądarka brzegowa dostarczająca opcjonalny segment trasy "text" elementu "TextValue" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="47e78-239">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="47e78-239">Page model action conventions</span></span>

<span data-ttu-id="47e78-240">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron.</span><span class="sxs-lookup"><span data-stu-id="47e78-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="47e78-241">Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.</span><span class="sxs-lookup"><span data-stu-id="47e78-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="47e78-242">W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="47e78-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="47e78-243">Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="47e78-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="47e78-244">**Konwencja modelu aplikacji folderu**</span><span class="sxs-lookup"><span data-stu-id="47e78-244">**Folder app model convention**</span></span>

<span data-ttu-id="47e78-245">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="47e78-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="47e78-246">Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader` do stron w folderze *OtherPages* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="47e78-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="47e78-247">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="47e78-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="47e78-249">**Konwencja modelu aplikacji strony**</span><span class="sxs-lookup"><span data-stu-id="47e78-249">**Page app model convention**</span></span>

<span data-ttu-id="47e78-250">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="47e78-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="47e78-251">Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="47e78-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="47e78-252">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="47e78-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="47e78-254">**Konfigurowanie filtru**</span><span class="sxs-lookup"><span data-stu-id="47e78-254">**Configure a filter**</span></span>

<span data-ttu-id="47e78-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować.</span><span class="sxs-lookup"><span data-stu-id="47e78-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="47e78-256">Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:</span><span class="sxs-lookup"><span data-stu-id="47e78-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="47e78-257">Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* .</span><span class="sxs-lookup"><span data-stu-id="47e78-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="47e78-258">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="47e78-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="47e78-259">W przeciwnym razie zostanie zastosowana `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="47e78-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="47e78-260">`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="47e78-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="47e78-261">Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="47e78-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="47e78-262">Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="47e78-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="47e78-264">**Konfigurowanie fabryki filtrów**</span><span class="sxs-lookup"><span data-stu-id="47e78-264">**Configure a filter factory**</span></span>

<span data-ttu-id="47e78-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="47e78-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="47e78-266">Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:</span><span class="sxs-lookup"><span data-stu-id="47e78-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="47e78-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="47e78-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="47e78-268">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="47e78-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="47e78-270">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="47e78-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="47e78-271">[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="47e78-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="47e78-272">Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="47e78-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="47e78-273">Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .</span><span class="sxs-lookup"><span data-stu-id="47e78-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="47e78-274">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="47e78-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="47e78-275">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="47e78-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47e78-276">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="47e78-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
