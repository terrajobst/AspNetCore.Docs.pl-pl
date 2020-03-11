---
title: Razor Pages Konwencji tras i aplikacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób Konwencja i konwencje dostawcy modelu aplikacji ułatwiają kontrolowanie routingu, odnajdywania i przetwarzania stron.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: f45e327051aba54d1cab67148eb540fb1a5cc149
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667861"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="84603-103">Razor Pages Konwencji tras i aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84603-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="84603-104">Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-104">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="84603-105">Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-105">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="84603-106">Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony.</span><span class="sxs-lookup"><span data-stu-id="84603-106">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="84603-107">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="84603-107">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="84603-108">Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="84603-108">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="84603-109">Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="84603-109">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="84603-110">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84603-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="84603-111">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="84603-111">Scenario</span></span> | <span data-ttu-id="84603-112">Przykład ilustruje...</span><span class="sxs-lookup"><span data-stu-id="84603-112">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="84603-113">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="84603-113">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="84603-114">Konwencje. Add</span><span class="sxs-lookup"><span data-stu-id="84603-114">Conventions.Add</span></span><ul><li><span data-ttu-id="84603-115">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-115">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="84603-116">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-116">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-117">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-117">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="84603-118">Dodaj szablon trasy i nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-118">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="84603-119">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-119">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="84603-120">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-120">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="84603-121">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-121">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="84603-122">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="84603-122">AddPageRoute</span></span></li></ul> | <span data-ttu-id="84603-123">Dodaj szablon trasy do stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="84603-123">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="84603-124">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="84603-124">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="84603-125">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-125">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-126">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-126">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-127">ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</span><span class="sxs-lookup"><span data-stu-id="84603-127">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="84603-128">Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-128">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="84603-129">Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="84603-129">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="84603-130">Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="84603-130">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="84603-131">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="84603-131">Route order</span></span>

<span data-ttu-id="84603-132">Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="84603-132">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="84603-133">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="84603-133">Order</span></span>            | <span data-ttu-id="84603-134">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="84603-134">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="84603-135">-1</span><span class="sxs-lookup"><span data-stu-id="84603-135">-1</span></span>               | <span data-ttu-id="84603-136">Trasa jest przetwarzana przed przetworzeniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="84603-136">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="84603-137">0</span><span class="sxs-lookup"><span data-stu-id="84603-137">0</span></span>                | <span data-ttu-id="84603-138">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="84603-138">Order isn't specified (default value).</span></span> <span data-ttu-id="84603-139">Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="84603-139">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="84603-140">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="84603-140">1, 2, &hellip; n</span></span> | <span data-ttu-id="84603-141">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-141">Specifies the route processing order.</span></span> |

<span data-ttu-id="84603-142">Przetwarzanie trasy zostało ustanowione według Konwencji:</span><span class="sxs-lookup"><span data-stu-id="84603-142">Route processing is established by convention:</span></span>

* <span data-ttu-id="84603-143">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="84603-143">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="84603-144">Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.</span><span class="sxs-lookup"><span data-stu-id="84603-144">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="84603-145">Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="84603-145">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="84603-146">Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-146">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="84603-147">Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="84603-147">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="84603-148">Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania.</span><span class="sxs-lookup"><span data-stu-id="84603-148">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="84603-149">Postaraj się, aby uprościć schemat routingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-149">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="84603-150">Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-150">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="84603-151">Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="84603-151">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="84603-152">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="84603-152">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="84603-153">Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="84603-153">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="84603-154">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="84603-154">Model conventions</span></span>

<span data-ttu-id="84603-155">Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-155">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="84603-156">Dodawanie Konwencji modelu trasy do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-156">Add a route model convention to all pages</span></span>

