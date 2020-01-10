---
title: Routing do akcji kontrolera w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób ASP.NET Core MVC używa programów pośredniczących routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/routing
ms.openlocfilehash: 8cf7e74df292a614f287eff8561a22187f6558ce
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866062"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>Routing do akcji kontrolera w ASP.NET Core

[Ryany Nowak](https://github.com/rynowak) i [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC używa [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje. Trasy są zdefiniowane w kodzie startowym lub atrybutów. Trasy opisują sposób dopasowywania ścieżek adresów URL do akcji. Trasy są również używane do generowania adresów URL (dla linków) wysyłanych w odpowiedziach.

Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut. Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany. Aby uzyskać więcej informacji, zobacz [Routing mieszany](#routing-mixed-ref-label) .

Ten dokument wyjaśnia interakcje między MVC i routingiem oraz sposób, w jaki typowe aplikacje MVC używają funkcji routingu. Zobacz [Routing](xref:fundamentals/routing) , aby uzyskać szczegółowe informacje na temat routingu zaawansowanego.

## <a name="setting-up-routing-middleware"></a>Konfigurowanie oprogramowania pośredniczącego routingu

W metodzie *konfigurowania* może zostać wyświetlony kod podobny do:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Wewnątrz wywołania do `UseMvc`, `MapRoute` służy do tworzenia pojedynczej trasy, która odnosi się do trasy `default`. Większość aplikacji MVC będzie używać trasy z szablonem podobnym do trasy `default`.

`"{controller=Home}/{action=Index}/{id?}"` szablonu trasy może być zgodna ze ścieżką URL podobną do `/Products/Details/5` i wyodrębniać wartości tras `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki. MVC podejmie próbę zlokalizowania kontrolera o nazwie `ProductsController` i uruchomi akcję `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Należy zauważyć, że w tym przykładzie powiązanie modelu będzie używać wartości `id = 5`, aby ustawić parametr `id` do `5` podczas wywoływania tej akcji. Aby uzyskać więcej informacji, zobacz [powiązanie modelu](../models/model-binding.md) .

Użycie trasy `default`:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Szablon trasy:

* `{controller=Home}` definiuje `Home` jako `controller` domyślny

* `{action=Index}` definiuje `Index` jako `action` domyślny

* `{id?}` definiuje `id` jako opcjonalne

Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania. Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) .

`"{controller=Home}/{action=Index}/{id?}"` może być zgodna ze ścieżką URL `/` i spowoduje utworzenie wartości tras `{ controller = Home, action = Index }`. Wartości `controller` i `action` używają wartości domyślnych, `id` nie produkuje wartości, ponieważ w ścieżce URL nie ma odpowiedniego segmentu. Aby można było wybrać akcję `HomeController` i `Index`, MVC będzie używać tych wartości tras:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Za pomocą tej definicji kontrolera i szablonu trasy, Akcja `HomeController.Index` będzie wykonywana dla dowolnej z następujących ścieżek URL:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Wygodna `UseMvcWithDefaultRoute`metody:

```csharp
app.UseMvcWithDefaultRoute();
```

Może służyć do zastępowania:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` i `UseMvcWithDefaultRoute` dodać wystąpienie `RouterMiddleware` do potoku programu pośredniczącego. MVC nie działa bezpośrednio w oprogramowaniu pośredniczącym i używa routingu do obsługi żądań. MVC jest połączony z trasami za pomocą wystąpienia `MvcRouteHandler`. Kod w `UseMvc` jest podobny do następującego:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` nie definiuje bezpośrednio żadnych tras, dodaje symbol zastępczy do kolekcji tras dla trasy `attribute`. `UseMvc(Action<IRouteBuilder>)` Przeciążenie umożliwia dodanie własnych tras, a także obsługuje routing atrybutów.  `UseMvc` i wszystkie jego odmiany dodają symbol zastępczy dla atrybutu trasa — atrybut jest zawsze dostępny niezależnie od sposobu konfigurowania `UseMvc`. `UseMvcWithDefaultRoute` definiuje domyślną trasę i obsługuje routing atrybutów. Sekcja [Routing atrybutów](#attribute-routing-ref-label) zawiera więcej szczegółów dotyczących routingu atrybutów.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Routing konwencjonalny

Trasa `default`:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

jest przykładem *konwencjonalnego routingu*. Nazywamy ten styl *konwencjonalnym routingiem* , ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:

* pierwszy segment ścieżki jest mapowany na nazwę kontrolera

* Druga mapowanie na nazwę akcji.

* trzeci segment jest używany w przypadku opcjonalnego `id` używanego do mapowania na jednostkę modelu

Korzystając z tej `default` trasy, Ścieżka adresu URL `/Products/List` jest mapowana na akcję `ProductsController.List` i `/Blog/Article/17` mapy `BlogController.Article`. To mapowanie jest oparte **tylko** na nazwach kontrolera i akcji, a nie na podstawie przestrzeni nazw, lokalizacji plików źródłowych ani parametrów metody.

> [!TIP]
> Użycie konwencjonalnego routingu z domyślną trasą umożliwia szybkie Kompilowanie aplikacji bez konieczności podawania nowego wzorca adresu URL dla każdej zdefiniowanej akcji. W przypadku aplikacji z akcjami w stylu CRUD zachowanie spójności adresów URL na kontrolerach może pomóc uprościć kod i uczynić interfejs użytkownika bardziej przewidywalny.

> [!WARNING]
> `id` jest zdefiniowany jako opcjonalny przez szablon trasy, co oznacza, że akcje mogą być wykonywane bez identyfikatora dostarczonego jako część adresu URL. Typowo to, co się stanie, jeśli `id` zostanie pominięty w adresie URL, zostanie ustawiony na `0` przez powiązanie modelu i w wyniku tego nie zostanie znaleziona żadna jednostka w `id == 0`dopasowania bazy danych. Routing atrybutu może dać szczegółowy formant, który ma być wymagany dla niektórych akcji, a nie dla innych. Zgodnie z Konwencją dokumentacja będzie zawierać parametry opcjonalne, takie jak `id`, gdy prawdopodobnie będą widoczne w prawidłowym użyciu.

## <a name="multiple-routes"></a>Wiele tras

Można dodać wiele tras wewnątrz `UseMvc`, dodając więcej wywołań do `MapRoute`. Dzięki temu można zdefiniować wiele Konwencji lub dodać trasy konwencjonalne, które są przeznaczone dla konkretnej akcji, na przykład:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Trasa `blog` w tym miejscu jest *dedykowaną tradycyjną trasą*, co oznacza, że używa systemu routingu konwencjonalnego, ale jest przeznaczona dla konkretnej akcji. Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i w związku z tym trasa będzie zawsze mapowana na `BlogController.Article`akcji.

Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, w jakiej zostały dodane. W tym przykładzie zostanie podjęta próba `blog` trasy przed trasą `default`.

> [!NOTE]
> *Dedykowane konwencjonalne trasy* często korzystają z parametrów trasy catch-all, takich jak `{*article}`, do przechwytywania pozostałej części ścieżki URL. Może to spowodować, że trasa "zbyt zachłanne" oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy. Umieść trasy "zachłanne" później w tabeli tras, aby rozwiązać ten problem.

### <a name="fallback"></a>Opcja rezerwowa

W ramach przetwarzania żądań MVC sprawdzi, czy wartości trasy mogą być używane do znajdowania kontrolera i akcji w aplikacji. Jeśli wartości trasy nie są zgodne z akcją, trasa nie jest uważana za dopasowanie i zostanie podjęta kolejna trasa. Jest to nazywane *Fallback*i ma na celu uproszczenie przypadków, w których trasy konwencjonalne nakładają się na siebie.

### <a name="disambiguating-actions"></a>Niejednoznaczne akcje

Gdy dwie akcje są zgodne z routingiem, MVC musi odróżnić się, aby wybrać najlepszy kandydat lub w przeciwnym razie zgłosić wyjątek. Na przykład:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Ten kontroler definiuje dwie akcje, które byłyby zgodne ze ścieżką URL `/Products/Edit/17` i trasy `{ controller = Products, action = Edit, id = 17 }`danych. Jest to typowy wzorzec dla kontrolerów MVC, gdzie `Edit(int)` pokazuje formularz służący do edytowania produktu, a `Edit(int, Product)` przetwarza opublikowany formularz. Aby ten możliwy składnik MVC musiał wybrać `Edit(int, Product)`, gdy żądanie jest `POST` HTTP i `Edit(int)`, gdy zlecenie HTTP jest inne.

`HttpPostAttribute` (`[HttpPost]`) to implementacja `IActionConstraint`, która będzie zezwalać na wybranie akcji tylko wtedy, gdy zlecenie HTTP jest `POST`. Obecność `IActionConstraint` powoduje, że `Edit(int, Product)` dopasowanie "lepszy" niż `Edit(int)`, więc `Edit(int, Product)` zostanie podjęta najpierw.

Należy napisać niestandardowe implementacje `IActionConstraint` w wyspecjalizowanych scenariuszach, ale ważne jest, aby zrozumieć rolę atrybutów, takich jak atrybuty `HttpPostAttribute`-like, które są zdefiniowane dla innych zleceń HTTP. W konwencjonalnym routingu typowe dla akcji używanie tej samej nazwy akcji, gdy są one częścią przepływu pracy `show form -> submit form`. Wygoda tego wzorca stanie się bardziej oczywista po przejrzeniu sekcji [zrozumienie IActionConstraint](#understanding-iactionconstraint) .

Jeśli wiele pasujących tras i MVC nie mogą znaleźć "najlepszej" trasy, wygeneruje `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Nazwy tras

Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwami tras:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Nazwy tras nadaj logicznej nazwie trasy, aby nazwana trasa mogła być używana do generowania adresów URL. Znacznie upraszcza to tworzenie adresów URL, gdy porządkowanie tras może spowodować skomplikowane generowanie adresów URL. Nazwy tras muszą być unikatowe w całej aplikacji.

Nazwy tras nie mają wpływu na Dopasowywanie adresów URL ani obsługę żądań; są one używane tylko do generowania adresów URL. [Routing](xref:fundamentals/routing) zawiera bardziej szczegółowe informacje na temat generowania adresów URL, w tym generowanie adresów URL w pomocnikach specyficznych dla MVC.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Routing atrybutów

Funkcja routingu atrybutów używa zestawu atrybutów do mapowania akcji bezpośrednio do szablonów tras. W poniższym przykładzie `app.UseMvc();` jest używany w metodzie `Configure` i nie jest przenoszona żadna trasa. `HomeController` będzie zgodna z zestawem adresów URL podobnym do tego, co `{controller=Home}/{action=Index}/{id?}` trasie domyślnej:

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

Akcja `HomeController.Index()` będzie wykonywana dla dowolnej ścieżki URL `/`, `/Home`lub `/Home/Index`.

> [!NOTE]
> W tym przykładzie przedstawiono najważniejsze różnice programistyczne między routingiem atrybutów i routingiem konwencjonalnym. Routing atrybutów wymaga więcej danych wejściowych w celu określenia trasy; konwencjonalne trasy domyślne obsługuje trasy bardziej zwięzłie. Jednak Routing atrybutu zezwala na (i wymaga) precyzyjnej kontroli, które szablony tras mają zastosowanie do poszczególnych akcji.

W przypadku routingu atrybutów nazwa kontrolera i nazwy akcji nie odgrywają **żadnej** roli, w której akcja jest zaznaczona. Ten przykład będzie pasował do tych samych adresów URL, co w poprzednim przykładzie.

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
> Powyższe szablony tras nie definiują parametrów trasy dla `action`, `area`i `controller`. W rzeczywistości te parametry tras są niedozwolone w trasach atrybutów. Ponieważ szablon trasy jest już skojarzony z akcją, nie ma sensu analizy nazwy akcji na podstawie adresu URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Routing atrybutów z atrybutami http [Verb]

Routing atrybutu może również używać atrybutów `Http[Verb]`, takich jak `HttpPostAttribute`. Wszystkie te atrybuty mogą akceptować szablon trasy. Ten przykład przedstawia dwie akcje, które pasują do tego samego szablonu trasy:

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

Dla ścieżki URL, takiej jak `/products` akcja `ProductsApi.ListProducts` zostanie wykonana, gdy zlecenie HTTP zostanie `GET` i `ProductsApi.CreateProduct` zostanie wykonane, gdy zlecenie HTTP jest `POST`. Routing atrybutu najpierw jest zgodny z adresem URL względem zestawu szablonów tras zdefiniowanych przez atrybuty trasy. Po dopasowaniu szablonu trasy do określenia, które akcje mogą być wykonywane, są stosowane ograniczenia `IActionConstraint`.

> [!TIP]
> Podczas kompilowania interfejsu API REST trudno jest użyć `[Route(...)]` na metodę akcji, ponieważ akcja akceptuje wszystkie metody HTTP. Lepiej jest używać bardziej szczegółowych `Http*Verb*Attributes`, aby precyzyjnie dowiedzieć się, co obsługuje interfejs API. Klienci interfejsów API REST powinni wiedzieć, jakie ścieżki i czasowniki HTTP mapują na określone operacje logiczne.

Ponieważ atrybut Route ma zastosowanie do określonej akcji, można łatwo wprowadzić parametry wymagane jako część definicji szablonu trasy. W tym przykładzie `id` jest wymagane jako część ścieżki URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Akcja `ProductsApi.GetProduct(int)` zostanie wykonana dla ścieżki URL, takiej jak `/products/3`, ale nie dla ścieżki URL, takiej jak `/products`. Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](../../fundamentals/routing.md) .

## <a name="route-name"></a>Nazwa trasy

Poniższy kod definiuje *nazwę trasy* `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Nazwy tras mogą służyć do generowania adresów URL na podstawie określonej trasy. Nazwy tras nie mają wpływu na zachowanie routingu w adresie URL i są używane tylko na potrzeby generowania adresów URL. Nazwy tras muszą być unikatowe w całej aplikacji.

> [!NOTE]
> W przeciwieństwie do konwencjonalnej *trasy domyślnej*, która definiuje `id` parametr jako opcjonalny (`{id?}`). Ta możliwość precyzyjnego określania interfejsów API ma zalety, takich jak umożliwienie `/products` i `/products/5` do wysłania do różnych akcji.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Łączenie tras

Aby mniej powtarzać Routing atrybutów, atrybuty trasy na kontrolerze są łączone z atrybutami trasy dla poszczególnych akcji. Wszystkie szablony tras zdefiniowane na kontrolerze są poprzedzone w celu rozesłania szablonów w akcjach. Umieszczenie atrybutu trasy na kontrolerze powoduje, że **wszystkie** akcje w kontrolerze używają routingu atrybutów.

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

W tym przykładzie ścieżka adresu URL `/products` może być zgodna `ProductsApi.ListProducts`, a ścieżka URL `/products/5` może pasować `ProductsApi.GetProduct(int)`. Obie te akcje pasują do `GET` HTTP, ponieważ są oznaczone `HttpGetAttribute`.

Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są łączone z szablonami tras zastosowanymi do kontrolera. Ten przykład dopasowuje zestaw ścieżek URL podobny do *trasy domyślnej*.

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

### <a name="ordering-attribute-routes"></a>Określanie kolejności tras atrybutów

W przeciwieństwie do konwencjonalnych tras, które są wykonywane w określonej kolejności, routing atrybutu kompiluje drzewo i dopasowuje wszystkie trasy jednocześnie. Zachowuje się tak, jakby wpisy trasy zostały umieszczone w idealnym porządku; najbardziej konkretne trasy mają możliwość wykonania przed bardziej ogólnymi trasami.

Przykładowo trasa, taka jak `blog/search/{topic}`, jest bardziej szczegółowa niż trasa, taka jak `blog/{*article}`. Logicznie domyślnie mówiąc `blog/search/{topic}` trasę "uruchomienia", ponieważ jest to jedyna rozsądna kolejność. Przy użyciu konwencjonalnego routingu deweloper jest odpowiedzialny za umieszczanie tras w odpowiedniej kolejności.

Trasy atrybutów mogą konfigurować kolejność przy użyciu właściwości `Order` wszystkich atrybutów tras dostarczonych przez platformę. Trasy są przetwarzane zgodnie z rosnącym sortowaniem właściwości `Order`. Kolejność domyślna to `0`. Ustawienie trasy przy użyciu `Order = -1` zostanie uruchomione przed trasami, które nie ustawiają kolejności. Ustawienie trasy przy użyciu `Order = 1` zostanie uruchomione po określeniu domyślnej kolejności tras.

> [!TIP]
> Należy unikać w zależności od `Order`. Jeśli przestrzeń adresów URL wymaga jawnych wartości kolejności, aby można było prawidłowo kierować trasy, to prawdopodobnie również jest myląca dla klientów. W ogólnym routingu atrybutów wybierz prawidłową trasę z dopasowywaniem adresów URL. Jeśli domyślna kolejność generowania adresów URL nie działa, użycie nazwy trasy jako przesłonięcia jest zwykle prostsze niż stosowanie właściwości `Order`.

Razor Pages Routing i kontroler MVC współdzielą implementację. Informacje o zamówieniu trasy w Razor Pages tematy są dostępne na stronie [Razor Pages trasy i konwencje aplikacji: kolejność tras](xref:razor-pages/razor-pages-conventions#route-order).

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Zastępowanie tokenu w szablonach tras ([Controller], [Action], [obszar])

Dla wygody, trasy atrybutu obsługują *zastępowanie tokenów* przez umieszczanie tokenu w nawiasach kwadratowych (`[`, `]`). Tokeny `[action]`, `[area]`i `[controller]` są zastępowane wartościami nazwy akcji, obszaru i nazwy kontrolera z akcji, w której jest zdefiniowana trasa. W poniższym przykładzie akcje są zgodne ze ścieżkami URL zgodnie z opisem w komentarzach:

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów. Powyższy przykład będzie zachowywać się tak samo jak poniższy kod:

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Trasy atrybutu można także łączyć z dziedziczeniem. Jest to szczególnie zaawansowane w połączeniu z zastępowaniem tokenu.

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

Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów. `[Route("[controller]/[action]", Name="[controller]_[action]")]` generuje unikatową nazwę trasy dla każdej akcji.

Aby dopasować ogranicznik zastępczy tokenu literału `[` lub `]`, należy to zrobić, powtarzając znak (`[[` lub `]]`).

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>Używanie transformatora parametrów do dostosowywania zastępowania tokenu

Zastępowanie tokenu można dostosować za pomocą transformatora parametrów. Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów. Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`.

`RouteTokenTransformerConvention` jest konwencją modelu aplikacji, która:

* Stosuje transformator parametrów do wszystkich tras atrybutów w aplikacji.
* Dostosowuje wartości tokenów tras w miarę ich wymiany.

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

`RouteTokenTransformerConvention` jest zarejestrowany jako opcja w `ConfigureServices`.

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

### <a name="multiple-routes"></a>Wiele tras

Routing atrybutów obsługuje definiowanie wielu tras, które docierają do tej samej akcji. Najczęstszym sposobem użycia tej metody jest naśladowanie zachowania *domyślnej trasy konwencjonalnej* , jak pokazano w następującym przykładzie:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Umieszczenie wielu atrybutów trasy na kontrolerze oznacza, że każda z nich będzie łączyć się z poszczególnymi atrybutami trasy w metodach akcji.

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

Gdy wiele atrybutów trasy (implementujących `IActionConstraint`) jest umieszczanych w akcji, każde ograniczenie akcji łączy się z szablonem trasy z atrybutu, który go zdefiniował.

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
> Chociaż używanie wielu tras w akcjach może wydawać się zaawansowane, lepiej jest zachować prosty i dobrze zdefiniowany obszar adresów URL aplikacji. Użyj wielu tras w przypadku akcji tylko wtedy, gdy jest to konieczne, na przykład do obsługi istniejących klientów.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Określanie opcjonalnych parametrów trasy, wartości domyślnych i ograniczeń

Trasy atrybutów obsługują tę samą składnię wbudowaną co konwencjonalne trasy, aby określić parametry opcjonalne, wartości domyślne i ograniczenia.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) .

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Niestandardowe atrybuty trasy przy użyciu `IRouteTemplateProvider`

Wszystkie atrybuty trasy podane w strukturze (`[Route(...)]`, `[HttpGet(...)]` itp.) implementują interfejs `IRouteTemplateProvider`. MVC szuka atrybutów klas kontrolera i metod akcji, gdy aplikacja zostanie uruchomiona i używa tych, które implementują `IRouteTemplateProvider`, aby skompilować początkowy zestaw tras.

Możesz zaimplementować `IRouteTemplateProvider`, aby zdefiniować własne atrybuty trasy. Każda `IRouteTemplateProvider` umożliwia zdefiniowanie pojedynczej trasy z niestandardowym szablonem trasy, kolejnością i nazwą:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

Ten atrybut z powyższego przykładu automatycznie ustawia `Template` do `"api/[controller]"` w przypadku zastosowania `[MyApiController]`.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Dostosowywanie tras atrybutów przy użyciu modelu aplikacji

*Model aplikacji* jest modelem obiektów tworzonym podczas uruchamiania ze wszystkimi metadanymi używanymi przez MVC do kierowania i wykonywania akcji. *Model aplikacji* zawiera wszystkie dane zebrane z atrybutów tras (za pośrednictwem `IRouteTemplateProvider`). Można napisać *konwencje* , aby zmodyfikować model aplikacji w czasie uruchamiania, aby dostosować sposób zachowania routingu. W tej sekcji przedstawiono prosty przykład dostosowywania routingu przy użyciu modelu aplikacji.

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Routing mieszany: Routing atrybutów a konwencjonalny Routing

Aplikacje MVC mogą łączyć użycie konwencjonalnego routingu i routingu atrybutów. Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.

Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut. Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany. Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa. **Każdy** atrybut trasy na kontrolerze powoduje kierowanie wszystkich akcji w atrybucie kontrolera.

> [!NOTE]
> Co odróżnia dwa typy systemów routingu, proces stosowany po adresie URL pasuje do szablonu trasy. W przypadku routingu konwencjonalnego wartości trasy z dopasowania są używane do wybierania akcji i kontrolera z tabeli odnośników wszystkich konwencjonalnych akcji kierowanych. W obszarze Routing atrybutów każdy szablon jest już skojarzony z akcją i nie jest wymagany dalsze wyszukiwanie.

## <a name="complex-segments"></a>Złożone segmenty

Złożone segmenty (na przykład `[Route("/dog{token}cat")]`) są przetwarzane przez dopasowanie literałów od prawej do lewej w sposób niezachłanney. Sprawdź [kod źródłowy](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) opisu. Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/8197).

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generowanie adresu URL

Aplikacje MVC mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji. Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany. Ta sekcja koncentruje się na funkcjach generowania adresów URL dostarczonych przez MVC i obejmuje tylko podstawowe informacje na temat sposobu działania generowania adresów URL. Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](../../fundamentals/routing.md) .

Interfejs `IUrlHelper` jest podstawową częścią infrastruktury między MVC i routingiem na potrzeby generowania adresów URL. Wystąpienie `IUrlHelper` dostępne za pomocą właściwości `Url` w obszarze Kontrolery, widoki i składniki widoku.

W tym przykładzie interfejs `IUrlHelper` jest używany przez właściwość `Controller.Url` do generowania adresu URL dla innej akcji.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Jeśli aplikacja używa domyślnej trasy konwencjonalnej, wartością zmiennej `url` będzie ciąg ścieżki adresu URL `/UrlGeneration/Destination`. Ta ścieżka URL jest tworzona za pomocą routingu przez połączenie wartości trasy z bieżącego żądania (wartości otoczenia), z wartościami przekazanymi do `Url.Action` i podstawianie tych wartości do szablonu trasy:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Każdy parametr trasy w szablonie trasy ma swoją wartość zastępowaną przez pasujące nazwy wartościami i wartościami otoczenia. Parametr trasy, który nie ma wartości, może korzystać z wartości domyślnej, jeśli ma taką wartość, lub być pominięty, jeśli jest opcjonalny (tak jak w przypadku `id` w tym przykładzie). Generowanie adresu URL zakończy się niepowodzeniem, jeśli którykolwiek z wymaganych parametrów trasy nie ma odpowiadającej wartości. Jeśli generowanie adresów URL kończy się niepowodzeniem dla trasy, kolejna trasa zostanie ponowiona do momentu przetworzenia wszystkich tras lub znalezienia dopasowania.

Na przykład `Url.Action` powyżej zakłada się, że konwencjonalne Routing, ale generowanie adresów URL działa podobnie z routingiem atrybutów, chociaż koncepcje różnią się. W przypadku routingu konwencjonalnego wartości tras są używane do rozszerzania szablonu, a wartości trasy dla `controller` i `action` zazwyczaj pojawiają się w tym szablonie — działa to, ponieważ adresy URL dopasowane przez Routing są zgodne z *Konwencją*. W obszarze Routing atrybutów wartości trasy dla `controller` i `action` nie mogą być wyświetlane w szablonie — są one używane do wyszukiwania szablonu do użycia.

W tym przykładzie zastosowano Routing atrybutów:

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC kompiluje tabelę odnośników wszystkich akcji przypisanych do atrybutu i dopasowuje wartości `controller` i `action`, aby wybrać szablon trasy do użycia na potrzeby generowania adresów URL. W przykładzie powyżej jest generowana `custom/url/to/destination`.

### <a name="generating-urls-by-action-name"></a>Generowanie adresów URL według nazwy akcji

`Url.Action` (`IUrlHelper` . `Action`) i wszystkie powiązane przeciążenia są oparte na tym pomysłie, aby określić, do czego chcesz utworzyć łącze, określając nazwę kontrolera i nazwę akcji.

> [!NOTE]
> W przypadku korzystania z `Url.Action`, bieżące wartości trasy dla `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` są częścią *wartości otoczenia* **i** *wartości*. Metoda `Url.Action`, zawsze używa bieżących wartości `action` i `controller` i wygeneruje ścieżkę URL, która kieruje do bieżącej akcji.

Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały wprowadzone podczas generowania adresu URL. Przy użyciu trasy, takiej jak `{a}/{b}/{c}/{d}` i wartości otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma wystarczającą ilość informacji do wygenerowania adresu URL bez dodatkowych wartości — ponieważ wszystkie parametry trasy mają wartość. W przypadku dodania wartości `{ d = Donovan }`wartość `{ d = David }` zostanie zignorowana, a wygenerowana Ścieżka adresu URL zostanie `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Ścieżki URL są hierarchiczne. W powyższym przykładzie, jeśli dodano wartość `{ c = Cheryl }`, obie wartości `{ c = Carol, d = David }` zostałyby zignorowane. W takim przypadku nie ma już wartości `d` a generowanie adresów URL zakończy się niepowodzeniem. Należy określić pożądaną wartość `c` i `d`.  Może wystąpić potrzeba napotkania tego problemu przy użyciu trasy domyślnej (`{controller}/{action}/{id?}`), ale w takim przypadku rzadko wystąpi to zachowanie, ponieważ `Url.Action` będzie zawsze jawnie określać `controller` i `action` wartość.

Dłuższe przeciążenia `Url.Action` również pobierają dodatkowy obiekt *wartości trasy* , aby zapewnić wartości parametrów trasy innych niż `controller` i `action`. Najczęściej zobaczysz ten sposób, który jest używany z `id`, takich jak `Url.Action("Buy", "Products", new { id = 17 })`. Według Konwencji obiekt *wartości trasy* jest zwykle obiektem typu anonimowego, ale może również być `IDictionary<>` lub *zwykłym starym obiektem platformy .NET*. Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Aby utworzyć bezwzględny adres URL, Użyj przeciążenia, które akceptuje `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generowanie adresów URL według trasy

Powyższy kod wygeneruje adres URL przez przekazanie go do kontrolera i nazwy akcji. `IUrlHelper` zapewnia także `Url.RouteUrl` rodziny metod. Te metody są podobne do `Url.Action`, ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy. Najbardziej typowym zastosowaniem jest określenie nazwy trasy do użycia określonej trasy do wygenerowania adresu URL, na ogół *bez* określania kontrolera lub nazwy akcji.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Generowanie adresów URL w kodzie HTML

`IHtmlHelper` udostępnia metody `HtmlHelper`, `Html.BeginForm` i `Html.ActionLink` generować odpowiednio elementy `<form>` i `<a>`. Te metody używają metody `Url.Action` do wygenerowania adresu URL i akceptują podobne argumenty. `Url.RouteUrl` pomocników dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink`, które mają podobną funkcjonalność.

TagHelpers Generuj adresy URL za pomocą `form` TagHelper i `<a>` TagHelper. Oba te zastosowania `IUrlHelper` do ich implementacji. Aby uzyskać więcej informacji, zobacz [Praca z formularzami](../views/working-with-forms.md) .

W widokach, `IUrlHelper` jest dostępna za pomocą właściwości `Url` dla dowolnej generacji adresów URL ad hoc, które nie są objęte powyższym.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Generowanie adresów URL w wynikach akcji

Powyższe przykłady przedstawiono przy użyciu `IUrlHelper` w kontrolerze, podczas gdy najbardziej typowym użyciem na kontrolerze jest wygenerowanie adresu URL jako części wyniku akcji.

Klasy bazowe `ControllerBase` i `Controller` zapewniają wygodne metody dla wyników akcji, które odwołują się do innej akcji. Jednym z typowych zastosowań jest przekierowanie po zaakceptowaniu danych wejściowych użytkownika.

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

Metody fabryki wyników akcji postępują zgodnie z podobnym wzorcem do metod w `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Specjalny przypadek dla dedykowanych tras konwencjonalnych

Funkcja routingu konwencjonalnego może używać specjalnego rodzaju definicji trasy o nazwie *dedykowanej, konwencjonalnej trasy*. W poniższym przykładzie trasa o nazwie `blog` jest dedykowaną umowną trasą.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Korzystając z tych definicji tras, `Url.Action("Index", "Home")` wygeneruje ścieżkę URL `/` z trasą `default`, ale dlaczego? Możesz odgadnąć wartości tras `{ controller = Home, action = Index }` wystarcza do wygenerowania adresu URL przy użyciu `blog`, a wynik zostanie `/blog?action=Index&controller=Home`.

Dedykowane konwencjonalne trasy polegają na specjalnym zachowaniu wartości domyślnych, które nie mają odpowiadającego parametru trasy, który uniemożliwia trasie "zbyt zachłanne" z generowaniem adresów URL. W takim przypadku wartości domyślne są `{ controller = Blog, action = Article }`, a ani `controller`, ani `action` nie pojawiają się jako parametr trasy. Gdy Routing wykonuje generowanie adresów URL, podane wartości muszą być zgodne z wartościami domyślnymi. Generowanie adresu URL przy użyciu `blog` nie powiedzie się, ponieważ wartości `{ controller = Home, action = Index }` nie pasują do `{ controller = Blog, action = Article }`. Następnie usługa routingu wraca do wypróbowania `default`, która kończy się powodzeniem.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Obszary

[Obszary](areas.md) są funkcją MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw routingu (dla akcji kontrolera) i struktury folderów (dla widoków). Użycie obszarów umożliwia aplikacji posiadanie wielu kontrolerów o tej samej nazwie, o ile mają one różne *obszary*. Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie innego parametru trasy, `area` do `controller` i `action`. W tej sekcji omówiono sposób, w jaki Routing współdziała z obszarami — zobacz [obszary](areas.md) , aby uzyskać szczegółowe informacje o tym, jak obszary są używane w widokach.

Poniższy przykład konfiguruje MVC do używania domyślnej trasy konwencjonalnej i *trasy obszaru* dla obszaru o nazwie `Blog`:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

W przypadku dopasowania ścieżki URL, takiej jak `/Manage/Users/AddUser`, Pierwsza trasa będzie generować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`. `area` wartość trasy jest generowana przez wartość domyślną dla `area`, w rzeczywistości trasa utworzona przez `MapAreaRoute` jest równoważna następującym:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dla `area` przy użyciu podanej nazwy obszaru, w tym przypadku `Blog`. Wartość domyślna zapewnia, że trasa zawsze generuje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresów URL.

> [!TIP]
> Routowanie konwencjonalne jest zależne od kolejności. Ogólnie rzecz biorąc, trasy z obszarami należy umieścić wcześniej w tabeli tras, ponieważ są one bardziej specyficzne niż trasy bez obszaru.

Korzystając z powyższego przykładu, wartości trasy będą zgodne z następującą akcją:

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` to oznacza, że w ramach obszaru znajduje się kontroler. Załóżmy, że ten kontroler znajduje się w obszarze `Blog`. Kontrolery bez atrybutu `[Area]` nie są członkami żadnego obszaru i **nie** będą zgodne, gdy wartość trasy `area` zostanie udostępniona przez Routing. W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu w celu zapewnienia kompletności — w przeciwnym razie kontrolery będą mieć konflikt nazw i generują błąd kompilatora. Przestrzenie nazw klas nie mają wpływu na Routing MVC.

Pierwsze dwa kontrolery są członkami obszarów i pasują tylko wtedy, gdy ich nazwa obszaru jest podana przez wartość `area` trasy. Trzeci kontroler nie jest członkiem żadnego obszaru i może pasować tylko wtedy, gdy żadna wartość `area` nie jest udostępniana przez Routing.

> [!NOTE]
> W warunkach pasujących do *nie ma żadnej wartości*, brak wartości `area` jest taka sama jak wtedy, gdy wartość `area` była zerowa lub ciągiem pustym.

Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` będzie dostępna jako *wartość otoczenia* dla routingu do użycia na potrzeby generowania adresów URL. Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Zrozumienie IActionConstraint

> [!NOTE]
> Ta sekcja jest głęboką szczegółoweą wewnętrznych struktur oraz jak MVC wybiera akcję do wykonania. Typowa aplikacja nie będzie potrzebować niestandardowego `IActionConstraint`

Prawdopodobnie już używasz `IActionConstraint`, nawet jeśli nie masz doświadczenia z interfejsem. Atrybut `[HttpGet]` i podobne atrybuty `[Http-VERB]` implementują `IActionConstraint` w celu ograniczenia wykonywania metody akcji.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Przy założeniu domyślnej trasy konwencjonalnej, Ścieżka adresu URL `/Products/Edit` będzie generować wartości `{ controller = Products, action = Edit }`, które byłyby zgodne z **obiema** akcjami przedstawionymi w tym miejscu. W `IActionConstraint` terminologia, że obie te akcje są uważane za kandydatów, ponieważ obydwie pasują do danych tras.

Gdy `HttpGetAttribute` wykonuje, zobaczy, że *Edycja ()* jest dopasowaniem do *Get* i nie jest dopasowaniem dla żadnego innego zlecenia http. Akcja `Edit(...)` nie ma zdefiniowanych żadnych ograniczeń i dlatego będzie pasować do dowolnego zlecenia HTTP. Tak więc założono, że `Edit(...)` są zgodne tylko `POST`. Jednak w przypadku `GET` obie akcje mogą nadal być zgodne, jednak akcja z `IActionConstraint` jest zawsze uznawana za *lepszą* niż akcja bez. Dlatego `Edit()` ma `[HttpGet]` jest uważany za bardziej szczegółowy i zostanie wybrany, jeśli obie akcje mogą być zgodne.

Koncepcyjnie, `IActionConstraint` jest formą *przeciążania*, ale zamiast przeciążania metod o tej samej nazwie, przeciążanie między akcjami, które pasują do tego samego adresu URL. Funkcja routingu atrybutów używa również `IActionConstraint` i może spowodować, że akcje z różnych kontrolerów są uznawane za kandydatów.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Implementowanie IActionConstraint

Najprostszym sposobem implementacji `IActionConstraint` jest utworzenie klasy pochodzącej od `System.Attribute` i umieszczenie jej na swoich akcjach i kontrolerach. MVC automatycznie odnajdzie wszelkie `IActionConstraint`, które są stosowane jako atrybuty. Możesz użyć modelu aplikacji, aby zastosować ograniczenia i prawdopodobnie jest to najbardziej elastyczne podejście, ponieważ pozwala to na to, w jaki sposób są stosowane.

W poniższym przykładzie ograniczenie wybiera akcję na podstawie *kodu kraju* z danych trasy. [Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

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

Użytkownik jest odpowiedzialny za implementację metody `Accept` i wybranie "Order" dla ograniczenia, które ma zostać wykonane. W takim przypadku Metoda `Accept` zwraca `true`, aby zauważyć, że akcja jest dopasowanie, gdy wartość trasy `country` jest zgodna. Różni się to od `RouteValueAttribute` w tym, że umożliwia powrót do akcji, która nie ma atrybutu. Przykład pokazuje, że jeśli zdefiniujesz akcję `en-US`, kod kraju, taki jak `fr-FR`, powróci do bardziej ogólnego kontrolera, który nie ma `[CountrySpecific(...)]` zastosowania.

Właściwość `Order` decyduje, który *etap* jest częścią tego ograniczenia. Ograniczenia akcji są uruchamiane w grupach na podstawie `Order`. Na przykład wszystkie atrybuty metody HTTP podane przez platformę używają tej samej wartości `Order`, aby były uruchamiane na tym samym etapie. Możesz mieć tyle etapów, ile potrzebujesz, aby zaimplementować odpowiednie zasady.

> [!TIP]
> Aby określić wartość dla `Order` należy wziąć pod uwagę, czy ograniczenie ma być stosowane przed metodami HTTP. Najpierw należy uruchomić mniejsze numery.
