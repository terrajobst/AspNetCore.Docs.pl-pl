---
title: Routing do akcji kontrolera w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób ASP.NET Core MVC używa programów pośredniczących routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: be7da9eeaf64c2f52c095b5179ccc22db43d57c3
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242581"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="670f4-103">Routing do akcji kontrolera w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="670f4-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="670f4-104">[Ryan Nowak](https://github.com/rynowak), [Kirka Larkin](https://twitter.com/serpent5)i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="670f4-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="670f4-105">Kontrolery ASP.NET Core używają [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) routingu do dopasowywania adresów URL żądań przychodzących i mapują je na [Akcje](#action).</span><span class="sxs-lookup"><span data-stu-id="670f4-105">ASP.NET Core controllers use the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to [actions](#action).</span></span>  <span data-ttu-id="670f4-106">Szablony tras:</span><span class="sxs-lookup"><span data-stu-id="670f4-106">Routes templates:</span></span>

* <span data-ttu-id="670f4-107">Są zdefiniowane w kodzie startowym lub atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-107">Are defined in startup code or attributes.</span></span>
* <span data-ttu-id="670f4-108">Opisz, w jaki sposób ścieżki URL są dopasowywane do [akcji](#action).</span><span class="sxs-lookup"><span data-stu-id="670f4-108">Describe how URL paths are matched to [actions](#action).</span></span>
* <span data-ttu-id="670f4-109">Służy do generowania adresów URL dla łączy.</span><span class="sxs-lookup"><span data-stu-id="670f4-109">Are used to generate URLs for links.</span></span> <span data-ttu-id="670f4-110">Wygenerowane linki są zwykle zwracane w odpowiedziach.</span><span class="sxs-lookup"><span data-stu-id="670f4-110">The generated links are typically returned in responses.</span></span>

<span data-ttu-id="670f4-111">Akcje są albo [podkierowane do Konwencji](#cr) lub [kierowane przez atrybut](#ar).</span><span class="sxs-lookup"><span data-stu-id="670f4-111">Actions are either [conventionally-routed](#cr) or [attribute-routed](#ar).</span></span> <span data-ttu-id="670f4-112">Umieszczenie trasy na kontrolerze lub [akcji](#action) sprawia, że jest ona kierowana przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="670f4-112">Placing a route on the controller or [action](#action) makes it attribute-routed.</span></span> <span data-ttu-id="670f4-113">Aby uzyskać więcej informacji, zobacz [Routing mieszany](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="670f4-113">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="670f4-114">Ten dokument:</span><span class="sxs-lookup"><span data-stu-id="670f4-114">This document:</span></span>

* <span data-ttu-id="670f4-115">Wyjaśnia interakcje między MVC i Routing:</span><span class="sxs-lookup"><span data-stu-id="670f4-115">Explains the interactions between MVC and routing:</span></span>
  * <span data-ttu-id="670f4-116">Jak typowe aplikacje MVC używają funkcji routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-116">How typical MVC apps make use of routing features.</span></span>
  * <span data-ttu-id="670f4-117">Obejmuje zarówno:</span><span class="sxs-lookup"><span data-stu-id="670f4-117">Covers both:</span></span>
    * <span data-ttu-id="670f4-118">[UNC routingu](#cr) zwykle używany z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="670f4-118">[Conventionally routing](#cr) typically used with controllers and views.</span></span>
    * <span data-ttu-id="670f4-119">*Routing atrybutów* używany z interfejsami API REST.</span><span class="sxs-lookup"><span data-stu-id="670f4-119">*Attribute routing* used with REST APIs.</span></span> <span data-ttu-id="670f4-120">Jeśli jesteś głównie zainteresowani routingiem interfejsów API REST, przejdź do sekcji [Routing atrybutów dla interfejsów API REST](#ar) .</span><span class="sxs-lookup"><span data-stu-id="670f4-120">If you're primarily interested in routing for REST APIs, jump to the [Attribute routing for REST APIs](#ar) section.</span></span>
  * <span data-ttu-id="670f4-121">Szczegóły dotyczące routingu zaawansowanego znajdują się w temacie [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="670f4-121">See [Routing](xref:fundamentals/routing) for advanced routing details.</span></span>
* <span data-ttu-id="670f4-122">Odnosi się do domyślnego systemu routingu dodanego w ASP.NET Core 3,0, nazywanego routingiem punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="670f4-122">Refers to the default routing system added in ASP.NET Core 3.0, called endpoint routing.</span></span> <span data-ttu-id="670f4-123">Możliwe jest używanie kontrolerów z poprzednią wersją routingu w celu zapewnienia zgodności.</span><span class="sxs-lookup"><span data-stu-id="670f4-123">It's possible to use controllers with the previous version of routing for compatibility purposes.</span></span> <span data-ttu-id="670f4-124">Instrukcje można znaleźć w [przewodniku migracji 2.2-3.0](xref:migration/22-to-30) .</span><span class="sxs-lookup"><span data-stu-id="670f4-124">See the [2.2-3.0 migration guide](xref:migration/22-to-30) for instructions.</span></span> <span data-ttu-id="670f4-125">Zapoznaj się z [wersją 2,2 tego dokumentu](xref:mvc/controllers/routing?view=aspnetcore-2.2) dotyczącą materiałów referencyjnych w starszym systemie routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-125">Refer to the [2.2 version of this document](xref:mvc/controllers/routing?view=aspnetcore-2.2) for reference material on the legacy routing system.</span></span>

<a name="cr"></a>

## <a name="set-up-conventional-route"></a><span data-ttu-id="670f4-126">Konfigurowanie trasy konwencjonalnej</span><span class="sxs-lookup"><span data-stu-id="670f4-126">Set up conventional route</span></span>

<span data-ttu-id="670f4-127">w przypadku korzystania z [routingu konwencjonalnego](#crd)`Startup.Configure` zwykle ma kod podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="670f4-127">`Startup.Configure` typically has code similar to the following when using [conventional routing](#crd):</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<span data-ttu-id="670f4-128">Wewnątrz wywołania do <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> służy do tworzenia pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-128">Inside the call to <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> is used to create a single route.</span></span> <span data-ttu-id="670f4-129">Pojedyncza trasa nosi nazwę `default` Route.</span><span class="sxs-lookup"><span data-stu-id="670f4-129">The single route is named `default` route.</span></span> <span data-ttu-id="670f4-130">Większość aplikacji z kontrolerami i widokami używa szablonu trasy podobnego do trasy `default`.</span><span class="sxs-lookup"><span data-stu-id="670f4-130">Most apps with controllers and views use a route template similar to the `default` route.</span></span> <span data-ttu-id="670f4-131">Interfejsy API REST powinny używać [routingu atrybutów](#ar).</span><span class="sxs-lookup"><span data-stu-id="670f4-131">REST APIs should use [attribute routing](#ar).</span></span>

<span data-ttu-id="670f4-132">`"{controller=Home}/{action=Index}/{id?}"`szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-132">The route template `"{controller=Home}/{action=Index}/{id?}"`:</span></span>

* <span data-ttu-id="670f4-133">Dopasowuje ścieżkę URL, taką jak `/Products/Details/5`</span><span class="sxs-lookup"><span data-stu-id="670f4-133">Matches a URL path like `/Products/Details/5`</span></span>
* <span data-ttu-id="670f4-134">Wyodrębnia wartości tras `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki.</span><span class="sxs-lookup"><span data-stu-id="670f4-134">Extracts the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="670f4-135">Wyodrębnienie wartości tras powoduje dopasowanie, jeśli aplikacja ma kontroler o nazwie `ProductsController` i akcję `Details`:</span><span class="sxs-lookup"><span data-stu-id="670f4-135">The extraction of route values results in a match if the app has a controller named `ProductsController` and a `Details` action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

 <span data-ttu-id="670f4-136">Metoda [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) jest dołączana do [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) i służy do wyświetlania informacji o routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-136">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

  * <span data-ttu-id="670f4-137">Model `/Products/Details/5` wiąże wartość `id = 5`, aby ustawić parametr `id` na `5`.</span><span class="sxs-lookup"><span data-stu-id="670f4-137">`/Products/Details/5` model binds the value of `id = 5` to set the `id` parameter to `5`.</span></span> <span data-ttu-id="670f4-138">Aby uzyskać więcej informacji, zobacz [powiązanie modelu](xref:mvc/models/model-binding) .</span><span class="sxs-lookup"><span data-stu-id="670f4-138">See [Model Binding](xref:mvc/models/model-binding) for more details.</span></span>
* <span data-ttu-id="670f4-139">`{controller=Home}` definiuje `Home` jako `controller`domyślny.</span><span class="sxs-lookup"><span data-stu-id="670f4-139">`{controller=Home}` defines `Home` as the default `controller`.</span></span>
* <span data-ttu-id="670f4-140">`{action=Index}` definiuje `Index` jako `action`domyślny.</span><span class="sxs-lookup"><span data-stu-id="670f4-140">`{action=Index}` defines `Index` as the default `action`.</span></span>
*  <span data-ttu-id="670f4-141">Znak `?` w `{id?}` definiuje `id` jako opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="670f4-141">The `?` character in `{id?}` defines `id` as optional.</span></span>
  * <span data-ttu-id="670f4-142">Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania.</span><span class="sxs-lookup"><span data-stu-id="670f4-142">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="670f4-143">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="670f4-143">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>
* <span data-ttu-id="670f4-144">Dopasowuje ścieżkę URL `/`.</span><span class="sxs-lookup"><span data-stu-id="670f4-144">Matches the URL path `/`.</span></span>
* <span data-ttu-id="670f4-145">Tworzy wartości trasy `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-145">Produces the route values `{ controller = Home, action = Index }`.</span></span>
* <span data-ttu-id="670f4-146">Metoda [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) jest dołączana do [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) i służy do wyświetlania informacji o routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-146">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>

<span data-ttu-id="670f4-147">Wartości `controller` i `action` używają wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="670f4-147">The values for `controller` and `action` make use of the default values.</span></span> <span data-ttu-id="670f4-148">`id` nie produkuje wartości, ponieważ w ścieżce URL nie ma odpowiedniego segmentu.</span><span class="sxs-lookup"><span data-stu-id="670f4-148">`id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="670f4-149">`/` dopasowuje się tylko wtedy, gdy istnieje `HomeController` i `Index` akcji:</span><span class="sxs-lookup"><span data-stu-id="670f4-149">`/` only matches if there exists a `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="670f4-150">Przy użyciu definicji kontrolera i szablonu trasy, Akcja `HomeController.Index` jest uruchamiana dla następujących ścieżek URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-150">Using the preceding controller definition and route template, the `HomeController.Index` action is run for the following URL paths:</span></span>

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

<span data-ttu-id="670f4-151">Ścieżka adresu URL `/` używa domyślnych kontrolerów szablonu trasy `Home` i akcji `Index`.</span><span class="sxs-lookup"><span data-stu-id="670f4-151">The URL path `/` uses the route template default `Home` controllers and `Index` action.</span></span> <span data-ttu-id="670f4-152">Ścieżka adresu URL `/Home` używa domyślnej akcji szablonu trasy `Index`.</span><span class="sxs-lookup"><span data-stu-id="670f4-152">The URL path `/Home` uses the route template default `Index` action.</span></span>

<span data-ttu-id="670f4-153">Wygodna <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>metody:</span><span class="sxs-lookup"><span data-stu-id="670f4-153">The convenience method <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>:</span></span>

```csharp
endpoints.MapDefaultControllerRoute();
```

<span data-ttu-id="670f4-154">Zastępując</span><span class="sxs-lookup"><span data-stu-id="670f4-154">Replaces:</span></span>

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="670f4-155">Routing jest konfigurowany przy użyciu <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> i <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="670f4-155">Routing is configured using the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> middleware.</span></span> <span data-ttu-id="670f4-156">Aby użyć kontrolerów:</span><span class="sxs-lookup"><span data-stu-id="670f4-156">To use controllers:</span></span>

* <span data-ttu-id="670f4-157">Wywołaj <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> wewnątrz `UseEndpoints` w celu mapowania kontrolerów z [routingiem atrybutów](#ar) .</span><span class="sxs-lookup"><span data-stu-id="670f4-157">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> inside `UseEndpoints` to map [attribute routed](#ar) controllers.</span></span>
* <span data-ttu-id="670f4-158">Wywołaj <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> lub <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, aby zamapować kontrolery z [Konwencją routingu](#cr) .</span><span class="sxs-lookup"><span data-stu-id="670f4-158">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> or <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, to map [conventionally routed](#cr) controllers.</span></span>

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a><span data-ttu-id="670f4-159">Routing konwencjonalny</span><span class="sxs-lookup"><span data-stu-id="670f4-159">Conventional routing</span></span>

<span data-ttu-id="670f4-160">Routing konwencjonalny jest używany z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="670f4-160">Conventional routing is used with controllers and views.</span></span> <span data-ttu-id="670f4-161">Trasa `default`:</span><span class="sxs-lookup"><span data-stu-id="670f4-161">The `default` route:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

<span data-ttu-id="670f4-162">jest przykładem *konwencjonalnego routingu*.</span><span class="sxs-lookup"><span data-stu-id="670f4-162">is an example of a *conventional routing*.</span></span> <span data-ttu-id="670f4-163">Jest on nazywany *konwencjonalnym routingiem* , ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-163">It's called *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="670f4-164">Pierwszy segment ścieżki, `{controller=Home}`, mapuje na nazwę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-164">The first path segment, `{controller=Home}`, maps to the controller name.</span></span>
* <span data-ttu-id="670f4-165">Drugi segment `{action=Index}`mapuje na nazwę [akcji](#action) .</span><span class="sxs-lookup"><span data-stu-id="670f4-165">The second segment, `{action=Index}`, maps to the [action](#action) name.</span></span>
* <span data-ttu-id="670f4-166">Trzeci segment `{id?}` jest używany w przypadku opcjonalnego `id`.</span><span class="sxs-lookup"><span data-stu-id="670f4-166">The third segment, `{id?}` is used for an optional `id`.</span></span> <span data-ttu-id="670f4-167">`?` w `{id?}` sprawia, że jest to opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="670f4-167">The `?` in `{id?}` makes it optional.</span></span> <span data-ttu-id="670f4-168">`id` jest używany do mapowania na jednostkę modelu.</span><span class="sxs-lookup"><span data-stu-id="670f4-168">`id` is used to map to a model entity.</span></span>

<span data-ttu-id="670f4-169">Korzystając z tej `default` trasy, ścieżka URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-169">Using this `default` route, the URL path:</span></span>

* <span data-ttu-id="670f4-170">`/Products/List` mapuje na akcję `ProductsController.List`.</span><span class="sxs-lookup"><span data-stu-id="670f4-170">`/Products/List` maps to the `ProductsController.List` action.</span></span>
* <span data-ttu-id="670f4-171">`/Blog/Article/17` Maps do `BlogController.Article` i zazwyczaj model wiąże `id` z 17.</span><span class="sxs-lookup"><span data-stu-id="670f4-171">`/Blog/Article/17` maps to `BlogController.Article` and typically model binds the `id` parameter to 17.</span></span>

<span data-ttu-id="670f4-172">To mapowanie:</span><span class="sxs-lookup"><span data-stu-id="670f4-172">This mapping:</span></span>

* <span data-ttu-id="670f4-173">Jest oparty **tylko**na nazwach kontrolera i [akcji](#action) .</span><span class="sxs-lookup"><span data-stu-id="670f4-173">Is based on the controller and [action](#action) names **only**.</span></span>
* <span data-ttu-id="670f4-174">Nie jest oparty na przestrzeniach nazw, lokalizacjach plików źródłowych ani parametrach metod.</span><span class="sxs-lookup"><span data-stu-id="670f4-174">Isn't based on namespaces, source file locations, or method parameters.</span></span>

<span data-ttu-id="670f4-175">Użycie konwencjonalnego routingu z domyślną trasą umożliwia tworzenie aplikacji bez konieczności korzystania z nowego wzorca adresu URL dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-175">Using conventional routing with the default route allows creating the app without having to come up with a new URL pattern for each action.</span></span> <span data-ttu-id="670f4-176">W przypadku aplikacji z akcjami w stylu [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) mających spójność dla adresów URL między kontrolerami:</span><span class="sxs-lookup"><span data-stu-id="670f4-176">For an app with [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) style actions, having consistency for the URLs across controllers:</span></span>

* <span data-ttu-id="670f4-177">Upraszcza kod.</span><span class="sxs-lookup"><span data-stu-id="670f4-177">Helps simplify the code.</span></span>
* <span data-ttu-id="670f4-178">Sprawia, że interfejs użytkownika jest bardziej przewidywalny.</span><span class="sxs-lookup"><span data-stu-id="670f4-178">Makes the UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="670f4-179">`id` w poprzednim kodzie jest zdefiniowany jako opcjonalny przez szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-179">The `id` in the preceding code is defined as optional by the route template.</span></span> <span data-ttu-id="670f4-180">Akcje można wykonać bez opcjonalnego identyfikatora podanego w ramach adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-180">Actions can execute without the optional ID provided as part of the URL.</span></span> <span data-ttu-id="670f4-181">Ogólnie rzecz biorąc, gdy`id` zostanie pominięty z adresu URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-181">Generally, when`id` is omitted from the URL:</span></span>
>
> * <span data-ttu-id="670f4-182">`id` jest ustawiony na `0` przez powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="670f4-182">`id` is set to `0` by model binding.</span></span>
> * <span data-ttu-id="670f4-183">Nie znaleziono jednostki w `id == 0`zgodnej z bazami danych.</span><span class="sxs-lookup"><span data-stu-id="670f4-183">No entity is found in the database matching `id == 0`.</span></span>
>
> <span data-ttu-id="670f4-184">[Routing atrybutu](#ar) zawiera szczegółowy formant, który umożliwia określanie identyfikatora wymaganego dla niektórych akcji, a nie dla innych.</span><span class="sxs-lookup"><span data-stu-id="670f4-184">[Attribute routing](#ar) provides fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="670f4-185">Zgodnie z Konwencją, dokumentacja zawiera parametry opcjonalne, takie jak `id`, gdy prawdopodobnie będą widoczne w prawidłowym użyciu.</span><span class="sxs-lookup"><span data-stu-id="670f4-185">By convention, the documentation includes optional parameters like `id` when they're likely to appear in correct usage.</span></span>

<span data-ttu-id="670f4-186">Większość aplikacji powinna wybrać podstawowy i opisowy schemat routingu, aby adresy URL były czytelne i zrozumiałe.</span><span class="sxs-lookup"><span data-stu-id="670f4-186">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="670f4-187">Domyślna trasa konwencjonalna `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="670f4-187">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="670f4-188">Obsługuje podstawowy i opisowy schemat routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-188">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="670f4-189">Jest użytecznym punktem wyjścia dla aplikacji opartych na interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="670f4-189">Is a useful starting point for UI-based apps.</span></span>
* <span data-ttu-id="670f4-190">Jest jedynym szablonem trasy wymaganym przez wiele aplikacji interfejsu użytkownika sieci Web.</span><span class="sxs-lookup"><span data-stu-id="670f4-190">Is the only route template needed for many web UI apps.</span></span> <span data-ttu-id="670f4-191">W przypadku większych aplikacji interfejsu użytkownika sieci Web inna trasa korzysta z [obszarów](#areas) , jeśli są one często używane.</span><span class="sxs-lookup"><span data-stu-id="670f4-191">For larger web UI apps, another route using [Areas](#areas) if frequently all that's needed.</span></span>

<span data-ttu-id="670f4-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> i <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:</span><span class="sxs-lookup"><span data-stu-id="670f4-192"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> :</span></span>

* <span data-ttu-id="670f4-193">Automatycznie Przypisz wartość **zamówienia** do punktów końcowych na podstawie kolejności, w której są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="670f4-193">Automatically assign an **order** value to their endpoints based on the order they are invoked.</span></span>

<span data-ttu-id="670f4-194">Routing punktów końcowych w ASP.NET Core 3,0 i nowszych:</span><span class="sxs-lookup"><span data-stu-id="670f4-194">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>

* <span data-ttu-id="670f4-195">Nie ma koncepcji tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-195">Doesn't have a concept of routes.</span></span>
* <span data-ttu-id="670f4-196">Nie oferuje gwarancji porządkowania dla wykonywania rozszerzalności, wszystkie punkty końcowe są przetwarzane jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="670f4-196">Doesn't provide ordering guarantees for the execution of extensibility,  all endpoints are processed at once.</span></span>

<span data-ttu-id="670f4-197">Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby zobaczyć, jak wbudowane implementacje routingu, takie jak <xref:Microsoft.AspNetCore.Routing.Route>, pasują do żądań.</span><span class="sxs-lookup"><span data-stu-id="670f4-197">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

<span data-ttu-id="670f4-198">[Routing atrybutów](#ar) został wyjaśniony w dalszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="670f4-198">[Attribute routing](#ar) is explained later in this document.</span></span>

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a><span data-ttu-id="670f4-199">Wiele konwencjonalnych tras</span><span class="sxs-lookup"><span data-stu-id="670f4-199">Multiple conventional routes</span></span>

<span data-ttu-id="670f4-200">W `UseEndpoints` można dodać wiele [konwencjonalnych tras](#cr) , dodając więcej wywołań do <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> i <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span><span class="sxs-lookup"><span data-stu-id="670f4-200">Multiple [conventional routes](#cr) can be added inside `UseEndpoints` by adding more calls to <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>.</span></span> <span data-ttu-id="670f4-201">Dzięki temu można definiować wiele Konwencji lub dodać do nich trasy konwencjonalne, które są przeznaczone dla konkretnej [akcji](#action), na przykład:</span><span class="sxs-lookup"><span data-stu-id="670f4-201">Doing so allows defining multiple conventions, or to adding conventional routes that are dedicated to a specific [action](#action), such as:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

<span data-ttu-id="670f4-202">Trasa `blog` w powyższym kodzie jest **dedykowaną konwencjonalną trasą**.</span><span class="sxs-lookup"><span data-stu-id="670f4-202">The `blog` route in the preceding code is a **dedicated conventional route**.</span></span> <span data-ttu-id="670f4-203">Jest on nazywany dedykowaną trasą konwencjonalną, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="670f4-203">It's called a dedicated conventional route because:</span></span>

* <span data-ttu-id="670f4-204">Używa ona [konwencjonalnego routingu](#cr).</span><span class="sxs-lookup"><span data-stu-id="670f4-204">It uses [conventional routing](#cr).</span></span>
* <span data-ttu-id="670f4-205">Jest on przeznaczony dla konkretnej [akcji](#action).</span><span class="sxs-lookup"><span data-stu-id="670f4-205">It's dedicated to a specific [action](#action).</span></span>

<span data-ttu-id="670f4-206">Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy `"blog/{*article}"` jako parametry:</span><span class="sxs-lookup"><span data-stu-id="670f4-206">Because `controller` and `action` don't appear in the route template `"blog/{*article}"` as parameters:</span></span>

* <span data-ttu-id="670f4-207">Mogą mieć tylko wartości domyślne `{ controller = "Blog", action = "Article" }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-207">They can only have the default values `{ controller = "Blog", action = "Article" }`.</span></span>
* <span data-ttu-id="670f4-208">Ta trasa zawsze jest mapowana na `BlogController.Article`akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-208">This route always maps to the action `BlogController.Article`.</span></span>

<span data-ttu-id="670f4-209">`/Blog`, `/Blog/Article`i `/Blog/{any-string}` są jedynymi ścieżkami URL, które pasują do trasy blogu.</span><span class="sxs-lookup"><span data-stu-id="670f4-209">`/Blog`, `/Blog/Article`, and `/Blog/{any-string}` are the only URL paths that match the blog route.</span></span>

<span data-ttu-id="670f4-210">Powyższy przykład:</span><span class="sxs-lookup"><span data-stu-id="670f4-210">The preceding example:</span></span>

* <span data-ttu-id="670f4-211">Trasa `blog` ma wyższy priorytet dla dopasowania niż trasy `default`, ponieważ jest dodawana jako pierwsza.</span><span class="sxs-lookup"><span data-stu-id="670f4-211">`blog` route has a higher priority for matches than the `default` route because it is added first.</span></span>
* <span data-ttu-id="670f4-212">Jest i przykładem routingu stylów [informacji](https://developer.mozilla.org/docs/Glossary/Slug) o sobie, w którym typowym jest nazwa artykułu w ramach adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-212">Is and example of [Slug](https://developer.mozilla.org/docs/Glossary/Slug) style routing where it's typical to have an article name as part of the URL.</span></span>

> [!WARNING]
> <span data-ttu-id="670f4-213">W ASP.NET Core 3,0 i nowszych routingu nie są:</span><span class="sxs-lookup"><span data-stu-id="670f4-213">In ASP.NET Core 3.0 and later, routing doesn't:</span></span>
> * <span data-ttu-id="670f4-214">Zdefiniuj koncepcję o nazwie *trasa*.</span><span class="sxs-lookup"><span data-stu-id="670f4-214">Define a concept called a *route*.</span></span> <span data-ttu-id="670f4-215">`UseRouting` dodaje dopasowanie trasy do potoku programu pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="670f4-215">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="670f4-216">Oprogramowanie pośredniczące `UseRouting` sprawdza zestaw punktów końcowych zdefiniowanych w aplikacji i wybiera najlepsze dopasowanie punktu końcowego na podstawie żądania.</span><span class="sxs-lookup"><span data-stu-id="670f4-216">The `UseRouting` middleware looks at the set of endpoints defined in the app, and selects the best endpoint match based on the request.</span></span>
> * <span data-ttu-id="670f4-217">Podaj gwarancje dotyczące kolejności wykonywania rozszerzalności, takich jak <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> lub <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span><span class="sxs-lookup"><span data-stu-id="670f4-217">Provide guarantees about the execution order of extensibility like <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> or <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span></span>
>
><span data-ttu-id="670f4-218">Zobacz [Routing](xref:fundamentals/routing) dla materiałów referencyjnych na trasie.</span><span class="sxs-lookup"><span data-stu-id="670f4-218">See [Routing](xref:fundamentals/routing) for reference material on routing.</span></span>

<a name="cro"></a>

### <a name="conventional-routing-order"></a><span data-ttu-id="670f4-219">Tradycyjna kolejność routingu</span><span class="sxs-lookup"><span data-stu-id="670f4-219">Conventional routing order</span></span>

<span data-ttu-id="670f4-220">Routing konwencjonalny pasuje do kombinacji akcji i kontrolera, które są zdefiniowane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="670f4-220">Conventional routing only matches a combination of action and controller that are defined by the app.</span></span> <span data-ttu-id="670f4-221">Jest to przeznaczone do uproszczenia sytuacji, w których trasy konwencjonalne nakładają się na siebie.</span><span class="sxs-lookup"><span data-stu-id="670f4-221">This is intended to simplify cases where conventional routes overlap.</span></span>
<span data-ttu-id="670f4-222">Dodanie tras przy użyciu <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>i <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> powoduje automatyczne przypisanie wartości zamówienia do punktów końcowych na podstawie kolejności, w której są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="670f4-222">Adding routes using <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>, and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="670f4-223">Dopasowania z trasy, która pojawiła się wcześniej, mają wyższy priorytet.</span><span class="sxs-lookup"><span data-stu-id="670f4-223">Matches from a route that appears earlier have a higher priority.</span></span> <span data-ttu-id="670f4-224">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-224">Conventional routing is order-dependent.</span></span> <span data-ttu-id="670f4-225">Ogólnie rzecz biorąc, trasy z obszarami powinny być umieszczone wcześniej, ponieważ są bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="670f4-225">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span> <span data-ttu-id="670f4-226">[Dedykowane konwencjonalne trasy](#dcr) z funkcją przechwytywania wszystkie parametry tras, takie jak `{*article}`, mogą spowodować zbyt [zachłanne](xref:fundamentals/routing#greedy)trasy, co oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-226">[Dedicated conventional routes](#dcr) with catch all route parameters like `{*article}` can make a route too [greedy](xref:fundamentals/routing#greedy), meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="670f4-227">Umieść trasy zachłanne w dalszej części tabeli tras, aby zapobiec dopasowania zachłanne.</span><span class="sxs-lookup"><span data-stu-id="670f4-227">Put the greedy routes later in the route table to prevent greedy matches.</span></span>

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a><span data-ttu-id="670f4-228">Rozwiązywanie niejednoznacznych akcji</span><span class="sxs-lookup"><span data-stu-id="670f4-228">Resolving ambiguous actions</span></span>

<span data-ttu-id="670f4-229">Gdy dwa punkty końcowe pasują do routingu, routing musi wykonać jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="670f4-229">When two endpoints match through routing, routing must do one of the following:</span></span>

* <span data-ttu-id="670f4-230">Wybierz najlepszego kandydata.</span><span class="sxs-lookup"><span data-stu-id="670f4-230">Choose the best candidate.</span></span>
* <span data-ttu-id="670f4-231">Zgłoś wyjątek.</span><span class="sxs-lookup"><span data-stu-id="670f4-231">Throw an exception.</span></span>

<span data-ttu-id="670f4-232">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="670f4-232">For example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

<span data-ttu-id="670f4-233">Poprzedni kontroler definiuje dwie akcje, które są zgodne:</span><span class="sxs-lookup"><span data-stu-id="670f4-233">The preceding controller defines two actions that match:</span></span>

* <span data-ttu-id="670f4-234">Ścieżka adresu URL `/Products33/Edit/17`</span><span class="sxs-lookup"><span data-stu-id="670f4-234">The URL path `/Products33/Edit/17`</span></span>
* <span data-ttu-id="670f4-235">Kierowanie `{ controller = Products33, action = Edit, id = 17 }`danych.</span><span class="sxs-lookup"><span data-stu-id="670f4-235">Route data `{ controller = Products33, action = Edit, id = 17 }`.</span></span>

<span data-ttu-id="670f4-236">Jest to typowy wzorzec dla kontrolerów MVC:</span><span class="sxs-lookup"><span data-stu-id="670f4-236">This is a typical pattern for MVC controllers:</span></span>

* <span data-ttu-id="670f4-237">`Edit(int)` wyświetla formularz służący do edytowania produktu.</span><span class="sxs-lookup"><span data-stu-id="670f4-237">`Edit(int)` displays a form to edit a product.</span></span>
* <span data-ttu-id="670f4-238">`Edit(int, Product)` przetwarza opublikowany formularz.</span><span class="sxs-lookup"><span data-stu-id="670f4-238">`Edit(int, Product)` processes  the posted form.</span></span>

<span data-ttu-id="670f4-239">Aby rozwiązać poprawność trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-239">To resolve the correct route:</span></span>

* <span data-ttu-id="670f4-240">`Edit(int, Product)` jest zaznaczone, gdy żądanie jest `POST`HTTP.</span><span class="sxs-lookup"><span data-stu-id="670f4-240">`Edit(int, Product)` is selected when the request is an HTTP `POST`.</span></span>
* <span data-ttu-id="670f4-241">`Edit(int)` jest wybierany, gdy [czasownik http](#verb) to coś innego.</span><span class="sxs-lookup"><span data-stu-id="670f4-241">`Edit(int)` is selected when the [HTTP verb](#verb) is anything else.</span></span> <span data-ttu-id="670f4-242">`Edit(int)` jest zazwyczaj wywoływany przez `GET`.</span><span class="sxs-lookup"><span data-stu-id="670f4-242">`Edit(int)` is generally called via `GET`.</span></span>

<span data-ttu-id="670f4-243"><xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, jest dostarczany do routingu, aby można było wybrać oparty na metodzie HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="670f4-243">The <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, is provided to routing so that it can choose based on the HTTP method of the request.</span></span> <span data-ttu-id="670f4-244">`HttpPostAttribute` zapewnia `Edit(int, Product)` lepszym dopasowaniem niż `Edit(int)`.</span><span class="sxs-lookup"><span data-stu-id="670f4-244">The `HttpPostAttribute` makes `Edit(int, Product)` a better match than `Edit(int)`.</span></span>

<span data-ttu-id="670f4-245">Ważne jest, aby zrozumieć rolę atrybutów, takich jak `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="670f4-245">It's important to understand the role of attributes like `HttpPostAttribute`.</span></span> <span data-ttu-id="670f4-246">Podobne atrybuty są zdefiniowane dla innych [czasowników HTTP](#verb).</span><span class="sxs-lookup"><span data-stu-id="670f4-246">Similar attributes are defined for other [HTTP verbs](#verb).</span></span> <span data-ttu-id="670f4-247">W przypadku [routingu konwencjonalnego](#cr)typowe dla akcji używanie tej samej nazwy akcji, gdy są one częścią formularza Pokaż, przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="670f4-247">In [conventional routing](#cr), it's common for actions to use the same action name when they're part of a show form, submit form workflow.</span></span> <span data-ttu-id="670f4-248">Na przykład zapoznaj [się z tematem badanie dwóch metod akcji edycji](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span><span class="sxs-lookup"><span data-stu-id="670f4-248">For example, see [Examine the two Edit action methods](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span></span>

<span data-ttu-id="670f4-249">Jeśli nie można wybrać najlepszego kandydata, zostanie zgłoszony <xref:System.Reflection.AmbiguousMatchException>, zawierający wiele pasujących punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="670f4-249">If routing can't choose a best candidate, an <xref:System.Reflection.AmbiguousMatchException> is thrown, listing the multiple matched endpoints.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a><span data-ttu-id="670f4-250">Nazwy tras konwencjonalnych</span><span class="sxs-lookup"><span data-stu-id="670f4-250">Conventional route names</span></span>

<span data-ttu-id="670f4-251">Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwami konwencjonalnych tras:</span><span class="sxs-lookup"><span data-stu-id="670f4-251">The strings  `"blog"` and `"default"` in the following examples are conventional route names:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="670f4-252">Nazwy tras nadają trasie nazwę logiczną.</span><span class="sxs-lookup"><span data-stu-id="670f4-252">The route names give the route a logical name.</span></span> <span data-ttu-id="670f4-253">Nazwana trasa może służyć do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-253">The named route can be used for URL generation.</span></span> <span data-ttu-id="670f4-254">Użycie nazwanej trasy upraszcza tworzenie adresów URL, gdy porządkowanie tras może spowodować skomplikowane generowanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-254">Using a named route simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="670f4-255">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-255">Route names must be unique application wide.</span></span>

<span data-ttu-id="670f4-256">Nazwy tras:</span><span class="sxs-lookup"><span data-stu-id="670f4-256">Route names:</span></span>

* <span data-ttu-id="670f4-257">Nie mają wpływu na pasujące adresy URL ani obsługę żądań.</span><span class="sxs-lookup"><span data-stu-id="670f4-257">Have no impact on URL matching or handling of requests.</span></span>
* <span data-ttu-id="670f4-258">Są używane tylko do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-258">Are used only for URL generation.</span></span>

<span data-ttu-id="670f4-259">Koncepcja nazwy trasy jest reprezentowana w obszarze Routing jako [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span><span class="sxs-lookup"><span data-stu-id="670f4-259">The route name concept is represented in routing as [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span></span> <span data-ttu-id="670f4-260">**Nazwa trasy** i nazwa **punktu końcowego**:</span><span class="sxs-lookup"><span data-stu-id="670f4-260">The terms **route name** and **endpoint name**:</span></span>

* <span data-ttu-id="670f4-261">Są zamienne.</span><span class="sxs-lookup"><span data-stu-id="670f4-261">Are interchangeable.</span></span>
* <span data-ttu-id="670f4-262">Który jest używany w dokumentacji i kodu zależy od opisanego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="670f4-262">Which one is used in documentation and code depends on the API being described.</span></span>

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a><span data-ttu-id="670f4-263">Routing atrybutów dla interfejsów API REST</span><span class="sxs-lookup"><span data-stu-id="670f4-263">Attribute routing for REST APIs</span></span>

<span data-ttu-id="670f4-264">Interfejsy API REST powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez [zlecenia http](#verb).</span><span class="sxs-lookup"><span data-stu-id="670f4-264">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by [HTTP verbs](#verb).</span></span>

<span data-ttu-id="670f4-265">Funkcja routingu atrybutów używa zestawu atrybutów do mapowania akcji bezpośrednio do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-265">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="670f4-266">Poniższy kod `StartUp.Configure` jest typowy dla interfejsu API REST i jest używany w następnym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-266">The following `StartUp.Configure` code is typical for a REST API and is used in the next sample:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

<span data-ttu-id="670f4-267">W poprzednim kodzie <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> jest wywoływana wewnątrz `UseEndpoints` w celu mapowania kontrolerów z routingiem atrybutu.</span><span class="sxs-lookup"><span data-stu-id="670f4-267">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> is called inside `UseEndpoints` to map attribute routed controllers.</span></span>

<span data-ttu-id="670f4-268">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-268">In the following example:</span></span>

* <span data-ttu-id="670f4-269">Poprzednia metoda `Configure` jest używana.</span><span class="sxs-lookup"><span data-stu-id="670f4-269">The preceding `Configure` method is used.</span></span>
* <span data-ttu-id="670f4-270">`HomeController` dopasowuje zestaw adresów URL podobny do tego, co jest zgodne z domyślną trasą konwencjonalny `{controller=Home}/{action=Index}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="670f4-270">`HomeController` matches a set of URLs similar to what the default conventional route `{controller=Home}/{action=Index}/{id?}` matches.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

<span data-ttu-id="670f4-271">Akcja `HomeController.Index` jest uruchamiana dla dowolnej ścieżki URL `/`, `/Home`, `/Home/Index`lub `/Home/Index/3`.</span><span class="sxs-lookup"><span data-stu-id="670f4-271">The `HomeController.Index` action is run for any of the URL paths `/`, `/Home`, `/Home/Index`, or `/Home/Index/3`.</span></span>

<span data-ttu-id="670f4-272">W tym przykładzie przedstawiono najważniejsze różnice programistyczne między routingiem atrybutów i [routingiem konwencjonalnym](#cr).</span><span class="sxs-lookup"><span data-stu-id="670f4-272">This example highlights a key programming difference between attribute routing and [conventional routing](#cr).</span></span> <span data-ttu-id="670f4-273">Routing atrybutów wymaga więcej danych wejściowych w celu określenia trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-273">Attribute routing requires more input to specify a route.</span></span> <span data-ttu-id="670f4-274">Konwencjonalne trasy domyślne obsługuje trasy bardziej zwięzłie.</span><span class="sxs-lookup"><span data-stu-id="670f4-274">The conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="670f4-275">Jednak Routing atrybutu zezwala na ścisłą kontrolę nad tym, które szablony tras mają zastosowanie do poszczególnych [akcji](#action).</span><span class="sxs-lookup"><span data-stu-id="670f4-275">However, attribute routing allows and requires precise control of which route templates apply to each [action](#action).</span></span>

<span data-ttu-id="670f4-276">W przypadku routingu atrybutów nazwa kontrolera i nazwy akcji nie odgrywają **żadnej** roli, w której akcja jest dopasowywana.</span><span class="sxs-lookup"><span data-stu-id="670f4-276">With attribute routing, the controller name and action names play **no** role in which action is matched.</span></span> <span data-ttu-id="670f4-277">Poniższy przykład dopasowuje te same adresy URL, co w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-277">The following example matches the same URLs as the previous example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="670f4-278">Następujący kod używa wymiany tokenów dla `action` i `controller`:</span><span class="sxs-lookup"><span data-stu-id="670f4-278">The following code uses token replacement for `action` and `controller`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

<span data-ttu-id="670f4-279">Następujący kod ma zastosowanie `[Route("[controller]/[action]")]` do kontrolera:</span><span class="sxs-lookup"><span data-stu-id="670f4-279">The following code applies `[Route("[controller]/[action]")]` to the controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="670f4-280">W poprzednim kodzie szablony metod `Index` muszą być poprzedzone `/` lub `~/` do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-280">In the preceding code, the `Index` method templates must prepend `/` or `~/` to the route templates.</span></span> <span data-ttu-id="670f4-281">Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są łączone z szablonami tras zastosowanymi do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-281">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span>

<span data-ttu-id="670f4-282">Zobacz [pierwszeństwo szablonu trasy](xref:fundamentals/routing#rtp) , aby uzyskać informacje o wyborze szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-282">See [Route template precedence](xref:fundamentals/routing#rtp) for information on route template selection.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="670f4-283">Zastrzeżone nazwy routingu</span><span class="sxs-lookup"><span data-stu-id="670f4-283">Reserved routing names</span></span>

<span data-ttu-id="670f4-284">Następujące słowa kluczowe są zastrzeżonymi nazwami parametrów trasy podczas korzystania z kontrolerów lub Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="670f4-284">The following keywords are reserved route parameter names when using Controllers or Razor Pages:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

<span data-ttu-id="670f4-285">Użycie `page` jako parametru trasy z routingiem atrybutu jest typowym błędem.</span><span class="sxs-lookup"><span data-stu-id="670f4-285">Using `page` as a route parameter with attribute routing is a common error.</span></span> <span data-ttu-id="670f4-286">Powoduje to niespójne i mylące zachowanie przy generowaniu adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-286">Doing that results in inconsistent and confusing behavior with URL generation.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

<span data-ttu-id="670f4-287">Specjalne nazwy parametrów są używane przez generowanie adresów URL w celu ustalenia, czy operacja generowania adresu URL odwołuje się do strony Razor lub do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-287">The special parameter names are used by the URL generation to determine if a URL generation operation refers to a Razor Page or to a Controller.</span></span>

<a name="verb"></a>

## <a name="http-verb-templates"></a><span data-ttu-id="670f4-288">Szablony czasowników HTTP</span><span class="sxs-lookup"><span data-stu-id="670f4-288">HTTP verb templates</span></span>

<span data-ttu-id="670f4-289">ASP.NET Core ma następujące szablony czasowników HTTP:</span><span class="sxs-lookup"><span data-stu-id="670f4-289">ASP.NET Core has the following HTTP verb templates:</span></span>

* <span data-ttu-id="670f4-290">[Narzędzia HttpGet](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span><span class="sxs-lookup"><span data-stu-id="670f4-290">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span></span>
* <span data-ttu-id="670f4-291">[HTTPPOST](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span><span class="sxs-lookup"><span data-stu-id="670f4-291">[[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span></span>
* <span data-ttu-id="670f4-292">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span><span class="sxs-lookup"><span data-stu-id="670f4-292">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span></span>
* <span data-ttu-id="670f4-293">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span><span class="sxs-lookup"><span data-stu-id="670f4-293">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span></span>
* <span data-ttu-id="670f4-294">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span><span class="sxs-lookup"><span data-stu-id="670f4-294">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span></span>
* <span data-ttu-id="670f4-295">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span><span class="sxs-lookup"><span data-stu-id="670f4-295">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span></span>

<a name="rt"></a>

### <a name="route-templates"></a><span data-ttu-id="670f4-296">Szablony tras</span><span class="sxs-lookup"><span data-stu-id="670f4-296">Route templates</span></span>

<span data-ttu-id="670f4-297">ASP.NET Core ma następujące szablony tras:</span><span class="sxs-lookup"><span data-stu-id="670f4-297">ASP.NET Core has the following route templates:</span></span>

* <span data-ttu-id="670f4-298">Wszystkie [Szablony czasowników HTTP](#verb) są szablonami tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-298">All the [HTTP verb templates](#verb) are route templates.</span></span>
* <span data-ttu-id="670f4-299">[Szlak](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="670f4-299">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span></span>

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a><span data-ttu-id="670f4-300">Routing atrybutów z atrybutami czasownik http</span><span class="sxs-lookup"><span data-stu-id="670f4-300">Attribute routing with Http verb attributes</span></span>

<span data-ttu-id="670f4-301">Rozważmy następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="670f4-301">Consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="670f4-302">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-302">In the preceding code:</span></span>

* <span data-ttu-id="670f4-303">Każda akcja zawiera atrybut `[HttpGet]`, który ogranicza dopasowywanie do żądań HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="670f4-303">Each action contains the `[HttpGet]` attribute, which constrains matching to HTTP GET requests only.</span></span>
* <span data-ttu-id="670f4-304">Akcja `GetProduct` obejmuje szablon `"{id}"`, dlatego `id` jest dołączany do szablonu `"api/[controller]"` na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="670f4-304">The `GetProduct` action includes the `"{id}"` template, therefore `id` is appended to the `"api/[controller]"` template on the controller.</span></span> <span data-ttu-id="670f4-305">Szablon metod jest `"api/[controller]/"{id}""`.</span><span class="sxs-lookup"><span data-stu-id="670f4-305">The methods template is `"api/[controller]/"{id}""`.</span></span> <span data-ttu-id="670f4-306">W związku z tym ta akcja dopasowuje tylko żądania GET dla formularza `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`itd.</span><span class="sxs-lookup"><span data-stu-id="670f4-306">Therefore this action only matches GET requests of for the form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`, etc.</span></span>
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* <span data-ttu-id="670f4-307">Akcja `GetIntProduct` zawiera szablon `"int/{id:int}")`.</span><span class="sxs-lookup"><span data-stu-id="670f4-307">The `GetIntProduct` action contains the `"int/{id:int}")` template.</span></span> <span data-ttu-id="670f4-308">Część `:int` szablonu ogranicza wartości trasy `id` do ciągów, które mogą być konwertowane na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="670f4-308">The `:int` portion of the template constrains the `id` route values to strings that can be converted to an integer.</span></span> <span data-ttu-id="670f4-309">Żądanie GET do `/api/test2/int/abc`:</span><span class="sxs-lookup"><span data-stu-id="670f4-309">A GET request to `/api/test2/int/abc`:</span></span>
  * <span data-ttu-id="670f4-310">Nie jest zgodna z tą akcją.</span><span class="sxs-lookup"><span data-stu-id="670f4-310">Doesn't match this action.</span></span>
  * <span data-ttu-id="670f4-311">Zwraca błąd [nie znaleziono 404](https://developer.mozilla.org/docs/Web/HTTP/Status/404) .</span><span class="sxs-lookup"><span data-stu-id="670f4-311">Returns a [404 Not Found](https://developer.mozilla.org/docs/Web/HTTP/Status/404) error.</span></span>
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* <span data-ttu-id="670f4-312">Akcja `GetInt2Product` zawiera `{id}` w szablonie, ale nie ogranicza `id` do wartości, które można przekonwertować na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="670f4-312">The `GetInt2Product` action contains `{id}` in the template, but doesn't constrain `id` to values that can be converted to an integer.</span></span> <span data-ttu-id="670f4-313">Żądanie GET do `/api/test2/int2/abc`:</span><span class="sxs-lookup"><span data-stu-id="670f4-313">A GET request to `/api/test2/int2/abc`:</span></span>
  * <span data-ttu-id="670f4-314">Pasuje do tej trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-314">Matches this route.</span></span>
  * <span data-ttu-id="670f4-315">Nie można skonwertować powiązania modelu `abc` na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="670f4-315">Model binding fails to convert `abc` to an integer.</span></span> <span data-ttu-id="670f4-316">`id` parametr metody jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="670f4-316">The `id` parameter of the method is integer.</span></span>
  * <span data-ttu-id="670f4-317">Zwraca [400 Nieprawidłowe żądanie](https://developer.mozilla.org/docs/Web/HTTP/Status/400) , ponieważ nie powiodło się przekonwertowanie powiązania modelu`abc` na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="670f4-317">Returns a [400 Bad Request](https://developer.mozilla.org/docs/Web/HTTP/Status/400) because model binding failed to convert`abc` to an integer.</span></span>
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

<span data-ttu-id="670f4-318">Funkcja routingu atrybutów może używać atrybutów <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute>, takich jak <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>i <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span><span class="sxs-lookup"><span data-stu-id="670f4-318">Attribute routing can use <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> attributes such as <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>, and <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span></span> <span data-ttu-id="670f4-319">Wszystkie atrybuty [czasowników HTTP](#verb) akceptują szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-319">All of the [HTTP verb](#verb) attributes accept a route template.</span></span> <span data-ttu-id="670f4-320">Poniższy przykład przedstawia dwie akcje, które pasują do tego samego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-320">The following example shows two actions that match the same route template:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

<span data-ttu-id="670f4-321">Używanie `/products3`ścieżki URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-321">Using the URL path `/products3`:</span></span>

* <span data-ttu-id="670f4-322">Akcja `MyProductsController.ListProducts` jest uruchamiana, gdy [zlecenie http](#verb) jest `GET`.</span><span class="sxs-lookup"><span data-stu-id="670f4-322">The `MyProductsController.ListProducts` action runs when the [HTTP verb](#verb) is `GET`.</span></span>
* <span data-ttu-id="670f4-323">Akcja `MyProductsController.CreateProduct` jest uruchamiana, gdy [zlecenie http](#verb) jest `POST`.</span><span class="sxs-lookup"><span data-stu-id="670f4-323">The `MyProductsController.CreateProduct` action runs when the [HTTP verb](#verb) is `POST`.</span></span>

<span data-ttu-id="670f4-324">Podczas kompilowania interfejsu API REST jest to rzadko konieczne, aby użyć `[Route(...)]` na metodę akcji, ponieważ akcja akceptuje wszystkie metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="670f4-324">When building a REST API, it's rare that you'll need to use `[Route(...)]` on an action method because the action accepts all HTTP methods.</span></span> <span data-ttu-id="670f4-325">Lepiej jest używać bardziej szczegółowego [atrybutu zlecenia http](#verb) , aby precyzyjnie dowiedzieć się, co obsługuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="670f4-325">It's better to use the more specific [HTTP verb attribute](#verb) to be precise about what your API supports.</span></span> <span data-ttu-id="670f4-326">Klienci interfejsów API REST powinni wiedzieć, jakie ścieżki i czasowniki HTTP mapują na określone operacje logiczne.</span><span class="sxs-lookup"><span data-stu-id="670f4-326">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="670f4-327">Interfejsy API REST powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="670f4-327">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="670f4-328">Oznacza to, że wiele operacji, na przykład Pobierz i Opublikuj dla tego samego zasobu logicznego, użyj tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-328">This means that many operations, for example, GET and POST on the same logical resource use the same URL.</span></span> <span data-ttu-id="670f4-329">Routing atrybutu zapewnia poziom kontroli, który jest wymagany do dokładnego projektowania układu publicznego punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="670f4-329">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="670f4-330">Ponieważ atrybut Route ma zastosowanie do określonej akcji, można łatwo wprowadzić parametry wymagane jako część definicji szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-330">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="670f4-331">W poniższym przykładzie `id` jest wymagane jako część ścieżki URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-331">In the following example, `id` is required as part of the URL path:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="670f4-332">Akcja `Products2ApiController.GetProduct(int)`:</span><span class="sxs-lookup"><span data-stu-id="670f4-332">The `Products2ApiController.GetProduct(int)` action:</span></span>

* <span data-ttu-id="670f4-333">Jest uruchamiany z ścieżką URL, taką jak `/products2/3`</span><span class="sxs-lookup"><span data-stu-id="670f4-333">Is run with URL path like `/products2/3`</span></span>
* <span data-ttu-id="670f4-334">Nie jest uruchamiany z ścieżką URL `/products2`.</span><span class="sxs-lookup"><span data-stu-id="670f4-334">Isn't run with the URL path `/products2`.</span></span>

<span data-ttu-id="670f4-335">Atrybut [[](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) Requests] umożliwia akcja ograniczenia obsługiwanych typów zawartości żądania.</span><span class="sxs-lookup"><span data-stu-id="670f4-335">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="670f4-336">Aby uzyskać więcej informacji, zobacz [Definiowanie obsługiwanych typów zawartości żądania przy użyciu atrybutu użycia](xref:web-api/index#consumes).</span><span class="sxs-lookup"><span data-stu-id="670f4-336">For more information, see [Define supported request content types with the Consumes attribute](xref:web-api/index#consumes).</span></span>

 <span data-ttu-id="670f4-337">Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="670f4-337">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

<span data-ttu-id="670f4-338">Aby uzyskać więcej informacji na `[ApiController]`, zobacz [atrybut ApiController](xref:web-api/index##apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="670f4-338">For more information on `[ApiController]`, see [ApiController attribute](xref:web-api/index##apicontroller-attribute).</span></span>

## <a name="route-name"></a><span data-ttu-id="670f4-339">Nazwa trasy</span><span class="sxs-lookup"><span data-stu-id="670f4-339">Route name</span></span>

<span data-ttu-id="670f4-340">Poniższy kod definiuje nazwę trasy `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="670f4-340">The following code  defines a route name of `Products_List`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="670f4-341">Nazwy tras mogą służyć do generowania adresów URL na podstawie określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-341">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="670f4-342">Nazwy tras:</span><span class="sxs-lookup"><span data-stu-id="670f4-342">Route names:</span></span>

* <span data-ttu-id="670f4-343">Nie ma wpływu na zachowanie w zakresie routingu zgodnego z adresem URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-343">Have no impact on the URL matching behavior of routing.</span></span>
* <span data-ttu-id="670f4-344">Są używane tylko na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-344">Are only used for URL generation.</span></span>

<span data-ttu-id="670f4-345">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-345">Route names must be unique application-wide.</span></span>

<span data-ttu-id="670f4-346">Należy odróżnić poprzedni kod od konwencjonalnej trasy domyślnej, która definiuje `id` parametr jako opcjonalny (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="670f4-346">Contrast the preceding code with the conventional default route, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="670f4-347">Możliwość precyzyjnego określania interfejsów API ma zalety, takich jak umożliwienie `/products` i `/products/5` do wysłania do różnych akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-347">The ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a><span data-ttu-id="670f4-348">Łączenie tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="670f4-348">Combining attribute routes</span></span>

<span data-ttu-id="670f4-349">Aby mniej powtarzać Routing atrybutów, atrybuty trasy na kontrolerze są łączone z atrybutami trasy dla poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-349">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="670f4-350">Wszystkie szablony tras zdefiniowane na kontrolerze są poprzedzone w celu rozesłania szablonów w akcjach.</span><span class="sxs-lookup"><span data-stu-id="670f4-350">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="670f4-351">Umieszczenie atrybutu trasy na kontrolerze powoduje, że **wszystkie** akcje w kontrolerze używają routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-351">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

<span data-ttu-id="670f4-352">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-352">In the preceding example:</span></span>

* <span data-ttu-id="670f4-353">Ścieżka adresu URL `/products` może być zgodna `ProductsApi.ListProducts`</span><span class="sxs-lookup"><span data-stu-id="670f4-353">The URL path `/products` can match `ProductsApi.ListProducts`</span></span>
* <span data-ttu-id="670f4-354">Ścieżka adresu URL `/products/5` może być zgodna `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="670f4-354">The URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span>

<span data-ttu-id="670f4-355">Obie te akcje pasują do `GET` HTTP, ponieważ są oznaczone atrybutem `[HttpGet]`.</span><span class="sxs-lookup"><span data-stu-id="670f4-355">Both of these actions only match HTTP `GET` because they're marked with the `[HttpGet]` attribute.</span></span>

<span data-ttu-id="670f4-356">Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są łączone z szablonami tras zastosowanymi do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-356">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="670f4-357">Poniższy przykład dopasowuje zestaw ścieżek adresów URL podobny do trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="670f4-357">The following example matches a set of URL paths similar to the default route.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="670f4-358">W poniższej tabeli opisano atrybuty `[Route]` w poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-358">The following table explains the `[Route]` attributes in the preceding code:</span></span>

| <span data-ttu-id="670f4-359">Atrybut</span><span class="sxs-lookup"><span data-stu-id="670f4-359">Attribute</span></span>               | <span data-ttu-id="670f4-360">Łączy się z `[Route("Home")]`</span><span class="sxs-lookup"><span data-stu-id="670f4-360">Combines with `[Route("Home")]`</span></span> | <span data-ttu-id="670f4-361">Definiuje szablon trasy</span><span class="sxs-lookup"><span data-stu-id="670f4-361">Defines route template</span></span> |
| ----------------- | ------------ | --------- |
| `[Route("")]` | <span data-ttu-id="670f4-362">Yes</span><span class="sxs-lookup"><span data-stu-id="670f4-362">Yes</span></span> | `"Home"` |
| `[Route("Index")]` | <span data-ttu-id="670f4-363">Yes</span><span class="sxs-lookup"><span data-stu-id="670f4-363">Yes</span></span> | `"Home/Index"` |
| `[Route("/")]` | <span data-ttu-id="670f4-364">**Nie**</span><span class="sxs-lookup"><span data-stu-id="670f4-364">**No**</span></span> | `""` |
| `[Route("About")]` | <span data-ttu-id="670f4-365">Yes</span><span class="sxs-lookup"><span data-stu-id="670f4-365">Yes</span></span> | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a><span data-ttu-id="670f4-366">Porządek trasy dla atrybutu</span><span class="sxs-lookup"><span data-stu-id="670f4-366">Attribute route order</span></span>

<span data-ttu-id="670f4-367">Routing kompiluje drzewo i dopasowuje wszystkie punkty końcowe jednocześnie:</span><span class="sxs-lookup"><span data-stu-id="670f4-367">Routing builds a tree and matches all endpoints simultaneously:</span></span>

* <span data-ttu-id="670f4-368">Wpisy trasy zachowują się tak, jakby zostały umieszczone w idealnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-368">The route entries behave as if placed in an ideal ordering.</span></span>
* <span data-ttu-id="670f4-369">Najbardziej konkretne trasy mają możliwość wykonania przed bardziej ogólnymi trasami.</span><span class="sxs-lookup"><span data-stu-id="670f4-369">The most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="670f4-370">Na przykład trasa atrybutu, taka jak `blog/search/{topic}`, jest bardziej precyzyjna niż trasa atrybutu, taka jak `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="670f4-370">For example, an attribute route like `blog/search/{topic}` is more specific than an attribute route like `blog/{*article}`.</span></span> <span data-ttu-id="670f4-371">Trasa `blog/search/{topic}` ma wyższy priorytet, ponieważ jest domyślnie bardziej specyficzna.</span><span class="sxs-lookup"><span data-stu-id="670f4-371">The `blog/search/{topic}` route has higher priority, by default, because it's more specific.</span></span> <span data-ttu-id="670f4-372">Przy użyciu [konwencjonalnego routingu](#cr)deweloper jest odpowiedzialny za umieszczanie tras w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-372">Using [conventional routing](#cr), the developer is responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="670f4-373">Trasy atrybutów mogą konfigurować kolejność przy użyciu właściwości <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order>.</span><span class="sxs-lookup"><span data-stu-id="670f4-373">Attribute routes can configure an order using the <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> property.</span></span> <span data-ttu-id="670f4-374">Wszystkie [atrybuty trasy](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) podane przez platformę obejmują `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-374">All of the framework provided [route attributes](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) include `Order` .</span></span> <span data-ttu-id="670f4-375">Trasy są przetwarzane zgodnie z rosnącym sortowaniem właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-375">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="670f4-376">Kolejność domyślna to `0`.</span><span class="sxs-lookup"><span data-stu-id="670f4-376">The default order is `0`.</span></span> <span data-ttu-id="670f4-377">Ustawienie trasy przy użyciu `Order = -1` jest uruchamiane przed trasami, które nie ustawiają kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-377">Setting a route using `Order = -1` runs before routes that don't set an order.</span></span> <span data-ttu-id="670f4-378">Ustawienie trasy przy użyciu `Order = 1` jest uruchamiane po określeniu domyślnej kolejności tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-378">Setting a route using `Order = 1` runs after default route ordering.</span></span>

<span data-ttu-id="670f4-379">**Należy unikać** w zależności od `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-379">**Avoid** depending on `Order`.</span></span> <span data-ttu-id="670f4-380">Jeśli przestrzeń adresów URL aplikacji wymaga jawnych wartości kolejności, aby można było prawidłowo kierować trasy, prawdopodobnie również jest myląca dla klientów.</span><span class="sxs-lookup"><span data-stu-id="670f4-380">If an app's URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="670f4-381">Ogólnie rzecz biorąc, routing atrybutów wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-381">In general, attribute routing selects the correct route with URL matching.</span></span> <span data-ttu-id="670f4-382">Jeśli domyślna kolejność generowania adresów URL nie działa, użycie nazwy trasy jako przesłonięcia jest zwykle prostsze niż stosowanie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-382">If the default order used for URL generation isn't working, using a route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="670f4-383">Należy wziąć pod uwagę następujące dwa kontrolery, które definiują `/home`pasujące do trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-383">Consider the following two controllers which both define the route matching `/home`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="670f4-384">Żądanie `/home` za pomocą powyższego kodu zgłasza wyjątek podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="670f4-384">Requesting `/home` with the preceding code throws an exception similar to the following:</span></span>

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

<span data-ttu-id="670f4-385">Dodanie `Order` do jednego z atrybutów trasy rozwiązuje niejednoznaczność:</span><span class="sxs-lookup"><span data-stu-id="670f4-385">Adding `Order` to one of the route attributes resolves the ambiguity:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

<span data-ttu-id="670f4-386">Za pomocą powyższego kodu `/home` uruchamia `HomeController.Index` punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="670f4-386">With the preceding code, `/home` runs the `HomeController.Index` endpoint.</span></span> <span data-ttu-id="670f4-387">Aby przejść do `MyDemoController.MyIndex`, zażądaj `/home/MyIndex`.</span><span class="sxs-lookup"><span data-stu-id="670f4-387">To get to the `MyDemoController.MyIndex`, request `/home/MyIndex`.</span></span> <span data-ttu-id="670f4-388">**Uwaga**:</span><span class="sxs-lookup"><span data-stu-id="670f4-388">**Note**:</span></span>

* <span data-ttu-id="670f4-389">Poprzedni kod jest przykładem lub słabym projektem routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-389">The preceding code is an example or poor routing design.</span></span> <span data-ttu-id="670f4-390">Został on użyty do zilustrowania właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-390">It was used to illustrate the `Order` property.</span></span>
* <span data-ttu-id="670f4-391">Właściwość `Order` rozwiązuje tylko niejednoznaczność, ale nie można dopasować tego szablonu.</span><span class="sxs-lookup"><span data-stu-id="670f4-391">The `Order` property only resolves the ambiguity, that template cannot be matched.</span></span> <span data-ttu-id="670f4-392">Lepszym rozwiązaniem jest usunięcie szablonu `[Route("Home")]`.</span><span class="sxs-lookup"><span data-stu-id="670f4-392">It would be better to remove the `[Route("Home")]` template.</span></span>

<span data-ttu-id="670f4-393">Zobacz [Razor Pages trasy i konwencje aplikacji: zamówienie trasy,](xref:razor-pages/razor-pages-conventions#route-order) Aby uzyskać informacje na temat kolejności tras z Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="670f4-393">See [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) for information on route order with Razor Pages.</span></span>

<span data-ttu-id="670f4-394">W niektórych przypadkach błąd HTTP 500 jest zwracany z niejednoznacznych tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-394">In some cases, an HTTP 500 error is returned with ambiguous routes.</span></span> <span data-ttu-id="670f4-395">Użyj [rejestrowania](xref:fundamentals/logging/index) , aby zobaczyć, które punkty końcowe spowodowały `AmbiguousMatchException`.</span><span class="sxs-lookup"><span data-stu-id="670f4-395">Use [logging](xref:fundamentals/logging/index) to see which endpoints caused the `AmbiguousMatchException`.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="670f4-396">Zastępowanie tokenu w szablonach tras [Controller], [Action], [obszar]</span><span class="sxs-lookup"><span data-stu-id="670f4-396">Token replacement in route templates [controller], [action], [area]</span></span>

<span data-ttu-id="670f4-397">Dla wygody atrybut trasy obsługują zastępowanie tokenów dla zarezerwowanych parametrów trasy przez załączanie tokenu w jednym z następujących:</span><span class="sxs-lookup"><span data-stu-id="670f4-397">For convenience, attribute routes support token replacement for reserved route parameters by enclosing a token in one of the following:</span></span>

* <span data-ttu-id="670f4-398">Kwadratowe nawiasy klamrowe: `[]`</span><span class="sxs-lookup"><span data-stu-id="670f4-398">Square braces: `[]`</span></span>
* <span data-ttu-id="670f4-399">Nawiasy klamrowe: `{}`</span><span class="sxs-lookup"><span data-stu-id="670f4-399">Curly braces: `{}`</span></span>

<span data-ttu-id="670f4-400">Tokeny `[action]`, `[area]`i `[controller]` są zastępowane wartościami nazwy akcji, obszaru i nazwy kontrolera z akcji, w której zdefiniowano trasę:</span><span class="sxs-lookup"><span data-stu-id="670f4-400">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="670f4-401">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-401">In the preceding code:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * <span data-ttu-id="670f4-402">Dopasowuje `/Products0/List`</span><span class="sxs-lookup"><span data-stu-id="670f4-402">Matches `/Products0/List`</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * <span data-ttu-id="670f4-403">Dopasowuje `/Products0/Edit/{id}`</span><span class="sxs-lookup"><span data-stu-id="670f4-403">Matches `/Products0/Edit/{id}`</span></span>

<span data-ttu-id="670f4-404">Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-404">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="670f4-405">Poprzedni przykład zachowuje się tak samo jak w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-405">The preceding example behaves the same as the following code:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="670f4-406">Trasy atrybutu można także łączyć z dziedziczeniem.</span><span class="sxs-lookup"><span data-stu-id="670f4-406">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="670f4-407">Jest to zaawansowane połączenie z zastępowaniem tokenu.</span><span class="sxs-lookup"><span data-stu-id="670f4-407">This is powerful combined with token replacement.</span></span> <span data-ttu-id="670f4-408">Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-408">Token replacement also applies to route names defined by attribute routes.</span></span>
<span data-ttu-id="670f4-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generuje unikatową nazwę trasy dla każdej akcji:</span><span class="sxs-lookup"><span data-stu-id="670f4-409">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generates a unique route name for each action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

<span data-ttu-id="670f4-410">Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-410">Token replacement also applies to route names defined by attribute routes.</span></span>
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
<span data-ttu-id="670f4-411">generuje unikatową nazwę trasy dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-411">generates a unique route name for each action.</span></span>

<span data-ttu-id="670f4-412">Aby dopasować ogranicznik zastępczy tokenu literału `[` lub `]`, należy to zrobić, powtarzając znak (`[[` lub `]]`).</span><span class="sxs-lookup"><span data-stu-id="670f4-412">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="670f4-413">Używanie transformatora parametrów do dostosowywania zastępowania tokenu</span><span class="sxs-lookup"><span data-stu-id="670f4-413">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="670f4-414">Zastępowanie tokenu można dostosować za pomocą transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="670f4-414">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="670f4-415">Transformator parametrów implementuje <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="670f4-415">A parameter transformer implements <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> and transforms the value of parameters.</span></span> <span data-ttu-id="670f4-416">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`:</span><span class="sxs-lookup"><span data-stu-id="670f4-416">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<span data-ttu-id="670f4-417"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> jest konwencją modelu aplikacji, która:</span><span class="sxs-lookup"><span data-stu-id="670f4-417">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> is an application model convention that:</span></span>

* <span data-ttu-id="670f4-418">Stosuje transformator parametrów do wszystkich tras atrybutów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-418">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="670f4-419">Dostosowuje wartości tokenów tras w miarę ich wymiany.</span><span class="sxs-lookup"><span data-stu-id="670f4-419">Customizes the attribute route token values as they are replaced.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

<span data-ttu-id="670f4-420">Poprzednia metoda `ListAll` pasuje do `/subscription-management/list-all`.</span><span class="sxs-lookup"><span data-stu-id="670f4-420">The preceding `ListAll` method matches `/subscription-management/list-all`.</span></span>

<span data-ttu-id="670f4-421">`RouteTokenTransformerConvention` jest zarejestrowany jako opcja w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="670f4-421">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

<span data-ttu-id="670f4-422">Zobacz [powiadomienia MDN Web docs](https://developer.mozilla.org/docs/Glossary/Slug) , aby zapoznać się z definicją informacji o sobie.</span><span class="sxs-lookup"><span data-stu-id="670f4-422">See [MDN web docs on Slug](https://developer.mozilla.org/docs/Glossary/Slug) for the definition of Slug.</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a><span data-ttu-id="670f4-423">Wiele tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="670f4-423">Multiple attribute routes</span></span>

<span data-ttu-id="670f4-424">Routing atrybutów obsługuje definiowanie wielu tras, które docierają do tej samej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-424">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="670f4-425">Najczęstszym sposobem użycia tej metody jest naśladowanie zachowania domyślnej trasy konwencjonalnej, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-425">The most common usage of this is to mimic the behavior of the default conventional route as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

<span data-ttu-id="670f4-426">Umieszczenie wielu atrybutów trasy na kontrolerze oznacza, że każda z nich łączy się z każdym z atrybutów tras w metodach akcji:</span><span class="sxs-lookup"><span data-stu-id="670f4-426">Putting multiple route attributes on the controller means that each one combines with each of the route attributes on the action methods:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

<span data-ttu-id="670f4-427">Wszystkie ograniczenia trasy [zlecenia http](#verb) implementują `IActionConstraint`.</span><span class="sxs-lookup"><span data-stu-id="670f4-427">All the [HTTP verb](#verb) route constraints implement `IActionConstraint`.</span></span>

<span data-ttu-id="670f4-428">Gdy wiele atrybutów trasy, które implementują <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> są umieszczane w akcji:</span><span class="sxs-lookup"><span data-stu-id="670f4-428">When multiple route attributes that implement <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are placed on an action:</span></span>

* <span data-ttu-id="670f4-429">Każde ograniczenie akcji łączy się z szablonem trasy zastosowanym do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-429">Each action constraint combines with the route template applied to the controller.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

<span data-ttu-id="670f4-430">Korzystanie z wielu tras w akcjach może wydawać się użyteczne i zaawansowane, dlatego lepiej jest zachować podstawową i dobrze zdefiniowaną przestrzeń adresów URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-430">Using multiple routes on actions might seem useful and powerful, it's better to keep your app's URL space basic and well defined.</span></span> <span data-ttu-id="670f4-431">Użyj wielu tras w przypadku akcji **tylko wtedy** , gdy jest to konieczne, na przykład w celu obsługi istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="670f4-431">Use multiple routes on actions **only** where needed, for example, to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="670f4-432">Określanie opcjonalnych parametrów trasy, wartości domyślnych i ograniczeń</span><span class="sxs-lookup"><span data-stu-id="670f4-432">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="670f4-433">Trasy atrybutów obsługują tę samą składnię wbudowaną co konwencjonalne trasy, aby określić parametry opcjonalne, wartości domyślne i ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="670f4-433">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

<span data-ttu-id="670f4-434">W poprzednim kodzie `[HttpPost("product/{id:int}")]` stosuje ograniczenie trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-434">In the preceding code, `[HttpPost("product/{id:int}")]` applies a route constraint.</span></span> <span data-ttu-id="670f4-435">Akcja `ProductsController.ShowProduct` jest dopasowywana tylko przez ścieżki URL, takie jak `/product/3`.</span><span class="sxs-lookup"><span data-stu-id="670f4-435">The `ProductsController.ShowProduct` action is matched only by URL paths like `/product/3`.</span></span> <span data-ttu-id="670f4-436">Część szablonu trasy `{id:int}` ogranicza ten segment tylko do liczb całkowitych.</span><span class="sxs-lookup"><span data-stu-id="670f4-436">The route template portion `{id:int}` constrains that segment to only integers.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="670f4-437">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="670f4-437">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="670f4-438">Niestandardowe atrybuty trasy przy użyciu IRouteTemplateProvider</span><span class="sxs-lookup"><span data-stu-id="670f4-438">Custom route attributes using IRouteTemplateProvider</span></span>

<span data-ttu-id="670f4-439">Wszystkie [atrybuty trasy](#rt) implementują <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span><span class="sxs-lookup"><span data-stu-id="670f4-439">All of the [route attributes](#rt) implement <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span></span> <span data-ttu-id="670f4-440">Środowisko uruchomieniowe ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="670f4-440">The ASP.NET Core runtime:</span></span>

* <span data-ttu-id="670f4-441">Wyszukuje atrybuty klas kontrolera i metod akcji podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-441">Looks for attributes on controller classes and action methods when the app starts.</span></span>
* <span data-ttu-id="670f4-442">Używa atrybutów, które implementują `IRouteTemplateProvider`, aby skompilować początkowy zestaw tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-442">Uses the attributes that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="670f4-443">Zaimplementuj `IRouteTemplateProvider`, aby zdefiniować niestandardowe atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-443">Implement `IRouteTemplateProvider` to define custom route attributes.</span></span> <span data-ttu-id="670f4-444">Każda `IRouteTemplateProvider` umożliwia zdefiniowanie pojedynczej trasy z niestandardowym szablonem trasy, kolejnością i nazwą:</span><span class="sxs-lookup"><span data-stu-id="670f4-444">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

<span data-ttu-id="670f4-445">Poprzednia metoda `Get` zwraca `Order = 2, Template = api/MyTestApi`.</span><span class="sxs-lookup"><span data-stu-id="670f4-445">The preceding `Get` method returns `Order = 2, Template = api/MyTestApi`.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a><span data-ttu-id="670f4-446">Dostosowywanie tras atrybutów przy użyciu modelu aplikacji</span><span class="sxs-lookup"><span data-stu-id="670f4-446">Use application model to customize attribute routes</span></span>

<span data-ttu-id="670f4-447">Model aplikacji:</span><span class="sxs-lookup"><span data-stu-id="670f4-447">The application model:</span></span>

* <span data-ttu-id="670f4-448">Jest modelem obiektu utworzonym podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="670f4-448">Is an object model created at startup.</span></span>
* <span data-ttu-id="670f4-449">Zawiera wszystkie metadane używane przez ASP.NET Core do kierowania i wykonywania akcji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-449">Contains all of the metadata used by ASP.NET Core to route and execute the actions in an app.</span></span>

<span data-ttu-id="670f4-450">Model aplikacji zawiera wszystkie dane zebrane z atrybutów tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-450">The application model includes all of the data gathered from route attributes.</span></span> <span data-ttu-id="670f4-451">Atrybuty trasy są udostępniane przez implementację `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="670f4-451">The data from route attributes is provided by the `IRouteTemplateProvider` implementation.</span></span> <span data-ttu-id="670f4-452">Obowiązujący</span><span class="sxs-lookup"><span data-stu-id="670f4-452">Conventions:</span></span>

* <span data-ttu-id="670f4-453">Można napisać, aby zmodyfikować model aplikacji, aby dostosować sposób zachowania routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-453">Can be written to modify the application model to customize how routing behaves.</span></span>
* <span data-ttu-id="670f4-454">Są odczytywane podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-454">Are read at app startup.</span></span>

<span data-ttu-id="670f4-455">W tej sekcji przedstawiono podstawowy przykład dostosowywania routingu przy użyciu modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-455">This section shows a basic example of customizing routing using application model.</span></span> <span data-ttu-id="670f4-456">Poniższy kod sprawia, że trasy są w przybliżeniu zgodne ze strukturą folderów projektu.</span><span class="sxs-lookup"><span data-stu-id="670f4-456">The following code makes routes roughly line up with the folder structure of the project.</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

<span data-ttu-id="670f4-457">Poniższy kod uniemożliwia stosowanie Konwencji `namespace` do kontrolerów, które są kierowane przez atrybut:</span><span class="sxs-lookup"><span data-stu-id="670f4-457">The following code prevents the `namespace` convention from being applied to controllers that are attribute routed:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

<span data-ttu-id="670f4-458">Na przykład następujący kontroler nie używa `NamespaceRoutingConvention`:</span><span class="sxs-lookup"><span data-stu-id="670f4-458">For example, the following controller doesn't use `NamespaceRoutingConvention`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

<span data-ttu-id="670f4-459">Metoda `NamespaceRoutingConvention.Apply`:</span><span class="sxs-lookup"><span data-stu-id="670f4-459">The `NamespaceRoutingConvention.Apply` method:</span></span>

* <span data-ttu-id="670f4-460">Nie robi nic, Jeśli kontroler ma przypisany atrybut.</span><span class="sxs-lookup"><span data-stu-id="670f4-460">Does nothing if the controller is attribute routed.</span></span>
* <span data-ttu-id="670f4-461">Ustawia szablon szablonu na podstawie `namespace`z usuniętym `namespace` bazowym.</span><span class="sxs-lookup"><span data-stu-id="670f4-461">Sets the controllers template based on the `namespace`, with the base `namespace` removed.</span></span>

<span data-ttu-id="670f4-462">`NamespaceRoutingConvention` można zastosować w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="670f4-462">The `NamespaceRoutingConvention` can be applied in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

<span data-ttu-id="670f4-463">Rozważmy na przykład następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="670f4-463">For example, consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

<span data-ttu-id="670f4-464">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-464">In the preceding code:</span></span>

* <span data-ttu-id="670f4-465">`namespace` podstawowe jest `My.Application`.</span><span class="sxs-lookup"><span data-stu-id="670f4-465">The base `namespace` is `My.Application`.</span></span>
* <span data-ttu-id="670f4-466">Pełna nazwa poprzedniego kontrolera jest `My.Application.Admin.Controllers.UsersController`.</span><span class="sxs-lookup"><span data-stu-id="670f4-466">The full name of the preceding controller is `My.Application.Admin.Controllers.UsersController`.</span></span>
* <span data-ttu-id="670f4-467">`NamespaceRoutingConvention` ustawia szablon controllers na `Admin/Controllers/Users/[action]/{id?`.</span><span class="sxs-lookup"><span data-stu-id="670f4-467">The `NamespaceRoutingConvention` sets the controllers template to `Admin/Controllers/Users/[action]/{id?`.</span></span>

<span data-ttu-id="670f4-468">`NamespaceRoutingConvention` można również zastosować jako atrybut na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="670f4-468">The `NamespaceRoutingConvention` can also be applied as an attribute on a controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="670f4-469">Routing mieszany: Routing atrybutów a konwencjonalny Routing</span><span class="sxs-lookup"><span data-stu-id="670f4-469">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="670f4-470">Aplikacje ASP.NET Core mogą łączyć użycie konwencjonalnych routingu i routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-470">ASP.NET Core apps can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="670f4-471">Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.</span><span class="sxs-lookup"><span data-stu-id="670f4-471">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="670f4-472">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="670f4-472">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="670f4-473">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="670f4-473">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="670f4-474">Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa.</span><span class="sxs-lookup"><span data-stu-id="670f4-474">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="670f4-475">**Każdy** atrybut trasy na kontrolerze powoduje kierowanie **wszystkich** akcji w atrybucie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-475">**Any** route attribute on the controller makes **all** actions in the controller attribute routed.</span></span>

<span data-ttu-id="670f4-476">Routing atrybutów i Routing konwencjonalny używają tego samego aparatu routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-476">Attribute routing and conventional routing use the same routing engine.</span></span>

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a><span data-ttu-id="670f4-477">Generowanie adresów URL i wartości otoczenia</span><span class="sxs-lookup"><span data-stu-id="670f4-477">URL Generation and ambient values</span></span>

<span data-ttu-id="670f4-478">Aplikacje mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-478">Apps can use routing URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="670f4-479">Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany.</span><span class="sxs-lookup"><span data-stu-id="670f4-479">Generating URLs eliminates hardcoding URLs, making code more robust and maintainable.</span></span> <span data-ttu-id="670f4-480">Ta sekcja koncentruje się na funkcjach generowania adresów URL udostępnianych przez MVC i obejmuje jedynie podstawowe informacje na temat sposobu działania generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-480">This section focuses on the URL generation features provided by MVC and only cover basics of how URL generation works.</span></span> <span data-ttu-id="670f4-481">Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="670f4-481">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="670f4-482">Interfejs <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> jest podstawowym elementem infrastruktury między MVC i routingiem na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-482">The <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> interface is the underlying element of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="670f4-483">Wystąpienie `IUrlHelper` jest dostępne za pomocą właściwości `Url` w obszarze Kontrolery, widoki i składniki widoku.</span><span class="sxs-lookup"><span data-stu-id="670f4-483">An instance of `IUrlHelper` is available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="670f4-484">W poniższym przykładzie interfejs `IUrlHelper` jest używany przez właściwość `Controller.Url` do generowania adresu URL dla innej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-484">In the following example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="670f4-485">Jeśli aplikacja używa domyślnej trasy konwencjonalnej, wartość zmiennej `url` jest ciągiem ścieżki adresu URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="670f4-485">If the app is using the default conventional route, the value of the `url` variable is the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="670f4-486">Ta ścieżka URL jest tworzona przez połączenie za pomocą routingu:</span><span class="sxs-lookup"><span data-stu-id="670f4-486">This URL path is created by routing by combining:</span></span>

* <span data-ttu-id="670f4-487">Wartości trasy z bieżącego żądania, które są nazywane **wartościami otoczenia**.</span><span class="sxs-lookup"><span data-stu-id="670f4-487">The route values from the current request, which are called **ambient values**.</span></span>
* <span data-ttu-id="670f4-488">Wartości przenoszone do `Url.Action` i podstawiające te wartości do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-488">The values passed to `Url.Action` and substituting those values into the route template:</span></span>

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="670f4-489">Każdy parametr trasy w szablonie trasy ma swoją wartość zastępowaną przez pasujące nazwy wartościami i wartościami otoczenia.</span><span class="sxs-lookup"><span data-stu-id="670f4-489">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="670f4-490">Parametr trasy, który nie ma wartości, może:</span><span class="sxs-lookup"><span data-stu-id="670f4-490">A route parameter that doesn't have a value can:</span></span>

* <span data-ttu-id="670f4-491">Użyj wartości domyślnej, jeśli ma ją.</span><span class="sxs-lookup"><span data-stu-id="670f4-491">Use a default value if it has one.</span></span>
* <span data-ttu-id="670f4-492">Można pominąć, jeśli jest to opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="670f4-492">Be skipped if it's optional.</span></span> <span data-ttu-id="670f4-493">Na przykład `id` z szablonu trasy `{controller}/{action}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="670f4-493">For example, the `id` from the  route template `{controller}/{action}/{id?}`.</span></span>

<span data-ttu-id="670f4-494">Generowanie adresu URL kończy się niepowodzeniem, jeśli którykolwiek z wymaganych parametrów trasy nie ma odpowiadającej mu wartości.</span><span class="sxs-lookup"><span data-stu-id="670f4-494">URL generation fails if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="670f4-495">Jeśli generowanie adresów URL kończy się niepowodzeniem dla trasy, kolejna trasa zostanie ponowiona do momentu przetworzenia wszystkich tras lub znalezienia dopasowania.</span><span class="sxs-lookup"><span data-stu-id="670f4-495">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="670f4-496">Powyższy przykład `Url.Action` zakłada, że [konwencjonalne Routing](#cr).</span><span class="sxs-lookup"><span data-stu-id="670f4-496">The preceding example of `Url.Action` assumes [conventional routing](#cr).</span></span> <span data-ttu-id="670f4-497">Generowanie adresów URL działa podobnie jak w przypadku [routingu atrybutów](#ar), chociaż koncepcje różnią się.</span><span class="sxs-lookup"><span data-stu-id="670f4-497">URL generation works similarly with [attribute routing](#ar), though the concepts are different.</span></span> <span data-ttu-id="670f4-498">Z routingiem konwencjonalnym:</span><span class="sxs-lookup"><span data-stu-id="670f4-498">With conventional routing:</span></span>

* <span data-ttu-id="670f4-499">Wartości trasy służą do rozszerzania szablonu.</span><span class="sxs-lookup"><span data-stu-id="670f4-499">The route values are used to expand a template.</span></span>
* <span data-ttu-id="670f4-500">Wartości trasy dla `controller` i `action` są zwykle wyświetlane w tym szablonie.</span><span class="sxs-lookup"><span data-stu-id="670f4-500">The route values for `controller` and `action` usually appear in that template.</span></span> <span data-ttu-id="670f4-501">Dzieje się tak, ponieważ adresy URL dopasowane przez Routing są zgodne z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="670f4-501">This works because the URLs matched by routing adhere to a convention.</span></span>

<span data-ttu-id="670f4-502">W poniższym przykładzie zastosowano Routing atrybutów:</span><span class="sxs-lookup"><span data-stu-id="670f4-502">The following example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

<span data-ttu-id="670f4-503">Akcja `Source` w poprzednim kodzie generuje `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="670f4-503">The `Source` action in the preceding code generates `custom/url/to/destination`.</span></span>

<span data-ttu-id="670f4-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> został dodany w ASP.NET Core 3,0 jako alternatywa dla `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="670f4-504"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> was added in ASP.NET Core 3.0 as an alternative to `IUrlHelper`.</span></span> <span data-ttu-id="670f4-505">`LinkGenerator` oferuje podobne, ale bardziej elastyczne funkcje.</span><span class="sxs-lookup"><span data-stu-id="670f4-505">`LinkGenerator` offers similar but more flexible functionality.</span></span> <span data-ttu-id="670f4-506">Każda inna metoda na `IUrlHelper` ma również odpowiednią rodzinę metod na `LinkGenerator`.</span><span class="sxs-lookup"><span data-stu-id="670f4-506">Each other the methods on `IUrlHelper` has a corresponding family of methods on `LinkGenerator` as well.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="670f4-507">Generowanie adresów URL według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="670f4-507">Generating URLs by action name</span></span>

<span data-ttu-id="670f4-508">Wartość [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator. GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)i wszystkie powiązane przeciążenia są przeznaczone do generowania docelowego punktu końcowego przez określenie nazwy kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-508">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator.GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*), and all related overloads all are designed to generate the target endpoint by specifying a controller name and action name.</span></span>

<span data-ttu-id="670f4-509">W przypadku korzystania z `Url.Action`bieżąca wartość trasy dla `controller` i `action` jest zapewniana przez środowisko uruchomieniowe:</span><span class="sxs-lookup"><span data-stu-id="670f4-509">When using `Url.Action`, the current route values for `controller` and `action` are provided by the runtime:</span></span>

* <span data-ttu-id="670f4-510">Wartość `controller` i `action` są częścią [wartości otoczenia](#ambient) i wartości.</span><span class="sxs-lookup"><span data-stu-id="670f4-510">The value of `controller` and `action` are part of both [ambient values](#ambient) and values.</span></span> <span data-ttu-id="670f4-511">Metoda `Url.Action` zawsze używa bieżących wartości `action` i `controller` i generuje ścieżkę URL, która kieruje do bieżącej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-511">The method `Url.Action` always uses the current values of `action` and `controller` and generates a URL path that routes to the current action.</span></span>

<span data-ttu-id="670f4-512">Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały dostarczone podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-512">Routing attempts to use the values in ambient values to fill in information that wasn't provided when generating a URL.</span></span> <span data-ttu-id="670f4-513">Rozważmy trasę, taką jak `{a}/{b}/{c}/{d}` z wartościami otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`:</span><span class="sxs-lookup"><span data-stu-id="670f4-513">Consider a route like `{a}/{b}/{c}/{d}` with ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`:</span></span>

* <span data-ttu-id="670f4-514">Routing ma wystarczającą ilość informacji do wygenerowania adresu URL bez dodatkowych wartości.</span><span class="sxs-lookup"><span data-stu-id="670f4-514">Routing has enough information to generate a URL without any additional values.</span></span>
* <span data-ttu-id="670f4-515">Routing ma wystarczającą ilość informacji, ponieważ wszystkie parametry tras mają wartość.</span><span class="sxs-lookup"><span data-stu-id="670f4-515">Routing has enough information because all route parameters have a value.</span></span>

<span data-ttu-id="670f4-516">W przypadku dodania wartości `{ d = Donovan }`:</span><span class="sxs-lookup"><span data-stu-id="670f4-516">If the value `{ d = Donovan }` is added:</span></span>

* <span data-ttu-id="670f4-517">Wartość `{ d = David }` jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="670f4-517">The value `{ d = David }` is ignored.</span></span>
* <span data-ttu-id="670f4-518">Wygenerowana Ścieżka adresu URL jest `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="670f4-518">The generated URL path is `Alice/Bob/Carol/Donovan`.</span></span>

<span data-ttu-id="670f4-519">**Ostrzeżenie**: ścieżki URL są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="670f4-519">**Warning**: URL paths are hierarchical.</span></span> <span data-ttu-id="670f4-520">W poprzednim przykładzie, jeśli wartość `{ c = Cheryl }` zostanie dodana:</span><span class="sxs-lookup"><span data-stu-id="670f4-520">In the preceding example, if the value `{ c = Cheryl }` is added:</span></span>

* <span data-ttu-id="670f4-521">Obie wartości `{ c = Carol, d = David }` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="670f4-521">Both of the values `{ c = Carol, d = David }` are ignored.</span></span>
* <span data-ttu-id="670f4-522">Nie ma już wartości `d` i generowanie adresu URL kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="670f4-522">There is no longer a value for `d` and URL generation fails.</span></span>
* <span data-ttu-id="670f4-523">Wymagane wartości `c` i `d` muszą być określone w celu wygenerowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-523">The desired values of `c` and `d` must be specified to generate a URL.</span></span>  

<span data-ttu-id="670f4-524">Może się spodziewać, że ten problem zostanie osiągnięty przy użyciu trasy domyślnej `{controller}/{action}/{id?}`.</span><span class="sxs-lookup"><span data-stu-id="670f4-524">You might expect to hit this problem with the default route `{controller}/{action}/{id?}`.</span></span> <span data-ttu-id="670f4-525">Ten problem występuje rzadko, ponieważ `Url.Action` zawsze jawnie określa `controller` i `action` wartość.</span><span class="sxs-lookup"><span data-stu-id="670f4-525">This problem is rare in practice because `Url.Action` always explicitly specifies a `controller` and `action` value.</span></span>

<span data-ttu-id="670f4-526">Kilka przeciążeń [adresu URL. akcja](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) przyjmuje obiekt wartości trasy, aby podać wartości parametrów trasy innych niż `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="670f4-526">Several overloads of [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) take a route values object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="670f4-527">Obiekt wartości trasy jest często używany z `id`.</span><span class="sxs-lookup"><span data-stu-id="670f4-527">The route values object is frequently used with `id`.</span></span> <span data-ttu-id="670f4-528">Na przykład `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="670f4-528">For example, `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="670f4-529">Obiekt wartości trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-529">The route values object:</span></span>

* <span data-ttu-id="670f4-530">Według Konwencji jest zwykle obiektem typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="670f4-530">By convention is usually an object of anonymous type.</span></span>
* <span data-ttu-id="670f4-531">Może to być `IDictionary<>` lub [poco](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span><span class="sxs-lookup"><span data-stu-id="670f4-531">Can be an `IDictionary<>` or a [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span></span>

<span data-ttu-id="670f4-532">Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="670f4-532">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="670f4-533">Poprzedni kod generuje `/Products/Buy/17?color=red`.</span><span class="sxs-lookup"><span data-stu-id="670f4-533">The preceding code generates `/Products/Buy/17?color=red`.</span></span>

<span data-ttu-id="670f4-534">Poniższy kod generuje bezwzględny adres URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-534">The following code generates an absolute URL:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="670f4-535">Aby utworzyć bezwzględny adres URL, użyj jednego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="670f4-535">To create an absolute URL, use one of the following:</span></span>

* <span data-ttu-id="670f4-536">Przeciążenie, które akceptuje `protocol`.</span><span class="sxs-lookup"><span data-stu-id="670f4-536">An overload that accepts a `protocol`.</span></span> <span data-ttu-id="670f4-537">Na przykład poprzedzający kod.</span><span class="sxs-lookup"><span data-stu-id="670f4-537">For example, the preceding code.</span></span>
* <span data-ttu-id="670f4-538">[LinkGenerator. GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), który domyślnie generuje bezwzględne identyfikatory URI.</span><span class="sxs-lookup"><span data-stu-id="670f4-538">[LinkGenerator.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), which generates absolute URIs by default.</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a><span data-ttu-id="670f4-539">Generuj adresy URL według trasy</span><span class="sxs-lookup"><span data-stu-id="670f4-539">Generate URLs by route</span></span>

<span data-ttu-id="670f4-540">Poprzedni kod wykazał wygenerowanie adresu URL przez przekazanie go do kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-540">The preceding code demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="670f4-541">`IUrlHelper` udostępnia również [RouteUrl z adresem URL.](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*)</span><span class="sxs-lookup"><span data-stu-id="670f4-541">`IUrlHelper` also provides the [Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) family of methods.</span></span> <span data-ttu-id="670f4-542">Te metody są podobne do [adresu URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-542">These methods are similar to [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="670f4-543">Najczęstsze użycie `Url.RouteUrl`:</span><span class="sxs-lookup"><span data-stu-id="670f4-543">The most common usage of `Url.RouteUrl`:</span></span>

* <span data-ttu-id="670f4-544">Określa nazwę trasy do wygenerowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-544">Specifies a route name to generate the URL.</span></span>
* <span data-ttu-id="670f4-545">Zwykle nie określa kontrolera ani nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-545">Generally doesn't specify a controller or action name.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

<span data-ttu-id="670f4-546">Następujący plik Razor generuje link HTML do `Destination_Route`:</span><span class="sxs-lookup"><span data-stu-id="670f4-546">The following Razor file generates an HTML link to the `Destination_Route`:</span></span>

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a><span data-ttu-id="670f4-547">Generuj adresy URL w formacie HTML i Razor</span><span class="sxs-lookup"><span data-stu-id="670f4-547">Generate URLs in HTML and Razor</span></span>

<span data-ttu-id="670f4-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> udostępnia metody <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> [HTML. BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) i [HTML. ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) w celu wygenerowania odpowiednio elementów `<form>` i `<a>`.</span><span class="sxs-lookup"><span data-stu-id="670f4-548"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> provides the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> methods [Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) and [Html.ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="670f4-549">Metody te używają metody [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) do generowania adresu URL i akceptują podobne argumenty.</span><span class="sxs-lookup"><span data-stu-id="670f4-549">These methods use the [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="670f4-550">`Url.RouteUrl` pomocników dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink`, które mają podobną funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="670f4-550">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="670f4-551">TagHelpers Generuj adresy URL za pomocą `form` TagHelper i `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="670f4-551">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="670f4-552">Oba te zastosowania `IUrlHelper` do ich implementacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-552">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="670f4-553">Aby uzyskać więcej informacji, zobacz [pomocników tagów w formularzach](xref:mvc/views/working-with-forms) .</span><span class="sxs-lookup"><span data-stu-id="670f4-553">See [Tag Helpers in forms](xref:mvc/views/working-with-forms) for more information.</span></span>

<span data-ttu-id="670f4-554">W widokach, `IUrlHelper` jest dostępna za pomocą właściwości `Url` dla dowolnej generacji adresów URL ad hoc, które nie są objęte powyższym.</span><span class="sxs-lookup"><span data-stu-id="670f4-554">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a><span data-ttu-id="670f4-555">Generowanie adresów URL w wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="670f4-555">URL generation in Action Results</span></span>

<span data-ttu-id="670f4-556">Poprzednie przykłady pokazano przy użyciu `IUrlHelper` w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="670f4-556">The preceding examples showed using `IUrlHelper` in a controller.</span></span> <span data-ttu-id="670f4-557">Najczęstszym sposobem użycia na kontrolerze jest wygenerowanie adresu URL jako części wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-557">The most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="670f4-558">Klasy bazowe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> i <xref:Microsoft.AspNetCore.Mvc.Controller> zapewniają wygodne metody dla wyników akcji, które odwołują się do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-558">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase> and <xref:Microsoft.AspNetCore.Mvc.Controller> base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="670f4-559">Jednym z typowych zastosowań jest przekierowanie po zaakceptowaniu danych wejściowych użytkownika:</span><span class="sxs-lookup"><span data-stu-id="670f4-559">One typical usage is to redirect after accepting user input:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

<span data-ttu-id="670f4-560">Metody fabryki wyników działania, takie jak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>, są zgodne z podobnym wzorcem do metod w `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="670f4-560">The action results factory methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="670f4-561">Specjalny przypadek dla dedykowanych tras konwencjonalnych</span><span class="sxs-lookup"><span data-stu-id="670f4-561">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="670f4-562">Funkcja [routingu konwencjonalnego](#cr) może używać specjalnego rodzaju definicji trasy o nazwie [dedykowanej, konwencjonalnej trasy](#dcr).</span><span class="sxs-lookup"><span data-stu-id="670f4-562">[Conventional routing](#cr) can use a special kind of route definition called a [dedicated conventional route](#dcr).</span></span> <span data-ttu-id="670f4-563">W poniższym przykładzie trasa o nazwie `blog` jest dedykowaną konwencjonalną trasą:</span><span class="sxs-lookup"><span data-stu-id="670f4-563">In the following example, the route named `blog` is a dedicated conventional route:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="670f4-564">Przy użyciu poprzednich definicji trasy `Url.Action("Index", "Home")` generuje ścieżkę URL `/` przy użyciu trasy `default`, ale dlaczego?</span><span class="sxs-lookup"><span data-stu-id="670f4-564">Using the preceding route definitions, `Url.Action("Index", "Home")` generates the URL path `/` using the `default` route, but why?</span></span> <span data-ttu-id="670f4-565">Możesz odgadnąć wartości tras `{ controller = Home, action = Index }` wystarcza do wygenerowania adresu URL przy użyciu `blog`, a wynik zostanie `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="670f4-565">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="670f4-566">[Dedykowane konwencjonalne trasy](#dcr) polegają na specjalnym zachowaniu wartości domyślnych, które nie mają odpowiadającego parametru trasy, który uniemożliwia zbyt [zachłanne](xref:fundamentals/routing#greedy) trasy z generowaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-566">[Dedicated conventional routes](#dcr) rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being too [greedy](xref:fundamentals/routing#greedy) with URL generation.</span></span> <span data-ttu-id="670f4-567">W takim przypadku wartości domyślne są `{ controller = Blog, action = Article }`, a ani `controller`, ani `action` nie pojawiają się jako parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-567">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="670f4-568">Gdy Routing wykonuje generowanie adresów URL, podane wartości muszą być zgodne z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="670f4-568">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="670f4-569">Generowanie adresu URL przy użyciu `blog` nie powiodło się, ponieważ wartości `{ controller = Home, action = Index }` nie pasują do `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-569">URL generation using `blog` fails because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="670f4-570">Następnie usługa routingu wraca do wypróbowania `default`, która kończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="670f4-570">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="670f4-571">Obszary</span><span class="sxs-lookup"><span data-stu-id="670f4-571">Areas</span></span>

<span data-ttu-id="670f4-572">[Obszary](xref:mvc/controllers/areas) są funkcją MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej:</span><span class="sxs-lookup"><span data-stu-id="670f4-572">[Areas](xref:mvc/controllers/areas) are an MVC feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="670f4-573">Przestrzeń nazw routingu dla akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-573">Routing namespace for controller actions.</span></span>
* <span data-ttu-id="670f4-574">Struktura folderów dla widoków.</span><span class="sxs-lookup"><span data-stu-id="670f4-574">Folder structure for views.</span></span>

<span data-ttu-id="670f4-575">Użycie obszarów umożliwia aplikacji posiadanie wielu kontrolerów o tej samej nazwie, o ile mają one różne obszary.</span><span class="sxs-lookup"><span data-stu-id="670f4-575">Using areas allows an app to have multiple controllers with the same name, as long as they have different areas.</span></span> <span data-ttu-id="670f4-576">Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie innego parametru trasy, `area` do `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="670f4-576">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="670f4-577">W tej sekcji omówiono sposób, w jaki Routing współdziała z obszarami.</span><span class="sxs-lookup"><span data-stu-id="670f4-577">This section discusses how routing interacts with areas.</span></span> <span data-ttu-id="670f4-578">Zobacz [obszary](xref:mvc/controllers/areas) , aby uzyskać szczegółowe informacje na temat sposobu używania obszarów w widokach.</span><span class="sxs-lookup"><span data-stu-id="670f4-578">See [Areas](xref:mvc/controllers/areas) for details about how areas are used with views.</span></span>

<span data-ttu-id="670f4-579">Poniższy przykład konfiguruje MVC do używania domyślnej trasy konwencjonalnej i trasy `area` dla `area` o nazwie `Blog`:</span><span class="sxs-lookup"><span data-stu-id="670f4-579">The following example configures MVC to use the default conventional route and an `area` route for an `area` named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="670f4-580">W poprzednim kodzie <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> jest wywoływana w celu utworzenia `"blog_route"`.</span><span class="sxs-lookup"><span data-stu-id="670f4-580">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> is called to create the `"blog_route"`.</span></span> <span data-ttu-id="670f4-581">Drugi parametr, `"Blog"`, jest nazwą obszaru.</span><span class="sxs-lookup"><span data-stu-id="670f4-581">The second parameter, `"Blog"`, is the area name.</span></span>

<span data-ttu-id="670f4-582">W przypadku dopasowania ścieżki URL, takiej jak `/Manage/Users/AddUser`, trasa `"blog_route"` generuje wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-582">When matching a URL path like `/Manage/Users/AddUser`, the `"blog_route"` route generates the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="670f4-583">`area` wartość trasy jest generowana przez wartość domyślną dla `area`.</span><span class="sxs-lookup"><span data-stu-id="670f4-583">The `area` route value is produced by a default value for `area`.</span></span> <span data-ttu-id="670f4-584">Trasa utworzona przez `MapAreaControllerRoute` jest równoważna następującym:</span><span class="sxs-lookup"><span data-stu-id="670f4-584">The route created by `MapAreaControllerRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

<span data-ttu-id="670f4-585">`MapAreaControllerRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dla `area` przy użyciu podanej nazwy obszaru, w tym przypadku `Blog`.</span><span class="sxs-lookup"><span data-stu-id="670f4-585">`MapAreaControllerRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="670f4-586">Wartość domyślna zapewnia, że trasa zawsze generuje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-586">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

<span data-ttu-id="670f4-587">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-587">Conventional routing is order-dependent.</span></span> <span data-ttu-id="670f4-588">Ogólnie rzecz biorąc, trasy z obszarami powinny być umieszczone wcześniej, ponieważ są bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="670f4-588">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span>

<span data-ttu-id="670f4-589">Korzystając z powyższego przykładu, wartości trasy `{ area = Blog, controller = Users, action = AddUser }` są zgodne z następującą akcją:</span><span class="sxs-lookup"><span data-stu-id="670f4-589">Using the preceding example, the route values `{ area = Blog, controller = Users, action = AddUser }` match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="670f4-590">Atrybut [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) wskazuje, co oznacza kontroler w ramach obszaru.</span><span class="sxs-lookup"><span data-stu-id="670f4-590">The [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute is what denotes a controller as part of an area.</span></span> <span data-ttu-id="670f4-591">Ten kontroler znajduje się w obszarze `Blog`.</span><span class="sxs-lookup"><span data-stu-id="670f4-591">This controller is in the `Blog` area.</span></span> <span data-ttu-id="670f4-592">Kontrolery bez atrybutu `[Area]` nie są członkami żadnego obszaru i **nie pasują** , gdy wartość trasy `area` jest udostępniana przez usługę Routing.</span><span class="sxs-lookup"><span data-stu-id="670f4-592">Controllers without an `[Area]` attribute are not members of any area, and do **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="670f4-593">W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-593">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

<span data-ttu-id="670f4-594">Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu na potrzeby kompletności.</span><span class="sxs-lookup"><span data-stu-id="670f4-594">The namespace of each controller is shown here for completeness.</span></span> <span data-ttu-id="670f4-595">Jeśli powyższe kontrolery używają tego samego obszaru nazw, wygenerowany zostanie błąd kompilatora.</span><span class="sxs-lookup"><span data-stu-id="670f4-595">If the preceding controllers uses the same namespace, a compiler error would be generated.</span></span> <span data-ttu-id="670f4-596">Przestrzenie nazw klas nie mają wpływu na Routing MVC.</span><span class="sxs-lookup"><span data-stu-id="670f4-596">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="670f4-597">Pierwsze dwa kontrolery są członkami obszarów i pasują tylko wtedy, gdy ich nazwa obszaru jest podana przez wartość `area` trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-597">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="670f4-598">Trzeci kontroler nie jest członkiem żadnego obszaru i może pasować tylko wtedy, gdy żadna wartość `area` nie jest udostępniana przez Routing.</span><span class="sxs-lookup"><span data-stu-id="670f4-598">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

<a name="aa"></a>

<span data-ttu-id="670f4-599">W warunkach pasujących do *nie ma żadnej wartości*, brak wartości `area` jest taka sama jak wtedy, gdy wartość `area` była zerowa lub ciągiem pustym.</span><span class="sxs-lookup"><span data-stu-id="670f4-599">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="670f4-600">Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` jest dostępna jako [wartość otoczenia](#ambient) dla routingu do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-600">When executing an action inside an area, the route value for `area` is available as an [ambient value](#ambient) for routing to use for URL generation.</span></span> <span data-ttu-id="670f4-601">Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="670f4-601">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<span data-ttu-id="670f4-602">Poniższy kod generuje adres URL `/Zebra/Users/AddUser`:</span><span class="sxs-lookup"><span data-stu-id="670f4-602">The following code generates a URL to `/Zebra/Users/AddUser`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a><span data-ttu-id="670f4-603">Definicja akcji</span><span class="sxs-lookup"><span data-stu-id="670f4-603">Action definition</span></span>

<span data-ttu-id="670f4-604">Metody publiczne na kontrolerze, z wyjątkiem tych z atrybutem nie będącym [akcją](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) , są akcjami.</span><span class="sxs-lookup"><span data-stu-id="670f4-604">Public methods on a controller, except those with the [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) attribute, are actions.</span></span>

## <a name="sample-code"></a><span data-ttu-id="670f4-605">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="670f4-605">Sample code</span></span>

 * <span data-ttu-id="670f4-606">Metoda [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) jest dołączana do [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) i służy do wyświetlania informacji o routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-606">The [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) method is included in the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) and is used to display routing information.</span></span>
* <span data-ttu-id="670f4-607">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="670f4-607">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="670f4-608">ASP.NET Core MVC używa [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.</span><span class="sxs-lookup"><span data-stu-id="670f4-608">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="670f4-609">Trasy są zdefiniowane w kodzie startowym lub atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-609">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="670f4-610">Trasy opisują sposób dopasowywania ścieżek adresów URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-610">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="670f4-611">Trasy są również używane do generowania adresów URL (dla linków) wysyłanych w odpowiedziach.</span><span class="sxs-lookup"><span data-stu-id="670f4-611">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="670f4-612">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="670f4-612">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="670f4-613">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="670f4-613">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="670f4-614">Aby uzyskać więcej informacji, zobacz [Routing mieszany](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="670f4-614">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="670f4-615">Ten dokument wyjaśnia interakcje między MVC i routingiem oraz sposób, w jaki typowe aplikacje MVC używają funkcji routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-615">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="670f4-616">Zobacz [Routing](xref:fundamentals/routing) , aby uzyskać szczegółowe informacje na temat routingu zaawansowanego.</span><span class="sxs-lookup"><span data-stu-id="670f4-616">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="670f4-617">Konfigurowanie oprogramowania pośredniczącego routingu</span><span class="sxs-lookup"><span data-stu-id="670f4-617">Setting up Routing Middleware</span></span>

<span data-ttu-id="670f4-618">W metodzie *konfigurowania* może zostać wyświetlony kod podobny do:</span><span class="sxs-lookup"><span data-stu-id="670f4-618">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="670f4-619">Wewnątrz wywołania do `UseMvc`, `MapRoute` służy do tworzenia pojedynczej trasy, która odnosi się do trasy `default`.</span><span class="sxs-lookup"><span data-stu-id="670f4-619">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="670f4-620">Większość aplikacji MVC będzie używać trasy z szablonem podobnym do trasy `default`.</span><span class="sxs-lookup"><span data-stu-id="670f4-620">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="670f4-621">`"{controller=Home}/{action=Index}/{id?}"` szablonu trasy może być zgodna ze ścieżką URL podobną do `/Products/Details/5` i wyodrębniać wartości tras `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki.</span><span class="sxs-lookup"><span data-stu-id="670f4-621">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="670f4-622">MVC podejmie próbę zlokalizowania kontrolera o nazwie `ProductsController` i uruchomi akcję `Details`:</span><span class="sxs-lookup"><span data-stu-id="670f4-622">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="670f4-623">Należy zauważyć, że w tym przykładzie powiązanie modelu będzie używać wartości `id = 5`, aby ustawić parametr `id` do `5` podczas wywoływania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-623">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="670f4-624">Aby uzyskać więcej informacji, zobacz [powiązanie modelu](../models/model-binding.md) .</span><span class="sxs-lookup"><span data-stu-id="670f4-624">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="670f4-625">Użycie trasy `default`:</span><span class="sxs-lookup"><span data-stu-id="670f4-625">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="670f4-626">Szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-626">The route template:</span></span>

* <span data-ttu-id="670f4-627">`{controller=Home}` definiuje `Home` jako `controller` domyślny</span><span class="sxs-lookup"><span data-stu-id="670f4-627">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="670f4-628">`{action=Index}` definiuje `Index` jako `action` domyślny</span><span class="sxs-lookup"><span data-stu-id="670f4-628">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="670f4-629">`{id?}` definiuje `id` jako opcjonalne</span><span class="sxs-lookup"><span data-stu-id="670f4-629">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="670f4-630">Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania.</span><span class="sxs-lookup"><span data-stu-id="670f4-630">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="670f4-631">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="670f4-631">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="670f4-632">`"{controller=Home}/{action=Index}/{id?}"` może być zgodna ze ścieżką URL `/` i spowoduje utworzenie wartości tras `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-632">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="670f4-633">Wartości `controller` i `action` używają wartości domyślnych, `id` nie produkuje wartości, ponieważ w ścieżce URL nie ma odpowiedniego segmentu.</span><span class="sxs-lookup"><span data-stu-id="670f4-633">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="670f4-634">Aby można było wybrać akcję `HomeController` i `Index`, MVC będzie używać tych wartości tras:</span><span class="sxs-lookup"><span data-stu-id="670f4-634">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="670f4-635">Za pomocą tej definicji kontrolera i szablonu trasy, Akcja `HomeController.Index` będzie wykonywana dla dowolnej z następujących ścieżek URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-635">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="670f4-636">Wygodna `UseMvcWithDefaultRoute`metody:</span><span class="sxs-lookup"><span data-stu-id="670f4-636">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="670f4-637">Może służyć do zastępowania:</span><span class="sxs-lookup"><span data-stu-id="670f4-637">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="670f4-638">`UseMvc` i `UseMvcWithDefaultRoute` dodać wystąpienie `RouterMiddleware` do potoku programu pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="670f4-638">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="670f4-639">MVC nie działa bezpośrednio w oprogramowaniu pośredniczącym i używa routingu do obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="670f4-639">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="670f4-640">MVC jest połączony z trasami za pomocą wystąpienia `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="670f4-640">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="670f4-641">Kod w `UseMvc` jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="670f4-641">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="670f4-642">`UseMvc` nie definiuje bezpośrednio żadnych tras, dodaje symbol zastępczy do kolekcji tras dla trasy `attribute`.</span><span class="sxs-lookup"><span data-stu-id="670f4-642">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="670f4-643">`UseMvc(Action<IRouteBuilder>)` Przeciążenie umożliwia dodanie własnych tras, a także obsługuje routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-643">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="670f4-644">`UseMvc` i wszystkie jego odmiany dodają symbol zastępczy dla atrybutu trasa — atrybut jest zawsze dostępny niezależnie od sposobu konfigurowania `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="670f4-644">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="670f4-645">`UseMvcWithDefaultRoute` definiuje domyślną trasę i obsługuje routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-645">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="670f4-646">Sekcja [Routing atrybutów](#attribute-routing-ref-label) zawiera więcej szczegółów dotyczących routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-646">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="670f4-647">Routing konwencjonalny</span><span class="sxs-lookup"><span data-stu-id="670f4-647">Conventional routing</span></span>

<span data-ttu-id="670f4-648">Trasa `default`:</span><span class="sxs-lookup"><span data-stu-id="670f4-648">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="670f4-649">Poprzedni kod jest przykładem konwencjonalnego routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-649">The preceding code is an example of a conventional routing.</span></span> <span data-ttu-id="670f4-650">Ten styl jest nazywany routingiem konwencjonalnym, ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:</span><span class="sxs-lookup"><span data-stu-id="670f4-650">This style is called conventional routing because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="670f4-651">Pierwszy segment ścieżki jest mapowany na nazwę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-651">The first path segment maps to the controller name.</span></span>
* <span data-ttu-id="670f4-652">Druga mapowanie na nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-652">The second maps to the action name.</span></span>
* <span data-ttu-id="670f4-653">Trzeci segment jest używany w przypadku opcjonalnego `id`.</span><span class="sxs-lookup"><span data-stu-id="670f4-653">The third segment is used for an optional `id`.</span></span> <span data-ttu-id="670f4-654">`id` mapuje do jednostki modelu.</span><span class="sxs-lookup"><span data-stu-id="670f4-654">`id` maps to a model entity.</span></span>

<span data-ttu-id="670f4-655">Korzystając z tej `default` trasy, Ścieżka adresu URL `/Products/List` jest mapowana na akcję `ProductsController.List` i `/Blog/Article/17` mapy `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="670f4-655">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="670f4-656">To mapowanie jest oparte **tylko** na nazwach kontrolera i akcji, a nie na podstawie przestrzeni nazw, lokalizacji plików źródłowych ani parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="670f4-656">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="670f4-657">Użycie konwencjonalnego routingu z domyślną trasą umożliwia szybkie Kompilowanie aplikacji bez konieczności podawania nowego wzorca adresu URL dla każdej zdefiniowanej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-657">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="670f4-658">W przypadku aplikacji z akcjami w stylu CRUD zachowanie spójności adresów URL na kontrolerach może pomóc uprościć kod i uczynić interfejs użytkownika bardziej przewidywalny.</span><span class="sxs-lookup"><span data-stu-id="670f4-658">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="670f4-659">`id` jest zdefiniowany jako opcjonalny przez szablon trasy, co oznacza, że akcje mogą być wykonywane bez identyfikatora dostarczonego jako część adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-659">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="670f4-660">Typowo to, co się stanie, jeśli `id` zostanie pominięty w adresie URL, zostanie ustawiony na `0` przez powiązanie modelu i w wyniku tego nie zostanie znaleziona żadna jednostka w `id == 0`dopasowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="670f4-660">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="670f4-661">Routing atrybutu może dać szczegółowy formant, który ma być wymagany dla niektórych akcji, a nie dla innych.</span><span class="sxs-lookup"><span data-stu-id="670f4-661">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="670f4-662">Zgodnie z Konwencją dokumentacja będzie zawierać parametry opcjonalne, takie jak `id`, gdy prawdopodobnie będą widoczne w prawidłowym użyciu.</span><span class="sxs-lookup"><span data-stu-id="670f4-662">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="670f4-663">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="670f4-663">Multiple routes</span></span>

<span data-ttu-id="670f4-664">Można dodać wiele tras wewnątrz `UseMvc`, dodając więcej wywołań do `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="670f4-664">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="670f4-665">Dzięki temu można zdefiniować wiele Konwencji lub dodać trasy konwencjonalne, które są przeznaczone dla konkretnej akcji, na przykład:</span><span class="sxs-lookup"><span data-stu-id="670f4-665">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="670f4-666">Trasa `blog` w tym miejscu jest *dedykowaną tradycyjną trasą*, co oznacza, że używa systemu routingu konwencjonalnego, ale jest przeznaczona dla konkretnej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-666">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="670f4-667">Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i w związku z tym trasa będzie zawsze mapowana na `BlogController.Article`akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-667">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="670f4-668">Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, w jakiej zostały dodane.</span><span class="sxs-lookup"><span data-stu-id="670f4-668">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="670f4-669">W tym przykładzie zostanie podjęta próba `blog` trasy przed trasą `default`.</span><span class="sxs-lookup"><span data-stu-id="670f4-669">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="670f4-670">*Dedykowane konwencjonalne trasy* często korzystają z parametrów trasy **catch-all** , takich jak `{*article}`, do przechwytywania pozostałej części ścieżki URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-670">*Dedicated conventional routes* often use **catch-all** route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="670f4-671">Może to spowodować, że trasa "zbyt zachłanne" oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-671">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="670f4-672">Umieść trasy "zachłanne" później w tabeli tras, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="670f4-672">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="670f4-673">Istnie</span><span class="sxs-lookup"><span data-stu-id="670f4-673">Fallback</span></span>

<span data-ttu-id="670f4-674">W ramach przetwarzania żądań MVC sprawdzi, czy wartości trasy mogą być używane do znajdowania kontrolera i akcji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-674">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="670f4-675">Jeśli wartości trasy nie są zgodne z akcją, trasa nie jest uważana za dopasowanie i zostanie podjęta kolejna trasa.</span><span class="sxs-lookup"><span data-stu-id="670f4-675">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="670f4-676">Jest to nazywane *Fallback*i ma na celu uproszczenie przypadków, w których trasy konwencjonalne nakładają się na siebie.</span><span class="sxs-lookup"><span data-stu-id="670f4-676">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="670f4-677">Niejednoznaczne akcje</span><span class="sxs-lookup"><span data-stu-id="670f4-677">Disambiguating actions</span></span>

<span data-ttu-id="670f4-678">Gdy dwie akcje są zgodne z routingiem, MVC musi odróżnić się, aby wybrać najlepszy kandydat lub w przeciwnym razie zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="670f4-678">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="670f4-679">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="670f4-679">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="670f4-680">Ten kontroler definiuje dwie akcje, które byłyby zgodne ze ścieżką URL `/Products/Edit/17` i trasy `{ controller = Products, action = Edit, id = 17 }`danych.</span><span class="sxs-lookup"><span data-stu-id="670f4-680">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="670f4-681">Jest to typowy wzorzec dla kontrolerów MVC, gdzie `Edit(int)` pokazuje formularz służący do edytowania produktu, a `Edit(int, Product)` przetwarza opublikowany formularz.</span><span class="sxs-lookup"><span data-stu-id="670f4-681">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="670f4-682">Aby ten możliwy składnik MVC musiał wybrać `Edit(int, Product)`, gdy żądanie jest `POST` HTTP i `Edit(int)`, gdy zlecenie HTTP jest inne.</span><span class="sxs-lookup"><span data-stu-id="670f4-682">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="670f4-683">`HttpPostAttribute` (`[HttpPost]`) to implementacja `IActionConstraint`, która będzie zezwalać na wybranie akcji tylko wtedy, gdy zlecenie HTTP jest `POST`.</span><span class="sxs-lookup"><span data-stu-id="670f4-683">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="670f4-684">Obecność `IActionConstraint` powoduje, że `Edit(int, Product)` dopasowanie "lepszy" niż `Edit(int)`, więc `Edit(int, Product)` zostanie podjęta najpierw.</span><span class="sxs-lookup"><span data-stu-id="670f4-684">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="670f4-685">Należy napisać niestandardowe implementacje `IActionConstraint` w wyspecjalizowanych scenariuszach, ale ważne jest, aby zrozumieć rolę atrybutów, takich jak atrybuty `HttpPostAttribute`-like, które są zdefiniowane dla innych zleceń HTTP.</span><span class="sxs-lookup"><span data-stu-id="670f4-685">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="670f4-686">W konwencjonalnym routingu typowe dla akcji używanie tej samej nazwy akcji, gdy są one częścią przepływu pracy `show form -> submit form`.</span><span class="sxs-lookup"><span data-stu-id="670f4-686">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="670f4-687">Wygoda tego wzorca stanie się bardziej oczywista po przejrzeniu sekcji [zrozumienie IActionConstraint](#understanding-iactionconstraint) .</span><span class="sxs-lookup"><span data-stu-id="670f4-687">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="670f4-688">Jeśli wiele pasujących tras i MVC nie mogą znaleźć "najlepszej" trasy, wygeneruje `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="670f4-688">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="670f4-689">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="670f4-689">Route names</span></span>

<span data-ttu-id="670f4-690">Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwami tras:</span><span class="sxs-lookup"><span data-stu-id="670f4-690">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="670f4-691">Nazwy tras nadaj logicznej nazwie trasy, aby nazwana trasa mogła być używana do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-691">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="670f4-692">Znacznie upraszcza to tworzenie adresów URL, gdy porządkowanie tras może spowodować skomplikowane generowanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-692">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="670f4-693">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-693">Route names must be unique application-wide.</span></span>

<span data-ttu-id="670f4-694">Nazwy tras nie mają wpływu na Dopasowywanie adresów URL ani obsługę żądań; są one używane tylko do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-694">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="670f4-695">[Routing](xref:fundamentals/routing) zawiera bardziej szczegółowe informacje na temat generowania adresów URL, w tym generowanie adresów URL w pomocnikach specyficznych dla MVC.</span><span class="sxs-lookup"><span data-stu-id="670f4-695">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="670f4-696">Routing atrybutów</span><span class="sxs-lookup"><span data-stu-id="670f4-696">Attribute routing</span></span>

<span data-ttu-id="670f4-697">Funkcja routingu atrybutów używa zestawu atrybutów do mapowania akcji bezpośrednio do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-697">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="670f4-698">W poniższym przykładzie `app.UseMvc();` jest używany w metodzie `Configure` i nie jest przenoszona żadna trasa.</span><span class="sxs-lookup"><span data-stu-id="670f4-698">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="670f4-699">`HomeController` będzie zgodna z zestawem adresów URL podobnym do tego, co `{controller=Home}/{action=Index}/{id?}` trasie domyślnej:</span><span class="sxs-lookup"><span data-stu-id="670f4-699">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="670f4-700">Akcja `HomeController.Index()` będzie wykonywana dla dowolnej ścieżki URL `/`, `/Home`lub `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="670f4-700">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="670f4-701">W tym przykładzie przedstawiono najważniejsze różnice programistyczne między routingiem atrybutów i routingiem konwencjonalnym.</span><span class="sxs-lookup"><span data-stu-id="670f4-701">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="670f4-702">Routing atrybutów wymaga więcej danych wejściowych w celu określenia trasy; konwencjonalne trasy domyślne obsługuje trasy bardziej zwięzłie.</span><span class="sxs-lookup"><span data-stu-id="670f4-702">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="670f4-703">Jednak Routing atrybutu zezwala na (i wymaga) precyzyjnej kontroli, które szablony tras mają zastosowanie do poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-703">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="670f4-704">W przypadku routingu atrybutów nazwa kontrolera i nazwy akcji nie odgrywają **żadnej** roli, w której akcja jest zaznaczona.</span><span class="sxs-lookup"><span data-stu-id="670f4-704">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="670f4-705">Ten przykład będzie pasował do tych samych adresów URL, co w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="670f4-705">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="670f4-706">Powyższe szablony tras nie definiują parametrów trasy dla `action`, `area`i `controller`.</span><span class="sxs-lookup"><span data-stu-id="670f4-706">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="670f4-707">W rzeczywistości te parametry tras są niedozwolone w trasach atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-707">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="670f4-708">Ponieważ szablon trasy jest już skojarzony z akcją, nie ma sensu analizy nazwy akcji na podstawie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-708">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="670f4-709">Routing atrybutów z atrybutami http [Verb]</span><span class="sxs-lookup"><span data-stu-id="670f4-709">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="670f4-710">Routing atrybutu może również używać atrybutów `Http[Verb]`, takich jak `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="670f4-710">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="670f4-711">Wszystkie te atrybuty mogą akceptować szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-711">All of these attributes can accept a route template.</span></span> <span data-ttu-id="670f4-712">Ten przykład przedstawia dwie akcje, które pasują do tego samego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-712">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="670f4-713">Dla ścieżki URL, takiej jak `/products` akcja `ProductsApi.ListProducts` zostanie wykonana, gdy zlecenie HTTP zostanie `GET` i `ProductsApi.CreateProduct` zostanie wykonane, gdy zlecenie HTTP jest `POST`.</span><span class="sxs-lookup"><span data-stu-id="670f4-713">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="670f4-714">Routing atrybutu najpierw jest zgodny z adresem URL względem zestawu szablonów tras zdefiniowanych przez atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-714">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="670f4-715">Po dopasowaniu szablonu trasy do określenia, które akcje mogą być wykonywane, są stosowane ograniczenia `IActionConstraint`.</span><span class="sxs-lookup"><span data-stu-id="670f4-715">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="670f4-716">Podczas kompilowania interfejsu API REST trudno jest użyć `[Route(...)]` na metodę akcji, ponieważ akcja akceptuje wszystkie metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="670f4-716">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="670f4-717">Lepiej jest używać bardziej szczegółowych `Http*Verb*Attributes`, aby precyzyjnie dowiedzieć się, co obsługuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="670f4-717">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="670f4-718">Klienci interfejsów API REST powinni wiedzieć, jakie ścieżki i czasowniki HTTP mapują na określone operacje logiczne.</span><span class="sxs-lookup"><span data-stu-id="670f4-718">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="670f4-719">Ponieważ atrybut Route ma zastosowanie do określonej akcji, można łatwo wprowadzić parametry wymagane jako część definicji szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-719">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="670f4-720">W tym przykładzie `id` jest wymagane jako część ścieżki URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-720">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="670f4-721">Akcja `ProductsApi.GetProduct(int)` zostanie wykonana dla ścieżki URL, takiej jak `/products/3`, ale nie dla ścieżki URL, takiej jak `/products`.</span><span class="sxs-lookup"><span data-stu-id="670f4-721">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="670f4-722">Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="670f4-722">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="670f4-723">Nazwa trasy</span><span class="sxs-lookup"><span data-stu-id="670f4-723">Route Name</span></span>

<span data-ttu-id="670f4-724">Poniższy kod definiuje *nazwę trasy* `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="670f4-724">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="670f4-725">Nazwy tras mogą służyć do generowania adresów URL na podstawie określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-725">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="670f4-726">Nazwy tras nie mają wpływu na zachowanie routingu w adresie URL i są używane tylko na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-726">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="670f4-727">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-727">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="670f4-728">W przeciwieństwie do konwencjonalnej *trasy domyślnej*, która definiuje `id` parametr jako opcjonalny (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="670f4-728">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="670f4-729">Ta możliwość precyzyjnego określania interfejsów API ma zalety, takich jak umożliwienie `/products` i `/products/5` do wysłania do różnych akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-729">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="670f4-730">Łączenie tras</span><span class="sxs-lookup"><span data-stu-id="670f4-730">Combining routes</span></span>

<span data-ttu-id="670f4-731">Aby mniej powtarzać Routing atrybutów, atrybuty trasy na kontrolerze są łączone z atrybutami trasy dla poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-731">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="670f4-732">Wszystkie szablony tras zdefiniowane na kontrolerze są poprzedzone w celu rozesłania szablonów w akcjach.</span><span class="sxs-lookup"><span data-stu-id="670f4-732">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="670f4-733">Umieszczenie atrybutu trasy na kontrolerze powoduje, że **wszystkie** akcje w kontrolerze używają routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-733">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="670f4-734">W tym przykładzie ścieżka adresu URL `/products` może być zgodna `ProductsApi.ListProducts`, a ścieżka URL `/products/5` może pasować `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="670f4-734">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="670f4-735">Obie te akcje pasują do `GET` HTTP, ponieważ są oznaczone `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="670f4-735">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="670f4-736">Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są łączone z szablonami tras zastosowanymi do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-736">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="670f4-737">Ten przykład dopasowuje zestaw ścieżek URL podobny do *trasy domyślnej*.</span><span class="sxs-lookup"><span data-stu-id="670f4-737">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="670f4-738">Określanie kolejności tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="670f4-738">Ordering attribute routes</span></span>

<span data-ttu-id="670f4-739">W przeciwieństwie do konwencjonalnych tras, które są wykonywane w określonej kolejności, routing atrybutu kompiluje drzewo i dopasowuje wszystkie trasy jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="670f4-739">In contrast to conventional routes, which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="670f4-740">Zachowuje się tak, jakby wpisy trasy zostały umieszczone w idealnym porządku; najbardziej konkretne trasy mają możliwość wykonania przed bardziej ogólnymi trasami.</span><span class="sxs-lookup"><span data-stu-id="670f4-740">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="670f4-741">Przykładowo trasa, taka jak `blog/search/{topic}`, jest bardziej szczegółowa niż trasa, taka jak `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="670f4-741">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="670f4-742">Logicznie domyślnie mówiąc `blog/search/{topic}` trasę "uruchomienia", ponieważ jest to jedyna rozsądna kolejność.</span><span class="sxs-lookup"><span data-stu-id="670f4-742">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="670f4-743">Przy użyciu konwencjonalnego routingu deweloper jest odpowiedzialny za umieszczanie tras w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-743">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="670f4-744">Trasy atrybutów mogą konfigurować kolejność przy użyciu właściwości `Order` wszystkich atrybutów tras dostarczonych przez platformę.</span><span class="sxs-lookup"><span data-stu-id="670f4-744">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="670f4-745">Trasy są przetwarzane zgodnie z rosnącym sortowaniem właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-745">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="670f4-746">Kolejność domyślna to `0`.</span><span class="sxs-lookup"><span data-stu-id="670f4-746">The default order is `0`.</span></span> <span data-ttu-id="670f4-747">Ustawienie trasy przy użyciu `Order = -1` zostanie uruchomione przed trasami, które nie ustawiają kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-747">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="670f4-748">Ustawienie trasy przy użyciu `Order = 1` zostanie uruchomione po określeniu domyślnej kolejności tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-748">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="670f4-749">Należy unikać w zależności od `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-749">Avoid depending on `Order`.</span></span> <span data-ttu-id="670f4-750">Jeśli przestrzeń adresów URL wymaga jawnych wartości kolejności, aby można było prawidłowo kierować trasy, to prawdopodobnie również jest myląca dla klientów.</span><span class="sxs-lookup"><span data-stu-id="670f4-750">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="670f4-751">W ogólnym routingu atrybutów wybierz prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-751">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="670f4-752">Jeśli domyślna kolejność generowania adresów URL nie działa, użycie nazwy trasy jako przesłonięcia jest zwykle prostsze niż stosowanie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-752">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="670f4-753">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="670f4-753">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="670f4-754">Informacje o zamówieniu trasy w Razor Pages tematy są dostępne na stronie [Razor Pages trasy i konwencje aplikacji: kolejność tras](xref:razor-pages/razor-pages-conventions#route-order).</span><span class="sxs-lookup"><span data-stu-id="670f4-754">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="670f4-755">Zastępowanie tokenu w szablonach tras ([Controller], [Action], [obszar])</span><span class="sxs-lookup"><span data-stu-id="670f4-755">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="670f4-756">Dla wygody, trasy atrybutu obsługują *zastępowanie tokenów* przez umieszczanie tokenu w nawiasach kwadratowych (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="670f4-756">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="670f4-757">Tokeny `[action]`, `[area]`i `[controller]` są zastępowane wartościami nazwy akcji, obszaru i nazwy kontrolera z akcji, w której jest zdefiniowana trasa.</span><span class="sxs-lookup"><span data-stu-id="670f4-757">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="670f4-758">W poniższym przykładzie akcje są zgodne ze ścieżkami URL zgodnie z opisem w komentarzach:</span><span class="sxs-lookup"><span data-stu-id="670f4-758">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="670f4-759">Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-759">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="670f4-760">Powyższy przykład będzie zachowywać się tak samo jak poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="670f4-760">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="670f4-761">Trasy atrybutu można także łączyć z dziedziczeniem.</span><span class="sxs-lookup"><span data-stu-id="670f4-761">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="670f4-762">Jest to szczególnie zaawansowane w połączeniu z zastępowaniem tokenu.</span><span class="sxs-lookup"><span data-stu-id="670f4-762">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="670f4-763">Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-763">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="670f4-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generuje unikatową nazwę trasy dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-764">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="670f4-765">Aby dopasować ogranicznik zastępczy tokenu literału `[` lub `]`, należy to zrobić, powtarzając znak (`[[` lub `]]`).</span><span class="sxs-lookup"><span data-stu-id="670f4-765">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="670f4-766">Używanie transformatora parametrów do dostosowywania zastępowania tokenu</span><span class="sxs-lookup"><span data-stu-id="670f4-766">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="670f4-767">Zastępowanie tokenu można dostosować za pomocą transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="670f4-767">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="670f4-768">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="670f4-768">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="670f4-769">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="670f4-769">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="670f4-770">`RouteTokenTransformerConvention` jest konwencją modelu aplikacji, która:</span><span class="sxs-lookup"><span data-stu-id="670f4-770">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="670f4-771">Stosuje transformator parametrów do wszystkich tras atrybutów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-771">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="670f4-772">Dostosowuje wartości tokenów tras w miarę ich wymiany.</span><span class="sxs-lookup"><span data-stu-id="670f4-772">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="670f4-773">`RouteTokenTransformerConvention` jest zarejestrowany jako opcja w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="670f4-773">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
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


::: moniker range="< aspnetcore-3.0"
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="670f4-774">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="670f4-774">Multiple Routes</span></span>

<span data-ttu-id="670f4-775">Routing atrybutów obsługuje definiowanie wielu tras, które docierają do tej samej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-775">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="670f4-776">Najczęstszym sposobem użycia tej metody jest naśladowanie zachowania *domyślnej trasy konwencjonalnej* , jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="670f4-776">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="670f4-777">Umieszczenie wielu atrybutów trasy na kontrolerze oznacza, że każda z nich będzie łączyć się z poszczególnymi atrybutami trasy w metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-777">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="670f4-778">Gdy wiele atrybutów trasy (implementujących `IActionConstraint`) jest umieszczanych w akcji, każde ograniczenie akcji łączy się z szablonem trasy z atrybutu, który go zdefiniował.</span><span class="sxs-lookup"><span data-stu-id="670f4-778">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="670f4-779">Chociaż używanie wielu tras w akcjach może wydawać się zaawansowane, lepiej jest zachować prosty i dobrze zdefiniowany obszar adresów URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-779">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="670f4-780">Użyj wielu tras w przypadku akcji tylko wtedy, gdy jest to konieczne, na przykład do obsługi istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="670f4-780">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="670f4-781">Określanie opcjonalnych parametrów trasy, wartości domyślnych i ograniczeń</span><span class="sxs-lookup"><span data-stu-id="670f4-781">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="670f4-782">Trasy atrybutów obsługują tę samą składnię wbudowaną co konwencjonalne trasy, aby określić parametry opcjonalne, wartości domyślne i ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="670f4-782">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="670f4-783">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="670f4-783">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="670f4-784">Niestandardowe atrybuty trasy przy użyciu `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="670f4-784">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="670f4-785">Wszystkie atrybuty trasy podane w strukturze (`[Route(...)]`, `[HttpGet(...)]` itp.) implementują interfejs `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="670f4-785">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="670f4-786">MVC szuka atrybutów klas kontrolera i metod akcji, gdy aplikacja zostanie uruchomiona i używa tych, które implementują `IRouteTemplateProvider`, aby skompilować początkowy zestaw tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-786">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="670f4-787">Możesz zaimplementować `IRouteTemplateProvider`, aby zdefiniować własne atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-787">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="670f4-788">Każda `IRouteTemplateProvider` umożliwia zdefiniowanie pojedynczej trasy z niestandardowym szablonem trasy, kolejnością i nazwą:</span><span class="sxs-lookup"><span data-stu-id="670f4-788">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="670f4-789">Ten atrybut z powyższego przykładu automatycznie ustawia `Template` do `"api/[controller]"` w przypadku zastosowania `[MyApiController]`.</span><span class="sxs-lookup"><span data-stu-id="670f4-789">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="670f4-790">Dostosowywanie tras atrybutów przy użyciu modelu aplikacji</span><span class="sxs-lookup"><span data-stu-id="670f4-790">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="670f4-791">*Model aplikacji* jest modelem obiektów tworzonym podczas uruchamiania ze wszystkimi metadanymi używanymi przez MVC do kierowania i wykonywania akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-791">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="670f4-792">*Model aplikacji* zawiera wszystkie dane zebrane z atrybutów tras (za pośrednictwem `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="670f4-792">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="670f4-793">Można napisać *konwencje* , aby zmodyfikować model aplikacji w czasie uruchamiania, aby dostosować sposób zachowania routingu.</span><span class="sxs-lookup"><span data-stu-id="670f4-793">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="670f4-794">W tej sekcji przedstawiono prosty przykład dostosowywania routingu przy użyciu modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-794">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="670f4-795">Routing mieszany: Routing atrybutów a konwencjonalny Routing</span><span class="sxs-lookup"><span data-stu-id="670f4-795">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="670f4-796">Aplikacje MVC mogą łączyć użycie konwencjonalnego routingu i routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="670f4-796">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="670f4-797">Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.</span><span class="sxs-lookup"><span data-stu-id="670f4-797">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="670f4-798">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="670f4-798">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="670f4-799">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="670f4-799">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="670f4-800">Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa.</span><span class="sxs-lookup"><span data-stu-id="670f4-800">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="670f4-801">**Każdy** atrybut trasy na kontrolerze powoduje kierowanie wszystkich akcji w atrybucie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="670f4-801">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="670f4-802">Co odróżnia dwa typy systemów routingu, proces stosowany po adresie URL pasuje do szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-802">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="670f4-803">W przypadku routingu konwencjonalnego wartości trasy z dopasowania są używane do wybierania akcji i kontrolera z tabeli odnośników wszystkich konwencjonalnych akcji kierowanych.</span><span class="sxs-lookup"><span data-stu-id="670f4-803">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="670f4-804">W obszarze Routing atrybutów każdy szablon jest już skojarzony z akcją i nie jest wymagany dalsze wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="670f4-804">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="670f4-805">Złożone segmenty</span><span class="sxs-lookup"><span data-stu-id="670f4-805">Complex segments</span></span>

<span data-ttu-id="670f4-806">Złożone segmenty (na przykład `[Route("/dog{token}cat")]`) są przetwarzane przez dopasowanie literałów od prawej do lewej w sposób niezachłanney.</span><span class="sxs-lookup"><span data-stu-id="670f4-806">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="670f4-807">Sprawdź [kod źródłowy](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) opisu.</span><span class="sxs-lookup"><span data-stu-id="670f4-807">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="670f4-808">Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="670f4-808">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="670f4-809">Generowanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="670f4-809">URL Generation</span></span>

<span data-ttu-id="670f4-810">Aplikacje MVC mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-810">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="670f4-811">Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany.</span><span class="sxs-lookup"><span data-stu-id="670f4-811">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="670f4-812">Ta sekcja koncentruje się na funkcjach generowania adresów URL dostarczonych przez MVC i obejmuje tylko podstawowe informacje na temat sposobu działania generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-812">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="670f4-813">Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="670f4-813">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="670f4-814">Interfejs `IUrlHelper` jest podstawową częścią infrastruktury między MVC i routingiem na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-814">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="670f4-815">Wystąpienie `IUrlHelper` dostępne za pomocą właściwości `Url` w obszarze Kontrolery, widoki i składniki widoku.</span><span class="sxs-lookup"><span data-stu-id="670f4-815">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="670f4-816">W tym przykładzie interfejs `IUrlHelper` jest używany przez właściwość `Controller.Url` do generowania adresu URL dla innej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-816">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="670f4-817">Jeśli aplikacja używa domyślnej trasy konwencjonalnej, wartością zmiennej `url` będzie ciąg ścieżki adresu URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="670f4-817">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="670f4-818">Ta ścieżka URL jest tworzona za pomocą routingu przez połączenie wartości trasy z bieżącego żądania (wartości otoczenia), z wartościami przekazanymi do `Url.Action` i podstawianie tych wartości do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="670f4-818">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="670f4-819">Każdy parametr trasy w szablonie trasy ma swoją wartość zastępowaną przez pasujące nazwy wartościami i wartościami otoczenia.</span><span class="sxs-lookup"><span data-stu-id="670f4-819">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="670f4-820">Parametr trasy, który nie ma wartości, może korzystać z wartości domyślnej, jeśli ma taką wartość, lub być pominięty, jeśli jest opcjonalny (tak jak w przypadku `id` w tym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="670f4-820">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="670f4-821">Generowanie adresu URL zakończy się niepowodzeniem, jeśli którykolwiek z wymaganych parametrów trasy nie ma odpowiadającej wartości.</span><span class="sxs-lookup"><span data-stu-id="670f4-821">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="670f4-822">Jeśli generowanie adresów URL kończy się niepowodzeniem dla trasy, kolejna trasa zostanie ponowiona do momentu przetworzenia wszystkich tras lub znalezienia dopasowania.</span><span class="sxs-lookup"><span data-stu-id="670f4-822">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="670f4-823">Na przykład `Url.Action` powyżej zakłada się, że konwencjonalne Routing, ale generowanie adresów URL działa podobnie z routingiem atrybutów, chociaż koncepcje różnią się.</span><span class="sxs-lookup"><span data-stu-id="670f4-823">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="670f4-824">W przypadku routingu konwencjonalnego wartości tras są używane do rozszerzania szablonu, a wartości trasy dla `controller` i `action` zazwyczaj pojawiają się w tym szablonie — działa to, ponieważ adresy URL dopasowane przez Routing są zgodne z *Konwencją*.</span><span class="sxs-lookup"><span data-stu-id="670f4-824">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="670f4-825">W obszarze Routing atrybutów wartości trasy dla `controller` i `action` nie mogą być wyświetlane w szablonie — są one używane do wyszukiwania szablonu do użycia.</span><span class="sxs-lookup"><span data-stu-id="670f4-825">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="670f4-826">W tym przykładzie zastosowano Routing atrybutów:</span><span class="sxs-lookup"><span data-stu-id="670f4-826">This example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="670f4-827">MVC kompiluje tabelę odnośników wszystkich akcji przypisanych do atrybutu i dopasowuje wartości `controller` i `action`, aby wybrać szablon trasy do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-827">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="670f4-828">W przykładzie powyżej jest generowana `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="670f4-828">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="670f4-829">Generowanie adresów URL według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="670f4-829">Generating URLs by action name</span></span>

<span data-ttu-id="670f4-830">`Url.Action` (`IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="670f4-830">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="670f4-831">`Action`) i wszystkie powiązane przeciążenia są oparte na tym pomysłie, aby określić, do czego chcesz utworzyć łącze, określając nazwę kontrolera i nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-831">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="670f4-832">W przypadku korzystania z `Url.Action`, bieżące wartości trasy dla `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` są częścią *wartości otoczenia* **i** *wartości*.</span><span class="sxs-lookup"><span data-stu-id="670f4-832">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="670f4-833">Metoda `Url.Action`, zawsze używa bieżących wartości `action` i `controller` i wygeneruje ścieżkę URL, która kieruje do bieżącej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-833">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="670f4-834">Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały wprowadzone podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-834">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="670f4-835">Przy użyciu trasy, takiej jak `{a}/{b}/{c}/{d}` i wartości otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma wystarczającą ilość informacji do wygenerowania adresu URL bez dodatkowych wartości — ponieważ wszystkie parametry trasy mają wartość.</span><span class="sxs-lookup"><span data-stu-id="670f4-835">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="670f4-836">W przypadku dodania wartości `{ d = Donovan }`wartość `{ d = David }` zostanie zignorowana, a wygenerowana Ścieżka adresu URL zostanie `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="670f4-836">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="670f4-837">Ścieżki URL są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="670f4-837">URL paths are hierarchical.</span></span> <span data-ttu-id="670f4-838">W powyższym przykładzie, jeśli dodano wartość `{ c = Cheryl }`, obie wartości `{ c = Carol, d = David }` zostałyby zignorowane.</span><span class="sxs-lookup"><span data-stu-id="670f4-838">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="670f4-839">W takim przypadku nie ma już wartości `d` a generowanie adresów URL zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="670f4-839">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="670f4-840">Należy określić pożądaną wartość `c` i `d`.</span><span class="sxs-lookup"><span data-stu-id="670f4-840">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="670f4-841">Może wystąpić potrzeba napotkania tego problemu przy użyciu trasy domyślnej (`{controller}/{action}/{id?}`), ale w takim przypadku rzadko wystąpi to zachowanie, ponieważ `Url.Action` będzie zawsze jawnie określać `controller` i `action` wartość.</span><span class="sxs-lookup"><span data-stu-id="670f4-841">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="670f4-842">Dłuższe przeciążenia `Url.Action` również pobierają dodatkowy obiekt *wartości trasy* , aby zapewnić wartości parametrów trasy innych niż `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="670f4-842">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="670f4-843">Najczęściej zobaczysz ten sposób, który jest używany z `id`, takich jak `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="670f4-843">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="670f4-844">Według Konwencji obiekt *wartości trasy* jest zwykle obiektem typu anonimowego, ale może również być `IDictionary<>` lub *zwykłym starym obiektem platformy .NET*.</span><span class="sxs-lookup"><span data-stu-id="670f4-844">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="670f4-845">Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="670f4-845">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="670f4-846">Aby utworzyć bezwzględny adres URL, Użyj przeciążenia, które akceptuje `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="670f4-846">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="670f4-847">Generowanie adresów URL według trasy</span><span class="sxs-lookup"><span data-stu-id="670f4-847">Generating URLs by route</span></span>

<span data-ttu-id="670f4-848">Powyższy kod wygeneruje adres URL przez przekazanie go do kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-848">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="670f4-849">`IUrlHelper` zapewnia także `Url.RouteUrl` rodziny metod.</span><span class="sxs-lookup"><span data-stu-id="670f4-849">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="670f4-850">Te metody są podobne do `Url.Action`, ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-850">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="670f4-851">Najbardziej typowym zastosowaniem jest określenie nazwy trasy do użycia określonej trasy do wygenerowania adresu URL, na ogół *bez* określania kontrolera lub nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-851">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="670f4-852">Generowanie adresów URL w kodzie HTML</span><span class="sxs-lookup"><span data-stu-id="670f4-852">Generating URLs in HTML</span></span>

<span data-ttu-id="670f4-853">`IHtmlHelper` udostępnia metody `HtmlHelper`, `Html.BeginForm` i `Html.ActionLink` generować odpowiednio elementy `<form>` i `<a>`.</span><span class="sxs-lookup"><span data-stu-id="670f4-853">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="670f4-854">Te metody używają metody `Url.Action` do wygenerowania adresu URL i akceptują podobne argumenty.</span><span class="sxs-lookup"><span data-stu-id="670f4-854">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="670f4-855">`Url.RouteUrl` pomocników dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink`, które mają podobną funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="670f4-855">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="670f4-856">TagHelpers Generuj adresy URL za pomocą `form` TagHelper i `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="670f4-856">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="670f4-857">Oba te zastosowania `IUrlHelper` do ich implementacji.</span><span class="sxs-lookup"><span data-stu-id="670f4-857">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="670f4-858">Aby uzyskać więcej informacji, zobacz [Praca z formularzami](../views/working-with-forms.md) .</span><span class="sxs-lookup"><span data-stu-id="670f4-858">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="670f4-859">W widokach, `IUrlHelper` jest dostępna za pomocą właściwości `Url` dla dowolnej generacji adresów URL ad hoc, które nie są objęte powyższym.</span><span class="sxs-lookup"><span data-stu-id="670f4-859">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="670f4-860">Generowanie adresów URL w wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="670f4-860">Generating URLS in Action Results</span></span>

<span data-ttu-id="670f4-861">Powyższe przykłady przedstawiono przy użyciu `IUrlHelper` w kontrolerze, podczas gdy najbardziej typowym użyciem na kontrolerze jest wygenerowanie adresu URL jako części wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-861">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="670f4-862">Klasy bazowe `ControllerBase` i `Controller` zapewniają wygodne metody dla wyników akcji, które odwołują się do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-862">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="670f4-863">Jednym z typowych zastosowań jest przekierowanie po zaakceptowaniu danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="670f4-863">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

<span data-ttu-id="670f4-864">Metody fabryki wyników akcji postępują zgodnie z podobnym wzorcem do metod w `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="670f4-864">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="670f4-865">Specjalny przypadek dla dedykowanych tras konwencjonalnych</span><span class="sxs-lookup"><span data-stu-id="670f4-865">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="670f4-866">Funkcja routingu konwencjonalnego może używać specjalnego rodzaju definicji trasy o nazwie *dedykowanej, konwencjonalnej trasy*.</span><span class="sxs-lookup"><span data-stu-id="670f4-866">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="670f4-867">W poniższym przykładzie trasa o nazwie `blog` jest dedykowaną umowną trasą.</span><span class="sxs-lookup"><span data-stu-id="670f4-867">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="670f4-868">Korzystając z tych definicji tras, `Url.Action("Index", "Home")` wygeneruje ścieżkę URL `/` z trasą `default`, ale dlaczego?</span><span class="sxs-lookup"><span data-stu-id="670f4-868">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="670f4-869">Możesz odgadnąć wartości tras `{ controller = Home, action = Index }` wystarcza do wygenerowania adresu URL przy użyciu `blog`, a wynik zostanie `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="670f4-869">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="670f4-870">Dedykowane konwencjonalne trasy polegają na specjalnym zachowaniu wartości domyślnych, które nie mają odpowiadającego parametru trasy, który uniemożliwia trasie "zbyt zachłanne" z generowaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-870">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="670f4-871">W takim przypadku wartości domyślne są `{ controller = Blog, action = Article }`, a ani `controller`, ani `action` nie pojawiają się jako parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-871">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="670f4-872">Gdy Routing wykonuje generowanie adresów URL, podane wartości muszą być zgodne z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="670f4-872">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="670f4-873">Generowanie adresu URL przy użyciu `blog` nie powiedzie się, ponieważ wartości `{ controller = Home, action = Index }` nie pasują do `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-873">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="670f4-874">Następnie usługa routingu wraca do wypróbowania `default`, która kończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="670f4-874">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="670f4-875">Obszary</span><span class="sxs-lookup"><span data-stu-id="670f4-875">Areas</span></span>

<span data-ttu-id="670f4-876">[Obszary](areas.md) są funkcją MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw routingu (dla akcji kontrolera) i struktury folderów (dla widoków).</span><span class="sxs-lookup"><span data-stu-id="670f4-876">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="670f4-877">Użycie obszarów umożliwia aplikacji posiadanie wielu kontrolerów o tej samej nazwie, o ile mają one różne *obszary*.</span><span class="sxs-lookup"><span data-stu-id="670f4-877">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="670f4-878">Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie innego parametru trasy, `area` do `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="670f4-878">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="670f4-879">W tej sekcji omówiono sposób, w jaki Routing współdziała z obszarami — zobacz [obszary](areas.md) , aby uzyskać szczegółowe informacje o tym, jak obszary są używane w widokach.</span><span class="sxs-lookup"><span data-stu-id="670f4-879">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="670f4-880">Poniższy przykład konfiguruje MVC do używania domyślnej trasy konwencjonalnej i *trasy obszaru* dla obszaru o nazwie `Blog`:</span><span class="sxs-lookup"><span data-stu-id="670f4-880">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="670f4-881">W przypadku dopasowania ścieżki URL, takiej jak `/Manage/Users/AddUser`, Pierwsza trasa będzie generować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-881">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="670f4-882">`area` wartość trasy jest generowana przez wartość domyślną dla `area`, w rzeczywistości trasa utworzona przez `MapAreaRoute` jest równoważna następującym:</span><span class="sxs-lookup"><span data-stu-id="670f4-882">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="670f4-883">`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dla `area` przy użyciu podanej nazwy obszaru, w tym przypadku `Blog`.</span><span class="sxs-lookup"><span data-stu-id="670f4-883">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="670f4-884">Wartość domyślna zapewnia, że trasa zawsze generuje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-884">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="670f4-885">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="670f4-885">Conventional routing is order-dependent.</span></span> <span data-ttu-id="670f4-886">Ogólnie rzecz biorąc, trasy z obszarami należy umieścić wcześniej w tabeli tras, ponieważ są one bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="670f4-886">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="670f4-887">Korzystając z powyższego przykładu, wartości trasy będą zgodne z następującą akcją:</span><span class="sxs-lookup"><span data-stu-id="670f4-887">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="670f4-888">`AreaAttribute` to oznacza, że w ramach obszaru znajduje się kontroler. Załóżmy, że ten kontroler znajduje się w obszarze `Blog`.</span><span class="sxs-lookup"><span data-stu-id="670f4-888">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="670f4-889">Kontrolery bez atrybutu `[Area]` nie są członkami żadnego obszaru i **nie** będą zgodne, gdy wartość trasy `area` zostanie udostępniona przez Routing.</span><span class="sxs-lookup"><span data-stu-id="670f4-889">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="670f4-890">W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="670f4-890">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="670f4-891">Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu w celu zapewnienia kompletności — w przeciwnym razie kontrolery będą mieć konflikt nazw i generują błąd kompilatora.</span><span class="sxs-lookup"><span data-stu-id="670f4-891">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="670f4-892">Przestrzenie nazw klas nie mają wpływu na Routing MVC.</span><span class="sxs-lookup"><span data-stu-id="670f4-892">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="670f4-893">Pierwsze dwa kontrolery są członkami obszarów i pasują tylko wtedy, gdy ich nazwa obszaru jest podana przez wartość `area` trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-893">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="670f4-894">Trzeci kontroler nie jest członkiem żadnego obszaru i może pasować tylko wtedy, gdy żadna wartość `area` nie jest udostępniana przez Routing.</span><span class="sxs-lookup"><span data-stu-id="670f4-894">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="670f4-895">W warunkach pasujących do *nie ma żadnej wartości*, brak wartości `area` jest taka sama jak wtedy, gdy wartość `area` była zerowa lub ciągiem pustym.</span><span class="sxs-lookup"><span data-stu-id="670f4-895">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="670f4-896">Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` będzie dostępna jako *wartość otoczenia* dla routingu do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-896">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="670f4-897">Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="670f4-897">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="670f4-898">Zrozumienie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="670f4-898">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="670f4-899">Ta sekcja jest głęboką szczegółoweą wewnętrznych struktur oraz jak MVC wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="670f4-899">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="670f4-900">Typowa aplikacja nie będzie potrzebować niestandardowego `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="670f4-900">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="670f4-901">Prawdopodobnie już używasz `IActionConstraint`, nawet jeśli nie masz doświadczenia z interfejsem.</span><span class="sxs-lookup"><span data-stu-id="670f4-901">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="670f4-902">Atrybut `[HttpGet]` i podobne atrybuty `[Http-VERB]` implementują `IActionConstraint` w celu ograniczenia wykonywania metody akcji.</span><span class="sxs-lookup"><span data-stu-id="670f4-902">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="670f4-903">Przy założeniu domyślnej trasy konwencjonalnej, Ścieżka adresu URL `/Products/Edit` będzie generować wartości `{ controller = Products, action = Edit }`, które byłyby zgodne z **obiema** akcjami przedstawionymi w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="670f4-903">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="670f4-904">W `IActionConstraint` terminologia, że obie te akcje są uważane za kandydatów, ponieważ obydwie pasują do danych tras.</span><span class="sxs-lookup"><span data-stu-id="670f4-904">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="670f4-905">Gdy `HttpGetAttribute` wykonuje, zobaczy, że *Edycja ()* jest dopasowaniem do *Get* i nie jest dopasowaniem dla żadnego innego zlecenia http.</span><span class="sxs-lookup"><span data-stu-id="670f4-905">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="670f4-906">Akcja `Edit(...)` nie ma zdefiniowanych żadnych ograniczeń i dlatego będzie pasować do dowolnego zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="670f4-906">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="670f4-907">Tak więc założono, że `Edit(...)` są zgodne tylko `POST`.</span><span class="sxs-lookup"><span data-stu-id="670f4-907">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="670f4-908">Jednak w przypadku `GET` obie akcje mogą nadal być zgodne, jednak akcja z `IActionConstraint` jest zawsze uznawana za *lepszą* niż akcja bez.</span><span class="sxs-lookup"><span data-stu-id="670f4-908">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="670f4-909">Dlatego `Edit()` ma `[HttpGet]` jest uważany za bardziej szczegółowy i zostanie wybrany, jeśli obie akcje mogą być zgodne.</span><span class="sxs-lookup"><span data-stu-id="670f4-909">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="670f4-910">Koncepcyjnie, `IActionConstraint` jest formą *przeciążania*, ale zamiast przeciążania metod o tej samej nazwie, przeciążanie między akcjami, które pasują do tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="670f4-910">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="670f4-911">Funkcja routingu atrybutów używa również `IActionConstraint` i może spowodować, że akcje z różnych kontrolerów są uznawane za kandydatów.</span><span class="sxs-lookup"><span data-stu-id="670f4-911">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="670f4-912">Implementowanie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="670f4-912">Implementing IActionConstraint</span></span>

<span data-ttu-id="670f4-913">Najprostszym sposobem implementacji `IActionConstraint` jest utworzenie klasy pochodzącej od `System.Attribute` i umieszczenie jej na swoich akcjach i kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="670f4-913">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="670f4-914">MVC automatycznie odnajdzie wszelkie `IActionConstraint`, które są stosowane jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="670f4-914">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="670f4-915">Możesz użyć modelu aplikacji, aby zastosować ograniczenia i prawdopodobnie jest to najbardziej elastyczne podejście, ponieważ pozwala to na to, w jaki sposób są stosowane.</span><span class="sxs-lookup"><span data-stu-id="670f4-915">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="670f4-916">W poniższym przykładzie ograniczenie wybiera akcję na podstawie *kodu kraju* z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="670f4-916">In the following example, a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="670f4-917">[Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="670f4-917">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="670f4-918">Użytkownik jest odpowiedzialny za implementację metody `Accept` i wybranie "Order" dla ograniczenia, które ma zostać wykonane.</span><span class="sxs-lookup"><span data-stu-id="670f4-918">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="670f4-919">W takim przypadku Metoda `Accept` zwraca `true`, aby zauważyć, że akcja jest dopasowanie, gdy wartość trasy `country` jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="670f4-919">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="670f4-920">Różni się to od `RouteValueAttribute` w tym, że umożliwia powrót do akcji, która nie ma atrybutu.</span><span class="sxs-lookup"><span data-stu-id="670f4-920">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="670f4-921">Przykład pokazuje, że jeśli zdefiniujesz akcję `en-US`, kod kraju, taki jak `fr-FR`, powróci do bardziej ogólnego kontrolera, który nie ma `[CountrySpecific(...)]` zastosowania.</span><span class="sxs-lookup"><span data-stu-id="670f4-921">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="670f4-922">Właściwość `Order` decyduje, który *etap* jest częścią tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="670f4-922">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="670f4-923">Ograniczenia akcji są uruchamiane w grupach na podstawie `Order`.</span><span class="sxs-lookup"><span data-stu-id="670f4-923">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="670f4-924">Na przykład wszystkie atrybuty metody HTTP podane przez platformę używają tej samej wartości `Order`, aby były uruchamiane na tym samym etapie.</span><span class="sxs-lookup"><span data-stu-id="670f4-924">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="670f4-925">Możesz mieć tyle etapów, ile potrzebujesz, aby zaimplementować odpowiednie zasady.</span><span class="sxs-lookup"><span data-stu-id="670f4-925">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="670f4-926">Aby określić wartość dla `Order` należy wziąć pod uwagę, czy ograniczenie ma być stosowane przed metodami HTTP.</span><span class="sxs-lookup"><span data-stu-id="670f4-926">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="670f4-927">Najpierw należy uruchomić mniejsze numery.</span><span class="sxs-lookup"><span data-stu-id="670f4-927">Lower numbers run first.</span></span>

::: moniker-end
