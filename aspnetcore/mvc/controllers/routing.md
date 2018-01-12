---
title: Routing do akcji kontrolera
author: rick-anderson
description: 
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: d6e230351eb2f4c8549b54d75fd8e345718e6109
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="33fe2-103">Routing do akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="33fe2-103">Routing to Controller Actions</span></span>

<span data-ttu-id="33fe2-104">Przez [Ryan Nowak](https://github.com/rynowak) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33fe2-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33fe2-105">Routing korzysta z platformy ASP.NET Core MVC [oprogramowanie pośredniczące](../../fundamentals/middleware.md) zgodne adresy URL żądań przychodzących i zamapowania ich do akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-105">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="33fe2-106">Trasy są definiowane w kod uruchomienia lub atrybutów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="33fe2-107">Trasy opisano, jak powinny być zgodne ścieżki adresu URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="33fe2-108">Trasy są również używane do generowania adresów URL (w przypadku połączeń) wysłała w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="33fe2-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="33fe2-109">Akcje albo są kierowane tradycyjnie ani trasowane atrybutu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="33fe2-110">Umożliwia wprowadzania trasę do kontrolera lub akcji atrybutu routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="33fe2-111">Zobacz [mieszanym routingu](#routing-mixed-ref-label) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="33fe2-112">Ten dokument zostanie opisano interakcje między MVC i routing i jak typowe upewnij aplikacji MVC korzystanie z funkcji routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="33fe2-113">Zobacz [Routing](xref:fundamentals/routing) szczegółowe informacje na temat zaawansowanego routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="33fe2-114">Konfigurowanie routingu oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="33fe2-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="33fe2-115">W Twojej *Konfiguruj* — metoda może wyświetlić kodu podobne do:</span><span class="sxs-lookup"><span data-stu-id="33fe2-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="33fe2-116">Wewnątrz wywołania `UseMvc`, `MapRoute` służy do tworzenia jedną trasę, która będzie nazywamy `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="33fe2-117">Większość aplikacji MVC użyje trasy przy użyciu szablonu, podobnie jak `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="33fe2-118">Szablon trasy `"{controller=Home}/{action=Index}/{id?}"` może dopasować Ścieżka adresu URL, takie jak `/Products/Details/5` , który wyodrębnia wartości trasy `{ controller = Products, action = Details, id = 5 }` przez tokenizing ścieżki.</span><span class="sxs-lookup"><span data-stu-id="33fe2-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="33fe2-119">MVC będzie podejmować próby zlokalizowania kontrolerze o nazwie `ProductsController` i Uruchom akcję `Details`:</span><span class="sxs-lookup"><span data-stu-id="33fe2-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="33fe2-120">Pamiętaj, że w tym przykładzie wiązania modelu używać wartości `id = 5` można ustawić `id` parametr `5` podczas wywoływania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="33fe2-121">Zobacz [powiązanie modelu](../models/model-binding.md) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="33fe2-122">Przy użyciu `default` trasy:</span><span class="sxs-lookup"><span data-stu-id="33fe2-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="33fe2-123">Szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="33fe2-123">The route template:</span></span>

* <span data-ttu-id="33fe2-124">`{controller=Home}`definiuje `Home` jako domyślny`controller`</span><span class="sxs-lookup"><span data-stu-id="33fe2-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="33fe2-125">`{action=Index}`definiuje `Index` jako domyślny`action`</span><span class="sxs-lookup"><span data-stu-id="33fe2-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="33fe2-126">`{id?}`definiuje `id` jako opcjonalna</span><span class="sxs-lookup"><span data-stu-id="33fe2-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="33fe2-127">Parametry opcjonalne trasy i domyślne nie musi być obecny w ścieżkę adresu URL, pod kątem dopasowania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-127">Default and optional route parameters do not need to be present in the URL path for a match.</span></span> <span data-ttu-id="33fe2-128">Zobacz [odwołania do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="33fe2-129">`"{controller=Home}/{action=Index}/{id?}"`można dopasować ścieżkę adresu URL `/` i spowoduje wartości trasy `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="33fe2-130">Wartości `controller` i `action` należy użyć wartości domyślnych `id` nie tworzy wartości, ponieważ nie nie odpowiadającym segmencie ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-130">The values for `controller` and `action` make use of the default values, `id` does not produce a value since there is no corresponding segment in the URL path.</span></span> <span data-ttu-id="33fe2-131">MVC użyje tych wartości trasy, aby wybrać `HomeController` i `Index` akcji:</span><span class="sxs-lookup"><span data-stu-id="33fe2-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="33fe2-132">Przy użyciu tej definicji kontrolera i szablon trasy `HomeController.Index` będzie można wykonać akcji dla wszystkich następujących ścieżkach adresu URL:</span><span class="sxs-lookup"><span data-stu-id="33fe2-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="33fe2-133">Metoda wygody `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="33fe2-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="33fe2-134">Może być używany do zastąpienia:</span><span class="sxs-lookup"><span data-stu-id="33fe2-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="33fe2-135">`UseMvc`i `UseMvcWithDefaultRoute` dodać wystąpienia `RouterMiddleware` do potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="33fe2-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="33fe2-136">MVC nie bezpośrednią interakcję z oprogramowaniem pośredniczącym i używa routingu do obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="33fe2-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="33fe2-137">MVC jest połączona z tras za pomocą wystąpienia `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="33fe2-138">Kod wewnątrz `UseMvc` jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="33fe2-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="33fe2-139">`UseMvc`bezpośrednio nie definiuje żadnych tras dodaje symbolu zastępczego do kolekcji tras dla `attribute` trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-139">`UseMvc` does not directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="33fe2-140">Przeciążenie `UseMvc(Action<IRouteBuilder>)` umożliwia dodanie własnych tras i obsługuje również trasami atrybutów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="33fe2-141">`UseMvc`i wszystkie jego zmian dodaje symbol zastępczy dla tras atrybutów — trasami atrybutów są zawsze dostępne, niezależnie od sposobu skonfigurowania `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="33fe2-142">`UseMvcWithDefaultRoute`definiuje domyślną trasę i obsługuje routing atrybutu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="33fe2-143">[Trasami atrybutów](#attribute-routing-ref-label) sekcja zawiera więcej informacji na temat trasami atrybutów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="33fe2-144">Konwencjonalne routingu</span><span class="sxs-lookup"><span data-stu-id="33fe2-144">Conventional routing</span></span>

<span data-ttu-id="33fe2-145">`default` Trasy:</span><span class="sxs-lookup"><span data-stu-id="33fe2-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="33fe2-146">Przykładem jest *routingu z konwencjonalnej*.</span><span class="sxs-lookup"><span data-stu-id="33fe2-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="33fe2-147">Nazywamy ten styl *routingu z konwencjonalnej* ponieważ definiuje on *Konwencji* dla ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="33fe2-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="33fe2-148">pierwszy segment ścieżki mapuje nazwę kontrolera</span><span class="sxs-lookup"><span data-stu-id="33fe2-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="33fe2-149">drugi mapuje nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-149">the second maps to the action name.</span></span>

* <span data-ttu-id="33fe2-150">trzeci segmentu jest używana do opcjonalny `id` służy do mapowania do modelu jednostki</span><span class="sxs-lookup"><span data-stu-id="33fe2-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="33fe2-151">Za pomocą tej `default` trasy, ścieżkę adresu URL `/Products/List` mapuje `ProductsController.List` akcji, i `/Blog/Article/17` mapuje `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="33fe2-152">To mapowanie opiera się na nazwy kontrolera i akcji **tylko** i nie jest oparte na przestrzeni nazw, lokalizacji źródłowych plików lub parametry metody.</span><span class="sxs-lookup"><span data-stu-id="33fe2-152">This mapping is based on the controller and action names **only** and is not based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="33fe2-153">Korzystania z konwencjonalnej routingu z trasy domyślnej umożliwia szybkie utworzenie aplikacji bez konieczności stworzyć nowy wzorzec adres URL dla każdej akcji, którą należy zdefiniować.</span><span class="sxs-lookup"><span data-stu-id="33fe2-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="33fe2-154">Dla aplikacji z akcjami CRUD styl o spójności dla adresów URL w kontrolerach może pomóc uprościć kod i wprowadzić bardziej przewidywalne interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33fe2-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="33fe2-155">`id` Jest zdefiniowany jako opcjonalne w szablonie trasy, co oznacza, że czynności użytkownika można wykonać bez Identyfikatora dostarczane jako część adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="33fe2-156">Zazwyczaj co się stanie, jeśli `id` pominięto w adresie URL jest, że zostanie ustawiona do `0` przez powiązanie modelu, a w rezultacie jednostki nie zostaną znalezione w odpowiadającym bazy danych `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="33fe2-157">Atrybut routingu można zapewniają precyzyjną kontrolę, aby identyfikator wymagane w przypadku niektórych działań, a nie do innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="33fe2-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="33fe2-158">Konwencja dokumentacja będzie zawierać następujące parametry opcjonalne, takich jak `id` po ich prawdopodobnie znajdują się w odpowiednie użycie.</span><span class="sxs-lookup"><span data-stu-id="33fe2-158">By convention the documentation will include optional parameters like `id` when they are likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="33fe2-159">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="33fe2-159">Multiple routes</span></span>

<span data-ttu-id="33fe2-160">Można dodać wiele tras wewnątrz `UseMvc` przez dodanie kolejnych wywołań do `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="33fe2-161">Dzięki temu można zdefiniować wiele konwencje lub dodać z konwencjonalnej tras, które są przeznaczone dla określonej akcji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="33fe2-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="33fe2-162">`blog` Tutaj trasa jest *dedykowanych trasy z konwencjonalnej*, co oznacza, że korzysta z konwencjonalnej systemu routingu, ale jest dedykowany do określonej akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="33fe2-163">Ponieważ `controller` i `action` nie są wyświetlane w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i w związku z tym zawsze przypisze tej trasy do akcji `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="33fe2-164">Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, są one dodawane.</span><span class="sxs-lookup"><span data-stu-id="33fe2-164">Routes in the route collection are ordered, and will be processed in the order they are added.</span></span> <span data-ttu-id="33fe2-165">Tak, w tym przykładzie `blog` trasy zostaną sprawdzone przed `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="33fe2-166">*Dedykowane tras z konwencjonalnej* często używają trasy wychwytywania parametry, takie jak `{*article}` do przechwytywania pozostała część ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="33fe2-167">Może być zbyt intensywnie trasę co oznacza, że jest on zgodny adresów URL, które mają być dopasowywane przez innych tras.</span><span class="sxs-lookup"><span data-stu-id="33fe2-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="33fe2-168">Później w tabeli tras, aby rozwiązać ten problem, należy umieścić trasy "zachłannego".</span><span class="sxs-lookup"><span data-stu-id="33fe2-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="33fe2-169">Powrotu</span><span class="sxs-lookup"><span data-stu-id="33fe2-169">Fallback</span></span>

<span data-ttu-id="33fe2-170">W ramach procesu przetwarzania żądania MVC zweryfikuje, że wartości trasy może służyć do znajdowania kontrolera i akcji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="33fe2-171">Jeśli akcja wartości trasy nie są zgodne. następnie trasy nie jest uznawane za zgodne i dalej trasy zostaną sprawdzone.</span><span class="sxs-lookup"><span data-stu-id="33fe2-171">If the route values don't match an action then the route is not considered a match, and the next route will be tried.</span></span> <span data-ttu-id="33fe2-172">Ta metoda jest wywoływana *rezerwowy*, i jest on przeznaczony do uprościć przypadków nakładają się tras z konwencjonalnej.</span><span class="sxs-lookup"><span data-stu-id="33fe2-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="33fe2-173">Akcje usuwania niejednoznaczności w nazwach</span><span class="sxs-lookup"><span data-stu-id="33fe2-173">Disambiguating actions</span></span>

<span data-ttu-id="33fe2-174">Gdy dwie akcje odpowiada za pośrednictwem routingu, MVC musi odróżniania do wybierz "Najważniejsze" kandydata w przeciwnym razie Zgłoś wyjątek.</span><span class="sxs-lookup"><span data-stu-id="33fe2-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="33fe2-175">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="33fe2-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="33fe2-176">Ten kontroler definiuje dwie akcje zgodne ścieżkę adresu URL `/Products/Edit/17` i przekazywanie danych `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="33fe2-177">Jest to typowy wzorzec kontrolerów MVC gdzie `Edit(int)` przedstawia formularz służący do edytowania produktu, i `Edit(int, Product)` przetwarza przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="33fe2-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="33fe2-178">Aby było to możliwe należy wybrać MVC `Edit(int, Product)` po żądaniu HTTP `POST` i `Edit(int)` po zlecenie HTTP jest inny.</span><span class="sxs-lookup"><span data-stu-id="33fe2-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="33fe2-179">`HttpPostAttribute` ( `[HttpPost]` ) To implementacja `IActionConstraint` umożliwiające tylko akcję, należy wybrać, jeśli polecenie HTTP jest żądaniem `POST`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="33fe2-180">Obecność `IActionConstraint` sprawia, że `Edit(int, Product)` lepiej pasuje niż `Edit(int)`, więc `Edit(int, Product)` zostaną sprawdzone najpierw.</span><span class="sxs-lookup"><span data-stu-id="33fe2-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="33fe2-181">Wystarczy do tworzenia niestandardowych `IActionConstraint` implementacje w scenariuszach specjalne, ale jego zrozumieć rolę atrybutów, takich jak `HttpPostAttribute` -podobne atrybuty są definiowane dla innych zleceń HTTP.</span><span class="sxs-lookup"><span data-stu-id="33fe2-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="33fe2-182">W routingu z konwencjonalnej jest typowe dla akcji używać tej samej nazwy akcji, gdy są one częścią `show form -> submit form` przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-182">In conventional routing it's common for actions to use the same action name when they are part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="33fe2-183">Wygodę tego wzorca staną się bardziej widoczne po przejrzeniu [IActionConstraint opis](#understanding-iactionconstraint) sekcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="33fe2-184">Jeśli wiele tras zgodne i MVC nie można znaleźć "najlepsze" trasy, zgłosi `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="33fe2-185">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="33fe2-185">Route names</span></span>

<span data-ttu-id="33fe2-186">Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwy tras:</span><span class="sxs-lookup"><span data-stu-id="33fe2-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="33fe2-187">Nazwy tras udostępniania trasy nazwę logiczną, aby nazwanej trasy może służyć do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="33fe2-188">Podczas generowania adresu URL skomplikowane kolejność trasy może spowodować znacznie ułatwia tworzenie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="33fe2-189">Nazwy tras muszą być unikatowe całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="33fe2-190">Nazwy tras nie mają wpływu na adres URL dopasowania lub obsługiwanie żądań; są one używane tylko do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-190">Route names have no impact on URL matching or handling of requests; they are used only for URL generation.</span></span> <span data-ttu-id="33fe2-191">[Routing](xref:fundamentals/routing) przedstawiono bardziej szczegółowe informacje o tym generowania adresu URL w pomocników specyficzne dla platformy MVC generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="33fe2-192">Atrybut routingu</span><span class="sxs-lookup"><span data-stu-id="33fe2-192">Attribute routing</span></span>

<span data-ttu-id="33fe2-193">Atrybut routingu korzysta z zestawu atrybutów mapowania akcje bezpośrednio na szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="33fe2-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="33fe2-194">W poniższym przykładzie `app.UseMvc();` jest używany w `Configure` — metoda i trasy jest przekazywana.</span><span class="sxs-lookup"><span data-stu-id="33fe2-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="33fe2-195">`HomeController` Będzie zgodny zestaw adresów URL podobny do trasy domyślnej `{controller=Home}/{action=Index}/{id?}` spełniają kryteria:</span><span class="sxs-lookup"><span data-stu-id="33fe2-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="33fe2-196">`HomeController.Index()` Będzie można wykonać akcji dla ścieżki adresu URL `/`, `/Home`, lub `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="33fe2-197">W tym przykładzie wyróżniono klucza programowania różnicę między atrybutu i konwencjonalnych routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="33fe2-198">Routing atrybut wymaga więcej dane wejściowe, aby określić trasy; trasy domyślnej z konwencjonalnej obsługuje więcej krótkiej formie trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="33fe2-199">Jednak trasami atrybutów umożliwia (i wymaga) precyzyjne dotyczą z których każda akcja szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="33fe2-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="33fe2-200">Za pomocą atrybutu routingu, nazwy kontrolera i nazwy akcji odtwarzać **nie** roli wybrana akcja.</span><span class="sxs-lookup"><span data-stu-id="33fe2-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="33fe2-201">W tym przykładzie będzie pasował do tych samych adresów URL co w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="33fe2-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="33fe2-202">Powyżej szablonów tras nie definiują parametry trasy dla `action`, `area`, i `controller`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="33fe2-203">W rzeczywistości te parametry trasy nie są dozwolone w tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="33fe2-204">Ponieważ szablon trasy jest już skojarzony z akcją, go nie ma sensu można przeanalizować nazwy akcji z adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="33fe2-205">Atrybut routingu z atrybutami Http [zlecenie]</span><span class="sxs-lookup"><span data-stu-id="33fe2-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="33fe2-206">Atrybut routingu, można także wprowadzić użycie `Http[Verb]` atrybutów, takich jak `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="33fe2-207">Wszystkie te atrybuty mogą akceptować szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="33fe2-208">W tym przykładzie przedstawiono dwie akcje, które pasuje do tego samego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="33fe2-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="33fe2-209">Dla ścieżki adresu URL, takie jak `/products` `ProductsApi.ListProducts` będzie można wykonać akcji, gdy jest zlecenie HTTP `GET` i `ProductsApi.CreateProduct` zostaną wykonane, jeśli polecenie HTTP jest żądaniem `POST`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="33fe2-210">Atrybut routingu najpierw odpowiada adresowi URL zestawem szablony trasy zdefiniowane przez atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="33fe2-211">Po zgodny szablon trasy, `IActionConstraint` ograniczenia są stosowane w celu określenia, jakie akcje mogą być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="33fe2-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="33fe2-212">Podczas konstruowania interfejsu API REST, jest rzadko, że można użyć `[Route(...)]` na metody akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="33fe2-213">Lepiej użyć konkretnego więcej `Http*Verb*Attributes` do dokładnego obsługuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="33fe2-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="33fe2-214">Klienci interfejsów API REST powinni znać ścieżki i zleceń HTTP mapowane na operacje logiczne określone.</span><span class="sxs-lookup"><span data-stu-id="33fe2-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="33fe2-215">Ponieważ trasa atrybutu ma zastosowanie do określonej akcji, jest proste parametrów wymaganych w ramach definicji szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="33fe2-216">W tym przykładzie `id` jest wymagana ścieżka adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="33fe2-217">`ProductsApi.GetProduct(int)` Będzie można wykonać akcji dla ścieżki adresu URL, takie jak `/products/3` , ale nie ścieżka adresu URL, takie jak `/products`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="33fe2-218">Zobacz [Routing](../../fundamentals/routing.md) pełen opis szablonów tras i powiązane opcje.</span><span class="sxs-lookup"><span data-stu-id="33fe2-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="33fe2-219">Nazwa trasy</span><span class="sxs-lookup"><span data-stu-id="33fe2-219">Route Name</span></span>

<span data-ttu-id="33fe2-220">Poniższy kod definiuje *nazwy trasy* z `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="33fe2-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="33fe2-221">Nazwy trasy mogą służyć do generowania adresu URL na podstawie określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="33fe2-222">Nazwy trasy nie mają wpływu na adres URL dopasowania zachowanie routingu i służą tylko do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="33fe2-223">Nazwy tras muszą być unikatowe całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="33fe2-224">Natomiast to z konwencjonalnej *trasy domyślnej*, który definiuje `id` parametr jako opcjonalny (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="33fe2-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="33fe2-225">Tę możliwość, aby precyzyjnie określić interfejsy API ma zalet, na przykład pozwala `/products` i `/products/5` zostać przekazana do różnych działań.</span><span class="sxs-lookup"><span data-stu-id="33fe2-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="33fe2-226">Łączenie tras</span><span class="sxs-lookup"><span data-stu-id="33fe2-226">Combining routes</span></span>

<span data-ttu-id="33fe2-227">Aby trasami atrybutów mniej powtarzane, atrybutów trasy dla kontrolera są łączone z atrybutów trasy dla poszczególnych działań.</span><span class="sxs-lookup"><span data-stu-id="33fe2-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="33fe2-228">Wszystkie szablony trasy zdefiniowane na kontrolerze jest dołączany na początku w szablonach tras na akcje.</span><span class="sxs-lookup"><span data-stu-id="33fe2-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="33fe2-229">Umieszczenie atrybut trasy na kontrolerze sprawia, że **wszystkie** akcji w kontrolerze korzystać z routingu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="33fe2-230">W tym przykładzie ścieżkę adresu URL `/products` może dopasować `ProductsApi.ListProducts`i Ścieżka adresu URL `/products/5` może dopasować `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="33fe2-231">Oba te akcje dopasowanie tylko HTTP `GET` ponieważ są one oznaczone `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-231">Both of these actions only match HTTP `GET` because they are decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="33fe2-232">Trasy szablony zastosować do akcji, które rozpoczynają się od `/` nie łączone z szablonami trasy stosowane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33fe2-232">Route templates applied to an action that begin with a `/` do not get combined with route templates applied to the controller.</span></span> <span data-ttu-id="33fe2-233">W tym przykładzie dopasowuje zestaw ścieżki adresu URL podobny do *trasy domyślnej*.</span><span class="sxs-lookup"><span data-stu-id="33fe2-233">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="33fe2-234">Porządkowanie tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="33fe2-234">Ordering attribute routes</span></span>

<span data-ttu-id="33fe2-235">W przeciwieństwie do konwencjonalnych tras, których wykonywany w kolejności zdefiniowanej trasami atrybutów buduje drzewo i jednocześnie dopasowuje wszystkie trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="33fe2-236">Zachowuje się jak — Jeśli wejścia dla trasy zostały umieszczone w kolejności idealne; specyficzny tras ma możliwość wykonania przed bardziej ogólne trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="33fe2-237">Na przykład trasy, takich jak `blog/search/{topic}` jest bardziej szczegółowy niż trasy, takich jak `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="33fe2-238">Mówiąc logicznie `blog/search/{topic}` trasy "", domyślnie uruchamia, ponieważ jest to tylko za pośrednictwem porządkowania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="33fe2-239">Korzystania z konwencjonalnej routingu, dewelopera jest odpowiedzialny za umieszczenie trasy w odpowiedni sposób.</span><span class="sxs-lookup"><span data-stu-id="33fe2-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="33fe2-240">Tras atrybutów można skonfigurować kolejność przy użyciu `Order` właściwości wszystkich atrybutów trasy struktura dostępna.</span><span class="sxs-lookup"><span data-stu-id="33fe2-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="33fe2-241">Trasy są przetwarzane zgodnie z rosnącym sortowania `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="33fe2-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="33fe2-242">Domyślna kolejność to `0`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-242">The default order is `0`.</span></span> <span data-ttu-id="33fe2-243">Konfigurowanie tras za pomocą `Order = -1` będzie wykonywany przed tras, na których nie należy ustawiać zamówienia.</span><span class="sxs-lookup"><span data-stu-id="33fe2-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="33fe2-244">Konfigurowanie tras za pomocą `Order = 1` zostanie uruchomiony po określeniu kolejności trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="33fe2-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="33fe2-245">Unikaj w zależności od `Order`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="33fe2-246">Jeśli adres URL obszaru wymaga wartości jawne kolejność trasy poprawnie, jest prawdopodobnie mylące również klientom.</span><span class="sxs-lookup"><span data-stu-id="33fe2-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="33fe2-247">Ogólnie rzecz biorąc trasami atrybutów wybierze poprawnej trasy z pasującymi adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="33fe2-248">Jeśli domyślna kolejność używany do generowania adresu URL nie działa, używając nazwy trasy zastąpienia jest zazwyczaj łatwiejsze niż w przypadku stosowania `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="33fe2-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="33fe2-249">Token zastępczy w szablonach tras ([kontrolera], [action] [obszaru])</span><span class="sxs-lookup"><span data-stu-id="33fe2-249">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="33fe2-250">Dla wygody, obsługi tras atrybutów *token zastępczy* umieszczając tokenu w nawiasy kwadratowe (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="33fe2-250">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="33fe2-251">Tokeny `[action]`, `[area]`, i `[controller]` zostaną zastąpione wartościami nazwy akcji, nazwy obszaru i nazwy kontrolera z akcji, których trasa jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="33fe2-251">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="33fe2-252">W tym przykładzie akcje może dopasować ścieżki adresu URL, zgodnie z opisem w komentarzach:</span><span class="sxs-lookup"><span data-stu-id="33fe2-252">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="33fe2-253">Token zastępczy występuje jako ostatni etap tworzenia tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-253">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="33fe2-254">Powyższy przykład będą zachowywać się taka sama jak następujący kod:</span><span class="sxs-lookup"><span data-stu-id="33fe2-254">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="33fe2-255">Tras atrybutów można również łączyć z dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="33fe2-255">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="33fe2-256">Jest to szczególnie wydajna, połączeniu z zastępujący tokenu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-256">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="33fe2-257">Token zastępczy ma również zastosowanie do nazwy trasy zdefiniowane przez atrybut trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-257">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="33fe2-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]`wygeneruje nazwę unikatową trasę dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="33fe2-259">Aby dopasować ogranicznik literału zastępujący tokenu `[` lub `]`, sekwencji ucieczki powtarzając znak (`[[` lub `]]`).</span><span class="sxs-lookup"><span data-stu-id="33fe2-259">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="33fe2-260">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="33fe2-260">Multiple Routes</span></span>

<span data-ttu-id="33fe2-261">Atrybut Obsługa routingu definiowania wiele tras, które dostarczone do tego samego działania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-261">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="33fe2-262">Użycie najbardziej typowe ma naśladować zachowanie *konwencjonalnych trasy domyślnej* jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="33fe2-262">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="33fe2-263">Wprowadzenie różnych atrybutów trasy na kontrolerze oznacza, że każda z nich zostaną połączone z każdym z atrybutów trasy dla metody akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-263">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="33fe2-264">Gdy różnych atrybutów trasy (które implementują `IActionConstraint`) są umieszczane w akcji, wówczas każde ograniczenie akcji łączy przy użyciu szablonu trasy z atrybut, który jest określony.</span><span class="sxs-lookup"><span data-stu-id="33fe2-264">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="33fe2-265">Podczas za pomocą wiele tras na akcje może wydawać się zaawansowanych, lepiej jest proste i dobrze zdefiniowany aktualizowania miejsca adres URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-265">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="33fe2-266">Użyj wiele tras na akcje tylko wtedy, gdy jest to wymagane, na przykład do obsługi istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-266">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="33fe2-267">Określenie parametrów opcjonalny atrybut trasy, wartości domyślne i ograniczenia</span><span class="sxs-lookup"><span data-stu-id="33fe2-267">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="33fe2-268">Tras atrybutów obsługuje takiej samej składni wbudowanego jako konwencjonalnej trasy, aby określić następujące parametry opcjonalne, wartości domyślnych i ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="33fe2-268">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="33fe2-269">Zobacz [odwołania do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-269">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="33fe2-270">Atrybuty niestandardowe trasy za pomocą`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="33fe2-270">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="33fe2-271">Wszystkie atrybuty trasy w ramach ( `[Route(...)]`, `[HttpGet(...)]` , itd.) implementuje `IRouteTemplateProvider` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-271">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="33fe2-272">MVC wyszukuje atrybuty klasy kontrolera i metody akcji uruchamiania i używa tych, które implementują aplikacji `IRouteTemplateProvider` do utworzenia wstępnego zestawu trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-272">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="33fe2-273">Można zaimplementować `IRouteTemplateProvider` do zdefiniowania własnych atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-273">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="33fe2-274">Każdy `IRouteTemplateProvider` można zdefiniować jedną trasę z szablonem tras niestandardowych kolejność i nazwy:</span><span class="sxs-lookup"><span data-stu-id="33fe2-274">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="33fe2-275">W powyższym przykładzie atrybut automatycznie ustawia `Template` do `"api/[controller]"` podczas `[MyApiController]` została zastosowana.</span><span class="sxs-lookup"><span data-stu-id="33fe2-275">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="33fe2-276">Dostosowywanie tras atrybutów za pomocą modelu aplikacji</span><span class="sxs-lookup"><span data-stu-id="33fe2-276">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="33fe2-277">*Model aplikacji* jest model obiektowy tworzone przy uruchamianiu ze wszystkimi metadane używane przez MVC do routingu i wykonaj czynności użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33fe2-277">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="33fe2-278">*Model aplikacji* obejmuje wszystkie dane zebrane z atrybutów trasy (za pośrednictwem `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="33fe2-278">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="33fe2-279">Można napisać *konwencje* można zmodyfikować modelu aplikacji w czasie uruchamiania, aby dostosować zachowanie routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-279">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="33fe2-280">W tej sekcji przedstawiono prosty przykład Dostosowywanie routingu za pomocą modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-280">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="33fe2-281">Mieszane routingu: atrybut w porównaniu z konwencjonalnej routingu</span><span class="sxs-lookup"><span data-stu-id="33fe2-281">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="33fe2-282">Aplikacji programu MVC, można mieszać korzystanie z konwencjonalnej i atrybut routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-282">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="33fe2-283">Jest to typowe tras z konwencjonalnej kontrolerów obsługujących strony HTML do przeglądarek, a atrybut routingu dla kontrolerów obsługujących interfejsów API REST.</span><span class="sxs-lookup"><span data-stu-id="33fe2-283">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="33fe2-284">Akcje albo są kierowane tradycyjnie ani trasowane atrybutu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-284">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="33fe2-285">Umożliwia wprowadzania trasę do kontrolera lub akcji atrybutu routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-285">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="33fe2-286">Akcje, które definiują tras atrybutów nie można osiągnąć za pomocą trasy z konwencjonalnej i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="33fe2-286">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="33fe2-287">**Wszelkie** atrybut trasy na kontrolerze powoduje, że wszystkie akcje w atrybucie kontrolera routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-287">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="33fe2-288">Co odróżnia dwa typy systemów routingu jest procesem zastosowane po adres URL jest zgodna z szablonem trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-288">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="33fe2-289">W routingu z konwencjonalnej, wartości trasy ze dopasowania służą do tabeli odnośników wszystkich akcji routingiem z konwencjonalnej wyboru tej akcji i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33fe2-289">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="33fe2-290">W atrybucie routingu, każdego szablonu jest już skojarzony z akcją i niezbędne jest Brak dalszych wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-290">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="33fe2-291">Generowania adresu URL</span><span class="sxs-lookup"><span data-stu-id="33fe2-291">URL Generation</span></span>

<span data-ttu-id="33fe2-292">MVC aplikacje mogą używać routingu w funkcji generowania adresu URL do generowania łączy URL akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-292">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="33fe2-293">Generowania adresów URL eliminuje hardcoding adresów URL, wprowadzenie kodu bardziej niezawodny i łatwy w obsłudze.</span><span class="sxs-lookup"><span data-stu-id="33fe2-293">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="33fe2-294">W tej sekcji koncentruje się na funkcje generowania adresu URL, które są oferowane przez MVC i obejmuje tylko podstawowe informacje dotyczące działania generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-294">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="33fe2-295">Zobacz [Routing](../../fundamentals/routing.md) szczegółowy opis generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-295">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="33fe2-296">`IUrlHelper` Interfejsu jest podstawowej infrastruktury między MVC i routing do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-296">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="33fe2-297">Można znaleźć wystąpienia `IUrlHelper` dostępne za pośrednictwem `Url` właściwości kontrolerów, widoków i składniki w widoku.</span><span class="sxs-lookup"><span data-stu-id="33fe2-297">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="33fe2-298">W tym przykładzie `IUrlHelper` interfejs jest używany przez `Controller.Url` właściwości do generowania adresu URL do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-298">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="33fe2-299">Jeśli aplikacja korzysta z konwencjonalnej domyślne trasy, wartości `url` zmienna będzie ciąg ścieżki adresu URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-299">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="33fe2-300">Ta ścieżka adresu URL jest tworzony przez routingu przez połączenie wartości tras z bieżącego żądania (otoczenia wartości) z wartości przekazanych do `Url.Action` i podstawianie tych wartości do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="33fe2-300">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="33fe2-301">Każdy parametr trasy w szablonie trasy ma wartość zastąpione nazw zgodną z wartości i wartości otoczenia.</span><span class="sxs-lookup"><span data-stu-id="33fe2-301">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="33fe2-302">Parametru trasy, który nie ma wartości można użyć wartości domyślnej, jeśli zawiera co najmniej pominięte, jeśli jest to pozycja opcjonalna (tak jak w przypadku liczby `id` w tym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="33fe2-302">A route parameter that does not have a value can use a default value if it has one, or be skipped if it is optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="33fe2-303">Generowania adresu URL zakończy się niepowodzeniem, jeśli parametr wszelkie wymagane trasy nie ma odpowiadającej jej wartości.</span><span class="sxs-lookup"><span data-stu-id="33fe2-303">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="33fe2-304">Jeśli generowania adresu URL trasy nie powiedzie się, dalej trasy zostanie podjęta próba dopóki wszystkie trasy zostały wypróbowane lub Znaleziono dopasowanie.</span><span class="sxs-lookup"><span data-stu-id="33fe2-304">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="33fe2-305">Przykład `Url.Action` powyżej zakłada routingu z konwencjonalnej, ale adres URL generowania działa podobnie trasami atrybutów, chociaż przedstawione koncepcje mają różne.</span><span class="sxs-lookup"><span data-stu-id="33fe2-305">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="33fe2-306">Z konwencjonalnej routingiem, wartości trasy są używane do rozwiń szablonu i wartości trasy dla `controller` i `action` zwykle pojawiają się w tym szablonie — to działa, ponieważ odpowiednia adresy URL są dopasowane wg routingu *Konwencji*.</span><span class="sxs-lookup"><span data-stu-id="33fe2-306">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="33fe2-307">W atrybucie routingu, wartości trasy `controller` i `action` nie mogą występować w szablonie — zamiast tego są one używane do odszukania jaki szablon zostanie użyty.</span><span class="sxs-lookup"><span data-stu-id="33fe2-307">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they are instead used to look up which template to use.</span></span>

<span data-ttu-id="33fe2-308">W tym przykładzie użyto trasami atrybutów:</span><span class="sxs-lookup"><span data-stu-id="33fe2-308">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="33fe2-309">MVC tworzy tabelę odnośników wszystkich akcji atrybutu kierowane i będzie odpowiadała `controller` i `action` wartości, aby wybrać szablon trasy do użycia podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-309">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="33fe2-310">W powyższym przykładowym `custom/url/to/destination` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="33fe2-310">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="33fe2-311">Generowania adresów URL według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="33fe2-311">Generating URLs by action name</span></span>

<span data-ttu-id="33fe2-312">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="33fe2-312">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="33fe2-313">`Action`) i wszystkich powiązanych przeciążenia wszystkie są oparte na pomysł, że chcesz określić, jakie łączysz przez określenie nazwy kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-313">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="33fe2-314">Korzystając z `Url.Action`, wartości bieżącej trasy `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` są częścią zarówno *otoczenia wartości* **i** *wartości*.</span><span class="sxs-lookup"><span data-stu-id="33fe2-314">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="33fe2-315">Metoda `Url.Action`, zawsze używa bieżącej wartości `action` i `controller` i wygeneruje ścieżki adresu URL, który kieruje do bieżącej akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-315">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="33fe2-316">Routing próbował wartości w otaczającym wartości Wypełnij informacje nie zostały podane podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-316">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="33fe2-317">Za pomocą trasy, takich jak `{a}/{b}/{c}/{d}` i wartości otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma za mało informacji do generowania adresu URL bez żadnych dodatkowych wartości — ponieważ rozesłać wszystkie parametry mają wartość.</span><span class="sxs-lookup"><span data-stu-id="33fe2-317">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="33fe2-318">Jeśli dodano wartość `{ d = Donovan }`, wartość `{ d = David }` zostałyby one zignorowane, a będzie wygenerowany ścieżkę adresu URL `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-318">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="33fe2-319">Adres URL ścieżki są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="33fe2-319">URL paths are hierarchical.</span></span> <span data-ttu-id="33fe2-320">W przykładzie przedstawionym powyżej należy dodać wartość `{ c = Cheryl }`, wartość `{ c = Carol, d = David }` zostałyby one zignorowane.</span><span class="sxs-lookup"><span data-stu-id="33fe2-320">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="33fe2-321">W takim przypadku nie mamy już wartość `d` i generowania adresu URL zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="33fe2-321">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="33fe2-322">Musisz określić żądaną wartość `c` i `d`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-322">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="33fe2-323">Można oczekiwać trafień tego problemu z trasa domyślna (`{controller}/{action}/{id?}`)-, ale rzadko wystąpi ten problem, w praktyce jako `Url.Action` będzie zawsze jawnie określać `controller` i `action` wartość.</span><span class="sxs-lookup"><span data-stu-id="33fe2-323">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="33fe2-324">Dłużej przeciążeń `Url.Action` również podjęcia dodatkowych *wartości trasy* obiekt do innego niż Podaj wartości dla parametrów trasy `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-324">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="33fe2-325">Zobaczysz najczęściej to używane z `id` jak `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-325">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="33fe2-326">Konwencja *wartości trasy* obiekt jest zwykle typu anonimowego, ale można go również `IDictionary<>` lub *zwykły stary obiekt .NET*.</span><span class="sxs-lookup"><span data-stu-id="33fe2-326">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="33fe2-327">Wartości dodatkowe trasy, które nie odpowiadają parametrów trasy są umieszczane w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-327">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="33fe2-328">Aby utworzyć bezwzględny adres URL, użyj przeciążenia, które akceptuje `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="33fe2-328">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="33fe2-329">Generowania adresów URL przez trasy</span><span class="sxs-lookup"><span data-stu-id="33fe2-329">Generating URLs by route</span></span>

<span data-ttu-id="33fe2-330">Powyższy kod przedstawiona wygenerowania adresu URL, przekazując nazwy kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-330">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="33fe2-331">`IUrlHelper`dostępne są także `Url.RouteUrl` rodziny metod.</span><span class="sxs-lookup"><span data-stu-id="33fe2-331">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="33fe2-332">Te metody są podobne do `Url.Action`, ale nie należy kopiować bieżące wartości `action` i `controller` wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-332">These methods are similar to `Url.Action`, but they do not copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="33fe2-333">Najbardziej typowe obciążenie jest określenie nazwy trasy na potrzeby generowania adresu URL, zazwyczaj określoną trasę *bez* określania nazwy kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-333">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="33fe2-334">Generowania adresów URL w formacie HTML</span><span class="sxs-lookup"><span data-stu-id="33fe2-334">Generating URLs in HTML</span></span>

<span data-ttu-id="33fe2-335">`IHtmlHelper`udostępnia `HtmlHelper` metody `Html.BeginForm` i `Html.ActionLink` do generowania `<form>` i `<a>` elementy odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="33fe2-335">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="33fe2-336">Użyj tych metod `Url.Action` do generowania adresu URL i akceptują argumentów podobne.</span><span class="sxs-lookup"><span data-stu-id="33fe2-336">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="33fe2-337">`Url.RouteUrl` Towarzyszami dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink` zawierające podobne funkcje.</span><span class="sxs-lookup"><span data-stu-id="33fe2-337">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="33fe2-338">TagHelpers generowania adresów URL za pośrednictwem `form` pomocnika tagów i `<a>` pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-338">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="33fe2-339">Oba te użyj `IUrlHelper` ich wdrażania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-339">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="33fe2-340">Zobacz [pracy z formularzami](../views/working-with-forms.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-340">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="33fe2-341">W widokach `IUrlHelper` jest dostępna za pośrednictwem `Url` właściwości dla dowolnego ad hoc generowania adresu URL nie pasuje do żadnego z powyższych.</span><span class="sxs-lookup"><span data-stu-id="33fe2-341">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="33fe2-342">Generowania adresów URL w wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="33fe2-342">Generating URLS in Action Results</span></span>

<span data-ttu-id="33fe2-343">Powyższe przykłady wykazały, przy użyciu `IUrlHelper` w kontrolerze podczas generowania adresu URL jako część wynik akcji najbardziej typowe obciążenie w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="33fe2-343">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="33fe2-344">`ControllerBase` i `Controller` klas podstawowych udostępniające podręczne metody dla wyników akcji, które odwołują się do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-344">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="33fe2-345">Jeden typowy sposób jest przekierowanie po akceptowanie danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33fe2-345">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="33fe2-346">Metody fabryki wyniki akcji wykonaj podobnego wzorca do metod `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-346">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="33fe2-347">Szczególnych przypadkach dla dedykowanych tras z konwencjonalnej</span><span class="sxs-lookup"><span data-stu-id="33fe2-347">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="33fe2-348">Routing z konwencjonalnej służy specjalny rodzaj definicji trasy o nazwie *dedykowanych trasy z konwencjonalnej*.</span><span class="sxs-lookup"><span data-stu-id="33fe2-348">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="33fe2-349">W poniższym przykładzie trasy o nazwie `blog` jest dedykowany trasy konwencjonalnej.</span><span class="sxs-lookup"><span data-stu-id="33fe2-349">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="33fe2-350">Przy użyciu tych definicji trasy `Url.Action("Index", "Home")` wygeneruje ścieżkę adresu URL `/` z `default` trasy, ale Dlaczego?</span><span class="sxs-lookup"><span data-stu-id="33fe2-350">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="33fe2-351">Może być odgadnąć wartości trasy `{ controller = Home, action = Index }` może być wystarczająca do generowania adresu URL za pomocą `blog`, i będą `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-351">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="33fe2-352">Dedykowany tras z konwencjonalnej polegać na specjalnego zachowania wartości domyślnych, które nie mają odpowiadającego mu parametru trasy, który uniemożliwia trasy "zbyt zachłannego" generacji adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-352">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="33fe2-353">W tym przypadku wartości domyślne są `{ controller = Blog, action = Article }`i ani `controller` ani `action` pojawia się jako parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-353">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="33fe2-354">Gdy routingu wykonuje generowania adresu URL, podanych wartości musi być zgodna z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="33fe2-354">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="33fe2-355">Przy użyciu generowania adresu URL `blog` zakończy się niepowodzeniem, ponieważ wartości `{ controller = Home, action = Index }` nie zgadzają się `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-355">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="33fe2-356">Routing następnie powraca do spróbuj `default`, który zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="33fe2-356">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="33fe2-357">Obszary</span><span class="sxs-lookup"><span data-stu-id="33fe2-357">Areas</span></span>

<span data-ttu-id="33fe2-358">[Obszary](areas.md) są funkcją MVC używane do organizowania funkcje w grupie jako osobne routingu — przestrzeń nazw (dla akcji kontrolera) i struktury folderów (dla widoków).</span><span class="sxs-lookup"><span data-stu-id="33fe2-358">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="33fe2-359">Za pomocą obszarów umożliwia aplikacji jest wiele kontrolerów o takiej samej nazwie — tak długo, jak długo mają różne *obszarów*.</span><span class="sxs-lookup"><span data-stu-id="33fe2-359">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="33fe2-360">Za pomocą obszarów tworzy hierarchię na potrzeby routingu, dodając inny parametru trasy, `area` do `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-360">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="33fe2-361">W tej sekcji będzie omawiać routingu współdziałania z obszarów — zobacz [obszarów](areas.md) szczegółowe informacje na temat używania obszarów z widokami.</span><span class="sxs-lookup"><span data-stu-id="33fe2-361">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="33fe2-362">Poniższy przykład konfiguruje MVC do użycia z konwencjonalnej trasy domyślnej i *trasy obszaru* obszaru o nazwie `Blog`:</span><span class="sxs-lookup"><span data-stu-id="33fe2-362">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="33fe2-363">Podczas dopasowywania Ścieżka adresu URL, takie jak `/Manage/Users/AddUser`, pierwszy trasy utworzy wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-363">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="33fe2-364">`area` Wartość trasy jest generowany przez wartość domyślną dla `area`, w rzeczywistości trasy utworzone przez `MapAreaRoute` jest odpowiednikiem następujące:</span><span class="sxs-lookup"><span data-stu-id="33fe2-364">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="33fe2-365">`MapAreaRoute`tworzy trasę przy użyciu wartości domyślnej i ograniczenie dla `area` przy użyciu nazwy podanego obszaru, w tym przypadku `Blog`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-365">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="33fe2-366">Wartość domyślna zapewnia trasy zawsze daje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-366">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="33fe2-367">Tradycyjnie routing jest zależny od kolejności.</span><span class="sxs-lookup"><span data-stu-id="33fe2-367">Conventional routing is order-dependent.</span></span> <span data-ttu-id="33fe2-368">Ogólnie rzecz biorąc trasy z obszarami powinna zostać umieszczona wcześniej w tabeli tras, ponieważ są one bardziej szczegółowy niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="33fe2-368">In general, routes with areas should be placed earlier in the route table as they are more specific than routes without an area.</span></span>

<span data-ttu-id="33fe2-369">W powyższym przykładzie wartości trasy spowoduje dopasowanie następującej akcji:</span><span class="sxs-lookup"><span data-stu-id="33fe2-369">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="33fe2-370">`AreaAttribute` Jest, co oznacza kontrolera jako część obszaru, możemy powiedzieć, że ten kontroler jest w `Blog` obszaru.</span><span class="sxs-lookup"><span data-stu-id="33fe2-370">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="33fe2-371">Kontrolery bez `[Area]` atrybut nie są członkami żadnego obszaru i będzie **nie** zgodne, kiedy `area` wartość trasy są dostarczane przez routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-371">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="33fe2-372">W poniższym przykładzie, tylko pierwszy kontroler na liście można dopasować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-372">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="33fe2-373">Przestrzeń nazw każdego kontrolera jest tu aby informacje były kompletne — w przeciwnym razie kontrolery byłyby nazewnictwa konflikt i wygenerowanie błędu kompilatora.</span><span class="sxs-lookup"><span data-stu-id="33fe2-373">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="33fe2-374">Przestrzenie nazw klasy nie mają wpływu na routingu MVC.</span><span class="sxs-lookup"><span data-stu-id="33fe2-374">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="33fe2-375">Pierwsze dwa kontrolery są członkami obszarów i dopasować tylko, gdy ich nazwy do odpowiedniego obszaru są dostarczane przez `area` wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-375">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="33fe2-376">Trzeci kontrolera nie jest elementem członkowskim obszaru, i może tylko dopasowania, gdy brak wartości `area` są dostarczane przez routingu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-376">The third controller is not a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="33fe2-377">Pod względem dopasowania *żadnej wartości*, braku `area` wartość jest taka sama tak, jakby wartość `area` zostały wartości null ani być pustym ciągiem.</span><span class="sxs-lookup"><span data-stu-id="33fe2-377">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="33fe2-378">Podczas wykonywania akcji w obszarze, wartości trasy `area` będą dostępne jako *otoczenia wartość* routingu do użycia podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-378">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="33fe2-379">Oznacza to, że domyślnie obszarów działania *trwałe* do generowania adresu URL, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="33fe2-379">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="33fe2-380">Opis IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="33fe2-380">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="33fe2-381">Ta sekcja jest szczegółowo wewnętrzne framework i jak MVC wybiera akcję w celu wykonania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-381">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="33fe2-382">Typowa aplikacja nie ma potrzeby niestandardowego`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="33fe2-382">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="33fe2-383">Prawdopodobnie zostały już wykorzystane `IActionConstraint` nawet, jeśli nie znasz z interfejsem.</span><span class="sxs-lookup"><span data-stu-id="33fe2-383">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="33fe2-384">`[HttpGet]` Atrybutu i podobne `[Http-VERB]` implementuje atrybuty `IActionConstraint` w celu ograniczenia wykonywania metody akcji.</span><span class="sxs-lookup"><span data-stu-id="33fe2-384">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="33fe2-385">Zakładając, że domyślną trasę konwencjonalnej ścieżkę adresu URL `/Products/Edit` dałby w efekcie wartości `{ controller = Products, action = Edit }`, który umożliwi dopasowanie **zarówno** akcji pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="33fe2-385">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="33fe2-386">W `IActionConstraint` terminologii czy mówi się, że oba te akcje są traktowane jako kandydatów — jak spełniają dane trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-386">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="33fe2-387">Gdy `HttpGetAttribute` wykonuje, jest wyświetlany tekst, który *Edit()* pasuje do *UZYSKAĆ* i nie ma dopasowania dla innych zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="33fe2-387">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and is not a match for any other HTTP verb.</span></span> <span data-ttu-id="33fe2-388">`Edit(...)` Akcji nie ma żadnych ograniczeń zdefiniowanych, a więc będzie odpowiadała żadnych zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="33fe2-388">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="33fe2-389">Dlatego przy założeniu `POST` — tylko `Edit(...)` zgodny.</span><span class="sxs-lookup"><span data-stu-id="33fe2-389">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="33fe2-390">Jednak dla `GET` zarówno akcje może nadal być zgodne z — jednak akcję z `IActionConstraint` zawsze jest uznawany za *lepsze* niż akcji bez.</span><span class="sxs-lookup"><span data-stu-id="33fe2-390">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="33fe2-391">Dlatego ponieważ `Edit()` ma `[HttpGet]` jest uznawany za bardziej szczegółowe i należy wybrać, jeśli obie akcje może dopasować.</span><span class="sxs-lookup"><span data-stu-id="33fe2-391">So because `Edit()` has `[HttpGet]` it is considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="33fe2-392">Koncepcyjnie `IActionConstraint` jest formą *przeładowanie*, ale zamiast przeładowanie metody o tej samej nazwie, jest przeładowanie między akcje, które spełniają tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33fe2-392">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it is overloading between actions that match the same URL.</span></span> <span data-ttu-id="33fe2-393">Atrybut routingu używa również `IActionConstraint` i może spowodować akcji z różnych kontrolerów obu rozważane kandydatów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-393">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="33fe2-394">Implementowanie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="33fe2-394">Implementing IActionConstraint</span></span>

<span data-ttu-id="33fe2-395">Najprostszym sposobem, aby zaimplementować `IActionConstraint` polega na utworzeniu klasy pochodzącej od `System.Attribute` i umieść go w tej akcji i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="33fe2-395">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="33fe2-396">MVC automatycznie wykryje żadnego `IActionConstraint` które są stosowane jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="33fe2-396">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="33fe2-397">Model aplikacji można użyć, aby zastosować ograniczenia, a jest to prawdopodobnie największą elastyczność, ponieważ umożliwia ona metaprogram sposób stosowania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-397">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they are applied.</span></span>

<span data-ttu-id="33fe2-398">W poniższym przykładzie ograniczenie wybiera akcję na podstawie *numer kierunkowy kraju* z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-398">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="33fe2-399">[Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="33fe2-399">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="33fe2-400">Jest odpowiedzialny za wdrażanie `Accept` — metoda i wybierając polecenie "Order" ograniczenia do wykonania.</span><span class="sxs-lookup"><span data-stu-id="33fe2-400">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="33fe2-401">W takim przypadku `Accept` metoda zwraca `true` do oznaczania akcji są zgodne po `country` dopasowania wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="33fe2-401">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="33fe2-402">Różni się to od `RouteValueAttribute` , ponieważ umożliwia rezerwowy bezatrybutowego akcją.</span><span class="sxs-lookup"><span data-stu-id="33fe2-402">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="33fe2-403">Przykład pokazuje, że jeśli zdefiniujesz `en-US` akcji, a następnie kod kraju, takich jak `fr-FR` powróci do kontrolera bardziej ogólnym, który nie ma `[CountrySpecific(...)]` zastosowane.</span><span class="sxs-lookup"><span data-stu-id="33fe2-403">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that does not have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="33fe2-404">`Order` Właściwość decyduje, które *etap* ograniczenie jest częścią.</span><span class="sxs-lookup"><span data-stu-id="33fe2-404">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="33fe2-405">Uruchom ograniczenia działań w grupach na podstawie `Order`.</span><span class="sxs-lookup"><span data-stu-id="33fe2-405">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="33fe2-406">Na przykład wszystkie platformy podane atrybuty metody HTTP używać tego samego `Order` tak, aby ich działania w ramach tego samego etapu.</span><span class="sxs-lookup"><span data-stu-id="33fe2-406">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="33fe2-407">Może mieć dowolną liczbę etapów należy wdrożyć odpowiednie zasady.</span><span class="sxs-lookup"><span data-stu-id="33fe2-407">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="33fe2-408">Podjęcie decyzji na wartość `Order` Pomyśl o czy Twoje ograniczenia powinny być stosowane przed metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="33fe2-408">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="33fe2-409">Niższych numerach uruchamiany najpierw.</span><span class="sxs-lookup"><span data-stu-id="33fe2-409">Lower numbers run first.</span></span>