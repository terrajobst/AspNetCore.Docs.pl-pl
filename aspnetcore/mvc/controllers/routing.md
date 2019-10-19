---
title: Routing do akcji kontrolera w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób ASP.NET Core MVC używa programów pośredniczących routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.
ms.author: riande
ms.date: 01/24/2019
uid: mvc/controllers/routing
ms.openlocfilehash: a0dbfbe60c151990581b494f81e500fe0b315f55
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589854"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="dec8d-103">Routing do akcji kontrolera w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dec8d-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="dec8d-104">[Ryany Nowak](https://github.com/rynowak) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dec8d-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dec8d-105">ASP.NET Core MVC używa [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.</span><span class="sxs-lookup"><span data-stu-id="dec8d-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="dec8d-106">Trasy są zdefiniowane w kodzie startowym lub atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="dec8d-107">Trasy opisują sposób dopasowywania ścieżek adresów URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="dec8d-108">Trasy są również używane do generowania adresów URL (dla linków) wysyłanych w odpowiedziach.</span><span class="sxs-lookup"><span data-stu-id="dec8d-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="dec8d-109">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="dec8d-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="dec8d-110">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="dec8d-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="dec8d-111">Aby uzyskać więcej informacji, zobacz [Routing mieszany](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="dec8d-112">Ten dokument wyjaśnia interakcje między MVC i routingiem oraz sposób, w jaki typowe aplikacje MVC używają funkcji routingu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="dec8d-113">Zobacz [Routing](xref:fundamentals/routing) , aby uzyskać szczegółowe informacje na temat routingu zaawansowanego.</span><span class="sxs-lookup"><span data-stu-id="dec8d-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="dec8d-114">Konfigurowanie oprogramowania pośredniczącego routingu</span><span class="sxs-lookup"><span data-stu-id="dec8d-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="dec8d-115">W metodzie *konfigurowania* może zostać wyświetlony kod podobny do:</span><span class="sxs-lookup"><span data-stu-id="dec8d-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="dec8d-116">Wewnątrz wywołania do `UseMvc`, `MapRoute` służy do tworzenia pojedynczej trasy, która odnosi się do trasy `default`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="dec8d-117">Większość aplikacji MVC będzie używać trasy z szablonem podobnym do trasy `default`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="dec8d-118">@No__t_0 szablonu trasy może być zgodna ze ścieżką URL podobną do `/Products/Details/5` i wyodrębniać wartości tras `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki.</span><span class="sxs-lookup"><span data-stu-id="dec8d-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="dec8d-119">MVC podejmie próbę zlokalizowania kontrolera o nazwie `ProductsController` i uruchomi akcję `Details`:</span><span class="sxs-lookup"><span data-stu-id="dec8d-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="dec8d-120">Należy zauważyć, że w tym przykładzie powiązanie modelu będzie używać wartości `id = 5`, aby ustawić parametr `id` do `5` podczas wywoływania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="dec8d-121">Aby uzyskać więcej informacji, zobacz [powiązanie modelu](../models/model-binding.md) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="dec8d-122">Użycie trasy `default`:</span><span class="sxs-lookup"><span data-stu-id="dec8d-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="dec8d-123">Szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="dec8d-123">The route template:</span></span>

* <span data-ttu-id="dec8d-124">`{controller=Home}` definiuje `Home` jako `controller` domyślny</span><span class="sxs-lookup"><span data-stu-id="dec8d-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="dec8d-125">`{action=Index}` definiuje `Index` jako `action` domyślny</span><span class="sxs-lookup"><span data-stu-id="dec8d-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="dec8d-126">`{id?}` definiuje `id` jako opcjonalne</span><span class="sxs-lookup"><span data-stu-id="dec8d-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="dec8d-127">Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania.</span><span class="sxs-lookup"><span data-stu-id="dec8d-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="dec8d-128">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="dec8d-129">`"{controller=Home}/{action=Index}/{id?}"` może być zgodna ze ścieżką URL `/` i spowoduje utworzenie wartości tras `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="dec8d-130">Wartości `controller` i `action` używają wartości domyślnych, `id` nie produkuje wartości, ponieważ w ścieżce URL nie ma odpowiedniego segmentu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="dec8d-131">Aby można było wybrać akcję `HomeController` i `Index`, MVC będzie używać tych wartości tras:</span><span class="sxs-lookup"><span data-stu-id="dec8d-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="dec8d-132">Za pomocą tej definicji kontrolera i szablonu trasy, Akcja `HomeController.Index` będzie wykonywana dla dowolnej z następujących ścieżek URL:</span><span class="sxs-lookup"><span data-stu-id="dec8d-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="dec8d-133">Wygodna `UseMvcWithDefaultRoute` metody:</span><span class="sxs-lookup"><span data-stu-id="dec8d-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="dec8d-134">Może służyć do zastępowania:</span><span class="sxs-lookup"><span data-stu-id="dec8d-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="dec8d-135">`UseMvc` i `UseMvcWithDefaultRoute` dodać wystąpienie `RouterMiddleware` do potoku programu pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="dec8d-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="dec8d-136">MVC nie działa bezpośrednio w oprogramowaniu pośredniczącym i używa routingu do obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="dec8d-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="dec8d-137">MVC jest połączony z trasami za pomocą wystąpienia `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="dec8d-138">Kod w `UseMvc` jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="dec8d-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="dec8d-139">`UseMvc` nie definiuje bezpośrednio żadnych tras, dodaje symbol zastępczy do kolekcji tras dla trasy `attribute`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="dec8d-140">@No__t_0 Przeciążenie umożliwia dodanie własnych tras, a także obsługuje routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="dec8d-141">`UseMvc` i wszystkie jego Wariacje dodają symbol zastępczy dla atrybutu trasy — atrybut jest zawsze dostępny niezależnie od sposobu konfigurowania `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="dec8d-142">`UseMvcWithDefaultRoute` definiuje domyślną trasę i obsługuje routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="dec8d-143">Sekcja [Routing atrybutów](#attribute-routing-ref-label) zawiera więcej szczegółów dotyczących routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="dec8d-144">Routing konwencjonalny</span><span class="sxs-lookup"><span data-stu-id="dec8d-144">Conventional routing</span></span>

<span data-ttu-id="dec8d-145">Trasa `default`:</span><span class="sxs-lookup"><span data-stu-id="dec8d-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="dec8d-146">jest przykładem *konwencjonalnego routingu*.</span><span class="sxs-lookup"><span data-stu-id="dec8d-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="dec8d-147">Nazywamy ten styl *konwencjonalnym routingiem* , ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:</span><span class="sxs-lookup"><span data-stu-id="dec8d-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="dec8d-148">pierwszy segment ścieżki jest mapowany na nazwę kontrolera</span><span class="sxs-lookup"><span data-stu-id="dec8d-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="dec8d-149">Druga mapowanie na nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-149">the second maps to the action name.</span></span>

* <span data-ttu-id="dec8d-150">trzeci segment jest używany w przypadku opcjonalnego `id` używanego do mapowania na jednostkę modelu</span><span class="sxs-lookup"><span data-stu-id="dec8d-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="dec8d-151">Korzystając z tej `default` trasy, Ścieżka adresu URL `/Products/List` jest mapowana na akcję `ProductsController.List` i `/Blog/Article/17` mapy `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="dec8d-152">To mapowanie jest oparte **tylko** na nazwach kontrolera i akcji, a nie na podstawie przestrzeni nazw, lokalizacji plików źródłowych ani parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="dec8d-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="dec8d-153">Użycie konwencjonalnego routingu z domyślną trasą umożliwia szybkie Kompilowanie aplikacji bez konieczności podawania nowego wzorca adresu URL dla każdej zdefiniowanej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="dec8d-154">W przypadku aplikacji z akcjami w stylu CRUD zachowanie spójności adresów URL na kontrolerach może pomóc uprościć kod i uczynić interfejs użytkownika bardziej przewidywalny.</span><span class="sxs-lookup"><span data-stu-id="dec8d-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="dec8d-155">@No__t_0 jest zdefiniowany jako opcjonalny przez szablon trasy, co oznacza, że akcje mogą być wykonywane bez identyfikatora dostarczonego jako część adresu URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="dec8d-156">Typowo to, co się stanie, jeśli `id` zostanie pominięty w adresie URL, zostanie ustawiony na `0` przez powiązanie modelu i w wyniku tego nie zostanie znaleziona żadna jednostka w `id == 0` dopasowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="dec8d-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="dec8d-157">Routing atrybutu może dać szczegółowy formant, który ma być wymagany dla niektórych akcji, a nie dla innych.</span><span class="sxs-lookup"><span data-stu-id="dec8d-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="dec8d-158">Zgodnie z Konwencją dokumentacja będzie zawierać parametry opcjonalne, takie jak `id`, gdy prawdopodobnie będą widoczne w prawidłowym użyciu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="dec8d-159">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="dec8d-159">Multiple routes</span></span>

<span data-ttu-id="dec8d-160">Można dodać wiele tras wewnątrz `UseMvc`, dodając więcej wywołań do `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="dec8d-161">Dzięki temu można zdefiniować wiele Konwencji lub dodać trasy konwencjonalne, które są przeznaczone dla konkretnej akcji, na przykład:</span><span class="sxs-lookup"><span data-stu-id="dec8d-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="dec8d-162">Trasa `blog` w tym miejscu jest *dedykowaną tradycyjną trasą*, co oznacza, że używa systemu routingu konwencjonalnego, ale jest przeznaczona dla konkretnej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="dec8d-163">Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i w związku z tym trasa będzie zawsze mapowana na `BlogController.Article` akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="dec8d-164">Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, w jakiej zostały dodane.</span><span class="sxs-lookup"><span data-stu-id="dec8d-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="dec8d-165">W tym przykładzie zostanie podjęta próba `blog` trasy przed trasą `default`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="dec8d-166">*Dedykowane konwencjonalne trasy* często korzystają z parametrów trasy catch-all, takich jak `{*article}`, do przechwytywania pozostałej części ścieżki URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="dec8d-167">Może to spowodować, że trasa "zbyt zachłanne" oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="dec8d-168">Umieść trasy "zachłanne" później w tabeli tras, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="dec8d-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="dec8d-169">Istnie</span><span class="sxs-lookup"><span data-stu-id="dec8d-169">Fallback</span></span>

<span data-ttu-id="dec8d-170">W ramach przetwarzania żądań MVC sprawdzi, czy wartości trasy mogą być używane do znajdowania kontrolera i akcji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="dec8d-171">Jeśli wartości trasy nie są zgodne z akcją, trasa nie jest uważana za dopasowanie i zostanie podjęta kolejna trasa.</span><span class="sxs-lookup"><span data-stu-id="dec8d-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="dec8d-172">Jest to nazywane *Fallback*i ma na celu uproszczenie przypadków, w których trasy konwencjonalne nakładają się na siebie.</span><span class="sxs-lookup"><span data-stu-id="dec8d-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="dec8d-173">Niejednoznaczne akcje</span><span class="sxs-lookup"><span data-stu-id="dec8d-173">Disambiguating actions</span></span>

<span data-ttu-id="dec8d-174">Gdy dwie akcje są zgodne z routingiem, MVC musi odróżnić się, aby wybrać najlepszy kandydat lub w przeciwnym razie zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="dec8d-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="dec8d-175">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="dec8d-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="dec8d-176">Ten kontroler definiuje dwie akcje, które byłyby zgodne ze ścieżką URL `/Products/Edit/17` i trasy `{ controller = Products, action = Edit, id = 17 }` danych.</span><span class="sxs-lookup"><span data-stu-id="dec8d-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="dec8d-177">Jest to typowy wzorzec dla kontrolerów MVC, gdzie `Edit(int)` pokazuje formularz służący do edytowania produktu, a `Edit(int, Product)` przetwarza opublikowany formularz.</span><span class="sxs-lookup"><span data-stu-id="dec8d-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="dec8d-178">Aby ten możliwy składnik MVC musiał wybrać `Edit(int, Product)`, gdy żądanie jest `POST` HTTP i `Edit(int)`, gdy zlecenie HTTP jest inne.</span><span class="sxs-lookup"><span data-stu-id="dec8d-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="dec8d-179">@No__t_0 (`[HttpPost]`) to implementacja `IActionConstraint`, która będzie zezwalać na wybranie akcji tylko wtedy, gdy zlecenie HTTP jest `POST`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="dec8d-180">Obecność `IActionConstraint` powoduje, że `Edit(int, Product)` dopasowanie "lepszy" niż `Edit(int)`, więc `Edit(int, Product)` zostanie podjęta najpierw.</span><span class="sxs-lookup"><span data-stu-id="dec8d-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="dec8d-181">Należy napisać niestandardowe implementacje `IActionConstraint` w wyspecjalizowanych scenariuszach, ale ważne jest, aby zrozumieć rolę atrybutów, takich jak atrybuty `HttpPostAttribute`-like, które są zdefiniowane dla innych zleceń HTTP.</span><span class="sxs-lookup"><span data-stu-id="dec8d-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="dec8d-182">W konwencjonalnym routingu typowe dla akcji używanie tej samej nazwy akcji, gdy są one częścią przepływu pracy `show form -> submit form`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="dec8d-183">Wygoda tego wzorca stanie się bardziej oczywista po przejrzeniu sekcji [zrozumienie IActionConstraint](#understanding-iactionconstraint) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="dec8d-184">Jeśli wiele pasujących tras i MVC nie mogą znaleźć "najlepszej" trasy, wygeneruje `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="dec8d-185">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="dec8d-185">Route names</span></span>

<span data-ttu-id="dec8d-186">Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwami tras:</span><span class="sxs-lookup"><span data-stu-id="dec8d-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="dec8d-187">Nazwy tras nadaj logicznej nazwie trasy, aby nazwana trasa mogła być używana do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="dec8d-188">Znacznie upraszcza to tworzenie adresów URL, gdy porządkowanie tras może spowodować skomplikowane generowanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="dec8d-189">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="dec8d-190">Nazwy tras nie mają wpływu na Dopasowywanie adresów URL ani obsługę żądań; są one używane tylko do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="dec8d-191">[Routing](xref:fundamentals/routing) zawiera bardziej szczegółowe informacje na temat generowania adresów URL, w tym generowanie adresów URL w pomocnikach specyficznych dla MVC.</span><span class="sxs-lookup"><span data-stu-id="dec8d-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="dec8d-192">Routing atrybutów</span><span class="sxs-lookup"><span data-stu-id="dec8d-192">Attribute routing</span></span>

<span data-ttu-id="dec8d-193">Funkcja routingu atrybutów używa zestawu atrybutów do mapowania akcji bezpośrednio do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="dec8d-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="dec8d-194">W poniższym przykładzie `app.UseMvc();` jest używany w metodzie `Configure` i nie jest przenoszona żadna trasa.</span><span class="sxs-lookup"><span data-stu-id="dec8d-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="dec8d-195">@No__t_0 będzie zgodna z zestawem adresów URL podobnym do tego, co `{controller=Home}/{action=Index}/{id?}` trasie domyślnej:</span><span class="sxs-lookup"><span data-stu-id="dec8d-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="dec8d-196">Akcja `HomeController.Index()` będzie wykonywana dla dowolnej ścieżki URL `/`, `/Home` lub `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="dec8d-197">W tym przykładzie przedstawiono najważniejsze różnice programistyczne między routingiem atrybutów i routingiem konwencjonalnym.</span><span class="sxs-lookup"><span data-stu-id="dec8d-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="dec8d-198">Routing atrybutów wymaga więcej danych wejściowych w celu określenia trasy; konwencjonalne trasy domyślne obsługuje trasy bardziej zwięzłie.</span><span class="sxs-lookup"><span data-stu-id="dec8d-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="dec8d-199">Jednak Routing atrybutu zezwala na (i wymaga) precyzyjnej kontroli, które szablony tras mają zastosowanie do poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="dec8d-200">W przypadku routingu atrybutów nazwa kontrolera i nazwy akcji nie odgrywają **żadnej** roli, w której akcja jest zaznaczona.</span><span class="sxs-lookup"><span data-stu-id="dec8d-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="dec8d-201">Ten przykład będzie pasował do tych samych adresów URL, co w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="dec8d-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="dec8d-202">Powyższe szablony tras nie definiują parametrów trasy dla `action`, `area` i `controller`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="dec8d-203">W rzeczywistości te parametry tras są niedozwolone w trasach atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="dec8d-204">Ponieważ szablon trasy jest już skojarzony z akcją, nie ma sensu analizy nazwy akcji na podstawie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="dec8d-205">Routing atrybutów z atrybutami http [Verb]</span><span class="sxs-lookup"><span data-stu-id="dec8d-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="dec8d-206">Routing atrybutu może również używać atrybutów `Http[Verb]`, takich jak `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="dec8d-207">Wszystkie te atrybuty mogą akceptować szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="dec8d-208">Ten przykład przedstawia dwie akcje, które pasują do tego samego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="dec8d-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="dec8d-209">Dla ścieżki URL, takiej jak `/products` akcja `ProductsApi.ListProducts` zostanie wykonana, gdy zlecenie HTTP zostanie `GET` i `ProductsApi.CreateProduct` zostanie wykonane, gdy zlecenie HTTP jest `POST`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="dec8d-210">Routing atrybutu najpierw jest zgodny z adresem URL względem zestawu szablonów tras zdefiniowanych przez atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="dec8d-211">Po dopasowaniu szablonu trasy do określenia, które akcje mogą być wykonywane, są stosowane ograniczenia `IActionConstraint`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="dec8d-212">Podczas kompilowania interfejsu API REST trudno jest użyć `[Route(...)]` na metodę akcji, ponieważ akcja akceptuje wszystkie metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="dec8d-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="dec8d-213">Lepiej jest używać bardziej szczegółowych `Http*Verb*Attributes`, aby precyzyjnie dowiedzieć się, co obsługuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="dec8d-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="dec8d-214">Klienci interfejsów API REST powinni wiedzieć, jakie ścieżki i czasowniki HTTP mapują na określone operacje logiczne.</span><span class="sxs-lookup"><span data-stu-id="dec8d-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="dec8d-215">Ponieważ atrybut Route ma zastosowanie do określonej akcji, można łatwo wprowadzić parametry wymagane jako część definicji szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="dec8d-216">W tym przykładzie `id` jest wymagane jako część ścieżki URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="dec8d-217">Akcja `ProductsApi.GetProduct(int)` zostanie wykonana dla ścieżki URL, takiej jak `/products/3`, ale nie dla ścieżki URL, takiej jak `/products`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="dec8d-218">Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](../../fundamentals/routing.md) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="dec8d-219">Nazwa trasy</span><span class="sxs-lookup"><span data-stu-id="dec8d-219">Route Name</span></span>

<span data-ttu-id="dec8d-220">Poniższy kod definiuje *nazwę trasy* `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="dec8d-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="dec8d-221">Nazwy tras mogą służyć do generowania adresów URL na podstawie określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="dec8d-222">Nazwy tras nie mają wpływu na zachowanie routingu w adresie URL i są używane tylko na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="dec8d-223">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="dec8d-224">W przeciwieństwie do konwencjonalnej *trasy domyślnej*, która definiuje `id` parametr jako opcjonalny (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="dec8d-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="dec8d-225">Ta możliwość precyzyjnego określania interfejsów API ma zalety, takich jak umożliwienie `/products` i `/products/5` do wysłania do różnych akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="dec8d-226">Łączenie tras</span><span class="sxs-lookup"><span data-stu-id="dec8d-226">Combining routes</span></span>

<span data-ttu-id="dec8d-227">Aby mniej powtarzać Routing atrybutów, atrybuty trasy na kontrolerze są łączone z atrybutami trasy dla poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="dec8d-228">Wszystkie szablony tras zdefiniowane na kontrolerze są poprzedzone w celu rozesłania szablonów w akcjach.</span><span class="sxs-lookup"><span data-stu-id="dec8d-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="dec8d-229">Umieszczenie atrybutu trasy na kontrolerze powoduje, że **wszystkie** akcje w kontrolerze używają routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="dec8d-230">W tym przykładzie ścieżka adresu URL `/products` może być zgodna `ProductsApi.ListProducts`, a ścieżka URL `/products/5` może pasować `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="dec8d-231">Obie te akcje pasują do `GET` HTTP, ponieważ są one w `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-231">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="dec8d-232">Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są łączone z szablonami tras zastosowanymi do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="dec8d-232">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="dec8d-233">Ten przykład dopasowuje zestaw ścieżek URL podobny do *trasy domyślnej*.</span><span class="sxs-lookup"><span data-stu-id="dec8d-233">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="dec8d-234">Określanie kolejności tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="dec8d-234">Ordering attribute routes</span></span>

<span data-ttu-id="dec8d-235">W przeciwieństwie do konwencjonalnych tras, które są wykonywane w określonej kolejności, routing atrybutu kompiluje drzewo i dopasowuje wszystkie trasy jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="dec8d-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="dec8d-236">Zachowuje się tak, jakby wpisy trasy zostały umieszczone w idealnym porządku; najbardziej konkretne trasy mają możliwość wykonania przed bardziej ogólnymi trasami.</span><span class="sxs-lookup"><span data-stu-id="dec8d-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="dec8d-237">Przykładowo trasa, taka jak `blog/search/{topic}`, jest bardziej szczegółowa niż trasa, taka jak `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="dec8d-238">Logicznie domyślnie mówiąc `blog/search/{topic}` trasę "uruchomienia", ponieważ jest to jedyna rozsądna kolejność.</span><span class="sxs-lookup"><span data-stu-id="dec8d-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="dec8d-239">Przy użyciu konwencjonalnego routingu deweloper jest odpowiedzialny za umieszczanie tras w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="dec8d-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="dec8d-240">Trasy atrybutów mogą konfigurować kolejność przy użyciu właściwości `Order` wszystkich atrybutów tras dostarczonych przez platformę.</span><span class="sxs-lookup"><span data-stu-id="dec8d-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="dec8d-241">Trasy są przetwarzane zgodnie z rosnącym sortowaniem właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="dec8d-242">Kolejność domyślna to `0`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-242">The default order is `0`.</span></span> <span data-ttu-id="dec8d-243">Ustawienie trasy przy użyciu `Order = -1` zostanie uruchomione przed trasami, które nie ustawiają kolejności.</span><span class="sxs-lookup"><span data-stu-id="dec8d-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="dec8d-244">Ustawienie trasy przy użyciu `Order = 1` zostanie uruchomione po określeniu domyślnej kolejności tras.</span><span class="sxs-lookup"><span data-stu-id="dec8d-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="dec8d-245">Należy unikać w zależności od `Order`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="dec8d-246">Jeśli przestrzeń adresów URL wymaga jawnych wartości kolejności, aby można było prawidłowo kierować trasy, to prawdopodobnie również jest myląca dla klientów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="dec8d-247">W ogólnym routingu atrybutów wybierz prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="dec8d-248">Jeśli domyślna kolejność generowania adresów URL nie działa, użycie nazwy trasy jako przesłonięcia jest zwykle prostsze niż stosowanie właściwości `Order`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="dec8d-249">Razor Pages Routing i kontroler MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="dec8d-249">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="dec8d-250">Informacje o zamówieniu trasy w Razor Pages tematy są dostępne na stronie [Razor Pages trasy i konwencje aplikacji: kolejność tras](xref:razor-pages/razor-pages-conventions#route-order).</span><span class="sxs-lookup"><span data-stu-id="dec8d-250">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="dec8d-251">Zastępowanie tokenu w szablonach tras ([Controller], [Action], [obszar])</span><span class="sxs-lookup"><span data-stu-id="dec8d-251">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="dec8d-252">Dla wygody, trasy atrybutu obsługują *zastępowanie tokenów* przez umieszczanie tokenu w nawiasach kwadratowych (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="dec8d-252">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="dec8d-253">Tokeny `[action]`, `[area]` i `[controller]` są zastępowane wartościami nazwy akcji, obszaru i nazwy kontrolera z akcji, w której jest zdefiniowana trasa.</span><span class="sxs-lookup"><span data-stu-id="dec8d-253">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="dec8d-254">W poniższym przykładzie akcje są zgodne ze ścieżkami URL zgodnie z opisem w komentarzach:</span><span class="sxs-lookup"><span data-stu-id="dec8d-254">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="dec8d-255">Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-255">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="dec8d-256">Powyższy przykład będzie zachowywać się tak samo jak poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="dec8d-256">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="dec8d-257">Trasy atrybutu można także łączyć z dziedziczeniem.</span><span class="sxs-lookup"><span data-stu-id="dec8d-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="dec8d-258">Jest to szczególnie zaawansowane w połączeniu z zastępowaniem tokenu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-258">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="dec8d-259">Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="dec8d-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generuje unikatową nazwę trasy dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="dec8d-261">Aby dopasować ogranicznik zastępczy tokenu literału `[` lub `]`, należy to zrobić, powtarzając znak (`[[` lub `]]`).</span><span class="sxs-lookup"><span data-stu-id="dec8d-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="dec8d-262">Używanie transformatora parametrów do dostosowywania zastępowania tokenu</span><span class="sxs-lookup"><span data-stu-id="dec8d-262">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="dec8d-263">Zastępowanie tokenu można dostosować za pomocą transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-263">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="dec8d-264">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-264">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="dec8d-265">Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-265">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="dec8d-266">@No__t_0 jest konwencją modelu aplikacji, która:</span><span class="sxs-lookup"><span data-stu-id="dec8d-266">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="dec8d-267">Stosuje transformator parametrów do wszystkich tras atrybutów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-267">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="dec8d-268">Dostosowuje wartości tokenów tras w miarę ich wymiany.</span><span class="sxs-lookup"><span data-stu-id="dec8d-268">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="dec8d-269">@No__t_0 jest zarejestrowany jako opcja w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-269">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="dec8d-270">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="dec8d-270">Multiple Routes</span></span>

<span data-ttu-id="dec8d-271">Routing atrybutów obsługuje definiowanie wielu tras, które docierają do tej samej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-271">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="dec8d-272">Najczęstszym sposobem użycia tej metody jest naśladowanie zachowania *domyślnej trasy konwencjonalnej* , jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="dec8d-272">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="dec8d-273">Umieszczenie wielu atrybutów trasy na kontrolerze oznacza, że każda z nich będzie łączyć się z poszczególnymi atrybutami trasy w metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-273">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="dec8d-274">Gdy wiele atrybutów trasy (implementujących `IActionConstraint`) jest umieszczanych w akcji, każde ograniczenie akcji łączy się z szablonem trasy z atrybutu, który go zdefiniował.</span><span class="sxs-lookup"><span data-stu-id="dec8d-274">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="dec8d-275">Chociaż używanie wielu tras w akcjach może wydawać się zaawansowane, lepiej jest zachować prosty i dobrze zdefiniowany obszar adresów URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-275">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="dec8d-276">Użyj wielu tras w przypadku akcji tylko wtedy, gdy jest to konieczne, na przykład do obsługi istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-276">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="dec8d-277">Określanie opcjonalnych parametrów trasy, wartości domyślnych i ograniczeń</span><span class="sxs-lookup"><span data-stu-id="dec8d-277">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="dec8d-278">Trasy atrybutów obsługują tę samą składnię wbudowaną co konwencjonalne trasy, aby określić parametry opcjonalne, wartości domyślne i ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="dec8d-278">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="dec8d-279">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-279">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="dec8d-280">Niestandardowe atrybuty trasy przy użyciu `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="dec8d-280">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="dec8d-281">Wszystkie atrybuty trasy podane w strukturze (`[Route(...)]`, `[HttpGet(...)]` itp.) implementują interfejs `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-281">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="dec8d-282">MVC szuka atrybutów klas kontrolera i metod akcji, gdy aplikacja zostanie uruchomiona i używa tych, które implementują `IRouteTemplateProvider`, aby skompilować początkowy zestaw tras.</span><span class="sxs-lookup"><span data-stu-id="dec8d-282">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="dec8d-283">Możesz zaimplementować `IRouteTemplateProvider`, aby zdefiniować własne atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-283">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="dec8d-284">Każda `IRouteTemplateProvider` umożliwia zdefiniowanie pojedynczej trasy z niestandardowym szablonem trasy, kolejnością i nazwą:</span><span class="sxs-lookup"><span data-stu-id="dec8d-284">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="dec8d-285">Ten atrybut z powyższego przykładu automatycznie ustawia `Template` do `"api/[controller]"` w przypadku zastosowania `[MyApiController]`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-285">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="dec8d-286">Dostosowywanie tras atrybutów przy użyciu modelu aplikacji</span><span class="sxs-lookup"><span data-stu-id="dec8d-286">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="dec8d-287">*Model aplikacji* jest modelem obiektów tworzonym podczas uruchamiania ze wszystkimi metadanymi używanymi przez MVC do kierowania i wykonywania akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-287">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="dec8d-288">*Model aplikacji* zawiera wszystkie dane zebrane z atrybutów tras (za pośrednictwem `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="dec8d-288">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="dec8d-289">Można napisać *konwencje* , aby zmodyfikować model aplikacji w czasie uruchamiania, aby dostosować sposób zachowania routingu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-289">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="dec8d-290">W tej sekcji przedstawiono prosty przykład dostosowywania routingu przy użyciu modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-290">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="dec8d-291">Routing mieszany: Routing atrybutów a konwencjonalny Routing</span><span class="sxs-lookup"><span data-stu-id="dec8d-291">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="dec8d-292">Aplikacje MVC mogą łączyć użycie konwencjonalnego routingu i routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-292">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="dec8d-293">Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.</span><span class="sxs-lookup"><span data-stu-id="dec8d-293">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="dec8d-294">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="dec8d-294">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="dec8d-295">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="dec8d-295">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="dec8d-296">Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa.</span><span class="sxs-lookup"><span data-stu-id="dec8d-296">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="dec8d-297">**Każdy** atrybut trasy na kontrolerze powoduje kierowanie wszystkich akcji w atrybucie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="dec8d-297">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="dec8d-298">Co odróżnia dwa typy systemów routingu, proces stosowany po adresie URL pasuje do szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-298">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="dec8d-299">W przypadku routingu konwencjonalnego wartości trasy z dopasowania są używane do wybierania akcji i kontrolera z tabeli odnośników wszystkich konwencjonalnych akcji kierowanych.</span><span class="sxs-lookup"><span data-stu-id="dec8d-299">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="dec8d-300">W obszarze Routing atrybutów każdy szablon jest już skojarzony z akcją i nie jest wymagany dalsze wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="dec8d-300">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="dec8d-301">Złożone segmenty</span><span class="sxs-lookup"><span data-stu-id="dec8d-301">Complex segments</span></span>

<span data-ttu-id="dec8d-302">Złożone segmenty (na przykład `[Route("/dog{token}cat")]`) są przetwarzane przez dopasowanie literałów od prawej do lewej w sposób niezachłanney.</span><span class="sxs-lookup"><span data-stu-id="dec8d-302">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="dec8d-303">Sprawdź [kod źródłowy](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) opisu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-303">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="dec8d-304">Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="dec8d-304">For more information, see [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="dec8d-305">Generowanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="dec8d-305">URL Generation</span></span>

<span data-ttu-id="dec8d-306">Aplikacje MVC mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-306">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="dec8d-307">Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany.</span><span class="sxs-lookup"><span data-stu-id="dec8d-307">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="dec8d-308">Ta sekcja koncentruje się na funkcjach generowania adresów URL dostarczonych przez MVC i obejmuje tylko podstawowe informacje na temat sposobu działania generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-308">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="dec8d-309">Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](../../fundamentals/routing.md) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-309">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="dec8d-310">Interfejs `IUrlHelper` jest podstawową częścią infrastruktury między MVC i routingiem na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-310">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="dec8d-311">Wystąpienie `IUrlHelper` dostępne za pomocą właściwości `Url` w obszarze Kontrolery, widoki i składniki widoku.</span><span class="sxs-lookup"><span data-stu-id="dec8d-311">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="dec8d-312">W tym przykładzie interfejs `IUrlHelper` jest używany przez właściwość `Controller.Url` do generowania adresu URL dla innej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-312">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="dec8d-313">Jeśli aplikacja używa domyślnej trasy konwencjonalnej, wartością zmiennej `url` będzie ciąg ścieżki adresu URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-313">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="dec8d-314">Ta ścieżka URL jest tworzona za pomocą routingu przez połączenie wartości trasy z bieżącego żądania (wartości otoczenia), z wartościami przekazanymi do `Url.Action` i podstawianie tych wartości do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="dec8d-314">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="dec8d-315">Każdy parametr trasy w szablonie trasy ma swoją wartość zastępowaną przez pasujące nazwy wartościami i wartościami otoczenia.</span><span class="sxs-lookup"><span data-stu-id="dec8d-315">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="dec8d-316">Parametr trasy, który nie ma wartości, może korzystać z wartości domyślnej, jeśli ma taką wartość, lub być pominięty, jeśli jest opcjonalny (tak jak w przypadku `id` w tym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="dec8d-316">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="dec8d-317">Generowanie adresu URL zakończy się niepowodzeniem, jeśli którykolwiek z wymaganych parametrów trasy nie ma odpowiadającej wartości.</span><span class="sxs-lookup"><span data-stu-id="dec8d-317">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="dec8d-318">Jeśli generowanie adresów URL kończy się niepowodzeniem dla trasy, kolejna trasa zostanie ponowiona do momentu przetworzenia wszystkich tras lub znalezienia dopasowania.</span><span class="sxs-lookup"><span data-stu-id="dec8d-318">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="dec8d-319">Na przykład `Url.Action` powyżej zakłada się, że konwencjonalne Routing, ale generowanie adresów URL działa podobnie z routingiem atrybutów, chociaż koncepcje różnią się.</span><span class="sxs-lookup"><span data-stu-id="dec8d-319">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="dec8d-320">W przypadku routingu konwencjonalnego wartości tras są używane do rozszerzania szablonu, a wartości trasy dla `controller` i `action` zazwyczaj pojawiają się w tym szablonie — działa to, ponieważ adresy URL dopasowane przez Routing są zgodne z *Konwencją*.</span><span class="sxs-lookup"><span data-stu-id="dec8d-320">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="dec8d-321">W obszarze Routing atrybutów wartości trasy dla `controller` i `action` nie mogą być wyświetlane w szablonie — są one używane do wyszukiwania szablonu do użycia.</span><span class="sxs-lookup"><span data-stu-id="dec8d-321">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="dec8d-322">W tym przykładzie zastosowano Routing atrybutów:</span><span class="sxs-lookup"><span data-stu-id="dec8d-322">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="dec8d-323">MVC kompiluje tabelę odnośników wszystkich akcji przypisanych do atrybutu i dopasowuje wartości `controller` i `action`, aby wybrać szablon trasy do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-323">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="dec8d-324">W przykładzie powyżej jest generowana `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-324">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="dec8d-325">Generowanie adresów URL według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="dec8d-325">Generating URLs by action name</span></span>

<span data-ttu-id="dec8d-326">`Url.Action` (`IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-326">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="dec8d-327">`Action`) i wszystkie powiązane przeciążenia są oparte na tym pomysłie, aby określić, do czego chcesz utworzyć łącze, określając nazwę kontrolera i nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-327">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="dec8d-328">W przypadku korzystania z `Url.Action`, bieżące wartości trasy dla `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` są częścią *wartości otoczenia* **i** *wartości*.</span><span class="sxs-lookup"><span data-stu-id="dec8d-328">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="dec8d-329">Metoda `Url.Action`, zawsze używa bieżących wartości `action` i `controller` i wygeneruje ścieżkę URL, która kieruje do bieżącej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-329">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="dec8d-330">Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały wprowadzone podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-330">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="dec8d-331">Przy użyciu trasy, takiej jak `{a}/{b}/{c}/{d}` i wartości otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma wystarczającą ilość informacji do wygenerowania adresu URL bez dodatkowych wartości — ponieważ wszystkie parametry trasy mają wartość.</span><span class="sxs-lookup"><span data-stu-id="dec8d-331">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="dec8d-332">W przypadku dodania wartości `{ d = Donovan }` wartość `{ d = David }` zostanie zignorowana, a wygenerowana Ścieżka adresu URL zostanie `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-332">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="dec8d-333">Ścieżki URL są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="dec8d-333">URL paths are hierarchical.</span></span> <span data-ttu-id="dec8d-334">W powyższym przykładzie, jeśli dodano wartość `{ c = Cheryl }`, obie wartości `{ c = Carol, d = David }` zostałyby zignorowane.</span><span class="sxs-lookup"><span data-stu-id="dec8d-334">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="dec8d-335">W takim przypadku nie ma już wartości `d` a generowanie adresów URL zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="dec8d-335">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="dec8d-336">Należy określić pożądaną wartość `c` i `d`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-336">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="dec8d-337">Może wystąpić potrzeba napotkania tego problemu przy użyciu trasy domyślnej (`{controller}/{action}/{id?}`), ale w takim przypadku rzadko wystąpi to zachowanie, ponieważ `Url.Action` będzie zawsze jawnie określać `controller` i `action` wartość.</span><span class="sxs-lookup"><span data-stu-id="dec8d-337">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="dec8d-338">Dłuższe przeciążenia `Url.Action` również pobierają dodatkowy obiekt *wartości trasy* , aby zapewnić wartości parametrów trasy innych niż `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-338">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="dec8d-339">Najczęściej zobaczysz ten sposób, który jest używany z `id`, takich jak `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-339">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="dec8d-340">Według Konwencji obiekt *wartości trasy* jest zwykle obiektem typu anonimowego, ale może również być `IDictionary<>` lub *zwykłym starym obiektem platformy .NET*.</span><span class="sxs-lookup"><span data-stu-id="dec8d-340">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="dec8d-341">Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="dec8d-341">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="dec8d-342">Aby utworzyć bezwzględny adres URL, Użyj przeciążenia, które akceptuje `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="dec8d-342">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="dec8d-343">Generowanie adresów URL według trasy</span><span class="sxs-lookup"><span data-stu-id="dec8d-343">Generating URLs by route</span></span>

<span data-ttu-id="dec8d-344">Powyższy kod wygeneruje adres URL przez przekazanie go do kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-344">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="dec8d-345">`IUrlHelper` zapewnia także `Url.RouteUrl` rodziny metod.</span><span class="sxs-lookup"><span data-stu-id="dec8d-345">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="dec8d-346">Te metody są podobne do `Url.Action`, ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-346">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="dec8d-347">Najbardziej typowym zastosowaniem jest określenie nazwy trasy do użycia określonej trasy do wygenerowania adresu URL, na ogół *bez* określania kontrolera lub nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-347">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="dec8d-348">Generowanie adresów URL w kodzie HTML</span><span class="sxs-lookup"><span data-stu-id="dec8d-348">Generating URLs in HTML</span></span>

<span data-ttu-id="dec8d-349">`IHtmlHelper` udostępnia metody `HtmlHelper`, `Html.BeginForm` i `Html.ActionLink` generować odpowiednio elementy `<form>` i `<a>`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-349">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="dec8d-350">Te metody używają metody `Url.Action` do wygenerowania adresu URL i akceptują podobne argumenty.</span><span class="sxs-lookup"><span data-stu-id="dec8d-350">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="dec8d-351">@No__t_0 pomocników dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink`, które mają podobną funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="dec8d-351">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="dec8d-352">TagHelpers Generuj adresy URL za pomocą `form` TagHelper i `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="dec8d-352">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="dec8d-353">Oba te zastosowania `IUrlHelper` do ich implementacji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-353">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="dec8d-354">Aby uzyskać więcej informacji, zobacz [Praca z formularzami](../views/working-with-forms.md) .</span><span class="sxs-lookup"><span data-stu-id="dec8d-354">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="dec8d-355">W widokach, `IUrlHelper` jest dostępna za pomocą właściwości `Url` dla dowolnej generacji adresów URL ad hoc, które nie są objęte powyższym.</span><span class="sxs-lookup"><span data-stu-id="dec8d-355">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="dec8d-356">Generowanie adresów URL w wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="dec8d-356">Generating URLS in Action Results</span></span>

<span data-ttu-id="dec8d-357">Powyższe przykłady przedstawiono przy użyciu `IUrlHelper` w kontrolerze, podczas gdy najbardziej typowym użyciem na kontrolerze jest wygenerowanie adresu URL jako części wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-357">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="dec8d-358">Klasy bazowe `ControllerBase` i `Controller` zapewniają wygodne metody dla wyników akcji, które odwołują się do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-358">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="dec8d-359">Jednym z typowych zastosowań jest przekierowanie po zaakceptowaniu danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="dec8d-359">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="dec8d-360">Metody fabryki wyników akcji postępują zgodnie z podobnym wzorcem do metod w `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-360">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="dec8d-361">Specjalny przypadek dla dedykowanych tras konwencjonalnych</span><span class="sxs-lookup"><span data-stu-id="dec8d-361">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="dec8d-362">Funkcja routingu konwencjonalnego może używać specjalnego rodzaju definicji trasy o nazwie *dedykowanej, konwencjonalnej trasy*.</span><span class="sxs-lookup"><span data-stu-id="dec8d-362">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="dec8d-363">W poniższym przykładzie trasa o nazwie `blog` jest dedykowaną umowną trasą.</span><span class="sxs-lookup"><span data-stu-id="dec8d-363">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="dec8d-364">Korzystając z tych definicji tras, `Url.Action("Index", "Home")` wygeneruje ścieżkę URL `/` z trasą `default`, ale dlaczego?</span><span class="sxs-lookup"><span data-stu-id="dec8d-364">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="dec8d-365">Możesz odgadnąć wartości tras `{ controller = Home, action = Index }` wystarcza do wygenerowania adresu URL przy użyciu `blog`, a wynik zostanie `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-365">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="dec8d-366">Dedykowane konwencjonalne trasy polegają na specjalnym zachowaniu wartości domyślnych, które nie mają odpowiadającego parametru trasy, który uniemożliwia trasie "zbyt zachłanne" z generowaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-366">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="dec8d-367">W takim przypadku wartości domyślne są `{ controller = Blog, action = Article }`, a ani `controller`, ani `action` nie pojawiają się jako parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-367">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="dec8d-368">Gdy Routing wykonuje generowanie adresów URL, podane wartości muszą być zgodne z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="dec8d-368">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="dec8d-369">Generowanie adresu URL przy użyciu `blog` nie powiedzie się, ponieważ wartości `{ controller = Home, action = Index }` nie pasują do `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-369">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="dec8d-370">Następnie usługa routingu wraca do wypróbowania `default`, która kończy się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="dec8d-370">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="dec8d-371">Obszary</span><span class="sxs-lookup"><span data-stu-id="dec8d-371">Areas</span></span>

<span data-ttu-id="dec8d-372">[Obszary](areas.md) są funkcją MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw routingu (dla akcji kontrolera) i struktury folderów (dla widoków).</span><span class="sxs-lookup"><span data-stu-id="dec8d-372">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="dec8d-373">Użycie obszarów umożliwia aplikacji posiadanie wielu kontrolerów o tej samej nazwie, o ile mają one różne *obszary*.</span><span class="sxs-lookup"><span data-stu-id="dec8d-373">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="dec8d-374">Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie innego parametru trasy, `area` do `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-374">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="dec8d-375">W tej sekcji omówiono sposób, w jaki Routing współdziała z obszarami — zobacz [obszary](areas.md) , aby uzyskać szczegółowe informacje o tym, jak obszary są używane w widokach.</span><span class="sxs-lookup"><span data-stu-id="dec8d-375">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="dec8d-376">Poniższy przykład konfiguruje MVC do używania domyślnej trasy konwencjonalnej i *trasy obszaru* dla obszaru o nazwie `Blog`:</span><span class="sxs-lookup"><span data-stu-id="dec8d-376">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="dec8d-377">W przypadku dopasowania ścieżki URL, takiej jak `/Manage/Users/AddUser`, Pierwsza trasa będzie generować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-377">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="dec8d-378">@No__t_0 wartość trasy jest generowana przez wartość domyślną dla `area`, w rzeczywistości trasa utworzona przez `MapAreaRoute` jest równoważna następującym:</span><span class="sxs-lookup"><span data-stu-id="dec8d-378">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="dec8d-379">`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dla `area` przy użyciu podanej nazwy obszaru, w tym przypadku `Blog`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-379">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="dec8d-380">Wartość domyślna zapewnia, że trasa zawsze generuje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-380">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="dec8d-381">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="dec8d-381">Conventional routing is order-dependent.</span></span> <span data-ttu-id="dec8d-382">Ogólnie rzecz biorąc, trasy z obszarami należy umieścić wcześniej w tabeli tras, ponieważ są one bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="dec8d-382">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="dec8d-383">Korzystając z powyższego przykładu, wartości trasy będą zgodne z następującą akcją:</span><span class="sxs-lookup"><span data-stu-id="dec8d-383">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="dec8d-384">@No__t_0 to oznacza, że w ramach obszaru znajduje się kontroler. Załóżmy, że ten kontroler znajduje się w obszarze `Blog`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-384">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="dec8d-385">Kontrolery bez atrybutu `[Area]` nie są członkami żadnego obszaru i **nie** będą zgodne, gdy wartość trasy `area` zostanie udostępniona przez Routing.</span><span class="sxs-lookup"><span data-stu-id="dec8d-385">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="dec8d-386">W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-386">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="dec8d-387">Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu w celu zapewnienia kompletności — w przeciwnym razie kontrolery będą mieć konflikt nazw i generują błąd kompilatora.</span><span class="sxs-lookup"><span data-stu-id="dec8d-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="dec8d-388">Przestrzenie nazw klas nie mają wpływu na Routing MVC.</span><span class="sxs-lookup"><span data-stu-id="dec8d-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="dec8d-389">Pierwsze dwa kontrolery są członkami obszarów i pasują tylko wtedy, gdy ich nazwa obszaru jest podana przez wartość `area` trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="dec8d-390">Trzeci kontroler nie jest członkiem żadnego obszaru i może pasować tylko wtedy, gdy żadna wartość `area` nie jest udostępniana przez Routing.</span><span class="sxs-lookup"><span data-stu-id="dec8d-390">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="dec8d-391">W warunkach pasujących do *nie ma żadnej wartości*, brak wartości `area` jest taka sama jak wtedy, gdy wartość `area` była zerowa lub ciągiem pustym.</span><span class="sxs-lookup"><span data-stu-id="dec8d-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="dec8d-392">Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` będzie dostępna jako *wartość otoczenia* dla routingu do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="dec8d-393">Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="dec8d-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="dec8d-394">Zrozumienie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="dec8d-394">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="dec8d-395">Ta sekcja jest głęboką szczegółoweą wewnętrznych struktur oraz jak MVC wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="dec8d-395">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="dec8d-396">Typowa aplikacja nie będzie potrzebować niestandardowego `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="dec8d-396">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="dec8d-397">Prawdopodobnie już używasz `IActionConstraint`, nawet jeśli nie masz doświadczenia z interfejsem.</span><span class="sxs-lookup"><span data-stu-id="dec8d-397">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="dec8d-398">Atrybut `[HttpGet]` i podobne atrybuty `[Http-VERB]` implementują `IActionConstraint` w celu ograniczenia wykonywania metody akcji.</span><span class="sxs-lookup"><span data-stu-id="dec8d-398">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="dec8d-399">Przy założeniu domyślnej trasy konwencjonalnej, Ścieżka adresu URL `/Products/Edit` będzie generować wartości `{ controller = Products, action = Edit }`, które byłyby zgodne z **obiema** akcjami przedstawionymi w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-399">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="dec8d-400">W `IActionConstraint` terminologia, że obie te akcje są uważane za kandydatów, ponieważ obydwie pasują do danych tras.</span><span class="sxs-lookup"><span data-stu-id="dec8d-400">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="dec8d-401">Gdy `HttpGetAttribute` wykonuje, zobaczy, że *Edycja ()* jest dopasowaniem do *Get* i nie jest dopasowaniem dla żadnego innego zlecenia http.</span><span class="sxs-lookup"><span data-stu-id="dec8d-401">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="dec8d-402">Akcja `Edit(...)` nie ma zdefiniowanych żadnych ograniczeń i dlatego będzie pasować do dowolnego zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="dec8d-402">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="dec8d-403">Tak więc założono, że `Edit(...)` są zgodne tylko `POST`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-403">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="dec8d-404">Jednak w przypadku `GET` obie akcje mogą nadal być zgodne, jednak akcja z `IActionConstraint` jest zawsze uznawana za *lepszą* niż akcja bez.</span><span class="sxs-lookup"><span data-stu-id="dec8d-404">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="dec8d-405">Dlatego `Edit()` ma `[HttpGet]` jest uważany za bardziej szczegółowy i zostanie wybrany, jeśli obie akcje mogą być zgodne.</span><span class="sxs-lookup"><span data-stu-id="dec8d-405">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="dec8d-406">Koncepcyjnie, `IActionConstraint` jest formą *przeciążania*, ale zamiast przeciążania metod o tej samej nazwie, przeciążanie między akcjami, które pasują do tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="dec8d-406">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="dec8d-407">Funkcja routingu atrybutów używa również `IActionConstraint` i może spowodować, że akcje z różnych kontrolerów są uznawane za kandydatów.</span><span class="sxs-lookup"><span data-stu-id="dec8d-407">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="dec8d-408">Implementowanie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="dec8d-408">Implementing IActionConstraint</span></span>

<span data-ttu-id="dec8d-409">Najprostszym sposobem implementacji `IActionConstraint` jest utworzenie klasy pochodzącej od `System.Attribute` i umieszczenie jej na swoich akcjach i kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="dec8d-409">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="dec8d-410">MVC automatycznie odnajdzie wszelkie `IActionConstraint`, które są stosowane jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="dec8d-410">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="dec8d-411">Możesz użyć modelu aplikacji, aby zastosować ograniczenia i prawdopodobnie jest to najbardziej elastyczne podejście, ponieważ pozwala to na to, w jaki sposób są stosowane.</span><span class="sxs-lookup"><span data-stu-id="dec8d-411">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="dec8d-412">W poniższym przykładzie ograniczenie wybiera akcję na podstawie *kodu kraju* z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="dec8d-412">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="dec8d-413">[Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="dec8d-413">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="dec8d-414">Użytkownik jest odpowiedzialny za implementację metody `Accept` i wybranie "Order" dla ograniczenia, które ma zostać wykonane.</span><span class="sxs-lookup"><span data-stu-id="dec8d-414">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="dec8d-415">W takim przypadku Metoda `Accept` zwraca `true`, aby zauważyć, że akcja jest dopasowanie, gdy wartość trasy `country` jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="dec8d-415">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="dec8d-416">Różni się to od `RouteValueAttribute` w tym, że umożliwia powrót do akcji, która nie ma atrybutu.</span><span class="sxs-lookup"><span data-stu-id="dec8d-416">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="dec8d-417">Przykład pokazuje, że jeśli zdefiniujesz akcję `en-US`, kod kraju, taki jak `fr-FR`, powróci do bardziej ogólnego kontrolera, który nie ma `[CountrySpecific(...)]` zastosowania.</span><span class="sxs-lookup"><span data-stu-id="dec8d-417">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="dec8d-418">Właściwość `Order` decyduje, który *etap* jest częścią tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="dec8d-418">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="dec8d-419">Ograniczenia akcji są uruchamiane w grupach na podstawie `Order`.</span><span class="sxs-lookup"><span data-stu-id="dec8d-419">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="dec8d-420">Na przykład wszystkie atrybuty metody HTTP podane przez platformę używają tej samej wartości `Order`, aby były uruchamiane na tym samym etapie.</span><span class="sxs-lookup"><span data-stu-id="dec8d-420">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="dec8d-421">Możesz mieć tyle etapów, ile potrzebujesz, aby zaimplementować odpowiednie zasady.</span><span class="sxs-lookup"><span data-stu-id="dec8d-421">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="dec8d-422">Aby określić wartość dla `Order` należy wziąć pod uwagę, czy ograniczenie ma być stosowane przed metodami HTTP.</span><span class="sxs-lookup"><span data-stu-id="dec8d-422">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="dec8d-423">Najpierw należy uruchomić mniejsze numery.</span><span class="sxs-lookup"><span data-stu-id="dec8d-423">Lower numbers run first.</span></span>
