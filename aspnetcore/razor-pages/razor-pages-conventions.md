---
title: Razor Pages Konwencji tras i aplikacji w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób Konwencja i konwencje dostawcy modelu aplikacji ułatwiają kontrolowanie routingu, odnajdywania i przetwarzania stron.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: d8377c0a0b8a29fe4b6a7fa67beeff84927c8b74
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114773"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="8e199-103">Razor Pages Konwencji tras i aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e199-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="8e199-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8e199-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8e199-105">Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="8e199-106">Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="8e199-107">Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="8e199-108">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="8e199-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="8e199-109">Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e199-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="8e199-110">Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="8e199-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="8e199-111">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e199-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="8e199-112">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="8e199-112">Scenario</span></span> | <span data-ttu-id="8e199-113">Przykład ilustruje...</span><span class="sxs-lookup"><span data-stu-id="8e199-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="8e199-114">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="8e199-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="8e199-115">Konwencje. Add</span><span class="sxs-lookup"><span data-stu-id="8e199-115">Conventions.Add</span></span><ul><li><span data-ttu-id="8e199-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="8e199-119">Dodaj szablon trasy i nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="8e199-120">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="8e199-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="8e199-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="8e199-124">Dodaj szablon trasy do stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="8e199-125">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="8e199-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="8e199-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-128">ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</span><span class="sxs-lookup"><span data-stu-id="8e199-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="8e199-129">Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="8e199-130">Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8e199-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="8e199-131">Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="8e199-131">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="8e199-132">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="8e199-132">Route order</span></span>

<span data-ttu-id="8e199-133">Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="8e199-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="8e199-134">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="8e199-134">Order</span></span>            | <span data-ttu-id="8e199-135">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="8e199-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="8e199-136">-1</span><span class="sxs-lookup"><span data-stu-id="8e199-136">-1</span></span>               | <span data-ttu-id="8e199-137">Trasa jest przetwarzana przed przetworzeniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="8e199-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="8e199-138">0</span><span class="sxs-lookup"><span data-stu-id="8e199-138">0</span></span>                | <span data-ttu-id="8e199-139">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="8e199-139">Order isn't specified (default value).</span></span> <span data-ttu-id="8e199-140">Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="8e199-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="8e199-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="8e199-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="8e199-142">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="8e199-143">Przetwarzanie trasy zostało ustanowione według Konwencji:</span><span class="sxs-lookup"><span data-stu-id="8e199-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="8e199-144">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="8e199-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="8e199-145">Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.</span><span class="sxs-lookup"><span data-stu-id="8e199-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="8e199-146">Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="8e199-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="8e199-147">Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="8e199-148">Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="8e199-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="8e199-149">Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania.</span><span class="sxs-lookup"><span data-stu-id="8e199-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="8e199-150">Postaraj się, aby uprościć schemat routingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="8e199-151">Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="8e199-152">Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="8e199-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="8e199-153">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="8e199-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="8e199-154">Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="8e199-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="8e199-155">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="8e199-155">Model conventions</span></span>

<span data-ttu-id="8e199-156">Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="8e199-157">Dodawanie Konwencji modelu trasy do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="8e199-158">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="8e199-159">Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-160">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`.</span><span class="sxs-lookup"><span data-stu-id="8e199-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="8e199-161">Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="8e199-162">Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="8e199-163">Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="8e199-164">Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="8e199-165">Szablon `{aboutTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="8e199-166">Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="8e199-167">Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="8e199-168">Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="8e199-169">Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-170">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-171">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-172">Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e199-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8e199-173">Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="8e199-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="8e199-174">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="8e199-177">Dodawanie Konwencji modelu aplikacji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="8e199-178">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="8e199-179">Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="8e199-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="8e199-180">Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów.</span><span class="sxs-lookup"><span data-stu-id="8e199-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="8e199-181">Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8e199-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="8e199-182">Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="8e199-183">Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader`do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-184">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="8e199-185">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="8e199-187">Dodawanie Konwencji modelu programu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="8e199-188">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-189">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="8e199-190">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-190">Page route action conventions</span></span>

