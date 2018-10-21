---
title: Routing do akcji kontrolera, w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ASP.NET Core MVC używa routingu oprogramowania pośredniczącego do dopasowania adresów URL żądań przychodzących i mapowania ich działania.
ms.author: riande
ms.date: 09/17/2018
uid: mvc/controllers/routing
ms.openlocfilehash: 2f6328a5efaa96fd8e4f0cafdbde77dd63a1548f
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477647"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="17df5-103">Routing do akcji kontrolera, w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17df5-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="17df5-104">Przez [Ryan Nowak](https://github.com/rynowak) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17df5-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17df5-105">Platforma ASP.NET Core MVC używa routingu [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do dopasowania adresów URL żądań przychodzących i mapowania ich działania.</span><span class="sxs-lookup"><span data-stu-id="17df5-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="17df5-106">Trasy są definiowane w kod uruchamiający lub atrybutów.</span><span class="sxs-lookup"><span data-stu-id="17df5-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="17df5-107">Trasy opisano, jak ścieżek URL powinny być dopasowane do akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="17df5-108">Trasy są również używane do generowania adresów URL (dla linków) wysłane w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="17df5-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="17df5-109">Akcje albo są tradycyjnie kierowane lub atrybut kierowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="17df5-110">Wprowadzenie do trasy na kontrolerze lub akcji sprawia, że atrybut kierowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="17df5-111">Zobacz [mieszane routingu](#routing-mixed-ref-label) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="17df5-112">W tym dokumencie wyjaśniono, interakcji między MVC i routing i jak typowe upewnij aplikacji MVC korzystać z funkcji routingu.</span><span class="sxs-lookup"><span data-stu-id="17df5-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="17df5-113">Zobacz [Routing](xref:fundamentals/routing) szczegółowe informacje na temat zaawansowanego routingu.</span><span class="sxs-lookup"><span data-stu-id="17df5-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="17df5-114">Konfigurowanie routingu oprogramowania pośredniczącego</span><span class="sxs-lookup"><span data-stu-id="17df5-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="17df5-115">W swojej *Konfiguruj* metoda może zostać wyświetlony kod podobny do:</span><span class="sxs-lookup"><span data-stu-id="17df5-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="17df5-116">Wewnątrz wywołania `UseMvc`, `MapRoute` służy do tworzenia pojedynczej trasy, która będzie nazywamy `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="17df5-117">Większość aplikacji MVC użyje trasę z szablonem, podobnie jak `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="17df5-118">Szablon trasy `"{controller=Home}/{action=Index}/{id?}"` może odnosić się do ścieżki adresu URL, takich jak `/Products/Details/5` , który wyodrębnia wartości trasy `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki.</span><span class="sxs-lookup"><span data-stu-id="17df5-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="17df5-119">MVC będzie podejmować próby zlokalizowania kontrolera, o nazwie `ProductsController` i uruchomić akcję `Details`:</span><span class="sxs-lookup"><span data-stu-id="17df5-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="17df5-120">Należy pamiętać, że w tym przykładzie wiązania modelu użyj wartości `id = 5` można ustawić `id` parametr `5` podczas wywoływania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="17df5-121">Zobacz [powiązań modelu](../models/model-binding.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="17df5-122">Za pomocą `default` trasy:</span><span class="sxs-lookup"><span data-stu-id="17df5-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="17df5-123">Szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="17df5-123">The route template:</span></span>

* <span data-ttu-id="17df5-124">`{controller=Home}` definiuje `Home` jako domyślny `controller`</span><span class="sxs-lookup"><span data-stu-id="17df5-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="17df5-125">`{action=Index}` definiuje `Index` jako domyślny `action`</span><span class="sxs-lookup"><span data-stu-id="17df5-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="17df5-126">`{id?}` definiuje `id` jako opcjonalny</span><span class="sxs-lookup"><span data-stu-id="17df5-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="17df5-127">Domyślnie i parametry opcjonalne trasy nie muszą znajdować się w ścieżce adresu URL, pod kątem dopasowania.</span><span class="sxs-lookup"><span data-stu-id="17df5-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="17df5-128">Zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="17df5-129">`"{controller=Home}/{action=Index}/{id?}"` może odnosić się do ścieżki adresu URL `/` i będzie generować wartości trasy `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="17df5-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="17df5-130">Wartości `controller` i `action` należy użyć wartości domyślnych `id` nie zwraca wartości, ponieważ nie odpowiadającym segmencie ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="17df5-131">MVC użyje tych wartości trasy, aby wybrać `HomeController` i `Index` akcji:</span><span class="sxs-lookup"><span data-stu-id="17df5-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="17df5-132">Przy użyciu tego kontrolera, definicji i szablon trasy `HomeController.Index` będzie można wykonać akcji dla żadnego z następujących ścieżek URL:</span><span class="sxs-lookup"><span data-stu-id="17df5-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="17df5-133">Wygodna metoda `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="17df5-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="17df5-134">Może służyć do zastąpienia:</span><span class="sxs-lookup"><span data-stu-id="17df5-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="17df5-135">`UseMvc` i `UseMvcWithDefaultRoute` dodaje wystąpienie `RouterMiddleware` do potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="17df5-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="17df5-136">MVC nie wchodzi w interakcje bezpośrednio za pomocą oprogramowania pośredniczącego i używa routingu w celu obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="17df5-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="17df5-137">MVC jest podłączony do tras przy użyciu wystąpienia `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="17df5-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="17df5-138">Kod wewnątrz `UseMvc` jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="17df5-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="17df5-139">`UseMvc` bezpośrednio nie definiuje żadnych tras, dodaje do kolekcji tras dla symbolu zastępczego `attribute` trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="17df5-140">Przeciążenie `UseMvc(Action<IRouteBuilder>)` pozwala dodać własne trasy i obsługuje również trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="17df5-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="17df5-141">`UseMvc` i wszystkie jego odmiany dodaje symbol zastępczy dla tras atrybutów — trasowanie atrybutów jest zawsze dostępna niezależnie od tego, jak skonfigurować `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="17df5-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="17df5-142">`UseMvcWithDefaultRoute` Definiuje trasę domyślną i obsługuje routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="17df5-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="17df5-143">[Trasowanie atrybutów](#attribute-routing-ref-label) sekcja zawiera szczegółowe informacje na temat trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="17df5-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="17df5-144">Tradycyjnie routing</span><span class="sxs-lookup"><span data-stu-id="17df5-144">Conventional routing</span></span>

<span data-ttu-id="17df5-145">`default` Trasy:</span><span class="sxs-lookup"><span data-stu-id="17df5-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="17df5-146">Oto przykład *tradycyjnie routing*.</span><span class="sxs-lookup"><span data-stu-id="17df5-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="17df5-147">Nazywamy tego stylu *tradycyjnie routing* ponieważ definiuje on *Konwencji* dla ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="17df5-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="17df5-148">pierwszy segment ścieżki mapowana na nazwę kontrolera</span><span class="sxs-lookup"><span data-stu-id="17df5-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="17df5-149">drugi mapuje nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-149">the second maps to the action name.</span></span>

* <span data-ttu-id="17df5-150">trzeci segmentu jest używana do opcjonalny `id` służy do mapowania jednostki modelu</span><span class="sxs-lookup"><span data-stu-id="17df5-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="17df5-151">Za pomocą tego `default` trasy, URL path `/Products/List` mapuje `ProductsController.List` akcji i `/Blog/Article/17` mapuje `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="17df5-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="17df5-152">To mapowanie jest oparte na nazwy kontrolera i akcji **tylko** i nie jest oparty na przestrzeni nazw, lokalizacji źródłowych plików lub parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="17df5-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="17df5-153">Za pomocą konwencjonalnych routing za pomocą trasy domyślnej pozwala na szybkie tworzenie aplikacji bez konieczności stworzyć nowy wzorzec adresu URL dla każdej akcji, jaką zdefiniujesz.</span><span class="sxs-lookup"><span data-stu-id="17df5-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="17df5-154">Dla aplikacji z akcjami stylu CRUD mające spójności dla adresów URL w kontrolerach pomaga uprościć kod i utworzyć bardziej przewidywalna interfejs użytkownika.</span><span class="sxs-lookup"><span data-stu-id="17df5-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="17df5-155">`id` Jest definiowany jako opcjonalną przez szablon trasy, co oznacza, że akcje można wykonać bez Identyfikatora podana jako część adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="17df5-156">Zazwyczaj co stanie się po `id` zostanie pominięty z adresu URL jest, że zostanie ustawiony na `0` przez powiązanie modelu, a w rezultacie nie jednostki zostanie znaleziony w dopasowywanie bazy danych `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="17df5-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="17df5-157">Trasowanie atrybutów daje precyzyjną kontrolę, można wprowadzić identyfikator wymagane w przypadku niektórych działań, a nie dla innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="17df5-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="17df5-158">Zgodnie z Konwencją dokumentacja będzie zawierać następujące parametry opcjonalne, takie jak `id` po prawdopodobnie będą się pojawiać w poprawne użycie.</span><span class="sxs-lookup"><span data-stu-id="17df5-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="17df5-159">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="17df5-159">Multiple routes</span></span>

<span data-ttu-id="17df5-160">Można dodać wiele tras wewnątrz `UseMvc` , dodając więcej wywołań `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="17df5-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="17df5-161">Dzięki temu można zdefiniować wiele konwencje lub można dodać konwencjonalne tras, które są przeznaczone dla określonej akcji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="17df5-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="17df5-162">`blog` Tras w tym miejscu jest *dedykowanych trasy konwencjonalne*, co oznacza, że korzysta z konwencjonalnych systemu routingu, ale jest dedykowany do określonej akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="17df5-163">Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i związku z tym ta trasa zawsze będzie zmapowana do akcji `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="17df5-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="17df5-164">Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, w której są dodawane.</span><span class="sxs-lookup"><span data-stu-id="17df5-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="17df5-165">Tak, w tym przykładzie `blog` trasy zostaną sprawdzone przed `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="17df5-166">*Trasy są konwencjonalne funkcje w wersji dedykowanej* często używają parametrów wychwytywania trasy, takich jak `{*article}` do przechwytywania pozostała część ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="17df5-167">Dzięki temu może być trasę zbyt intensywnie co oznacza, czy jest on zgodny adresów URL, które mają zostać dopasowane przez innych tras.</span><span class="sxs-lookup"><span data-stu-id="17df5-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="17df5-168">W dalszej części tabeli tras, aby rozwiązać ten problem, należy umieścić trasy "zachłanne".</span><span class="sxs-lookup"><span data-stu-id="17df5-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="17df5-169">Rezerwowe</span><span class="sxs-lookup"><span data-stu-id="17df5-169">Fallback</span></span>

<span data-ttu-id="17df5-170">W ramach procesu przetwarzania żądania MVC sprawdzi, czy wartości trasy można znaleźć kontrolerów i akcji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="17df5-171">Jeśli wartości trasy nie są zgodne akcję trasy nie zostanie on uznany za zgodny i dalej trasy zostaną sprawdzone.</span><span class="sxs-lookup"><span data-stu-id="17df5-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="17df5-172">Jest to nazywane *rezerwowego*, a ma ona przeznaczona do uproszczenia przypadków nakładają się konwencjonalne trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="17df5-173">Usuwanie niejednoznaczności dotyczących działań</span><span class="sxs-lookup"><span data-stu-id="17df5-173">Disambiguating actions</span></span>

<span data-ttu-id="17df5-174">Gdy dwie akcje odpowiada za pomocą routingu, MVC muszą rozróżniać do wybierz opcję "Najważniejsze" Release candidate lub — w przeciwnym razie Zgłoś wyjątek.</span><span class="sxs-lookup"><span data-stu-id="17df5-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="17df5-175">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="17df5-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="17df5-176">Ten kontroler definiuje dwie akcje, zgodne ze ścieżką URL `/Products/Edit/17` i kierować dane `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="17df5-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="17df5-177">Jest to typowy wzorzec kontrolerów MVC gdzie `Edit(int)` przedstawia formularz produktu, edytować i `Edit(int, Product)` przetwarza przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="17df5-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="17df5-178">Aby to umożliwić MVC będzie trzeba będzie wybrać `Edit(int, Product)` po żądaniu HTTP `POST` i `Edit(int)` po czasownik HTTP jest inny.</span><span class="sxs-lookup"><span data-stu-id="17df5-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="17df5-179">`HttpPostAttribute` ( `[HttpPost]` ) To implementacja `IActionConstraint` umożliwiające tylko akcję, należy wybrać, jeśli polecenie HTTP jest żądaniem `POST`.</span><span class="sxs-lookup"><span data-stu-id="17df5-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="17df5-180">Obecność `IActionConstraint` sprawia, że `Edit(int, Product)` lepiej pasuje niż `Edit(int)`, więc `Edit(int, Product)` zostaną sprawdzone najpierw.</span><span class="sxs-lookup"><span data-stu-id="17df5-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="17df5-181">Wystarczy napisać niestandardowy `IActionConstraint` implementacji w specjalistycznych scenariuszy, ale jego zrozumieć rolę atrybutów, takich jak `HttpPostAttribute` -podobne atrybuty są zdefiniowane dla innych poleceń HTTP.</span><span class="sxs-lookup"><span data-stu-id="17df5-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="17df5-182">W routingu konwencjonalne jest typowe dla działania, aby użyć tej samej nazwy akcji, gdy są one częścią `show form -> submit form` przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="17df5-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="17df5-183">Jako udogodnienie tego wzorca staną się bardziej widoczne po zapoznaniu się z [IActionConstraint opis](#understanding-iactionconstraint) sekcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="17df5-184">Jeśli wiele tras zgodnych z MVC nie można odnaleźć trasy "Najważniejsze", spowoduje zgłoszenie `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="17df5-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="17df5-185">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="17df5-185">Route names</span></span>

<span data-ttu-id="17df5-186">Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwy tras:</span><span class="sxs-lookup"><span data-stu-id="17df5-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="17df5-187">Nazwy tras nazwij trasy logiczną tak, aby nazwanej trasy może służyć do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="17df5-188">Jeśli kolejność trasy może spowodować, że Generowanie adresu URL skomplikowane znacznie upraszcza tworzenie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="17df5-189">Nazwy tras muszą być unikatowe całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="17df5-190">Nazwy tras nie mają wpływu na adres URL pasujących lub obsługi żądań; służą one wyłącznie do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="17df5-191">[Routing](xref:fundamentals/routing) zawiera bardziej szczegółowe informacje dotyczące Generowanie adresu URL, w tym Generowanie adresu URL w pomocników specyficzne dla platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="17df5-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="17df5-192">Routing atrybutów</span><span class="sxs-lookup"><span data-stu-id="17df5-192">Attribute routing</span></span>

<span data-ttu-id="17df5-193">Routing atrybutu używa zestaw atrybutów do mapowania akcji bezpośrednio do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="17df5-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="17df5-194">W poniższym przykładzie `app.UseMvc();` jest używany w `Configure` metody i żadna trasa jest przekazywany.</span><span class="sxs-lookup"><span data-stu-id="17df5-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="17df5-195">`HomeController` Pokaże zestaw adresów URL, podobnie jak trasa domyślna `{controller=Home}/{action=Index}/{id?}` umożliwi dopasowanie:</span><span class="sxs-lookup"><span data-stu-id="17df5-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="17df5-196">`HomeController.Index()` Będzie można wykonać akcji dla wszystkich ścieżek URL `/`, `/Home`, lub `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="17df5-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="17df5-197">W tym przykładzie podkreślono programowania Najważniejszą różnicą między atrybut i konwencjonalne routingu.</span><span class="sxs-lookup"><span data-stu-id="17df5-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="17df5-198">Routing atrybutów wymaga więcej dane wejściowe do określania tras; trasa domyślna konwencjonalne obsługuje tras bardziej zwięzły.</span><span class="sxs-lookup"><span data-stu-id="17df5-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="17df5-199">Jednak trasowanie atrybutów umożliwia (i wymaga) precyzyjną kontrolę, które szablonów tras dotyczą każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="17df5-200">Z routingiem, nazwy kontrolera i akcji, nazwy atrybutów odtwarzania **nie** roli wybrana akcja.</span><span class="sxs-lookup"><span data-stu-id="17df5-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="17df5-201">W tym przykładzie będzie odpowiadał tych samych adresów URL, jak w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="17df5-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="17df5-202">Powyższe szablonów tras nie Definiuj parametry trasy `action`, `area`, i `controller`.</span><span class="sxs-lookup"><span data-stu-id="17df5-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="17df5-203">W rzeczywistości te parametry trasy nie są dozwolone w tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="17df5-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="17df5-204">Ponieważ szablon trasy jest już skojarzony z akcją, go nie miałoby sensu można przeanalizować nazwy akcji z adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="17df5-205">Atrybut, routing za pomocą atrybutów Http [polecenie]</span><span class="sxs-lookup"><span data-stu-id="17df5-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="17df5-206">Routing atrybut może być użycie `Http[Verb]` atrybuty takie jak `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="17df5-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="17df5-207">Wszystkie te atrybuty mogą akceptować szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="17df5-208">W tym przykładzie przedstawiono dwie akcje, które pasują do tego samego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="17df5-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="17df5-209">Dla ścieżki adresu URL, takich jak `/products` `ProductsApi.ListProducts` będzie można wykonać akcji, jeśli polecenie HTTP jest żądaniem `GET` i `ProductsApi.CreateProduct` zostaną wykonane, jeśli polecenie HTTP jest żądaniem `POST`.</span><span class="sxs-lookup"><span data-stu-id="17df5-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="17df5-210">Najpierw trasowanie atrybutów zgodny z adresem URL, względem zestaw szablonów trasy zdefiniowane przez atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="17df5-211">Gdy szablon trasy jest zgodny, `IActionConstraint` ograniczenia są stosowane w celu określenia, akcje, które mogą być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="17df5-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="17df5-212">Podczas tworzenia interfejsu API REST, jest rzadkie, że można użyć `[Route(...)]` na metody akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="17df5-213">Zaleca się używać więcej określonych `Http*Verb*Attributes` do dokładnego interfejs API obsługuje.</span><span class="sxs-lookup"><span data-stu-id="17df5-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="17df5-214">Klienci interfejsów API REST, powinni wiedzieć, ścieżki i zleceń HTTP mapowane na operacje logiczne określone.</span><span class="sxs-lookup"><span data-stu-id="17df5-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="17df5-215">Ponieważ trasa atrybutu ma zastosowanie do określonej akcji, jest można łatwo stworzyć parametrów wymaganych w ramach definicji szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="17df5-216">W tym przykładzie `id` jest wymagany ze ścieżką URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="17df5-217">`ProductsApi.GetProduct(int)` Będzie można wykonać akcji dla ścieżki adresu URL, takich jak `/products/3` , ale nie ścieżka adresu URL, takich jak `/products`.</span><span class="sxs-lookup"><span data-stu-id="17df5-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="17df5-218">Zobacz [Routing](../../fundamentals/routing.md) pełny opis szablonów tras i powiązanych opcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="17df5-219">Nazwa trasy</span><span class="sxs-lookup"><span data-stu-id="17df5-219">Route Name</span></span>

<span data-ttu-id="17df5-220">Poniższy kod definiuje *nazwy trasy* z `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="17df5-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="17df5-221">Nazwy tras może służyć do generowania adresu URL na podstawie określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="17df5-222">Nazwy tras nie mają wpływu na adres URL pasujące do zachowania routingu i są używane tylko na potrzeby generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="17df5-223">Nazwy tras muszą być unikatowe całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="17df5-224">Natomiast to za pomocą konwencjonalnych *trasy domyślnej*, która definiuje `id` jako opcjonalny parametr (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="17df5-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="17df5-225">Tę możliwość, aby precyzyjnie określić interfejsy API ma zalety, na przykład pozwala `/products` i `/products/5` zostać przekazana do różnych działań.</span><span class="sxs-lookup"><span data-stu-id="17df5-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="17df5-226">Łączenie trasy</span><span class="sxs-lookup"><span data-stu-id="17df5-226">Combining routes</span></span>

<span data-ttu-id="17df5-227">Aby trasowanie atrybutów mniej powtarzalne, atrybuty trasy na kontrolerze są połączone za pomocą atrybutów trasy na poszczególne akcje.</span><span class="sxs-lookup"><span data-stu-id="17df5-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="17df5-228">Wszystkie szablony trasy zdefiniowane na kontrolerze jest dołączony do szablonów tras na akcje.</span><span class="sxs-lookup"><span data-stu-id="17df5-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="17df5-229">Wprowadzenie do atrybutów trasy na kontrolerze sprawia, że **wszystkich** akcji w kontrolerze Użyj trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="17df5-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="17df5-230">W tym przykładzie ze ścieżką URL `/products` może odnosić się do `ProductsApi.ListProducts`i Ścieżka adresu URL `/products/5` może odnosić się do `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="17df5-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="17df5-231">Oba te akcje dopasowanie tylko HTTP `GET` ponieważ są one `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="17df5-231">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="17df5-232">Kierowanie zastosowanych akcji, które zaczynają się od szablonów `/` nie uzyskać w połączeniu z szablonów tras stosowane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="17df5-232">Route templates applied to an action that begin with a `/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="17df5-233">W tym przykładzie dopasowuje zestaw ścieżek URL podobny do *trasy domyślnej*.</span><span class="sxs-lookup"><span data-stu-id="17df5-233">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="17df5-234">Kolejność trasy atrybutu</span><span class="sxs-lookup"><span data-stu-id="17df5-234">Ordering attribute routes</span></span>

<span data-ttu-id="17df5-235">W przeciwieństwie do konwencjonalnych tras, które są wykonywane w kolejności zdefiniowanej trasowanie atrybutów tworzy drzewa, a jednocześnie dopasowuje wszystkie trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="17df5-236">To zachowuje się jak — Jeśli wejścia dla trasy zostały umieszczone w idealnym kolejność; bardziej konkretny od pozostałych tras ma możliwość wykonania przed tras ogólniejszych.</span><span class="sxs-lookup"><span data-stu-id="17df5-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="17df5-237">Na przykład, takich jak trasy `blog/search/{topic}` jest bardziej szczegółowe niż trasy, takich jak `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="17df5-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="17df5-238">Logicznie wypowiedzi `blog/search/{topic}` trasy "działa" najpierw domyślnie, ponieważ tylko rozsądne kolejności.</span><span class="sxs-lookup"><span data-stu-id="17df5-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="17df5-239">Za pomocą konwencjonalnych routingu, deweloper jest odpowiedzialny za wprowadzanie do tras w odpowiedni sposób.</span><span class="sxs-lookup"><span data-stu-id="17df5-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="17df5-240">Tras atrybutów można skonfigurować kolejność przy użyciu `Order` właściwości wszystkich atrybutów trasy struktura dostępna.</span><span class="sxs-lookup"><span data-stu-id="17df5-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="17df5-241">Trasy są przetwarzane zgodnie z rosnącym swoistego `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="17df5-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="17df5-242">Domyślna kolejność to `0`.</span><span class="sxs-lookup"><span data-stu-id="17df5-242">The default order is `0`.</span></span> <span data-ttu-id="17df5-243">Konfigurowanie tras za pomocą `Order = -1` będzie wykonywany przed tras, na których nie należy ustawiać zamówienia.</span><span class="sxs-lookup"><span data-stu-id="17df5-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="17df5-244">Konfigurowanie tras za pomocą `Order = 1` zostanie uruchomiony po określeniu kolejności trasy domyślne.</span><span class="sxs-lookup"><span data-stu-id="17df5-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="17df5-245">Należy unikać w zależności od `Order`.</span><span class="sxs-lookup"><span data-stu-id="17df5-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="17df5-246">Jeśli przestrzeni URL wymaga jawnego kolejności wartości przekierowywany poprawnie, będzie ona wówczas prawdopodobnie mylące dla klientów, jak również.</span><span class="sxs-lookup"><span data-stu-id="17df5-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="17df5-247">Ogólnie rzecz biorąc trasowanie atrybutów wybierze poprawnej trasy z pasującymi adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="17df5-248">Jeśli domyślna kolejność użyto do generowania adresu URL nie działa, przy użyciu nazwy trasy zastąpienia jest zwykle łatwiejsze niż stosowanie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="17df5-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="17df5-249">Strony razor routingu i MVC kontroler routingu udziału wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="17df5-249">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="17df5-250">Informacje o kolejność trasy w tematach stron Razor znajduje się w temacie [konwencje tras i aplikacji dla stron Razor: kolejność trasy](xref:razor-pages/razor-pages-conventions#route-order).</span><span class="sxs-lookup"><span data-stu-id="17df5-250">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="17df5-251">Token zastępczy w szablonach tras ([controller] [action] [obszaru])</span><span class="sxs-lookup"><span data-stu-id="17df5-251">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="17df5-252">Dla wygody, obsługi tras atrybutów *token zastępczy* , umieszczając token w nawiasach kwadratowych (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="17df5-252">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="17df5-253">Tokeny `[action]`, `[area]`, i `[controller]` są zastępowane wartościami nazwy akcji, nazwy obszaru i nazwy kontrolera, w ramach akcji, której trasa jest zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="17df5-253">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="17df5-254">W poniższym przykładzie akcje odnosić się do ścieżki adresu URL zgodnie z opisem w komentarzach:</span><span class="sxs-lookup"><span data-stu-id="17df5-254">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="17df5-255">Zastępowania tokenu jest wykonywane jako ostatni krok w procesie tworzenia tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="17df5-255">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="17df5-256">Powyższy przykład będą zachowywać się taka sama jak poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="17df5-256">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="17df5-257">Tras atrybutów można również łączyć z dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="17df5-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="17df5-258">Jest to szczególnie wydajna, w połączeniu z zastępowania tokenu.</span><span class="sxs-lookup"><span data-stu-id="17df5-258">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="17df5-259">Zastępowania tokenu dotyczy także nazwy tras zdefiniowanych przez atrybut trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="17df5-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generuje trasy unikatową nazwę dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="17df5-261">Aby dopasować ogranicznik literału zastępowania tokenu `[` lub `]`, zmienić jego znaczenie, powtarzając znak (`[[` lub `]]`).</span><span class="sxs-lookup"><span data-stu-id="17df5-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="17df5-262">Użyj transformatora parametru, aby dostosować zastępowania tokenu</span><span class="sxs-lookup"><span data-stu-id="17df5-262">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="17df5-263">Używanie transformatora parametru można dostosować zastępowania tokenu.</span><span class="sxs-lookup"><span data-stu-id="17df5-263">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="17df5-264">Transformer parametr implementuje `IOutboundParameterTransformer` i przekształca wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="17df5-264">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="17df5-265">Na przykład niestandardowy `SlugifyParameterTransformer` zmiany transformatora parametrów `SubscriptionManagement` trasy wartość `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="17df5-265">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="17df5-266">`RouteTokenTransformerConvention` Jest Konwencja modelu aplikacji który:</span><span class="sxs-lookup"><span data-stu-id="17df5-266">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="17df5-267">Transformer parametru dotyczy wszystkich tras atrybutów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-267">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="17df5-268">Dostosowuje wartości tokenu trasy atrybutu, ponieważ zostaną zastąpione.</span><span class="sxs-lookup"><span data-stu-id="17df5-268">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="17df5-269">`RouteTokenTransformerConvention` Jest zarejestrowany jako opcja w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="17df5-269">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

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

### <a name="multiple-routes"></a><span data-ttu-id="17df5-270">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="17df5-270">Multiple Routes</span></span>

<span data-ttu-id="17df5-271">Atrybut routingu obsługuje definiowanie wiele tras, docierających do tego samego działania.</span><span class="sxs-lookup"><span data-stu-id="17df5-271">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="17df5-272">Najbardziej typowe użycia tego jest naśladują zachowanie *konwencjonalne trasy domyślnej* jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="17df5-272">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="17df5-273">Umieszczenie różnych atrybutów trasy na kontrolerze oznacza, że każdy z nich zostaną połączone z każdym z atrybutów trasy na metody akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-273">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="17df5-274">Gdy wiele tras atrybutów (które implementują `IActionConstraint`) są umieszczane w akcji, a następnie każde ograniczenie akcji łączy się z szablonem trasy z atrybutu, który on zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-274">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="17df5-275">Podczas przy użyciu wielu tras w akcji może wydawać się zaawansowanych, lepiej jest proste i dobrze zdefiniowanych, zapewnienie przestrzeni adres URL Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-275">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="17df5-276">Użyj wielu tras w akcji, tylko wtedy, gdy jest to wymagane, na przykład do obsługi istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="17df5-276">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="17df5-277">Określanie parametrów opcjonalnych tras atrybutów, wartości domyślne i ograniczenia</span><span class="sxs-lookup"><span data-stu-id="17df5-277">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="17df5-278">Tras atrybutów obsługuje tej samej składni wbudowanego jako konwencjonalne trasy, aby określić następujące parametry opcjonalne, wartości domyślne i ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="17df5-278">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="17df5-279">Zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-279">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="17df5-280">Atrybuty trasy niestandardowej za pomocą `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="17df5-280">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="17df5-281">Wszystkie atrybuty trasy udostępniane w ramach ( `[Route(...)]`, `[HttpGet(...)]` , itp.) implementuje `IRouteTemplateProvider` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="17df5-281">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="17df5-282">MVC wyszukuje atrybutów dotyczących kontrolera klas i metod akcji po uruchomieniu aplikacji korzysta z tymi, które implementują `IRouteTemplateProvider` tworzenie początkowej zbiór tras.</span><span class="sxs-lookup"><span data-stu-id="17df5-282">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="17df5-283">Możesz zaimplementować `IRouteTemplateProvider` do definiowania atrybutów trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-283">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="17df5-284">Każdy `IRouteTemplateProvider` można zdefiniować jedną trasę z szablonem trasy niestandardowej zamówienia, a nazwą:</span><span class="sxs-lookup"><span data-stu-id="17df5-284">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="17df5-285">Ten atrybut z powyższego przykładu automatycznie ustawia `Template` do `"api/[controller]"` podczas `[MyApiController]` jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-285">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="17df5-286">Dostosowywanie tras atrybutów za pomocą modelu aplikacji</span><span class="sxs-lookup"><span data-stu-id="17df5-286">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="17df5-287">*Model aplikacji* to model obiektów utworzonych podczas uruchamiania za pomocą wszystkich metadanych używane przez MVC do kierowania i wykonywania akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-287">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="17df5-288">*Model aplikacji* obejmuje wszystkie dane zebrane z tras atrybutów (za pośrednictwem `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="17df5-288">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="17df5-289">Można napisać *konwencje* do modyfikowania modelu aplikacji w chwili uruchomienia, aby dostosować sposób działania routingu.</span><span class="sxs-lookup"><span data-stu-id="17df5-289">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="17df5-290">W tej sekcji przedstawiono prosty przykład Dostosowywanie routingu za pomocą modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-290">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="17df5-291">Mieszane routingu: atrybut vs konwencjonalne routingu</span><span class="sxs-lookup"><span data-stu-id="17df5-291">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="17df5-292">Aplikacji MVC można łączyć użytkowania konwencjonalne i atrybut routingu.</span><span class="sxs-lookup"><span data-stu-id="17df5-292">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="17df5-293">Jest to zazwyczaj za pomocą konwencjonalnych trasy dla kontrolerów obsługujących stron HTML do przeglądarek, a atrybut routingu dla kontrolerów obsługujących interfejsów API REST.</span><span class="sxs-lookup"><span data-stu-id="17df5-293">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="17df5-294">Akcje albo są tradycyjnie kierowane lub atrybut kierowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-294">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="17df5-295">Wprowadzenie do trasy na kontrolerze lub akcji sprawia, że atrybut kierowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-295">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="17df5-296">Akcje, które definiują tras atrybutów nie można osiągnąć za pomocą konwencjonalnych trasy i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="17df5-296">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="17df5-297">**Wszelkie** atrybut trasy na kontrolerze sprawia, że wszystkie akcje w atrybucie kontrolera kierowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-297">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="17df5-298">Czym wyróżnia dwa rodzaje systemów routingu polega na stosowane po adres URL jest zgodny szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-298">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="17df5-299">W routingu konwencjonalne wartości tras z dopasowania są używane do wyboru akcji i kontrolera tabeli odnośników wszystkich akcji trasowane konwencjonalnych.</span><span class="sxs-lookup"><span data-stu-id="17df5-299">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="17df5-300">W trasowanie atrybutów, każdy szablon jest już skojarzony z akcją, i jest wymagane żadne dalsze wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="17df5-300">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="17df5-301">Złożone segmentów</span><span class="sxs-lookup"><span data-stu-id="17df5-301">Complex segments</span></span>

<span data-ttu-id="17df5-302">Złożone segmenty (na przykład `[Route("/dog{token}cat")]`), są przetwarzane przez dopasowanie się literały od prawej do lewej w sposób niezachłanne.</span><span class="sxs-lookup"><span data-stu-id="17df5-302">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="17df5-303">Zobacz [kod źródłowy](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) opis.</span><span class="sxs-lookup"><span data-stu-id="17df5-303">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="17df5-304">Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/aspnet/Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="17df5-304">For more information, see [this issue](https://github.com/aspnet/Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="17df5-305">Generowanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="17df5-305">URL Generation</span></span>

<span data-ttu-id="17df5-306">Aplikacji MVC można użyć routing firmy funkcjom generowania adresu URL do generowania łączy z adresami URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-306">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="17df5-307">Generowania adresów URL, który eliminuje hardcoding adresów URL, dzięki czemu kod, bardziej niezawodnego i łatwego w utrzymaniu.</span><span class="sxs-lookup"><span data-stu-id="17df5-307">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="17df5-308">Ta sekcja koncentruje się na funkcjom generowania adresu URL, które są dostarczane przez MVC i obejmuje tylko podstawowe informacje dotyczące sposobu działania Generowanie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-308">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="17df5-309">Zobacz [Routing](../../fundamentals/routing.md) szczegółowy opis Generowanie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-309">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="17df5-310">`IUrlHelper` Interfejs jest podstawowy element infrastrukturę między MVC i routing do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-310">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="17df5-311">Można znaleźć wystąpienia `IUrlHelper` dostępne za pośrednictwem `Url` właściwość w kontrolery, widoki i składniki widoków.</span><span class="sxs-lookup"><span data-stu-id="17df5-311">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="17df5-312">W tym przykładzie `IUrlHelper` interfejs jest używany przez `Controller.Url` właściwości do generowania adresu URL, do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-312">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="17df5-313">Jeśli aplikacja używa konwencjonalne domyślne trasy, wartości `url` zmienna będzie ciąg ścieżki adresu URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="17df5-313">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="17df5-314">Ta ścieżka adresu URL jest tworzony przez routingu, łącząc wartości tras z bieżącego żądania (otoczenia wartości), przy użyciu wartości przekazanych do `Url.Action` i podstawiając te wartości do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="17df5-314">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="17df5-315">Każdy parametr trasy w szablonie trasy ma wartość zastąpione pasujące nazwy wartości i wartości otoczenia.</span><span class="sxs-lookup"><span data-stu-id="17df5-315">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="17df5-316">Parametru trasy, który nie ma wartości można użyć wartości domyślnej, jeśli ma jedną lub zostać pominięta, jeżeli jest to opcjonalne (podobnie jak w przypadku właściwości `id` w tym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="17df5-316">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="17df5-317">Generowanie adresu URL zakończy się niepowodzeniem, jeśli parametr wszelkie wymagane trasy nie ma odpowiedniej wartości.</span><span class="sxs-lookup"><span data-stu-id="17df5-317">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="17df5-318">Generowanie adresu URL zakończy się niepowodzeniem dla danej trasy, trasa dalej zostanie podjęta próba dopóki wykonano wszystkie trasy lub nie zostanie znalezione dopasowanie.</span><span class="sxs-lookup"><span data-stu-id="17df5-318">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="17df5-319">Przykład `Url.Action` powyżej zakłada konwencjonalne routingu, ale adres URL generowania działa podobnie trasowanie atrybutów, chociaż przedstawione koncepcje mają różne.</span><span class="sxs-lookup"><span data-stu-id="17df5-319">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="17df5-320">Za pomocą konwencjonalnych routingu, wartości trasy są używane do szablonu i wartości trasy dla `controller` i `action` zwykle są wyświetlane w tym szablonie — to działa, ponieważ stosować pasujący do routingu adresów URL *Konwencji*.</span><span class="sxs-lookup"><span data-stu-id="17df5-320">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="17df5-321">W atrybucie routingu, wartości trasy `controller` i `action` nie nie mogą znajdować się w szablonie — zamiast tego służą one do wyszukania w wyborze szablonu.</span><span class="sxs-lookup"><span data-stu-id="17df5-321">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="17df5-322">W tym przykładzie użyto trasowanie atrybutów:</span><span class="sxs-lookup"><span data-stu-id="17df5-322">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="17df5-323">MVC kompiluje tabelę odnośników wszystkich akcji atrybutu kierowane i pokaże `controller` i `action` wartości, aby wybrać szablon trasy do użycia na potrzeby generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-323">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="17df5-324">W powyższym przykładzie `custom/url/to/destination` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="17df5-324">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="17df5-325">Generowania adresów URL, nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="17df5-325">Generating URLs by action name</span></span>

<span data-ttu-id="17df5-326">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="17df5-326">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="17df5-327">`Action`) i wszystkie powiązane przeciążenia, które wszystkie są oparte na pomysł, że chcesz określić, co prowadzi Link przez określenie nazwy kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-327">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="17df5-328">Korzystając z `Url.Action`, wartości bieżącej trasy `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` należą do obu *otoczenia wartości* **i** *wartości*.</span><span class="sxs-lookup"><span data-stu-id="17df5-328">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="17df5-329">Metoda `Url.Action`, zawsze używa bieżącej wartości `action` i `controller` i wygeneruje Ścieżka adresu URL, który kieruje do bieżącej akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-329">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="17df5-330">Routing prób na potrzeby wartości w wartościach otoczenia Wypełnij informacje, które nie zostały podane podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-330">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="17df5-331">Przy użyciu trasy, takich jak `{a}/{b}/{c}/{d}` wartości otoczenia i `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma za mało informacji do generowania adresu URL bez żadnych dodatkowych wartości — ponieważ kierować wszystkie parametry mają wartość.</span><span class="sxs-lookup"><span data-stu-id="17df5-331">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="17df5-332">Jeśli dodano wartość `{ d = Donovan }`, wartość `{ d = David }` będą ignorowane, a będzie wygenerowaną ścieżkę adresu URL `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="17df5-332">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="17df5-333">Adres URL ścieżki są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="17df5-333">URL paths are hierarchical.</span></span> <span data-ttu-id="17df5-334">W przykładzie przedstawionym powyżej należy usprawniło `{ c = Cheryl }`, obie wartości `{ c = Carol, d = David }` będą ignorowane.</span><span class="sxs-lookup"><span data-stu-id="17df5-334">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="17df5-335">W takim przypadku nie mamy już wartość `d` i generowanie adresu URL zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="17df5-335">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="17df5-336">Musisz określić żądaną wartość `c` i `d`.</span><span class="sxs-lookup"><span data-stu-id="17df5-336">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="17df5-337">Można oczekiwać, trafienia problem związany z trasa domyślna (`{controller}/{action}/{id?}`)-, ale rzadko wystąpi ten problem, w praktyce jako `Url.Action` będzie zawsze jawnie określić `controller` i `action` wartość.</span><span class="sxs-lookup"><span data-stu-id="17df5-337">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="17df5-338">Dłużej przeciążenia `Url.Action` skorzystać z dodatkowych *wartości trasy* obiektu do innego niż Podaj wartości dla parametrów trasy `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="17df5-338">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="17df5-339">Najczęściej zobaczysz następujący używane z `id` takich jak `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="17df5-339">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="17df5-340">Zgodnie z Konwencją *wartości trasy* obiektu jest zazwyczaj obiekt typu anonimowego, ale może też być `IDictionary<>` lub *zwykły stary obiekt .NET*.</span><span class="sxs-lookup"><span data-stu-id="17df5-340">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="17df5-341">Wszelkie wartości dodatkowe trasy, które nie są zgodne z parametrów trasy są umieszczane w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="17df5-341">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="17df5-342">Aby utworzyć bezwzględny adres URL, użyj przeciążenia, które akceptuje `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="17df5-342">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="17df5-343">Generowania adresów URL przez trasy</span><span class="sxs-lookup"><span data-stu-id="17df5-343">Generating URLs by route</span></span>

<span data-ttu-id="17df5-344">Powyższy kod przedstawione wygenerowania adresu URL, przekazując nazwy akcji i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="17df5-344">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="17df5-345">`IUrlHelper` udostępnia również `Url.RouteUrl` rodziny metod.</span><span class="sxs-lookup"><span data-stu-id="17df5-345">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="17df5-346">Te metody są podobne do `Url.Action`, ale ich nie Kopiuj bieżące wartości `action` i `controller` wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-346">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="17df5-347">Najbardziej typowe obciążenie jest określenie nazwy trasy na potrzeby generowania adresu URL, zazwyczaj określoną trasę *bez* określający nazwę kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-347">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="17df5-348">Generowania adresów URL w formacie HTML</span><span class="sxs-lookup"><span data-stu-id="17df5-348">Generating URLs in HTML</span></span>

<span data-ttu-id="17df5-349">`IHtmlHelper` udostępnia `HtmlHelper` metody `Html.BeginForm` i `Html.ActionLink` do generowania `<form>` i `<a>` elementy odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="17df5-349">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="17df5-350">Metody te za pomocą `Url.Action` metodę w celu wygenerowania adresu URL i akceptują podobne argumentów.</span><span class="sxs-lookup"><span data-stu-id="17df5-350">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="17df5-351">`Url.RouteUrl` Towarzyszami dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink` które mają podobne funkcje.</span><span class="sxs-lookup"><span data-stu-id="17df5-351">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="17df5-352">TagHelpers generowania adresów URL za pośrednictwem `form` pomocnika tagów i `<a>` pomocnika tagów.</span><span class="sxs-lookup"><span data-stu-id="17df5-352">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="17df5-353">Oba te użyj `IUrlHelper` ich wdrażania.</span><span class="sxs-lookup"><span data-stu-id="17df5-353">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="17df5-354">Zobacz [Praca z formularzami](../views/working-with-forms.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="17df5-354">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="17df5-355">W widokach `IUrlHelper` jest dostępna za pośrednictwem `Url` właściwość dla dowolnego Generowanie adresu URL ad hoc nie pasuje do żadnego z powyższych.</span><span class="sxs-lookup"><span data-stu-id="17df5-355">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="17df5-356">Generowania adresów URL w wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="17df5-356">Generating URLS in Action Results</span></span>

<span data-ttu-id="17df5-357">Powyższe przykłady wykazały, za pomocą `IUrlHelper` w kontrolerze, gdy najbardziej typowe obciążenie w kontrolerze jest do generowania adresu URL jako część wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-357">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="17df5-358">`ControllerBase` i `Controller` klas bazowych udostępniające podręczne metody dla wyników akcji, odwołujące się do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-358">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="17df5-359">Jeden typowy jest przekierowania po zaakceptowaniu dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="17df5-359">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="17df5-360">Metodami factory wyników akcji wykonaj podobny wzorzec do metod na `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="17df5-360">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="17df5-361">Przypadek specjalny dla dedykowanych konwencjonalne tras</span><span class="sxs-lookup"><span data-stu-id="17df5-361">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="17df5-362">Tradycyjnie routing można użyć specjalnego rodzaju definicji trasy o nazwie *dedykowanych trasy konwencjonalne*.</span><span class="sxs-lookup"><span data-stu-id="17df5-362">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="17df5-363">W poniższym przykładzie trasy o nazwie `blog` dedykowanych trasy są konwencjonalne funkcje.</span><span class="sxs-lookup"><span data-stu-id="17df5-363">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="17df5-364">Korzystając z tych definicji trasy `Url.Action("Index", "Home")` wygeneruje ze ścieżką URL `/` z `default` trasy, ale Dlaczego?</span><span class="sxs-lookup"><span data-stu-id="17df5-364">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="17df5-365">Może być odgadnięcie wartości trasy `{ controller = Home, action = Index }` może być wystarczające do generowania adresu URL przy użyciu `blog`, a wynik byłby `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="17df5-365">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="17df5-366">Dedykowany trasy konwencjonalne zależą od specjalnego zachowania w wartości domyślne, które nie mają odpowiednich parametru trasy, który uniemożliwia trasy "zbyt zachłannego" za pomocą Generowanie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-366">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="17df5-367">W tym przypadku wartości domyślne są `{ controller = Blog, action = Article }`, a `controller` ani `action` pojawia się jako parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-367">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="17df5-368">Gdy routing wykonuje generowanie adresu URL, podanych wartości muszą być zgodne z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="17df5-368">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="17df5-369">Za pomocą generowania adresu URL `blog` zakończy się niepowodzeniem, ponieważ wartości `{ controller = Home, action = Index }` nie są zgodne `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="17df5-369">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="17df5-370">Routing następnie powraca do wypróbowania `default`, który zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="17df5-370">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="17df5-371">Obszary</span><span class="sxs-lookup"><span data-stu-id="17df5-371">Areas</span></span>

<span data-ttu-id="17df5-372">[Obszary](areas.md) są funkcją MVC, używane do organizowania powiązanych funkcji do grupy jako osobne routingu — przestrzeń nazw (dla akcji kontrolera) i struktury ich folderów (w przypadku widoków).</span><span class="sxs-lookup"><span data-stu-id="17df5-372">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="17df5-373">Za pomocą obszarów umożliwia aplikacji istnieje wiele kontrolerów o takiej samej nazwie — tak długo, jak długo mają różne *obszarów*.</span><span class="sxs-lookup"><span data-stu-id="17df5-373">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="17df5-374">Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area` do `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="17df5-374">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="17df5-375">W tej sekcji omówi, jak routing współdziała z obszarami — zobacz [obszarów](areas.md) szczegółowe informacje na temat używania obszarów za pomocą widoków.</span><span class="sxs-lookup"><span data-stu-id="17df5-375">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="17df5-376">Poniższy przykład umożliwia skonfigurowanie MVC w celu użycia konwencjonalne trasy domyślnej i *trasy obszaru* obszaru o nazwie `Blog`:</span><span class="sxs-lookup"><span data-stu-id="17df5-376">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="17df5-377">Podczas dopasowywania ścieżki adresu URL, takich jak `/Manage/Users/AddUser`, pierwsza trasa będzie generować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="17df5-377">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="17df5-378">`area` Wartość trasy jest generowany przez wartość domyślną dla `area`, w rzeczywistości trasy utworzone przez `MapAreaRoute` jest odpowiednikiem następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="17df5-378">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="17df5-379">`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenie `area` przy użyciu podanej nazwy obszaru na, w tym przypadku `Blog`.</span><span class="sxs-lookup"><span data-stu-id="17df5-379">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="17df5-380">Wartość domyślna zapewnia trasy zawsze daje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-380">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="17df5-381">Tradycyjnie routing jest zależna od kolejności.</span><span class="sxs-lookup"><span data-stu-id="17df5-381">Conventional routing is order-dependent.</span></span> <span data-ttu-id="17df5-382">Ogólnie rzecz biorąc trasy z obszarami należy umieścić we wcześniejszej części tabeli tras, ponieważ są one bardziej szczegółowe niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="17df5-382">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="17df5-383">Korzystając z powyższego przykładu, wartości trasy będzie odpowiadać następującej akcji:</span><span class="sxs-lookup"><span data-stu-id="17df5-383">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="17df5-384">`AreaAttribute` To, co oznacza, kontroler jako część obszaru, mówi się, że ten kontroler jest w `Blog` obszaru.</span><span class="sxs-lookup"><span data-stu-id="17df5-384">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="17df5-385">Kontrolery bez `[Area]` atrybut nie są członkami żadnego obszaru i będzie **nie** pasuje, gdy `area` trasy podaje routingu.</span><span class="sxs-lookup"><span data-stu-id="17df5-385">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="17df5-386">W poniższym przykładzie, tylko pierwszy kontroler na liście może odnosić się wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="17df5-386">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="17df5-387">Przestrzeń nazw w każdym kontrolerze przedstawiono tutaj, aby informacje były kompletne — w przeciwnym razie kontrolerów miałby nazewnictwa w konflikcie i generują błędy kompilatora.</span><span class="sxs-lookup"><span data-stu-id="17df5-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="17df5-388">Klasy w przestrzeni nazw nie mają wpływu na routingu MVC.</span><span class="sxs-lookup"><span data-stu-id="17df5-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="17df5-389">Pierwsze dwa kontrolery są elementami członkowskimi obszarów i tylko dopasowania, gdy ich nazwy poszczególnych obszarów są dostarczane przez `area` wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="17df5-390">Trzeci kontroler nie jest elementem członkowskim dowolnego obszaru i może tylko dopasowania, gdy brak wartości parametru `area` są dostarczane przez routingu.</span><span class="sxs-lookup"><span data-stu-id="17df5-390">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="17df5-391">Pod względem dopasowania *żadnej wartości*, braku `area` wartość jest taka sama tak, jakby wartość `area` zostały o wartości null ani być ciągiem pustym.</span><span class="sxs-lookup"><span data-stu-id="17df5-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="17df5-392">Podczas wykonywania akcji wewnątrz obszaru, trasy wartość `area` będą dostępne jako *otoczenia wartość* routingu służące do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="17df5-393">Oznacza to, że domyślnie obszarów działania *umocowany* do generowania adresu URL, jak pokazano w następującym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="17df5-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="17df5-394">Opis IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="17df5-394">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="17df5-395">Ta sekcja jest rozszerzony na elementy wewnętrzne framework i jak MVC wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="17df5-395">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="17df5-396">Typowa aplikacja nie ma potrzeby niestandardowego `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="17df5-396">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="17df5-397">Prawdopodobnie używano już `IActionConstraint` nawet, jeśli nie znasz przy użyciu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="17df5-397">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="17df5-398">`[HttpGet]` Atrybutu i podobnych zastosowaniach `[Http-VERB]` implementację interfejsu `IActionConstraint` w celu ograniczenia wykonywania metody akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-398">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="17df5-399">Zakładając, że domyślną trasę konwencjonalne ze ścieżką URL `/Products/Edit` dałby w efekcie wartości `{ controller = Products, action = Edit }`, który umożliwi dopasowanie **zarówno** akcji, pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="17df5-399">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="17df5-400">W `IActionConstraint` terminologii będzie mówi się, że oba te akcje są traktowane jako kandydatów — jak były ze sobą zgodne dane trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-400">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="17df5-401">Gdy `HttpGetAttribute` jest wykonywana, jest wyświetlany tekst, który *Edit()* pasuje do *UZYSKAĆ* i nie ma dopasowania dla innych zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="17df5-401">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="17df5-402">`Edit(...)` Akcji nie ma żadnych ograniczeń zdefiniowanych i dlatego pokaże wszelkie zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="17df5-402">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="17df5-403">Tak zakładając, że `POST` — tylko `Edit(...)` zgodny.</span><span class="sxs-lookup"><span data-stu-id="17df5-403">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="17df5-404">Jednak dla `GET` obie akcje można nadal dopasować — jednak akcję z `IActionConstraint` zawsze jest uważany za *lepsze* niż akcję bez.</span><span class="sxs-lookup"><span data-stu-id="17df5-404">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="17df5-405">Dlatego ponieważ `Edit()` ma `[HttpGet]` jest uważany za bardziej szczegółowe i zostanie wybrany, jeżeli może odnosić się do obu akcji.</span><span class="sxs-lookup"><span data-stu-id="17df5-405">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="17df5-406">Model `IActionConstraint` jest formą *przeciążenie*, ale zamiast przeciążenia metod o tej samej nazwie, jest przeciążenie między akcjami, które pasują do tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="17df5-406">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="17df5-407">Routing atrybutu używa również `IActionConstraint` i może doprowadzić do działania z różnych kontrolerów oba są traktowane jako kandydatów.</span><span class="sxs-lookup"><span data-stu-id="17df5-407">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="17df5-408">Implementowanie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="17df5-408">Implementing IActionConstraint</span></span>

<span data-ttu-id="17df5-409">Najprostszym sposobem wdrożenia `IActionConstraint` polega na utworzeniu klasy pochodzącej od `System.Attribute` i umieść ją w akcji i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="17df5-409">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="17df5-410">MVC automatycznie wykryje, dowolny `IActionConstraint` które są stosowane jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="17df5-410">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="17df5-411">Model aplikacji można użyć, aby zastosować ograniczenia i jest to prawdopodobnie najbardziej elastycznym podejściem, ponieważ umożliwia ona metaprogram, w jaki sposób są one stosowane.</span><span class="sxs-lookup"><span data-stu-id="17df5-411">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="17df5-412">W poniższym przykładzie ograniczenie wybiera akcję na podstawie *numer kierunkowy kraju* z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="17df5-412">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="17df5-413">[Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="17df5-413">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="17df5-414">Odpowiedzialność za wdrażanie `Accept` metody i wybierając polecenie "Order" ograniczenia do wykonania.</span><span class="sxs-lookup"><span data-stu-id="17df5-414">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="17df5-415">W tym przypadku `Accept` metoda zwraca `true` do oznaczania Akcja odnosi się do dopasowania podczas `country` trasy do dopasowania wartości.</span><span class="sxs-lookup"><span data-stu-id="17df5-415">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="17df5-416">To różni się od `RouteValueAttribute` , ponieważ umożliwia powrót do działania z bezatrybutowego.</span><span class="sxs-lookup"><span data-stu-id="17df5-416">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="17df5-417">Przykład pokazuje, że jeśli zdefiniujesz `en-US` akcji, a następnie kod kraju, takich jak `fr-FR` powróci do kontrolera bardziej ogólnym, który nie ma `[CountrySpecific(...)]` stosowane.</span><span class="sxs-lookup"><span data-stu-id="17df5-417">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="17df5-418">`Order` Właściwość decyduje, które *etapu* ograniczenie jest częścią.</span><span class="sxs-lookup"><span data-stu-id="17df5-418">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="17df5-419">Ograniczenia akcji uruchamiania w grupach na podstawie `Order`.</span><span class="sxs-lookup"><span data-stu-id="17df5-419">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="17df5-420">Na przykład Framework podane atrybuty metody HTTP używać tego samego `Order` wartość, aby były one uruchamiane w tym samym etapie.</span><span class="sxs-lookup"><span data-stu-id="17df5-420">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="17df5-421">Może mieć dowolną liczbę etapie, ponieważ musisz zaimplementować żądane zasady.</span><span class="sxs-lookup"><span data-stu-id="17df5-421">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="17df5-422">Aby wybrać wartość `Order` Pomyśl o czy swoje ograniczenia powinny być stosowane przed metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="17df5-422">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="17df5-423">Niższych numerach uruchomiony jako pierwszy.</span><span class="sxs-lookup"><span data-stu-id="17df5-423">Lower numbers run first.</span></span>