<span data-ttu-id="84603-157">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.</span><span class="sxs-lookup"><span data-stu-id="84603-157">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="84603-158">Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-158">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-159">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`.</span><span class="sxs-lookup"><span data-stu-id="84603-159">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="84603-160">Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-160">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="84603-161">Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-161">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="84603-162">Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-162">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="84603-163">Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-163">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="84603-164">Szablon `{aboutTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-164">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="84603-165">Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-165">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="84603-166">Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-166">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="84603-167">Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-167">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="84603-168">Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-168">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-169">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-169">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-170">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-170">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-171">Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="84603-171">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="84603-172">Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="84603-172">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="84603-173">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-173">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="84603-176">Dodawanie Konwencji modelu aplikacji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-176">Add an app model convention to all pages</span></span>

<span data-ttu-id="84603-177">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-177">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="84603-178">Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="84603-178">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="84603-179">Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów.</span><span class="sxs-lookup"><span data-stu-id="84603-179">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="84603-180">Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="84603-180">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="84603-181">Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-181">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="84603-182">Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader`do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-182">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-183">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-183">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="84603-184">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-184">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="84603-186">Dodawanie Konwencji modelu programu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-186">Add a handler model convention to all pages</span></span>

<span data-ttu-id="84603-187">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.</span><span class="sxs-lookup"><span data-stu-id="84603-187">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-188">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-188">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="84603-189">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-189">Page route action conventions</span></span>

<span data-ttu-id="84603-190">Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.</span><span class="sxs-lookup"><span data-stu-id="84603-190">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="84603-191">Konwencja modelu trasy folderu</span><span class="sxs-lookup"><span data-stu-id="84603-191">Folder route model convention</span></span>

<span data-ttu-id="84603-192">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="84603-192">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="84603-193">Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="84603-193">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="84603-194">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-194">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="84603-195">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-195">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="84603-196">Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-196">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-197">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-197">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-198">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-198">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-199">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-199">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="84603-202">Konwencja modelu trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-202">Page route model convention</span></span>

<span data-ttu-id="84603-203">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="84603-203">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="84603-204">Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="84603-204">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="84603-205">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-205">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="84603-206">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-206">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="84603-207">Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-207">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-208">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-208">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-209">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-209">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-210">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-210">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="84603-213">Dostosowywanie tras stron przy użyciu transformatora parametrów</span><span class="sxs-lookup"><span data-stu-id="84603-213">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="84603-214">Trasy stron generowane przez ASP.NET Core mogą być dostosowywane przy użyciu transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="84603-214">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="84603-215">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="84603-215">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="84603-216">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="84603-216">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="84603-217">Konwencja modelu trasy strony `PageRouteTransformerConvention` stosuje transformator parametrów do segmentów nazw folderów i plików w przypadku automatycznie generowanych tras stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-217">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="84603-218">Na przykład plik Razor Pages w */Pages/SubscriptionManagement/ViewAll.cshtml* będzie miał swoją trasę ponownie zapisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="84603-218">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="84603-219">`PageRouteTransformerConvention` przekształca tylko automatycznie generowane segmenty trasy strony, która pochodzi z folderu Razor Pages i nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="84603-219">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="84603-220">Nie przekształca segmentów tras dodanych z dyrektywą `@page`.</span><span class="sxs-lookup"><span data-stu-id="84603-220">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="84603-221">Konwencja nie przetwarza również tras dodanych przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="84603-221">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="84603-222">`PageRouteTransformerConvention` jest zarejestrowana jako opcja w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="84603-222">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="84603-223">Konfigurowanie trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-223">Configure a page route</span></span>

<span data-ttu-id="84603-224">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony.</span><span class="sxs-lookup"><span data-stu-id="84603-224">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="84603-225">Wygenerowane linki do strony używają określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-225">Generated links to the page use your specified route.</span></span> <span data-ttu-id="84603-226">`AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-226">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="84603-227">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="84603-227">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="84603-228">Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="84603-228">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="84603-229">Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="84603-229">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="84603-230">Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="84603-230">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="84603-231">Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:</span><span class="sxs-lookup"><span data-stu-id="84603-231">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="84603-234">Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact`lub trasy niestandardowej, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="84603-234">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="84603-235">Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="84603-235">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="84603-238">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="84603-238">Page model action conventions</span></span>

<span data-ttu-id="84603-239">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron.</span><span class="sxs-lookup"><span data-stu-id="84603-239">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="84603-240">Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.</span><span class="sxs-lookup"><span data-stu-id="84603-240">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="84603-241">W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="84603-241">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="84603-242">Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="84603-242">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="84603-243">**Konwencja modelu aplikacji folderu**</span><span class="sxs-lookup"><span data-stu-id="84603-243">**Folder app model convention**</span></span>

<span data-ttu-id="84603-244">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="84603-244">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="84603-245">Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader`do stron w folderze *OtherPages* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-245">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="84603-246">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-246">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="84603-248">**Konwencja modelu aplikacji strony**</span><span class="sxs-lookup"><span data-stu-id="84603-248">**Page app model convention**</span></span>

<span data-ttu-id="84603-249">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="84603-249">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="84603-250">Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader`do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="84603-250">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="84603-251">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-251">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="84603-253">**Konfigurowanie filtru**</span><span class="sxs-lookup"><span data-stu-id="84603-253">**Configure a filter**</span></span>

<span data-ttu-id="84603-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować.</span><span class="sxs-lookup"><span data-stu-id="84603-254"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="84603-255">Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:</span><span class="sxs-lookup"><span data-stu-id="84603-255">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="84603-256">Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* .</span><span class="sxs-lookup"><span data-stu-id="84603-256">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="84603-257">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="84603-257">If the condition passes, a header is added.</span></span> <span data-ttu-id="84603-258">W przeciwnym razie zostanie zastosowana `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="84603-258">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="84603-259">`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="84603-259">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="84603-260">Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="84603-260">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="84603-261">Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-261">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="84603-263">**Konfigurowanie fabryki filtrów**</span><span class="sxs-lookup"><span data-stu-id="84603-263">**Configure a filter factory**</span></span>

<span data-ttu-id="84603-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-264"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="84603-265">Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-265">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="84603-266">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-266">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="84603-267">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-267">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="84603-269">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="84603-269">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="84603-270">[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="84603-270">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="84603-271">Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="84603-271">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="84603-272">Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .</span><span class="sxs-lookup"><span data-stu-id="84603-272">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="84603-273">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-273">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="84603-274">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="84603-274">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84603-275">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="84603-275">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="84603-276">Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-276">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="84603-277">Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-277">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="84603-278">Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony.</span><span class="sxs-lookup"><span data-stu-id="84603-278">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="84603-279">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="84603-279">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="84603-280">Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="84603-280">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="84603-281">Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="84603-281">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="84603-282">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84603-282">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="84603-283">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="84603-283">Scenario</span></span> | <span data-ttu-id="84603-284">Przykład ilustruje...</span><span class="sxs-lookup"><span data-stu-id="84603-284">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="84603-285">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="84603-285">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="84603-286">Konwencje. Add</span><span class="sxs-lookup"><span data-stu-id="84603-286">Conventions.Add</span></span><ul><li><span data-ttu-id="84603-287">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-287">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="84603-288">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-288">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-289">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-289">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="84603-290">Dodaj szablon trasy i nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-290">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="84603-291">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-291">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="84603-292">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-292">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="84603-293">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-293">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="84603-294">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="84603-294">AddPageRoute</span></span></li></ul> | <span data-ttu-id="84603-295">Dodaj szablon trasy do stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="84603-295">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="84603-296">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="84603-296">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="84603-297">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-297">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-298">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-298">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-299">ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</span><span class="sxs-lookup"><span data-stu-id="84603-299">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="84603-300">Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-300">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="84603-301">Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="84603-301">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="84603-302">Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="84603-302">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="84603-303">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="84603-303">Route order</span></span>

<span data-ttu-id="84603-304">Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="84603-304">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="84603-305">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="84603-305">Order</span></span>            | <span data-ttu-id="84603-306">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="84603-306">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="84603-307">-1</span><span class="sxs-lookup"><span data-stu-id="84603-307">-1</span></span>               | <span data-ttu-id="84603-308">Trasa jest przetwarzana przed przetworzeniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="84603-308">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="84603-309">0</span><span class="sxs-lookup"><span data-stu-id="84603-309">0</span></span>                | <span data-ttu-id="84603-310">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="84603-310">Order isn't specified (default value).</span></span> <span data-ttu-id="84603-311">Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="84603-311">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="84603-312">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="84603-312">1, 2, &hellip; n</span></span> | <span data-ttu-id="84603-313">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-313">Specifies the route processing order.</span></span> |

<span data-ttu-id="84603-314">Przetwarzanie trasy zostało ustanowione według Konwencji:</span><span class="sxs-lookup"><span data-stu-id="84603-314">Route processing is established by convention:</span></span>

* <span data-ttu-id="84603-315">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="84603-315">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="84603-316">Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.</span><span class="sxs-lookup"><span data-stu-id="84603-316">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="84603-317">Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="84603-317">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="84603-318">Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-318">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="84603-319">Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="84603-319">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="84603-320">Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania.</span><span class="sxs-lookup"><span data-stu-id="84603-320">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="84603-321">Postaraj się, aby uprościć schemat routingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-321">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="84603-322">Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-322">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="84603-323">Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="84603-323">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="84603-324">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="84603-324">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="84603-325">Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="84603-325">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="84603-326">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="84603-326">Model conventions</span></span>

<span data-ttu-id="84603-327">Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-327">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="84603-328">Dodawanie Konwencji modelu trasy do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-328">Add a route model convention to all pages</span></span>

<span data-ttu-id="84603-329">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.</span><span class="sxs-lookup"><span data-stu-id="84603-329">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="84603-330">Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-330">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-331">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`.</span><span class="sxs-lookup"><span data-stu-id="84603-331">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="84603-332">Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-332">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="84603-333">Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-333">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="84603-334">Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-334">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="84603-335">Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-335">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="84603-336">Szablon `{aboutTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-336">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="84603-337">Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-337">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="84603-338">Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-338">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="84603-339">Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-339">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="84603-340">Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-340">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-341">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-341">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-342">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-342">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-343">Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="84603-343">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="84603-344">Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="84603-344">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="84603-345">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-345">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="84603-348">Dodawanie Konwencji modelu aplikacji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-348">Add an app model convention to all pages</span></span>

<span data-ttu-id="84603-349">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-349">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="84603-350">Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="84603-350">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="84603-351">Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów.</span><span class="sxs-lookup"><span data-stu-id="84603-351">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="84603-352">Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="84603-352">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="84603-353">Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-353">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="84603-354">Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader`do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-354">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-355">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-355">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="84603-356">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-356">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="84603-358">Dodawanie Konwencji modelu programu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-358">Add a handler model convention to all pages</span></span>

<span data-ttu-id="84603-359">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.</span><span class="sxs-lookup"><span data-stu-id="84603-359">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-360">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-360">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="84603-361">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-361">Page route action conventions</span></span>

<span data-ttu-id="84603-362">Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.</span><span class="sxs-lookup"><span data-stu-id="84603-362">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="84603-363">Konwencja modelu trasy folderu</span><span class="sxs-lookup"><span data-stu-id="84603-363">Folder route model convention</span></span>

<span data-ttu-id="84603-364">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="84603-364">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="84603-365">Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="84603-365">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="84603-366">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-366">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="84603-367">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-367">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="84603-368">Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-368">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-369">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-369">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-370">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-370">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-371">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-371">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="84603-374">Konwencja modelu trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-374">Page route model convention</span></span>

<span data-ttu-id="84603-375">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="84603-375">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="84603-376">Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="84603-376">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="84603-377">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-377">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="84603-378">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-378">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="84603-379">Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-379">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-380">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-380">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-381">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-381">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-382">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-382">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="84603-385">Dostosowywanie tras stron przy użyciu transformatora parametrów</span><span class="sxs-lookup"><span data-stu-id="84603-385">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="84603-386">Trasy stron generowane przez ASP.NET Core mogą być dostosowywane przy użyciu transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="84603-386">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="84603-387">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="84603-387">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="84603-388">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="84603-388">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="84603-389">Konwencja modelu trasy strony `PageRouteTransformerConvention` stosuje transformator parametrów do segmentów nazw folderów i plików w przypadku automatycznie generowanych tras stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-389">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="84603-390">Na przykład plik Razor Pages w */Pages/SubscriptionManagement/ViewAll.cshtml* będzie miał swoją trasę ponownie zapisany z `/SubscriptionManagement/ViewAll` do `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="84603-390">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="84603-391">`PageRouteTransformerConvention` przekształca tylko automatycznie generowane segmenty trasy strony, która pochodzi z folderu Razor Pages i nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="84603-391">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="84603-392">Nie przekształca segmentów tras dodanych z dyrektywą `@page`.</span><span class="sxs-lookup"><span data-stu-id="84603-392">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="84603-393">Konwencja nie przetwarza również tras dodanych przez <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="84603-393">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="84603-394">`PageRouteTransformerConvention` jest zarejestrowana jako opcja w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="84603-394">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="84603-395">Konfigurowanie trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-395">Configure a page route</span></span>

<span data-ttu-id="84603-396">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony.</span><span class="sxs-lookup"><span data-stu-id="84603-396">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="84603-397">Wygenerowane linki do strony używają określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-397">Generated links to the page use your specified route.</span></span> <span data-ttu-id="84603-398">`AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-398">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="84603-399">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="84603-399">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="84603-400">Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="84603-400">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="84603-401">Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="84603-401">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="84603-402">Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="84603-402">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="84603-403">Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:</span><span class="sxs-lookup"><span data-stu-id="84603-403">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="84603-406">Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact`lub trasy niestandardowej, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="84603-406">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="84603-407">Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="84603-407">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="84603-410">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="84603-410">Page model action conventions</span></span>

<span data-ttu-id="84603-411">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron.</span><span class="sxs-lookup"><span data-stu-id="84603-411">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="84603-412">Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.</span><span class="sxs-lookup"><span data-stu-id="84603-412">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="84603-413">W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="84603-413">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="84603-414">Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="84603-414">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="84603-415">**Konwencja modelu aplikacji folderu**</span><span class="sxs-lookup"><span data-stu-id="84603-415">**Folder app model convention**</span></span>

<span data-ttu-id="84603-416">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="84603-416">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="84603-417">Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader`do stron w folderze *OtherPages* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-417">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="84603-418">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-418">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="84603-420">**Konwencja modelu aplikacji strony**</span><span class="sxs-lookup"><span data-stu-id="84603-420">**Page app model convention**</span></span>

<span data-ttu-id="84603-421">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="84603-421">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="84603-422">Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader`do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="84603-422">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="84603-423">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-423">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="84603-425">**Konfigurowanie filtru**</span><span class="sxs-lookup"><span data-stu-id="84603-425">**Configure a filter**</span></span>

<span data-ttu-id="84603-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować.</span><span class="sxs-lookup"><span data-stu-id="84603-426"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="84603-427">Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:</span><span class="sxs-lookup"><span data-stu-id="84603-427">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="84603-428">Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* .</span><span class="sxs-lookup"><span data-stu-id="84603-428">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="84603-429">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="84603-429">If the condition passes, a header is added.</span></span> <span data-ttu-id="84603-430">W przeciwnym razie zostanie zastosowana `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="84603-430">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="84603-431">`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="84603-431">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="84603-432">Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="84603-432">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="84603-433">Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-433">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="84603-435">**Konfigurowanie fabryki filtrów**</span><span class="sxs-lookup"><span data-stu-id="84603-435">**Configure a filter factory**</span></span>

<span data-ttu-id="84603-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-436"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="84603-437">Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-437">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="84603-438">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-438">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="84603-439">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-439">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="84603-441">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="84603-441">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="84603-442">[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="84603-442">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="84603-443">Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="84603-443">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="84603-444">Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .</span><span class="sxs-lookup"><span data-stu-id="84603-444">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="84603-445">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-445">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="84603-446">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="84603-446">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84603-447">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="84603-447">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="84603-448">Dowiedz się, w jaki sposób używać [konwencji dotyczących trasy strony i dostawcy modelu aplikacji](xref:mvc/controllers/application-model#conventions) do kontrolowania routingu, odnajdywania i przetwarzania stron w aplikacjach Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-448">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="84603-449">Jeśli trzeba skonfigurować niestandardowe trasy stron dla poszczególnych stron, skonfiguruj Routing do stron z [Konwencją AddPageRoute](#configure-a-page-route) opisanej w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-449">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="84603-450">Aby określić trasę strony, dodać segmenty tras lub dodać parametry do trasy, użyj dyrektywy `@page` strony.</span><span class="sxs-lookup"><span data-stu-id="84603-450">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="84603-451">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="84603-451">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="84603-452">Istnieją słowa zastrzeżone, których nie można używać jako segmentów tras ani nazw parametrów.</span><span class="sxs-lookup"><span data-stu-id="84603-452">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="84603-453">Aby uzyskać więcej informacji, zobacz [Routing: zastrzeżone nazwy routingu](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="84603-453">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="84603-454">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84603-454">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="84603-455">Scenariusz</span><span class="sxs-lookup"><span data-stu-id="84603-455">Scenario</span></span> | <span data-ttu-id="84603-456">Przykład ilustruje...</span><span class="sxs-lookup"><span data-stu-id="84603-456">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="84603-457">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="84603-457">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="84603-458">Konwencje. Add</span><span class="sxs-lookup"><span data-stu-id="84603-458">Conventions.Add</span></span><ul><li><span data-ttu-id="84603-459">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-459">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="84603-460">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-460">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-461">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-461">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="84603-462">Dodaj szablon trasy i nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-462">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="84603-463">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-463">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="84603-464">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-464">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="84603-465">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-465">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="84603-466">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="84603-466">AddPageRoute</span></span></li></ul> | <span data-ttu-id="84603-467">Dodaj szablon trasy do stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="84603-467">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="84603-468">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="84603-468">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="84603-469">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-469">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-470">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="84603-470">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="84603-471">ConfigureFilter (Klasa filtru, wyrażenie lambda lub fabryka filtrów)</span><span class="sxs-lookup"><span data-stu-id="84603-471">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="84603-472">Dodaj nagłówek do stron w folderze, Dodaj nagłówek do jednej strony i skonfiguruj [fabrykę filtrów](xref:mvc/controllers/filters#ifilterfactory) , aby dodać nagłówek do stron aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-472">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="84603-473">Konwencje Razor Pages są dodawane i konfigurowane przy użyciu metody rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> do <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> w kolekcji usług w klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="84603-473">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="84603-474">Poniższe przykłady Konwencji zostały omówione w dalszej części tego tematu:</span><span class="sxs-lookup"><span data-stu-id="84603-474">The following convention examples are explained later in this topic:</span></span>

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

## <a name="route-order"></a><span data-ttu-id="84603-475">Kolejność tras</span><span class="sxs-lookup"><span data-stu-id="84603-475">Route order</span></span>

<span data-ttu-id="84603-476">Trasy określają <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> do przetwarzania (dopasowanie trasy).</span><span class="sxs-lookup"><span data-stu-id="84603-476">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="84603-477">Zamówienie</span><span class="sxs-lookup"><span data-stu-id="84603-477">Order</span></span>            | <span data-ttu-id="84603-478">Zachowanie</span><span class="sxs-lookup"><span data-stu-id="84603-478">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="84603-479">-1</span><span class="sxs-lookup"><span data-stu-id="84603-479">-1</span></span>               | <span data-ttu-id="84603-480">Trasa jest przetwarzana przed przetworzeniem innych tras.</span><span class="sxs-lookup"><span data-stu-id="84603-480">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="84603-481">0</span><span class="sxs-lookup"><span data-stu-id="84603-481">0</span></span>                | <span data-ttu-id="84603-482">Nie określono kolejności (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="84603-482">Order isn't specified (default value).</span></span> <span data-ttu-id="84603-483">Przypisanie `Order` (`Order = null`) domyślnie trasy `Order` do 0 (zero) do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="84603-483">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="84603-484">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="84603-484">1, 2, &hellip; n</span></span> | <span data-ttu-id="84603-485">Określa kolejność przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-485">Specifies the route processing order.</span></span> |

<span data-ttu-id="84603-486">Przetwarzanie trasy zostało ustanowione według Konwencji:</span><span class="sxs-lookup"><span data-stu-id="84603-486">Route processing is established by convention:</span></span>

* <span data-ttu-id="84603-487">Trasy są przetwarzane w kolejności sekwencyjnej (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="84603-487">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="84603-488">Gdy trasy mają takie same `Order`, najpierw pasuje do najbardziej określonej trasy, a następnie mniej konkretnych tras.</span><span class="sxs-lookup"><span data-stu-id="84603-488">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="84603-489">Gdy trasy o tej samej `Order` i tej samej liczbie parametrów pasują do adresu URL żądania, trasy są przetwarzane w kolejności, w jakiej są dodawane do <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="84603-489">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="84603-490">Jeśli to możliwe, unikaj w zależności od ustalonej kolejności przetwarzania trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-490">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="84603-491">Ogólnie rzecz biorąc, routing wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="84603-491">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="84603-492">Jeśli musisz ustawić właściwości `Order` trasy, aby poprawnie kierować żądania, schemat routingu aplikacji jest prawdopodobnie mylący dla klientów i jest nierozsądny do utrzymania.</span><span class="sxs-lookup"><span data-stu-id="84603-492">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="84603-493">Postaraj się, aby uprościć schemat routingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-493">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="84603-494">Przykładowa aplikacja wymaga jawnej kolejności przetwarzania trasy, aby przedstawić kilka scenariuszy routingu przy użyciu jednej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-494">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="84603-495">Należy jednak podjąć próbę uniknięcia praktycznego ustawienia `Order` tras w aplikacjach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="84603-495">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="84603-496">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="84603-496">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="84603-497">Informacje o zamówieniu trasy w tematach MVC są dostępne w obszarze [routing do akcji kontrolera: porządkowanie tras atrybutów](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="84603-497">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="84603-498">Konwencje modelu</span><span class="sxs-lookup"><span data-stu-id="84603-498">Model conventions</span></span>

<span data-ttu-id="84603-499">Dodaj delegata dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, aby dodać [konwencje modelu](xref:mvc/controllers/application-model#conventions) , które mają zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-499">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="84603-500">Dodawanie Konwencji modelu trasy do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-500">Add a route model convention to all pages</span></span>

<span data-ttu-id="84603-501">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu trasy strony.</span><span class="sxs-lookup"><span data-stu-id="84603-501">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="84603-502">Przykładowa aplikacja dodaje szablon `{globalTemplate?}` trasy do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-502">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-503">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `1`.</span><span class="sxs-lookup"><span data-stu-id="84603-503">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="84603-504">Zapewnia to zachowanie dopasowania trasy w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-504">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="84603-505">Szablon trasy dla `TheContactPage/{text?}` został dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-505">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="84603-506">Trasa strony kontaktowej ma domyślną kolejność `null` (`Order = 0`), więc pasuje przed szablonem `{globalTemplate?}` trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-506">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="84603-507">Szablon trasy `{aboutTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-507">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="84603-508">Szablon `{aboutTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-508">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="84603-509">Gdy strona informacje jest wymagana w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-509">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="84603-510">Szablon trasy `{otherPagesTemplate?}` zostanie dodany w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-510">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="84603-511">Szablon `{otherPagesTemplate?}` jest przyznany `Order` `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-511">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="84603-512">Gdy zażądana jest jakakolwiek strona w folderze *Pages/OtherPages* z parametrem trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-512">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-513">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-513">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-514">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-514">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-515">Opcje Razor Pages, takie jak dodawanie <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, są dodawane po dodaniu MVC do kolekcji usług w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="84603-515">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="84603-516">Aby zapoznać się z przykładem, zobacz przykładową [aplikację](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="84603-516">For an example, see the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="84603-517">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-517">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![Strona informacje jest wymagana z segmentem trasy GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="84603-520">Dodawanie Konwencji modelu aplikacji do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-520">Add an app model convention to all pages</span></span>

<span data-ttu-id="84603-521">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84603-521">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="84603-522">Aby przedstawić te i inne konwencje w dalszej części tematu, przykładowa aplikacja zawiera klasę `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="84603-522">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="84603-523">Konstruktor klasy akceptuje `name` ciąg i `values` tablicę ciągów.</span><span class="sxs-lookup"><span data-stu-id="84603-523">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="84603-524">Te wartości są używane w `OnResultExecuting` metodzie do ustawiania nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="84603-524">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="84603-525">Pełna Klasa jest wyświetlana w sekcji [konwencje akcji modelu strony](#page-model-action-conventions) w dalszej części tematu.</span><span class="sxs-lookup"><span data-stu-id="84603-525">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="84603-526">Aplikacja Przykładowa używa klasy `AddHeaderAttribute` do dodawania nagłówka, `GlobalHeader`do wszystkich stron w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-526">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-527">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-527">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="84603-528">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-528">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że GlobalHeader został dodany.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="84603-530">Dodawanie Konwencji modelu programu obsługi do wszystkich stron</span><span class="sxs-lookup"><span data-stu-id="84603-530">Add a handler model convention to all pages</span></span>

<span data-ttu-id="84603-531">Użyj <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> do kolekcji wystąpień <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention>, które są stosowane podczas konstruowania modelu obsługi stron.</span><span class="sxs-lookup"><span data-stu-id="84603-531">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="84603-532">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-532">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="84603-533">Konwencje akcji trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-533">Page route action conventions</span></span>

<span data-ttu-id="84603-534">Domyślny dostawca modelu trasy pochodzący z <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> wywołuje konwencje, które zostały zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania tras stron.</span><span class="sxs-lookup"><span data-stu-id="84603-534">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="84603-535">Konwencja modelu trasy folderu</span><span class="sxs-lookup"><span data-stu-id="84603-535">Folder route model convention</span></span>

<span data-ttu-id="84603-536">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="84603-536">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="84603-537">Przykładowa aplikacja używa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> do dodawania szablonu trasy `{otherPagesTemplate?}` do stron w folderze *OtherPages* :</span><span class="sxs-lookup"><span data-stu-id="84603-537">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="84603-538">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-538">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="84603-539">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-539">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="84603-540">Jeśli zażądano strony w folderze *Pages/OtherPages* z wartością parametru trasy (na przykład `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-540">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-541">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-541">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-542">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-542">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-543">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-543">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Żądanie Strona1 w folderze OtherPages z segmentem trasy GlobalRouteValue i OtherPagesRouteValue.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="84603-546">Konwencja modelu trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-546">Page route model convention</span></span>

<span data-ttu-id="84603-547">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="84603-547">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="84603-548">Przykładowa aplikacja używa `AddPageRouteModelConvention` do dodawania szablonu trasy `{aboutTemplate?}` do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="84603-548">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="84603-549">Właściwość <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> dla <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> jest ustawiona na `2`.</span><span class="sxs-lookup"><span data-stu-id="84603-549">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="84603-550">Dzięki temu szablon dla `{globalTemplate?}` (ustawiony wcześniej w temacie do `1`) ma priorytet dla pierwszej pozycji wartości danych trasy, gdy zostanie podana wartość pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-550">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="84603-551">Jeśli na stronie informacje zażądano wartości parametru trasy w `/About/RouteDataValue`, "RouteDataValue" jest ładowany do `RouteData.Values["globalTemplate"]` (`Order = 1`), a nie `RouteData.Values["aboutTemplate"]` (`Order = 2`) z powodu ustawienia właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="84603-551">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="84603-552">Wszędzie tam, gdzie to możliwe, nie ustawiaj `Order`, co spowoduje `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="84603-552">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="84603-553">Należy polegać na routingu w celu wybrania odpowiedniej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-553">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="84603-554">Zażądaj przykładowej strony o `localhost:5000/About/GlobalRouteValue/AboutRouteValue` i sprawdź wynik:</span><span class="sxs-lookup"><span data-stu-id="84603-554">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![Żądanie strony about z segmentami trasy dla GlobalRouteValue i AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a><span data-ttu-id="84603-557">Konfigurowanie trasy strony</span><span class="sxs-lookup"><span data-stu-id="84603-557">Configure a page route</span></span>

<span data-ttu-id="84603-558">Użyj <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>, aby skonfigurować trasę do strony pod określoną ścieżką strony.</span><span class="sxs-lookup"><span data-stu-id="84603-558">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="84603-559">Wygenerowane linki do strony używają określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-559">Generated links to the page use your specified route.</span></span> <span data-ttu-id="84603-560">`AddPageRoute` używa `AddPageRouteModelConvention` do ustanowienia trasy.</span><span class="sxs-lookup"><span data-stu-id="84603-560">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="84603-561">Przykładowa aplikacja tworzy trasę do `/TheContactPage` dla *Contact. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="84603-561">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="84603-562">Na stronie kontakt można także uzyskać dostęp do `/Contact` za pośrednictwem swojej trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="84603-562">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="84603-563">Niestandardowa trasa przykładowej aplikacji do strony kontaktowej umożliwia określenie opcjonalnego segmentu trasy `text` (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="84603-563">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="84603-564">Strona zawiera również ten opcjonalny segment w swojej dyrektywie `@page` na wypadek, gdy odwiedzający uzyskuje dostęp do strony przy użyciu trasy `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="84603-564">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="84603-565">Należy pamiętać, że adres URL wygenerowany dla linku **kontaktowego** na renderowanej stronie odzwierciedla zaktualizowaną trasę:</span><span class="sxs-lookup"><span data-stu-id="84603-565">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Link do kontaktu z przykładową aplikacją na pasku nawigacyjnym](razor-pages-conventions/_static/contact-link.png)

![Sprawdzanie linku kontaktu w renderowanym kodzie HTML wskazuje, że odwołanie href jest ustawione na wartość "/TheContactPage"](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="84603-568">Odwiedź stronę kontaktu na swojej zwykłej trasie, `/Contact`lub trasy niestandardowej, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="84603-568">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="84603-569">Jeśli podasz dodatkowy segment trasy `text`, na stronie zostanie wyświetlony segment zakodowany w formacie HTML:</span><span class="sxs-lookup"><span data-stu-id="84603-569">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Przykład przeglądarki Microsoft Edge podawania opcjonalne 'text' segment trasy "Wartość tekstowa" w adresie URL.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="84603-572">Konwencje akcji modelu strony</span><span class="sxs-lookup"><span data-stu-id="84603-572">Page model action conventions</span></span>

<span data-ttu-id="84603-573">Domyślny dostawca modelu strony, który implementuje <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> wywołuje konwencje, które są zaprojektowane w celu zapewnienia punktów rozszerzalności do konfigurowania modeli stron.</span><span class="sxs-lookup"><span data-stu-id="84603-573">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="84603-574">Konwencje te są przydatne podczas kompilowania i modyfikowania scenariuszy przetwarzania i odnajdywania stron.</span><span class="sxs-lookup"><span data-stu-id="84603-574">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="84603-575">W przykładach w tej sekcji Przykładowa aplikacja używa klasy `AddHeaderAttribute`, która jest <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, która ma zastosowanie do nagłówka odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="84603-575">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="84603-576">Przy użyciu konwencji, przykład pokazuje, jak zastosować atrybut do wszystkich stron w folderze i do pojedynczej strony.</span><span class="sxs-lookup"><span data-stu-id="84603-576">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="84603-577">**Konwencja modelu aplikacji folderu**</span><span class="sxs-lookup"><span data-stu-id="84603-577">**Folder app model convention**</span></span>

<span data-ttu-id="84603-578">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję w wystąpieniach <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla wszystkich stron w określonym folderze.</span><span class="sxs-lookup"><span data-stu-id="84603-578">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="84603-579">Przykład ilustruje użycie `AddFolderApplicationModelConvention` przez dodanie nagłówka, `OtherPagesHeader`do stron w folderze *OtherPages* aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-579">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="84603-580">Zażądaj przykładowej strony w `localhost:5000/OtherPages/Page1` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-580">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi strony OtherPages/Strona1 pokazują, że OtherPagesHeader został dodany.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="84603-582">**Konwencja modelu aplikacji strony**</span><span class="sxs-lookup"><span data-stu-id="84603-582">**Page app model convention**</span></span>

<span data-ttu-id="84603-583">Użyj <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*>, aby utworzyć i dodać <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention>, który wywołuje akcję na <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> dla strony o określonej nazwie.</span><span class="sxs-lookup"><span data-stu-id="84603-583">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="84603-584">Przykład ilustruje użycie `AddPageApplicationModelConvention` przez dodanie nagłówka, `AboutHeader`do strony informacje:</span><span class="sxs-lookup"><span data-stu-id="84603-584">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="84603-585">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-585">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że AboutHeader został dodany.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="84603-587">**Konfigurowanie filtru**</span><span class="sxs-lookup"><span data-stu-id="84603-587">**Configure a filter**</span></span>

<span data-ttu-id="84603-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określony filtr, aby zastosować.</span><span class="sxs-lookup"><span data-stu-id="84603-588"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="84603-589">Można zaimplementować klasę filtru, ale Przykładowa aplikacja pokazuje, jak zaimplementować filtr w wyrażeniu lambda, które jest zaimplementowane w tle jako fabryka, która zwraca filtr:</span><span class="sxs-lookup"><span data-stu-id="84603-589">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="84603-590">Model aplikacji strony służy do sprawdzania ścieżki względnej dla segmentów, które prowadzą do strony PAGE2 w folderze *OtherPages* .</span><span class="sxs-lookup"><span data-stu-id="84603-590">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="84603-591">Jeśli warunek zostanie spełniony, zostanie dodany nagłówek.</span><span class="sxs-lookup"><span data-stu-id="84603-591">If the condition passes, a header is added.</span></span> <span data-ttu-id="84603-592">W przeciwnym razie zostanie zastosowana `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="84603-592">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="84603-593">`EmptyFilter` jest [filtrem akcji](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="84603-593">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="84603-594">Ponieważ filtry akcji są ignorowane przez Razor Pages, `EmptyFilter` nie ma wpływu zgodnie z oczekiwaniami, jeśli ścieżka nie zawiera `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="84603-594">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="84603-595">Zażądaj strony PAGE2 próbki w `localhost:5000/OtherPages/Page2` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-595">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header jest dodawany do odpowiedzi dla PAGE2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="84603-597">**Konfigurowanie fabryki filtrów**</span><span class="sxs-lookup"><span data-stu-id="84603-597">**Configure a filter factory**</span></span>

<span data-ttu-id="84603-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> konfiguruje określoną fabrykę, aby zastosować [filtry](xref:mvc/controllers/filters) do wszystkich Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-598"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="84603-599">Przykładowa aplikacja zawiera przykładowe użycie [fabryki filtrów](xref:mvc/controllers/filters#ifilterfactory) przez dodanie nagłówka, `FilterFactoryHeader`, z dwiema wartościami na stronach aplikacji:</span><span class="sxs-lookup"><span data-stu-id="84603-599">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="84603-600">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="84603-600">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="84603-601">Zażądaj strony dotyczącej próbki w `localhost:5000/About` i sprawdź nagłówki, aby wyświetlić wyniki:</span><span class="sxs-lookup"><span data-stu-id="84603-601">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Nagłówki odpowiedzi na stronie informacje pokazują, że dodano dwa nagłówki FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="84603-603">Filtry MVC i filtr strony (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="84603-603">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="84603-604">[Filtry akcji](xref:mvc/controllers/filters#action-filters) MVC są ignorowane przez Razor Pages, ponieważ Razor Pages używają metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="84603-604">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="84603-605">Dostępne są inne typy filtrów MVC: [autoryzacja](xref:mvc/controllers/filters#authorization-filters), [wyjątek](xref:mvc/controllers/filters#exception-filters), [zasób](xref:mvc/controllers/filters#resource-filters)i [wynik](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="84603-605">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="84603-606">Aby uzyskać więcej informacji, zobacz temat [filtry](xref:mvc/controllers/filters) .</span><span class="sxs-lookup"><span data-stu-id="84603-606">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="84603-607">Filtr strony (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) jest filtrem, który ma zastosowanie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="84603-607">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="84603-608">Aby uzyskać więcej informacji, zobacz [metody filtrowania dla Razor Pages](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="84603-608">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84603-609">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="84603-609">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>

::: moniker-end