<span data-ttu-id="8e199-191">Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="8e199-192">Konwencja modelu trasy folderu</span><span class="sxs-lookup"><span data-stu-id="8e199-192">Folder route model convention</span></span>

<span data-ttu-id="8e199-193">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="8e199-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="8e199-194">Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="8e199-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="8e199-195">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="8e199-196">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="8e199-197">Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-198">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-199">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-200">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="8e199-203">Konwencja modelu trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-203">Page route model convention</span></span>

<span data-ttu-id="8e199-204">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8e199-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="8e199-205">Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="8e199-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="8e199-206">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="8e199-207">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="8e199-208">Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-209">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-210">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-211">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="8e199-214">Dostosowywanie tras stron przy użyciu transformatora parametrów</span><span class="sxs-lookup"><span data-stu-id="8e199-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="8e199-215">Trasy stron generowane przez ASP.NET Core mogą być dostosowywane przy użyciu transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e199-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="8e199-216">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e199-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="8e199-217">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="8e199-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="8e199-218">Konwencja modelu trasy strony `PageRouteTransformerConvention` stosuje transformator parametrów do segmentów nazw folderów i plików w przypadku automatycznie generowanych tras stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="8e199-219">Na przykład plik Razor Pages w */Pages/SubscriptionManagement/ViewAll.cshtml* będzie miał swoją trasę ponownie zapisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="8e199-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="8e199-220">`PageRouteTransformerConvention` przekształca tylko automatycznie generowane segmenty trasy strony, która pochodzi z folderu Razor Pages i nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="8e199-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="8e199-221">Nie przekształca segmentów tras dodanych z dyrektywą `@page`.</span><span class="sxs-lookup"><span data-stu-id="8e199-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="8e199-222">Konwencja nie przetwarza również tras dodanych przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="8e199-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="8e199-223">`PageRouteTransformerConvention` jest zarejestrowana jako opcja w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8e199-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="8e199-224">Konfigurowanie trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-224">Configure a page route</span></span>

<span data-ttu-id="8e199-225">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="8e199-226">Wygenerowane linki do strony używają określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="8e199-227">`AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="8e199-228">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8e199-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="8e199-229">Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="8e199-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="8e199-230">Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="8e199-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="8e199-231">Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="8e199-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="8e199-232">Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:</span><span class="sxs-lookup"><span data-stu-id="8e199-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="8e199-235">Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact`lub trasy niestandardowej, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="8e199-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="8e199-236">Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="8e199-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="8e199-239">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="8e199-239">Page model action conventions</span></span>

<span data-ttu-id="8e199-240">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="8e199-241">Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="8e199-242">W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="8e199-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="8e199-243">Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="8e199-244">**Konwencja modelu aplikacji folderu**</span><span class="sxs-lookup"><span data-stu-id="8e199-244">**Folder app model convention**</span></span>

<span data-ttu-id="8e199-245">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="8e199-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="8e199-246">Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader`do stron w folderze *OtherPages* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="8e199-247">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="8e199-249">**Konwencja modelu aplikacji strony**</span><span class="sxs-lookup"><span data-stu-id="8e199-249">**Page app model convention**</span></span>

<span data-ttu-id="8e199-250">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8e199-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="8e199-251">Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader`do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="8e199-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="8e199-252">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="8e199-254">**Konfigurowanie filtru**</span><span class="sxs-lookup"><span data-stu-id="8e199-254">**Configure a filter**</span></span>

<span data-ttu-id="8e199-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować.</span><span class="sxs-lookup"><span data-stu-id="8e199-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="8e199-256">Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:</span><span class="sxs-lookup"><span data-stu-id="8e199-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="8e199-257">Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* .</span><span class="sxs-lookup"><span data-stu-id="8e199-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="8e199-258">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8e199-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="8e199-259">W przeciwnym razie zostanie zastosowana `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="8e199-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="8e199-260">`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="8e199-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="8e199-261">Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="8e199-262">Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="8e199-264">**Konfigurowanie fabryki filtrów**</span><span class="sxs-lookup"><span data-stu-id="8e199-264">**Configure a filter factory**</span></span>

