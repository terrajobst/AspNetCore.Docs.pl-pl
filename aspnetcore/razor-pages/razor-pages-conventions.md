---
title: Razor konwencje tras i aplikacji stron w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak konwencje tras i aplikacji dostawcy modelu pomóc routingu strony kontroli, odnajdywania i przetwarzania.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 59c8af648b50deb51f3762c14348d08acd48886e
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724445"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="7bef4-103">Razor konwencje tras i aplikacji stron w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7bef4-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="7bef4-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7bef4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7bef4-105">Dowiedz się, jak użyć strony [tras i aplikacji modelu Konwencji dostawcy](xref:mvc/controllers/application-model#conventions) do kontrolowania strony routingu, odnajdywania i przetwarzania w aplikacjach stron Razor.</span><span class="sxs-lookup"><span data-stu-id="7bef4-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="7bef4-106">Gdy trzeba skonfigurować trasy niestandardowej strony do poszczególnych stron, należy skonfigurować routing do stron z [Konwencji AddPageRoute](#configure-a-page-route) opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="7bef4-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="7bef4-107">Aby określić trasę strony, Dodaj trasę segmentów lub dodać parametry do trasy, przejść do strony `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="7bef4-108">Aby uzyskać więcej informacji, zobacz [trasy niestandardowe](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="7bef4-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="7bef4-109">Brak słów zastrzeżonych, których nie można użyć jako trasy segmentów lub nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="7bef4-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="7bef4-110">Aby uzyskać więcej informacji, zobacz [routingu: Zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="7bef4-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="7bef4-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7bef4-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="7bef4-112">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="7bef4-112">Scenario</span></span> | <span data-ttu-id="7bef4-113">W przykładzie pokazano...</span><span class="sxs-lookup"><span data-stu-id="7bef4-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="7bef4-114">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="7bef4-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="7bef4-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="7bef4-115">Conventions.Add</span></span><ul><li><span data-ttu-id="7bef4-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="7bef4-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="7bef4-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="7bef4-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="7bef4-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="7bef4-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="7bef4-119">Dodaj szablon trasy i nagłówek do strony aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bef4-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="7bef4-120">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="7bef4-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="7bef4-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="7bef4-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="7bef4-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="7bef4-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="7bef4-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="7bef4-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="7bef4-124">Dodaj szablon trasy do stron w folderze i jednej strony.</span><span class="sxs-lookup"><span data-stu-id="7bef4-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="7bef4-125">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="7bef4-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="7bef4-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="7bef4-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="7bef4-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="7bef4-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="7bef4-128">ConfigureFilter (Filtr klasy, wyrażenie lambda lub filtra)</span><span class="sxs-lookup"><span data-stu-id="7bef4-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="7bef4-129">Dodanie nagłówka do stron w folderze, dodanie nagłówka do jednej strony, a następnie skonfiguruj [filtra](xref:mvc/controllers/filters#ifilterfactory) na dodanie nagłówka do strony aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bef4-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="7bef4-130">Konwencje stron razor są dodawane i skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> metodę rozszerzenia, aby <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usługi w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="7bef4-131">W poniższych przykładach Konwencji są wyjaśnione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="7bef4-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="7bef4-132">Kolejność trasy</span><span class="sxs-lookup"><span data-stu-id="7bef4-132">Route order</span></span>

<span data-ttu-id="7bef4-133">Określanie tras <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="7bef4-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="7bef4-134">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="7bef4-134">Order</span></span>            | <span data-ttu-id="7bef4-135">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="7bef4-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="7bef4-136">-1</span><span class="sxs-lookup"><span data-stu-id="7bef4-136">-1</span></span>               | <span data-ttu-id="7bef4-137">Trasy są przetwarzane przed inne trasy są przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="7bef4-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="7bef4-138">0</span><span class="sxs-lookup"><span data-stu-id="7bef4-138">0</span></span>                | <span data-ttu-id="7bef4-139">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="7bef4-139">Order isn't specified (default value).</span></span> <span data-ttu-id="7bef4-140">Przypisanie nie `Order` (`Order = null`) domyślne trasy `Order` na 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="7bef4-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="7bef4-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="7bef4-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="7bef4-142">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="7bef4-143">Zgodnie z Konwencją tworzy się przetwarzanie trasy:</span><span class="sxs-lookup"><span data-stu-id="7bef4-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="7bef4-144">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="7bef4-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="7bef4-145">Kiedy trasy mają taką samą `Order`, maksymalnie określoną trasę jest dopasowywany najpierw następuje mniej określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="7bef4-146">Gdy trasy z takimi samymi `Order` i taką samą liczbę parametrów dopasowania w adresie URL żądania, trasy są przetwarzane w kolejności, które są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="7bef4-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="7bef4-147">Jeśli to możliwe Unikaj w zależności od ustalonych trasy kolejność przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="7bef4-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="7bef4-148">Ogólnie rzecz biorąc routingu wybiera poprawny trasę z pasującymi adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7bef4-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="7bef4-149">Jeśli musisz ustawić trasy `Order` właściwości w celu kierowania żądań poprawnie, schemat routingu aplikacji jest prawdopodobnie mylące dla klientów powolnymi i słabymi do zachowania.</span><span class="sxs-lookup"><span data-stu-id="7bef4-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="7bef4-150">Wyszukiwanie uprościć routingu schemat aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bef4-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="7bef4-151">Przykładowa aplikacja wymaga jawnego trasę w protokole przetwarzania zamówienia, aby zademonstrować kilku scenariuszy routingu za pomocą pojedynczej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bef4-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="7bef4-152">Jednakże, należy podjąć w celu uniknięcia rozwiązaniem jest ustawienie trasy `Order` w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="7bef4-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="7bef4-153">Strony razor routingu i MVC kontroler routingu udziału wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="7bef4-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="7bef4-154">Informacje o kolejność trasy w tematach MVC znajduje się w temacie [Routing do akcji kontrolera: Kolejność trasy w atrybucie](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="7bef4-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="7bef4-155">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="7bef4-155">Model conventions</span></span>

<span data-ttu-id="7bef4-156">Dodawanie delegatów dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> dodać [modelu Konwencji](xref:mvc/controllers/application-model#conventions) które są stosowane do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="7bef4-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="7bef4-157">Dodawanie modelu Konwencji tras do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="7bef4-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="7bef4-158">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> wystąpień, które są stosowane podczas trasy strony modelu konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="7bef4-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="7bef4-159">Przykładowa aplikacja dodaje `{globalTemplate?}` szablon trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7bef4-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="7bef4-160"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `1`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="7bef4-161">Dzięki temu Następująca trasa pasujące do zachowania w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7bef4-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="7bef4-162">Szablon trasy dla `TheContactPage/{text?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="7bef4-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="7bef4-163">Trasa strony kontaktu ma domyślną kolejność `null` (`Order = 0`), tak, aby odpowiadała przed `{globalTemplate?}` szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="7bef4-164">`{aboutTemplate?}` Szablon trasy zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="7bef4-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="7bef4-165">`{aboutTemplate?}` Znajduje się szablon `Order` z `2`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="7bef4-166">Po żądaniu strony informacje o `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bef4-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="7bef4-167">`{otherPagesTemplate?}` Szablon trasy zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="7bef4-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="7bef4-168">`{otherPagesTemplate?}` Znajduje się szablon `Order` z `2`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="7bef4-169">Po dowolnej stronie *stron/OtherPages* folderu jest żądanego przy użyciu parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bef4-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="7bef4-170">Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="7bef4-171">Polegaj na routingu, aby wybrać poprawny trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="7bef4-172">Opcje strony razor, takich jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane, gdy MVC zostanie dodany do kolekcji usługi w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7bef4-173">Aby uzyskać przykład, zobacz [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="7bef4-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="7bef4-174">Żądanie próbki o stronę w `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Za pomocą tras segment GlobalRouteValue żądaniu strony informacje.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="7bef4-177">Dodawanie aplikacji modelu Konwencji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="7bef4-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="7bef4-178">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> wystąpień, które są stosowane podczas stronie konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bef4-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="7bef4-179">Aby zademonstrować to i inne konwencje w dalszej części tematu, zawiera przykładową aplikację `AddHeaderAttribute` klasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="7bef4-180">Konstruktor klasy akceptuje `name` ciągu i `values` tablicy ciągów.</span><span class="sxs-lookup"><span data-stu-id="7bef4-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="7bef4-181">Te wartości są używane w jego `OnResultExecuting` metodę, aby ustawić nagłówek odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7bef4-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="7bef4-182">Pełne klasy jest wyświetlany w [strony modelu działania Konwencji](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="7bef4-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="7bef4-183">Ta aplikacja używa przykładowych `AddHeaderAttribute` klasy, aby dodać nagłówek `GlobalHeader`, do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7bef4-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="7bef4-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bef4-184">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="7bef4-185">Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony informacje pokazują, że dodano GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="7bef4-187">Dodaj Konwencji modelu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="7bef4-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="7bef4-188">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> wystąpień, które są stosowane podczas stronie konstruowania modelu obsługi.</span><span class="sxs-lookup"><span data-stu-id="7bef4-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="7bef4-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bef4-189">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="7bef4-190">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="7bef4-190">Page route action conventions</span></span>

<span data-ttu-id="7bef4-191">Domyślnego dostawcę modelu trasy, która pochodzi od klasy <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które są przeznaczone do zapewnienia punkty rozszerzeń do konfigurowania tras strony.</span><span class="sxs-lookup"><span data-stu-id="7bef4-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="7bef4-192">Folder trasy modelu Konwencji</span><span class="sxs-lookup"><span data-stu-id="7bef4-192">Folder route model convention</span></span>

<span data-ttu-id="7bef4-193">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="7bef4-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="7bef4-194">Ta aplikacja używa przykładowych <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> dodać `{otherPagesTemplate?}` szablon trasy do stron w *OtherPages* folderu:</span><span class="sxs-lookup"><span data-stu-id="7bef4-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="7bef4-195"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `2`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="7bef4-196">Gwarantuje to, że szablon `{globalTemplate?}` (wcześniej w temacie, aby `1`) otrzymuje priorytet dla pierwszego dane trasy wartość pozycji, gdy została podana wartość jedną trasę.</span><span class="sxs-lookup"><span data-stu-id="7bef4-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="7bef4-197">Jeśli na stronie w programie *stron/OtherPages* zażądano folderu z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bef4-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="7bef4-198">Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="7bef4-199">Polegaj na routingu, aby wybrać poprawny trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="7bef4-200">Żądanie strony Strona 1 przykładu w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Strona 1 w folderze OtherPages żądanie z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="7bef4-203">Strona trasy modelu Konwencji</span><span class="sxs-lookup"><span data-stu-id="7bef4-203">Page route model convention</span></span>

<span data-ttu-id="7bef4-204">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="7bef4-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="7bef4-205">Ta aplikacja używa przykładowych `AddPageRouteModelConvention` dodać `{aboutTemplate?}` szablon trasy do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="7bef4-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="7bef4-206"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> ustawiono `2`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="7bef4-207">Gwarantuje to, że szablon `{globalTemplate?}` (wcześniej w temacie, aby `1`) otrzymuje priorytet dla pierwszego dane trasy wartość pozycji, gdy została podana wartość jedną trasę.</span><span class="sxs-lookup"><span data-stu-id="7bef4-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="7bef4-208">Jeśli wartością parametru trasy w żądaniu strony informacje `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`) i nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) ze względu na ustawienie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7bef4-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="7bef4-209">Wszędzie tam, gdzie to możliwe, nie należy ustawiać `Order`, które powoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="7bef4-210">Polegaj na routingu, aby wybrać poprawny trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="7bef4-211">Żądanie próbki o stronę w `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Informacje o stronie żądania segmentów trasy GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="7bef4-214">Użyj transformatora parametru, aby dostosować stronę trasy</span><span class="sxs-lookup"><span data-stu-id="7bef4-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="7bef4-215">Używanie transformatora parametru można dostosować trasy strony, wygenerowane przez platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7bef4-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="7bef4-216">Transformer parametr implementuje `IOutboundParameterTransformer` i przekształca wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="7bef4-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="7bef4-217">Na przykład niestandardowy `SlugifyParameterTransformer` zmiany transformatora parametrów `SubscriptionManagement` trasy wartość `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="7bef4-218">`PageRouteTransformerConvention` Strony trasy modelu Konwencją transformatora parametr do segmentów nazwę folderu i pliku tras automatycznie wygenerowanych stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7bef4-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="7bef4-219">Na przykład pliku stron Razor */Pages/SubscriptionManagement/ViewAll.cshtml* miałby trasę przepisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="7bef4-220">`PageRouteTransformerConvention` tylko przy użyciu automatycznie generowanego segmenty trasy strony ze stron Razor folder i nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="7bef4-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="7bef4-221">Go nie przekształci segmentów trasa dodana z `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="7bef4-222">Konwencja również nie przekształci trasy dodane przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="7bef4-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="7bef4-223">`PageRouteTransformerConvention` Jest zarejestrowany jako opcja w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7bef4-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="7bef4-224">Skonfiguruj trasy strony</span><span class="sxs-lookup"><span data-stu-id="7bef4-224">Configure a page route</span></span>

<span data-ttu-id="7bef4-225">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> do konfigurowania tras do strony w ścieżce określonej strony.</span><span class="sxs-lookup"><span data-stu-id="7bef4-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="7bef4-226">Wygenerowany łącza do strony Użyj określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="7bef4-227">`AddPageRoute` używa `AddPageRouteModelConvention` nawiązać trasy.</span><span class="sxs-lookup"><span data-stu-id="7bef4-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="7bef4-228">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7bef4-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="7bef4-229">Na stronie kontakt osiągalna na `/Contact` za pomocą tej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="7bef4-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="7bef4-230">Przykładowej aplikacji niestandardowe trasy do strony kontaktu umożliwia opcjonalny `text` trasy segmentu (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="7bef4-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="7bef4-231">Strona zawiera także ten segment opcjonalny w jego `@page` dyrektywy, w przypadku, gdy użytkownik uzyskuje dostęp do strony od jego `/Contact` trasy:</span><span class="sxs-lookup"><span data-stu-id="7bef4-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="7bef4-232">Należy zauważyć, że adres URL jest generowane dla **skontaktuj się z pomocą** łącze na renderowanej stronie odzwierciedla zaktualizowany trasy:</span><span class="sxs-lookup"><span data-stu-id="7bef4-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Przykładowy link skontaktuj się z aplikacji na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Inspekcja łącze kontaktu w postaci HTML wskazuje, że odwołanie hipertekstowe jest ustawiona na "/ TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="7bef4-235">Odwiedź stronę skontaktuj się z pomocą pod jedną trasę zwykłych `/Contact`, lub tras niestandardowych `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="7bef4-236">Jeśli podasz dodatkowego `text` segmentu trasy, na stronie znajdują się segment kodowany w formacie HTML, aby podać:</span><span class="sxs-lookup"><span data-stu-id="7bef4-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="7bef4-239">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="7bef4-239">Page model action conventions</span></span>

<span data-ttu-id="7bef4-240">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są przeznaczone do Podaj punkty rozszerzeń do skonfigurowania modeli strony.</span><span class="sxs-lookup"><span data-stu-id="7bef4-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="7bef4-241">Konwencje te są przydatne podczas tworzenia i modyfikowania strony odnajdowania i scenariusze przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="7bef4-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="7bef4-242">W przykładach w tej sekcji Przykładowa aplikacja używa `AddHeaderAttribute` klasy, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która odnosi się nagłówek odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="7bef4-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="7bef4-243">Za pomocą Konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i jednej strony.</span><span class="sxs-lookup"><span data-stu-id="7bef4-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="7bef4-244">**Folder aplikacji modelu Konwencji**</span><span class="sxs-lookup"><span data-stu-id="7bef4-244">**Folder app model convention**</span></span>

<span data-ttu-id="7bef4-245">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> wystąpienia dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="7bef4-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="7bef4-246">W przykładzie pokazano użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka `OtherPagesHeader`, do stron wewnątrz *OtherPages* folder aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7bef4-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="7bef4-247">Żądanie strony Strona 1 przykładu w `localhost:5000/OtherPages/Page1` i sprawdzić te nagłówki, aby wyświetlić wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi dla strony strona OtherPages/1 pokazują, że dodano OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="7bef4-249">**Strona aplikacji modelu Konwencji**</span><span class="sxs-lookup"><span data-stu-id="7bef4-249">**Page app model convention**</span></span>

<span data-ttu-id="7bef4-250">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> tworzyć i dodawać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="7bef4-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="7bef4-251">W przykładzie pokazano użycie `AddPageApplicationModelConvention` przez dodanie nagłówka `AboutHeader`, do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="7bef4-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="7bef4-252">Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony informacje pokazują, że dodano AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="7bef4-254">**Skonfigurować filtr**</span><span class="sxs-lookup"><span data-stu-id="7bef4-254">**Configure a filter**</span></span>

<span data-ttu-id="7bef4-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Konfiguruje określony filtr do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="7bef4-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="7bef4-256">Można zaimplementować Filtr klasy, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, która jest zaimplementowana w tle, ponieważ fabryka, która zwraca filtru:</span><span class="sxs-lookup"><span data-stu-id="7bef4-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="7bef4-257">Model aplikacji strony służy do sprawdzania ścieżkę względną do segmentów, które mogą prowadzić do strony Strona2 *OtherPages* folderu.</span><span class="sxs-lookup"><span data-stu-id="7bef4-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="7bef4-258">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="7bef4-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="7bef4-259">Jeśli nie, `EmptyFilter` jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="7bef4-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="7bef4-260">`EmptyFilter` jest [filtr akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="7bef4-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="7bef4-261">Ponieważ filtry akcji są ignorowane przez stron Razor `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="7bef4-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="7bef4-262">Żądanie strony Strona2 przykładu w `localhost:5000/OtherPages/Page2` i sprawdzić te nagłówki, aby wyświetlić wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi Strona2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="7bef4-264">**Konfigurowanie fabryki filtru**</span><span class="sxs-lookup"><span data-stu-id="7bef4-264">**Configure a filter factory**</span></span>

<span data-ttu-id="7bef4-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Umożliwia skonfigurowanie określonej fabryki, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich stron Razor.</span><span class="sxs-lookup"><span data-stu-id="7bef4-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="7bef4-266">Przykładowa aplikacja zawiera przykład za pomocą [filtra](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka `FilterFactoryHeader`, za pomocą dwóch wartości do stron aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7bef4-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="7bef4-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bef4-267">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="7bef4-268">Żądanie próbki o stronę w `localhost:5000/About` i sprawdzić te nagłówki, aby wyświetlić wynik:</span><span class="sxs-lookup"><span data-stu-id="7bef4-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony informacje pokazują, czy dwa nagłówki FilterFactoryHeader został dodany.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="7bef4-270">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="7bef4-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="7bef4-271">MVC [filtry akcji](xref:mvc/controllers/filters#action-filters) są ignorowane przez stron Razor, ponieważ stron Razor korzystać z metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="7bef4-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="7bef4-272">Inne typy filtrów platformy MVC są dostępne do użycia: [Autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasobów](xref:mvc/controllers/filters#resource-filters), i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="7bef4-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="7bef4-273">Aby uzyskać więcej informacji, zobacz [filtry](xref:mvc/controllers/filters) tematu.</span><span class="sxs-lookup"><span data-stu-id="7bef4-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="7bef4-274">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtr, który ma zastosowanie do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="7bef4-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="7bef4-275">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla stron Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="7bef4-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7bef4-276">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7bef4-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