<span data-ttu-id="8e199-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="8e199-266">Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="8e199-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-267">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="8e199-268">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="8e199-270">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="8e199-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="8e199-271">[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="8e199-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="8e199-272">Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="8e199-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="8e199-273">Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .</span><span class="sxs-lookup"><span data-stu-id="8e199-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="8e199-274">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="8e199-275">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="8e199-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e199-276">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8e199-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="8e199-277">Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-277">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="8e199-278">Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-278">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="8e199-279">Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-279">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="8e199-280">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="8e199-280">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="8e199-281">Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e199-281">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="8e199-282">Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="8e199-282">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="8e199-283">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e199-283">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="8e199-284">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="8e199-284">Scenario</span></span> | <span data-ttu-id="8e199-285">Przykład ilustruje...</span><span class="sxs-lookup"><span data-stu-id="8e199-285">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="8e199-286">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="8e199-286">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="8e199-287">Konwencje. Add</span><span class="sxs-lookup"><span data-stu-id="8e199-287">Conventions.Add</span></span><ul><li><span data-ttu-id="8e199-288">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-288">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-289">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-289">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-290">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-290">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="8e199-291">Dodaj szablon trasy i nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-291">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="8e199-292">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-292">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="8e199-293">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-293">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-294">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-294">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-295">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="8e199-295">AddPageRoute</span></span></li></ul> | <span data-ttu-id="8e199-296">Dodaj szablon trasy do stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-296">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="8e199-297">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="8e199-297">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="8e199-298">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-298">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-299">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-299">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-300">ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</span><span class="sxs-lookup"><span data-stu-id="8e199-300">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="8e199-301">Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-301">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="8e199-302">Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8e199-302">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="8e199-303">Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="8e199-303">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="8e199-304">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="8e199-304">Route order</span></span>

<span data-ttu-id="8e199-305">Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="8e199-305">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="8e199-306">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="8e199-306">Order</span></span>            | <span data-ttu-id="8e199-307">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="8e199-307">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="8e199-308">-1</span><span class="sxs-lookup"><span data-stu-id="8e199-308">-1</span></span>               | <span data-ttu-id="8e199-309">Trasa jest przetwarzana przed przetworzeniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="8e199-309">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="8e199-310">0</span><span class="sxs-lookup"><span data-stu-id="8e199-310">0</span></span>                | <span data-ttu-id="8e199-311">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="8e199-311">Order isn't specified (default value).</span></span> <span data-ttu-id="8e199-312">Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="8e199-312">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="8e199-313">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="8e199-313">1, 2, &hellip; n</span></span> | <span data-ttu-id="8e199-314">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-314">Specifies the route processing order.</span></span> |

<span data-ttu-id="8e199-315">Przetwarzanie trasy zostało ustanowione według Konwencji:</span><span class="sxs-lookup"><span data-stu-id="8e199-315">Route processing is established by convention:</span></span>

* <span data-ttu-id="8e199-316">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="8e199-316">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="8e199-317">Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.</span><span class="sxs-lookup"><span data-stu-id="8e199-317">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="8e199-318">Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="8e199-318">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="8e199-319">Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-319">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="8e199-320">Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="8e199-320">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="8e199-321">Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania.</span><span class="sxs-lookup"><span data-stu-id="8e199-321">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="8e199-322">Postaraj się, aby uprościć schemat routingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-322">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="8e199-323">Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-323">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="8e199-324">Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="8e199-324">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="8e199-325">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="8e199-325">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="8e199-326">Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="8e199-326">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="8e199-327">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="8e199-327">Model conventions</span></span>

<span data-ttu-id="8e199-328">Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-328">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="8e199-329">Dodawanie Konwencji modelu trasy do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-329">Add a route model convention to all pages</span></span>

<span data-ttu-id="8e199-330">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-330">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="8e199-331">Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-331">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-332">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`.</span><span class="sxs-lookup"><span data-stu-id="8e199-332">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="8e199-333">Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-333">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="8e199-334">Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-334">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="8e199-335">Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-335">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="8e199-336">Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-336">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="8e199-337">Szablon `{aboutTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-337">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="8e199-338">Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-338">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="8e199-339">Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-339">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="8e199-340">Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-340">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="8e199-341">Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-341">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-342">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-342">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-343">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-343">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-344">Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e199-344">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8e199-345">Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="8e199-345">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="8e199-346">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-346">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="8e199-349">Dodawanie Konwencji modelu aplikacji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-349">Add an app model convention to all pages</span></span>

<span data-ttu-id="8e199-350">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-350">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="8e199-351">Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="8e199-351">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="8e199-352">Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów.</span><span class="sxs-lookup"><span data-stu-id="8e199-352">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="8e199-353">Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8e199-353">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="8e199-354">Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-354">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="8e199-355">Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader`do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-355">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-356">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-356">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="8e199-357">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-357">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="8e199-359">Dodawanie Konwencji modelu programu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-359">Add a handler model convention to all pages</span></span>

<span data-ttu-id="8e199-360">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-360">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-361">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-361">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="8e199-362">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-362">Page route action conventions</span></span>

<span data-ttu-id="8e199-363">Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-363">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="8e199-364">Konwencja modelu trasy folderu</span><span class="sxs-lookup"><span data-stu-id="8e199-364">Folder route model convention</span></span>

<span data-ttu-id="8e199-365">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="8e199-365">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="8e199-366">Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="8e199-366">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="8e199-367">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-367">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="8e199-368">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-368">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="8e199-369">Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-369">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-370">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-370">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-371">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-371">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-372">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-372">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="8e199-375">Konwencja modelu trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-375">Page route model convention</span></span>

<span data-ttu-id="8e199-376">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8e199-376">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="8e199-377">Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="8e199-377">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="8e199-378">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-378">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="8e199-379">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-379">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="8e199-380">Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-380">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-381">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-381">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-382">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-382">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-383">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-383">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="8e199-386">Dostosowywanie tras stron przy użyciu transformatora parametrów</span><span class="sxs-lookup"><span data-stu-id="8e199-386">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="8e199-387">Trasy stron generowane przez ASP.NET Core mogą być dostosowywane przy użyciu transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e199-387">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="8e199-388">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e199-388">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="8e199-389">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="8e199-389">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="8e199-390">Konwencja modelu trasy strony `PageRouteTransformerConvention` stosuje transformator parametrów do segmentów nazw folderów i plików w przypadku automatycznie generowanych tras stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-390">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="8e199-391">Na przykład plik Razor Pages w */Pages/SubscriptionManagement/ViewAll.cshtml* będzie miał swoją trasę ponownie zapisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="8e199-391">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="8e199-392">`PageRouteTransformerConvention` przekształca tylko automatycznie generowane segmenty trasy strony, która pochodzi z folderu Razor Pages i nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="8e199-392">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="8e199-393">Nie przekształca segmentów tras dodanych z dyrektywą `@page`.</span><span class="sxs-lookup"><span data-stu-id="8e199-393">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="8e199-394">Konwencja nie przetwarza również tras dodanych przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="8e199-394">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="8e199-395">`PageRouteTransformerConvention` jest zarejestrowana jako opcja w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8e199-395">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="8e199-396">Konfigurowanie trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-396">Configure a page route</span></span>

<span data-ttu-id="8e199-397">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-397">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="8e199-398">Wygenerowane linki do strony używają określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-398">Generated links to the page use your specified route.</span></span> <span data-ttu-id="8e199-399">`AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-399">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="8e199-400">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8e199-400">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="8e199-401">Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="8e199-401">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="8e199-402">Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="8e199-402">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="8e199-403">Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="8e199-403">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="8e199-404">Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:</span><span class="sxs-lookup"><span data-stu-id="8e199-404">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="8e199-407">Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact`lub trasy niestandardowej, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="8e199-407">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="8e199-408">Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="8e199-408">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="8e199-411">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="8e199-411">Page model action conventions</span></span>

<span data-ttu-id="8e199-412">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-412">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="8e199-413">Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-413">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="8e199-414">W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="8e199-414">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="8e199-415">Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-415">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="8e199-416">**Konwencja modelu aplikacji folderu**</span><span class="sxs-lookup"><span data-stu-id="8e199-416">**Folder app model convention**</span></span>

<span data-ttu-id="8e199-417">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="8e199-417">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="8e199-418">Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader`do stron w folderze *OtherPages* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-418">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="8e199-419">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-419">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="8e199-421">**Konwencja modelu aplikacji strony**</span><span class="sxs-lookup"><span data-stu-id="8e199-421">**Page app model convention**</span></span>

<span data-ttu-id="8e199-422">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8e199-422">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="8e199-423">Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader`do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="8e199-423">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="8e199-424">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-424">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="8e199-426">**Konfigurowanie filtru**</span><span class="sxs-lookup"><span data-stu-id="8e199-426">**Configure a filter**</span></span>

<span data-ttu-id="8e199-427"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować.</span><span class="sxs-lookup"><span data-stu-id="8e199-427"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="8e199-428">Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:</span><span class="sxs-lookup"><span data-stu-id="8e199-428">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="8e199-429">Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* .</span><span class="sxs-lookup"><span data-stu-id="8e199-429">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="8e199-430">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8e199-430">If the condition passes, a header is added.</span></span> <span data-ttu-id="8e199-431">W przeciwnym razie zostanie zastosowana `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="8e199-431">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="8e199-432">`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="8e199-432">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="8e199-433">Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-433">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="8e199-434">Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-434">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="8e199-436">**Konfigurowanie fabryki filtrów**</span><span class="sxs-lookup"><span data-stu-id="8e199-436">**Configure a filter factory**</span></span>

<span data-ttu-id="8e199-437"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-437"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="8e199-438">Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-438">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="8e199-439">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-439">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="8e199-440">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-440">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="8e199-442">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="8e199-442">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="8e199-443">[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="8e199-443">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="8e199-444">Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="8e199-444">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="8e199-445">Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .</span><span class="sxs-lookup"><span data-stu-id="8e199-445">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="8e199-446">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-446">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="8e199-447">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="8e199-447">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e199-448">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8e199-448">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8e199-449">Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-449">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="8e199-450">Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-450">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="8e199-451">Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-451">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="8e199-452">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="8e199-452">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="8e199-453">Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="8e199-453">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="8e199-454">Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="8e199-454">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="8e199-455">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e199-455">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="8e199-456">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="8e199-456">Scenario</span></span> | <span data-ttu-id="8e199-457">Przykład ilustruje...</span><span class="sxs-lookup"><span data-stu-id="8e199-457">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="8e199-458">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="8e199-458">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="8e199-459">Konwencje. Add</span><span class="sxs-lookup"><span data-stu-id="8e199-459">Conventions.Add</span></span><ul><li><span data-ttu-id="8e199-460">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-460">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-461">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-461">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-462">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-462">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="8e199-463">Dodaj szablon trasy i nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-463">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="8e199-464">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-464">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="8e199-465">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-465">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-466">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-466">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="8e199-467">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="8e199-467">AddPageRoute</span></span></li></ul> | <span data-ttu-id="8e199-468">Dodaj szablon trasy do stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-468">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="8e199-469">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="8e199-469">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="8e199-470">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-470">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-471">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="8e199-471">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="8e199-472">ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</span><span class="sxs-lookup"><span data-stu-id="8e199-472">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="8e199-473">Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-473">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="8e199-474">Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8e199-474">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="8e199-475">Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="8e199-475">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="8e199-476">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="8e199-476">Route order</span></span>

<span data-ttu-id="8e199-477">Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="8e199-477">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="8e199-478">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="8e199-478">Order</span></span>            | <span data-ttu-id="8e199-479">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="8e199-479">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="8e199-480">-1</span><span class="sxs-lookup"><span data-stu-id="8e199-480">-1</span></span>               | <span data-ttu-id="8e199-481">Trasa jest przetwarzana przed przetworzeniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="8e199-481">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="8e199-482">0</span><span class="sxs-lookup"><span data-stu-id="8e199-482">0</span></span>                | <span data-ttu-id="8e199-483">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="8e199-483">Order isn't specified (default value).</span></span> <span data-ttu-id="8e199-484">Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="8e199-484">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="8e199-485">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="8e199-485">1, 2, &hellip; n</span></span> | <span data-ttu-id="8e199-486">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-486">Specifies the route processing order.</span></span> |

<span data-ttu-id="8e199-487">Przetwarzanie trasy zostało ustanowione według Konwencji:</span><span class="sxs-lookup"><span data-stu-id="8e199-487">Route processing is established by convention:</span></span>

* <span data-ttu-id="8e199-488">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="8e199-488">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="8e199-489">Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.</span><span class="sxs-lookup"><span data-stu-id="8e199-489">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="8e199-490">Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="8e199-490">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="8e199-491">Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-491">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="8e199-492">Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="8e199-492">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="8e199-493">Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania.</span><span class="sxs-lookup"><span data-stu-id="8e199-493">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="8e199-494">Postaraj się, aby uprościć schemat routingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-494">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="8e199-495">Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-495">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="8e199-496">Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="8e199-496">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="8e199-497">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="8e199-497">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="8e199-498">Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="8e199-498">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="8e199-499">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="8e199-499">Model conventions</span></span>

<span data-ttu-id="8e199-500">Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-500">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="8e199-501">Dodawanie Konwencji modelu trasy do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-501">Add a route model convention to all pages</span></span>

<span data-ttu-id="8e199-502">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-502">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="8e199-503">Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-503">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-504">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`.</span><span class="sxs-lookup"><span data-stu-id="8e199-504">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="8e199-505">Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-505">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="8e199-506">Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-506">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="8e199-507">Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-507">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="8e199-508">Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-508">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="8e199-509">Szablon `{aboutTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-509">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="8e199-510">Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-510">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="8e199-511">Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-511">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="8e199-512">Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-512">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="8e199-513">Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-513">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-514">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-514">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-515">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-515">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-516">Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8e199-516">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8e199-517">Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="8e199-517">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="8e199-518">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-518">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="8e199-521">Dodawanie Konwencji modelu aplikacji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-521">Add an app model convention to all pages</span></span>

<span data-ttu-id="8e199-522">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e199-522">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="8e199-523">Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="8e199-523">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="8e199-524">Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów.</span><span class="sxs-lookup"><span data-stu-id="8e199-524">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="8e199-525">Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8e199-525">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="8e199-526">Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="8e199-526">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="8e199-527">Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader`do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-527">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-528">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-528">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="8e199-529">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-529">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="8e199-531">Dodawanie Konwencji modelu programu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="8e199-531">Add a handler model convention to all pages</span></span>

<span data-ttu-id="8e199-532">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-532">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="8e199-533">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-533">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="8e199-534">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-534">Page route action conventions</span></span>

<span data-ttu-id="8e199-535">Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-535">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="8e199-536">Konwencja modelu trasy folderu</span><span class="sxs-lookup"><span data-stu-id="8e199-536">Folder route model convention</span></span>

<span data-ttu-id="8e199-537">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="8e199-537">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="8e199-538">Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="8e199-538">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="8e199-539">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-539">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="8e199-540">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-540">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="8e199-541">Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-541">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-542">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-542">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-543">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-543">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-544">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-544">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="8e199-547">Konwencja modelu trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-547">Page route model convention</span></span>

<span data-ttu-id="8e199-548">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8e199-548">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="8e199-549">Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="8e199-549">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="8e199-550">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-550">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="8e199-551">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-551">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="8e199-552">Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="8e199-552">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="8e199-553">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="8e199-553">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="8e199-554">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-554">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="8e199-555">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="8e199-555">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="8e199-558">Konfigurowanie trasy strony</span><span class="sxs-lookup"><span data-stu-id="8e199-558">Configure a page route</span></span>

<span data-ttu-id="8e199-559">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-559">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="8e199-560">Wygenerowane linki do strony używają określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-560">Generated links to the page use your specified route.</span></span> <span data-ttu-id="8e199-561">`AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.</span><span class="sxs-lookup"><span data-stu-id="8e199-561">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="8e199-562">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8e199-562">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="8e199-563">Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="8e199-563">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="8e199-564">Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="8e199-564">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="8e199-565">Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="8e199-565">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="8e199-566">Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:</span><span class="sxs-lookup"><span data-stu-id="8e199-566">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="8e199-569">Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact`lub trasy niestandardowej, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="8e199-569">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="8e199-570">Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="8e199-570">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="8e199-573">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="8e199-573">Page model action conventions</span></span>

<span data-ttu-id="8e199-574">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-574">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="8e199-575">Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.</span><span class="sxs-lookup"><span data-stu-id="8e199-575">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="8e199-576">W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="8e199-576">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="8e199-577">Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="8e199-577">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="8e199-578">**Konwencja modelu aplikacji folderu**</span><span class="sxs-lookup"><span data-stu-id="8e199-578">**Folder app model convention**</span></span>

<span data-ttu-id="8e199-579">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="8e199-579">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="8e199-580">Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader`do stron w folderze *OtherPages* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-580">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="8e199-581">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-581">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="8e199-583">**Konwencja modelu aplikacji strony**</span><span class="sxs-lookup"><span data-stu-id="8e199-583">**Page app model convention**</span></span>

<span data-ttu-id="8e199-584">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8e199-584">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="8e199-585">Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader`do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="8e199-585">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="8e199-586">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-586">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="8e199-588">**Konfigurowanie filtru**</span><span class="sxs-lookup"><span data-stu-id="8e199-588">**Configure a filter**</span></span>

<span data-ttu-id="8e199-589"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować.</span><span class="sxs-lookup"><span data-stu-id="8e199-589"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="8e199-590">Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:</span><span class="sxs-lookup"><span data-stu-id="8e199-590">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="8e199-591">Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* .</span><span class="sxs-lookup"><span data-stu-id="8e199-591">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="8e199-592">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="8e199-592">If the condition passes, a header is added.</span></span> <span data-ttu-id="8e199-593">W przeciwnym razie zostanie zastosowana `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="8e199-593">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="8e199-594">`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="8e199-594">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="8e199-595">Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="8e199-595">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="8e199-596">Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-596">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="8e199-598">**Konfigurowanie fabryki filtrów**</span><span class="sxs-lookup"><span data-stu-id="8e199-598">**Configure a filter factory**</span></span>

<span data-ttu-id="8e199-599"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-599"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="8e199-600">Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8e199-600">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="8e199-601">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e199-601">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="8e199-602">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="8e199-602">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="8e199-604">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="8e199-604">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="8e199-605">[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="8e199-605">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="8e199-606">Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="8e199-606">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="8e199-607">Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .</span><span class="sxs-lookup"><span data-stu-id="8e199-607">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="8e199-608">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8e199-608">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="8e199-609">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="8e199-609">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e199-610">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8e199-610">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
