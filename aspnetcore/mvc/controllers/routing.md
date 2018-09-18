---
title: Routing do akcji kontrolera, w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ASP.NET Core MVC używa routingu oprogramowania pośredniczącego do dopasowania adresów URL żądań przychodzących i mapowania ich działania.
ms.author: riande
ms.date: 09/17/2018
uid: mvc/controllers/routing
ms.openlocfilehash: d66c2f14adf55dd0c4a7c3adfad7e5737e4deda1
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011656"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>Routing do akcji kontrolera, w programie ASP.NET Core

Przez [Ryan Nowak](https://github.com/rynowak) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Platforma ASP.NET Core MVC używa routingu [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do dopasowania adresów URL żądań przychodzących i mapowania ich działania. Trasy są definiowane w kod uruchamiający lub atrybutów. Trasy opisano, jak ścieżek URL powinny być dopasowane do akcji. Trasy są również używane do generowania adresów URL (dla linków) wysłane w odpowiedzi. 

Akcje albo są tradycyjnie kierowane lub atrybut kierowany. Wprowadzenie do trasy na kontrolerze lub akcji sprawia, że atrybut kierowany. Zobacz [mieszane routingu](#routing-mixed-ref-label) Aby uzyskać więcej informacji.

W tym dokumencie wyjaśniono, interakcji między MVC i routing i jak typowe upewnij aplikacji MVC korzystać z funkcji routingu. Zobacz [Routing](xref:fundamentals/routing) szczegółowe informacje na temat zaawansowanego routingu.

## <a name="setting-up-routing-middleware"></a>Konfigurowanie routingu oprogramowania pośredniczącego

W swojej *Konfiguruj* metoda może zostać wyświetlony kod podobny do:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Wewnątrz wywołania `UseMvc`, `MapRoute` służy do tworzenia pojedynczej trasy, która będzie nazywamy `default` trasy. Większość aplikacji MVC użyje trasę z szablonem, podobnie jak `default` trasy.

Szablon trasy `"{controller=Home}/{action=Index}/{id?}"` może odnosić się do ścieżki adresu URL, takich jak `/Products/Details/5` , który wyodrębnia wartości trasy `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki. MVC będzie podejmować próby zlokalizowania kontrolera, o nazwie `ProductsController` i uruchomić akcję `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Należy pamiętać, że w tym przykładzie wiązania modelu użyj wartości `id = 5` można ustawić `id` parametr `5` podczas wywoływania tej akcji. Zobacz [powiązań modelu](../models/model-binding.md) Aby uzyskać więcej informacji.

Za pomocą `default` trasy:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Szablon trasy:

* `{controller=Home}` definiuje `Home` jako domyślny `controller`

* `{action=Index}` definiuje `Index` jako domyślny `action`

* `{id?}` definiuje `id` jako opcjonalny

Domyślnie i parametry opcjonalne trasy nie muszą znajdować się w ścieżce adresu URL, pod kątem dopasowania. Zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.

`"{controller=Home}/{action=Index}/{id?}"` może odnosić się do ścieżki adresu URL `/` i będzie generować wartości trasy `{ controller = Home, action = Index }`. Wartości `controller` i `action` należy użyć wartości domyślnych `id` nie zwraca wartości, ponieważ nie odpowiadającym segmencie ścieżki adresu URL. MVC użyje tych wartości trasy, aby wybrać `HomeController` i `Index` akcji:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Przy użyciu tego kontrolera, definicji i szablon trasy `HomeController.Index` będzie można wykonać akcji dla żadnego z następujących ścieżek URL:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Wygodna metoda `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Może służyć do zastąpienia:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` i `UseMvcWithDefaultRoute` dodaje wystąpienie `RouterMiddleware` do potoku oprogramowania pośredniczącego. MVC nie wchodzi w interakcje bezpośrednio za pomocą oprogramowania pośredniczącego i używa routingu w celu obsługi żądań. MVC jest podłączony do tras przy użyciu wystąpienia `MvcRouteHandler`. Kod wewnątrz `UseMvc` jest podobny do następującego:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` bezpośrednio nie definiuje żadnych tras, dodaje do kolekcji tras dla symbolu zastępczego `attribute` trasy. Przeciążenie `UseMvc(Action<IRouteBuilder>)` pozwala dodać własne trasy i obsługuje również trasowanie atrybutów.  `UseMvc` i wszystkie jego odmiany dodaje symbol zastępczy dla tras atrybutów — trasowanie atrybutów jest zawsze dostępna niezależnie od tego, jak skonfigurować `UseMvc`. `UseMvcWithDefaultRoute` Definiuje trasę domyślną i obsługuje routing atrybutów. [Trasowanie atrybutów](#attribute-routing-ref-label) sekcja zawiera szczegółowe informacje na temat trasowanie atrybutów.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Tradycyjnie routing

`default` Trasy:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Oto przykład *tradycyjnie routing*. Nazywamy tego stylu *tradycyjnie routing* ponieważ definiuje on *Konwencji* dla ścieżki adresu URL:

* pierwszy segment ścieżki mapowana na nazwę kontrolera

* drugi mapuje nazwy akcji.

* trzeci segmentu jest używana do opcjonalny `id` służy do mapowania jednostki modelu

Za pomocą tego `default` trasy, URL path `/Products/List` mapuje `ProductsController.List` akcji i `/Blog/Article/17` mapuje `BlogController.Article`. To mapowanie jest oparte na nazwy kontrolera i akcji **tylko** i nie jest oparty na przestrzeni nazw, lokalizacji źródłowych plików lub parametrów metody.

> [!TIP]
> Za pomocą konwencjonalnych routing za pomocą trasy domyślnej pozwala na szybkie tworzenie aplikacji bez konieczności stworzyć nowy wzorzec adresu URL dla każdej akcji, jaką zdefiniujesz. Dla aplikacji z akcjami stylu CRUD mające spójności dla adresów URL w kontrolerach pomaga uprościć kod i utworzyć bardziej przewidywalna interfejs użytkownika.

> [!WARNING]
> `id` Jest definiowany jako opcjonalną przez szablon trasy, co oznacza, że akcje można wykonać bez Identyfikatora podana jako część adresu URL. Zazwyczaj co stanie się po `id` zostanie pominięty z adresu URL jest, że zostanie ustawiony na `0` przez powiązanie modelu, a w rezultacie nie jednostki zostanie znaleziony w dopasowywanie bazy danych `id == 0`. Trasowanie atrybutów daje precyzyjną kontrolę, można wprowadzić identyfikator wymagane w przypadku niektórych działań, a nie dla innych użytkowników. Zgodnie z Konwencją dokumentacja będzie zawierać następujące parametry opcjonalne, takie jak `id` po prawdopodobnie będą się pojawiać w poprawne użycie.

## <a name="multiple-routes"></a>Wiele tras

Można dodać wiele tras wewnątrz `UseMvc` , dodając więcej wywołań `MapRoute`. Dzięki temu można zdefiniować wiele konwencje lub można dodać konwencjonalne tras, które są przeznaczone dla określonej akcji, takich jak:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`blog` Tras w tym miejscu jest *dedykowanych trasy konwencjonalne*, co oznacza, że korzysta z konwencjonalnych systemu routingu, ale jest dedykowany do określonej akcji. Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i związku z tym ta trasa zawsze będzie zmapowana do akcji `BlogController.Article`.

Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, w której są dodawane. Tak, w tym przykładzie `blog` trasy zostaną sprawdzone przed `default` trasy.

> [!NOTE]
> *Trasy są konwencjonalne funkcje w wersji dedykowanej* często używają parametrów wychwytywania trasy, takich jak `{*article}` do przechwytywania pozostała część ścieżki adresu URL. Dzięki temu może być trasę zbyt intensywnie co oznacza, czy jest on zgodny adresów URL, które mają zostać dopasowane przez innych tras. W dalszej części tabeli tras, aby rozwiązać ten problem, należy umieścić trasy "zachłanne".

### <a name="fallback"></a>Rezerwowe

W ramach procesu przetwarzania żądania MVC sprawdzi, czy wartości trasy można znaleźć kontrolerów i akcji w aplikacji. Jeśli wartości trasy nie są zgodne akcję trasy nie zostanie on uznany za zgodny i dalej trasy zostaną sprawdzone. Jest to nazywane *rezerwowego*, a ma ona przeznaczona do uproszczenia przypadków nakładają się konwencjonalne trasy.

### <a name="disambiguating-actions"></a>Usuwanie niejednoznaczności dotyczących działań

Gdy dwie akcje odpowiada za pomocą routingu, MVC muszą rozróżniać do wybierz opcję "Najważniejsze" Release candidate lub — w przeciwnym razie Zgłoś wyjątek. Na przykład:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Ten kontroler definiuje dwie akcje, zgodne ze ścieżką URL `/Products/Edit/17` i kierować dane `{ controller = Products, action = Edit, id = 17 }`. Jest to typowy wzorzec kontrolerów MVC gdzie `Edit(int)` przedstawia formularz produktu, edytować i `Edit(int, Product)` przetwarza przesłanego formularza. Aby to umożliwić MVC będzie trzeba będzie wybrać `Edit(int, Product)` po żądaniu HTTP `POST` i `Edit(int)` po czasownik HTTP jest inny.

`HttpPostAttribute` ( `[HttpPost]` ) To implementacja `IActionConstraint` umożliwiające tylko akcję, należy wybrać, jeśli polecenie HTTP jest żądaniem `POST`. Obecność `IActionConstraint` sprawia, że `Edit(int, Product)` lepiej pasuje niż `Edit(int)`, więc `Edit(int, Product)` zostaną sprawdzone najpierw.

Wystarczy napisać niestandardowy `IActionConstraint` implementacji w specjalistycznych scenariuszy, ale jego zrozumieć rolę atrybutów, takich jak `HttpPostAttribute` -podobne atrybuty są zdefiniowane dla innych poleceń HTTP. W routingu konwencjonalne jest typowe dla działania, aby użyć tej samej nazwy akcji, gdy są one częścią `show form -> submit form` przepływu pracy. Jako udogodnienie tego wzorca staną się bardziej widoczne po zapoznaniu się z [IActionConstraint opis](#understanding-iactionconstraint) sekcji.

Jeśli wiele tras zgodnych z MVC nie można odnaleźć trasy "Najważniejsze", spowoduje zgłoszenie `AmbiguousActionException`.

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>Nazwy tras

Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwy tras:


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Nazwy tras nazwij trasy logiczną tak, aby nazwanej trasy może służyć do generowania adresu URL. Jeśli kolejność trasy może spowodować, że Generowanie adresu URL skomplikowane znacznie upraszcza tworzenie adresu URL. Nazwy tras muszą być unikatowe całej aplikacji.

Nazwy tras nie mają wpływu na adres URL pasujących lub obsługi żądań; służą one wyłącznie do generowania adresu URL. [Routing](xref:fundamentals/routing) zawiera bardziej szczegółowe informacje dotyczące Generowanie adresu URL, w tym Generowanie adresu URL w pomocników specyficzne dla platformy MVC.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Routing atrybutów

Routing atrybutu używa zestaw atrybutów do mapowania akcji bezpośrednio do szablonów tras. W poniższym przykładzie `app.UseMvc();` jest używany w `Configure` metody i żadna trasa jest przekazywany. `HomeController` Pokaże zestaw adresów URL, podobnie jak trasa domyślna `{controller=Home}/{action=Index}/{id?}` umożliwi dopasowanie:

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

`HomeController.Index()` Będzie można wykonać akcji dla wszystkich ścieżek URL `/`, `/Home`, lub `/Home/Index`.

> [!NOTE]
> W tym przykładzie podkreślono programowania Najważniejszą różnicą między atrybut i konwencjonalne routingu. Routing atrybutów wymaga więcej dane wejściowe do określania tras; trasa domyślna konwencjonalne obsługuje tras bardziej zwięzły. Jednak trasowanie atrybutów umożliwia (i wymaga) precyzyjną kontrolę, które szablonów tras dotyczą każdej akcji.

Z routingiem, nazwy kontrolera i akcji, nazwy atrybutów odtwarzania **nie** roli wybrana akcja. W tym przykładzie będzie odpowiadał tych samych adresów URL, jak w poprzednim przykładzie.

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
> Powyższe szablonów tras nie Definiuj parametry trasy `action`, `area`, i `controller`. W rzeczywistości te parametry trasy nie są dozwolone w tras atrybutów. Ponieważ szablon trasy jest już skojarzony z akcją, go nie miałoby sensu można przeanalizować nazwy akcji z adresu URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Atrybut, routing za pomocą atrybutów Http [polecenie]

Routing atrybut może być użycie `Http[Verb]` atrybuty takie jak `HttpPostAttribute`. Wszystkie te atrybuty mogą akceptować szablon trasy. W tym przykładzie przedstawiono dwie akcje, które pasują do tego samego szablonu trasy:

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

Dla ścieżki adresu URL, takich jak `/products` `ProductsApi.ListProducts` będzie można wykonać akcji, jeśli polecenie HTTP jest żądaniem `GET` i `ProductsApi.CreateProduct` zostaną wykonane, jeśli polecenie HTTP jest żądaniem `POST`. Najpierw trasowanie atrybutów zgodny z adresem URL, względem zestaw szablonów trasy zdefiniowane przez atrybuty trasy. Gdy szablon trasy jest zgodny, `IActionConstraint` ograniczenia są stosowane w celu określenia, akcje, które mogą być wykonywane.

> [!TIP]
> Podczas tworzenia interfejsu API REST, jest rzadkie, że można użyć `[Route(...)]` na metody akcji. Zaleca się używać więcej określonych `Http*Verb*Attributes` do dokładnego interfejs API obsługuje. Klienci interfejsów API REST, powinni wiedzieć, ścieżki i zleceń HTTP mapowane na operacje logiczne określone.

Ponieważ trasa atrybutu ma zastosowanie do określonej akcji, jest można łatwo stworzyć parametrów wymaganych w ramach definicji szablonu trasy. W tym przykładzie `id` jest wymagany ze ścieżką URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` Będzie można wykonać akcji dla ścieżki adresu URL, takich jak `/products/3` , ale nie ścieżka adresu URL, takich jak `/products`. Zobacz [Routing](../../fundamentals/routing.md) pełny opis szablonów tras i powiązanych opcji.

## <a name="route-name"></a>Nazwa trasy

Poniższy kod definiuje *nazwy trasy* z `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Nazwy tras może służyć do generowania adresu URL na podstawie określonej trasy. Nazwy tras nie mają wpływu na adres URL pasujące do zachowania routingu i są używane tylko na potrzeby generowania adresu URL. Nazwy tras muszą być unikatowe całej aplikacji.

> [!NOTE]
> Natomiast to za pomocą konwencjonalnych *trasy domyślnej*, która definiuje `id` jako opcjonalny parametr (`{id?}`). Tę możliwość, aby precyzyjnie określić interfejsy API ma zalety, na przykład pozwala `/products` i `/products/5` zostać przekazana do różnych działań.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Łączenie trasy

Aby trasowanie atrybutów mniej powtarzalne, atrybuty trasy na kontrolerze są połączone za pomocą atrybutów trasy na poszczególne akcje. Wszystkie szablony trasy zdefiniowane na kontrolerze jest dołączony do szablonów tras na akcje. Wprowadzenie do atrybutów trasy na kontrolerze sprawia, że **wszystkich** akcji w kontrolerze Użyj trasowanie atrybutów.

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

W tym przykładzie ze ścieżką URL `/products` może odnosić się do `ProductsApi.ListProducts`i Ścieżka adresu URL `/products/5` może odnosić się do `ProductsApi.GetProduct(int)`. Oba te akcje dopasowanie tylko HTTP `GET` ponieważ są one `HttpGetAttribute`.

Kierowanie zastosowanych akcji, które zaczynają się od szablonów `/` nie uzyskać w połączeniu z szablonów tras stosowane do kontrolera. W tym przykładzie dopasowuje zestaw ścieżek URL podobny do *trasy domyślnej*.

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

### <a name="ordering-attribute-routes"></a>Kolejność trasy atrybutu

W przeciwieństwie do konwencjonalnych tras, które są wykonywane w kolejności zdefiniowanej trasowanie atrybutów tworzy drzewa, a jednocześnie dopasowuje wszystkie trasy. To zachowuje się jak — Jeśli wejścia dla trasy zostały umieszczone w idealnym kolejność; bardziej konkretny od pozostałych tras ma możliwość wykonania przed tras ogólniejszych.

Na przykład, takich jak trasy `blog/search/{topic}` jest bardziej szczegółowe niż trasy, takich jak `blog/{*article}`. Logicznie wypowiedzi `blog/search/{topic}` trasy "działa" najpierw domyślnie, ponieważ tylko rozsądne kolejności. Za pomocą konwencjonalnych routingu, deweloper jest odpowiedzialny za wprowadzanie do tras w odpowiedni sposób.

Tras atrybutów można skonfigurować kolejność przy użyciu `Order` właściwości wszystkich atrybutów trasy struktura dostępna. Trasy są przetwarzane zgodnie z rosnącym swoistego `Order` właściwości. Domyślna kolejność to `0`. Konfigurowanie tras za pomocą `Order = -1` będzie wykonywany przed tras, na których nie należy ustawiać zamówienia. Konfigurowanie tras za pomocą `Order = 1` zostanie uruchomiony po określeniu kolejności trasy domyślne.

> [!TIP]
> Należy unikać w zależności od `Order`. Jeśli przestrzeni URL wymaga jawnego kolejności wartości przekierowywany poprawnie, będzie ona wówczas prawdopodobnie mylące dla klientów, jak również. Ogólnie rzecz biorąc trasowanie atrybutów wybierze poprawnej trasy z pasującymi adresu URL. Jeśli domyślna kolejność użyto do generowania adresu URL nie działa, przy użyciu nazwy trasy zastąpienia jest zwykle łatwiejsze niż stosowanie `Order` właściwości.

Strony razor routingu i MVC kontroler routingu udziału wdrożenia. Informacje o kolejność trasy w tematach stron Razor znajduje się w temacie [konwencje tras i aplikacji dla stron Razor: kolejność trasy](xref:razor-pages/razor-pages-conventions#route-order).

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Token zastępczy w szablonach tras ([controller] [action] [obszaru])

Dla wygody, obsługi tras atrybutów *token zastępczy* , umieszczając token w nawiasach kwadratowych (`[`, `]`). Tokeny `[action]`, `[area]`, i `[controller]` zostaną zastąpione wartościami nazwy akcji, nazwy obszaru i nazwy kontrolera, w ramach akcji, której trasa jest zdefiniowana. W tym przykładzie akcji może dopasować ścieżki adresu URL, zgodnie z opisem w komentarzach:

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Zastępowania tokenu jest wykonywane jako ostatni krok w procesie tworzenia tras atrybutów. Powyższy przykład będą zachowywać się taka sama jak poniższy kod:

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Tras atrybutów można również łączyć z dziedziczenia. Jest to szczególnie wydajna, w połączeniu z zastępowania tokenu.

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

Zastępowania tokenu dotyczy także nazwy tras zdefiniowanych przez atrybut trasy. `[Route("[controller]/[action]", Name="[controller]_[action]")]` wygeneruje nazwę trasy unikatowy dla każdej akcji.

Aby dopasować ogranicznik literału zastępowania tokenu `[` lub `]`, zmienić jego znaczenie, powtarzając znak (`[[` lub `]]`).

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Wiele tras

Atrybut routingu obsługuje definiowanie wiele tras, docierających do tego samego działania. Najbardziej typowe użycia tego jest naśladują zachowanie *konwencjonalne trasy domyślnej* jak pokazano w poniższym przykładzie:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Umieszczenie różnych atrybutów trasy na kontrolerze oznacza, że każdy z nich zostaną połączone z każdym z atrybutów trasy na metody akcji.

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

Gdy wiele tras atrybutów (które implementują `IActionConstraint`) są umieszczane w akcji, a następnie każde ograniczenie akcji łączy się z szablonem trasy z atrybutu, który on zdefiniowany.

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
> Podczas przy użyciu wielu tras w akcji może wydawać się zaawansowanych, lepiej jest proste i dobrze zdefiniowanych, zapewnienie przestrzeni adres URL Twojej aplikacji. Użyj wielu tras w akcji, tylko wtedy, gdy jest to wymagane, na przykład do obsługi istniejących klientów.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Określanie parametrów opcjonalnych tras atrybutów, wartości domyślne i ograniczenia

Tras atrybutów obsługuje tej samej składni wbudowanego jako konwencjonalne trasy, aby określić następujące parametry opcjonalne, wartości domyślne i ograniczenia.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Zobacz [odwołanie do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Atrybuty trasy niestandardowej za pomocą `IRouteTemplateProvider`

Wszystkie atrybuty trasy udostępniane w ramach ( `[Route(...)]`, `[HttpGet(...)]` , itp.) implementuje `IRouteTemplateProvider` interfejsu. MVC wyszukuje atrybutów dotyczących kontrolera klas i metod akcji po uruchomieniu aplikacji korzysta z tymi, które implementują `IRouteTemplateProvider` tworzenie początkowej zbiór tras.

Możesz zaimplementować `IRouteTemplateProvider` do definiowania atrybutów trasy. Każdy `IRouteTemplateProvider` można zdefiniować jedną trasę z szablonem trasy niestandardowej zamówienia, a nazwą:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

Ten atrybut z powyższego przykładu automatycznie ustawia `Template` do `"api/[controller]"` podczas `[MyApiController]` jest stosowany.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Dostosowywanie tras atrybutów za pomocą modelu aplikacji

*Model aplikacji* to model obiektów utworzonych podczas uruchamiania za pomocą wszystkich metadanych używane przez MVC do kierowania i wykonywania akcji. *Model aplikacji* obejmuje wszystkie dane zebrane z tras atrybutów (za pośrednictwem `IRouteTemplateProvider`). Można napisać *konwencje* do modyfikowania modelu aplikacji w chwili uruchomienia, aby dostosować sposób działania routingu. W tej sekcji przedstawiono prosty przykład Dostosowywanie routingu za pomocą modelu aplikacji.

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Mieszane routingu: atrybut vs konwencjonalne routingu

Aplikacji MVC można łączyć użytkowania konwencjonalne i atrybut routingu. Jest to zazwyczaj za pomocą konwencjonalnych trasy dla kontrolerów obsługujących stron HTML do przeglądarek, a atrybut routingu dla kontrolerów obsługujących interfejsów API REST.

Akcje albo są tradycyjnie kierowane lub atrybut kierowany. Wprowadzenie do trasy na kontrolerze lub akcji sprawia, że atrybut kierowany. Akcje, które definiują tras atrybutów nie można osiągnąć za pomocą konwencjonalnych trasy i na odwrót. **Wszelkie** atrybut trasy na kontrolerze sprawia, że wszystkie akcje w atrybucie kontrolera kierowany.

> [!NOTE]
> Czym wyróżnia dwa rodzaje systemów routingu polega na stosowane po adres URL jest zgodny szablon trasy. W routingu konwencjonalne wartości tras z dopasowania są używane do wyboru akcji i kontrolera tabeli odnośników wszystkich akcji trasowane konwencjonalnych. W trasowanie atrybutów, każdy szablon jest już skojarzony z akcją, i jest wymagane żadne dalsze wyszukiwanie.

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generowanie adresu URL

Aplikacji MVC można użyć routing firmy funkcjom generowania adresu URL do generowania łączy z adresami URL do akcji. Generowania adresów URL, który eliminuje hardcoding adresów URL, dzięki czemu kod, bardziej niezawodnego i łatwego w utrzymaniu. Ta sekcja koncentruje się na funkcjom generowania adresu URL, które są dostarczane przez MVC i obejmuje tylko podstawowe informacje dotyczące sposobu działania Generowanie adresu URL. Zobacz [Routing](../../fundamentals/routing.md) szczegółowy opis Generowanie adresu URL.

`IUrlHelper` Interfejs jest podstawowy element infrastrukturę między MVC i routing do generowania adresu URL. Można znaleźć wystąpienia `IUrlHelper` dostępne za pośrednictwem `Url` właściwość w kontrolery, widoki i składniki widoków.

W tym przykładzie `IUrlHelper` interfejs jest używany przez `Controller.Url` właściwości do generowania adresu URL, do innej akcji.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Jeśli aplikacja używa konwencjonalne domyślne trasy, wartości `url` zmienna będzie ciąg ścieżki adresu URL `/UrlGeneration/Destination`. Ta ścieżka adresu URL jest tworzony przez routingu, łącząc wartości tras z bieżącego żądania (otoczenia wartości), przy użyciu wartości przekazanych do `Url.Action` i podstawiając te wartości do szablonu trasy:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Każdy parametr trasy w szablonie trasy ma wartość zastąpione pasujące nazwy wartości i wartości otoczenia. Parametru trasy, który nie ma wartości można użyć wartości domyślnej, jeśli ma jedną lub zostać pominięta, jeżeli jest to opcjonalne (podobnie jak w przypadku właściwości `id` w tym przykładzie). Generowanie adresu URL zakończy się niepowodzeniem, jeśli parametr wszelkie wymagane trasy nie ma odpowiedniej wartości. Generowanie adresu URL zakończy się niepowodzeniem dla danej trasy, trasa dalej zostanie podjęta próba dopóki wykonano wszystkie trasy lub nie zostanie znalezione dopasowanie.

Przykład `Url.Action` powyżej zakłada konwencjonalne routingu, ale adres URL generowania działa podobnie trasowanie atrybutów, chociaż przedstawione koncepcje mają różne. Za pomocą konwencjonalnych routingu, wartości trasy są używane do szablonu i wartości trasy dla `controller` i `action` zwykle są wyświetlane w tym szablonie — to działa, ponieważ stosować pasujący do routingu adresów URL *Konwencji*. W atrybucie routingu, wartości trasy `controller` i `action` nie nie mogą znajdować się w szablonie — zamiast tego służą one do wyszukania w wyborze szablonu.

W tym przykładzie użyto trasowanie atrybutów:

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC kompiluje tabelę odnośników wszystkich akcji atrybutu kierowane i pokaże `controller` i `action` wartości, aby wybrać szablon trasy do użycia na potrzeby generowania adresu URL. W powyższym przykładzie `custom/url/to/destination` jest generowany.

### <a name="generating-urls-by-action-name"></a>Generowania adresów URL, nazwy akcji

`Url.Action` (`IUrlHelper` . `Action`) i wszystkie powiązane przeciążenia, które wszystkie są oparte na pomysł, że chcesz określić, co prowadzi Link przez określenie nazwy kontrolera i nazwy akcji.

> [!NOTE]
> Korzystając z `Url.Action`, wartości bieżącej trasy `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` należą do obu *otoczenia wartości* **i** *wartości*. Metoda `Url.Action`, zawsze używa bieżącej wartości `action` i `controller` i wygeneruje Ścieżka adresu URL, który kieruje do bieżącej akcji.

Routing prób na potrzeby wartości w wartościach otoczenia Wypełnij informacje, które nie zostały podane podczas generowania adresu URL. Przy użyciu trasy, takich jak `{a}/{b}/{c}/{d}` wartości otoczenia i `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma za mało informacji do generowania adresu URL bez żadnych dodatkowych wartości — ponieważ kierować wszystkie parametry mają wartość. Jeśli dodano wartość `{ d = Donovan }`, wartość `{ d = David }` będą ignorowane, a będzie wygenerowaną ścieżkę adresu URL `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Adres URL ścieżki są hierarchiczne. W przykładzie przedstawionym powyżej należy usprawniło `{ c = Cheryl }`, obie wartości `{ c = Carol, d = David }` będą ignorowane. W takim przypadku nie mamy już wartość `d` i generowanie adresu URL zakończy się niepowodzeniem. Musisz określić żądaną wartość `c` i `d`.  Można oczekiwać, trafienia problem związany z trasa domyślna (`{controller}/{action}/{id?}`)-, ale rzadko wystąpi ten problem, w praktyce jako `Url.Action` będzie zawsze jawnie określić `controller` i `action` wartość.

Dłużej przeciążenia `Url.Action` skorzystać z dodatkowych *wartości trasy* obiektu do innego niż Podaj wartości dla parametrów trasy `controller` i `action`. Najczęściej zobaczysz następujący używane z `id` takich jak `Url.Action("Buy", "Products", new { id = 17 })`. Zgodnie z Konwencją *wartości trasy* obiektu jest zazwyczaj obiekt typu anonimowego, ale może też być `IDictionary<>` lub *zwykły stary obiekt .NET*. Wszelkie wartości dodatkowe trasy, które nie są zgodne z parametrów trasy są umieszczane w ciągu zapytania.

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Aby utworzyć bezwzględny adres URL, użyj przeciążenia, które akceptuje `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generowania adresów URL przez trasy

Powyższy kod przedstawione wygenerowania adresu URL, przekazując nazwy akcji i kontrolerów. `IUrlHelper` udostępnia również `Url.RouteUrl` rodziny metod. Te metody są podobne do `Url.Action`, ale ich nie Kopiuj bieżące wartości `action` i `controller` wartości trasy. Najbardziej typowe obciążenie jest określenie nazwy trasy na potrzeby generowania adresu URL, zazwyczaj określoną trasę *bez* określający nazwę kontrolera lub akcji.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Generowania adresów URL w formacie HTML

`IHtmlHelper` udostępnia `HtmlHelper` metody `Html.BeginForm` i `Html.ActionLink` do generowania `<form>` i `<a>` elementy odpowiednio. Metody te za pomocą `Url.Action` metodę w celu wygenerowania adresu URL i akceptują podobne argumentów. `Url.RouteUrl` Towarzyszami dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink` które mają podobne funkcje.

TagHelpers generowania adresów URL za pośrednictwem `form` pomocnika tagów i `<a>` pomocnika tagów. Oba te użyj `IUrlHelper` ich wdrażania. Zobacz [Praca z formularzami](../views/working-with-forms.md) Aby uzyskać więcej informacji.

W widokach `IUrlHelper` jest dostępna za pośrednictwem `Url` właściwość dla dowolnego Generowanie adresu URL ad hoc nie pasuje do żadnego z powyższych.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Generowania adresów URL w wynikach akcji

Powyższe przykłady wykazały, za pomocą `IUrlHelper` w kontrolerze, gdy najbardziej typowe obciążenie w kontrolerze jest do generowania adresu URL jako część wyniku akcji.

`ControllerBase` i `Controller` klas bazowych udostępniające podręczne metody dla wyników akcji, odwołujące się do innej akcji. Jeden typowy jest przekierowania po zaakceptowaniu dane wejściowe użytkownika.

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

Metodami factory wyników akcji wykonaj podobny wzorzec do metod na `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Przypadek specjalny dla dedykowanych konwencjonalne tras

Tradycyjnie routing można użyć specjalnego rodzaju definicji trasy o nazwie *dedykowanych trasy konwencjonalne*. W poniższym przykładzie trasy o nazwie `blog` dedykowanych trasy są konwencjonalne funkcje.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Korzystając z tych definicji trasy `Url.Action("Index", "Home")` wygeneruje ze ścieżką URL `/` z `default` trasy, ale Dlaczego? Może być odgadnięcie wartości trasy `{ controller = Home, action = Index }` może być wystarczające do generowania adresu URL przy użyciu `blog`, a wynik byłby `/blog?action=Index&controller=Home`.

Dedykowany trasy konwencjonalne zależą od specjalnego zachowania w wartości domyślne, które nie mają odpowiednich parametru trasy, który uniemożliwia trasy "zbyt zachłannego" za pomocą Generowanie adresu URL. W tym przypadku wartości domyślne są `{ controller = Blog, action = Article }`, a `controller` ani `action` pojawia się jako parametru trasy. Gdy routing wykonuje generowanie adresu URL, podanych wartości muszą być zgodne z wartościami domyślnymi. Za pomocą generowania adresu URL `blog` zakończy się niepowodzeniem, ponieważ wartości `{ controller = Home, action = Index }` nie są zgodne `{ controller = Blog, action = Article }`. Routing następnie powraca do wypróbowania `default`, który zakończy się pomyślnie.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Obszary

[Obszary](areas.md) są funkcją MVC, używane do organizowania powiązanych funkcji do grupy jako osobne routingu — przestrzeń nazw (dla akcji kontrolera) i struktury ich folderów (w przypadku widoków). Za pomocą obszarów umożliwia aplikacji istnieje wiele kontrolerów o takiej samej nazwie — tak długo, jak długo mają różne *obszarów*. Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area` do `controller` i `action`. W tej sekcji omówi, jak routing współdziała z obszarami — zobacz [obszarów](areas.md) szczegółowe informacje na temat używania obszarów za pomocą widoków.

Poniższy przykład umożliwia skonfigurowanie MVC w celu użycia konwencjonalne trasy domyślnej i *trasy obszaru* obszaru o nazwie `Blog`:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Podczas dopasowywania ścieżki adresu URL, takich jak `/Manage/Users/AddUser`, pierwsza trasa będzie generować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`. `area` Wartość trasy jest generowany przez wartość domyślną dla `area`, w rzeczywistości trasy utworzone przez `MapAreaRoute` jest odpowiednikiem następujących czynności:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenie `area` przy użyciu podanej nazwy obszaru na, w tym przypadku `Blog`. Wartość domyślna zapewnia trasy zawsze daje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresu URL.

> [!TIP]
> Tradycyjnie routing jest zależna od kolejności. Ogólnie rzecz biorąc trasy z obszarami należy umieścić we wcześniejszej części tabeli tras, ponieważ są one bardziej szczegółowe niż trasy bez obszaru.

Korzystając z powyższego przykładu, wartości trasy będzie odpowiadać następującej akcji:

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` To, co oznacza, kontroler jako część obszaru, mówi się, że ten kontroler jest w `Blog` obszaru. Kontrolery bez `[Area]` atrybut nie są członkami żadnego obszaru i będzie **nie** pasuje, gdy `area` trasy podaje routingu. W poniższym przykładzie, tylko pierwszy kontroler na liście może odnosić się wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Przestrzeń nazw w każdym kontrolerze przedstawiono tutaj, aby informacje były kompletne — w przeciwnym razie kontrolerów miałby nazewnictwa w konflikcie i generują błędy kompilatora. Klasy w przestrzeni nazw nie mają wpływu na routingu MVC.

Pierwsze dwa kontrolery są elementami członkowskimi obszarów i tylko dopasowania, gdy ich nazwy poszczególnych obszarów są dostarczane przez `area` wartości trasy. Trzeci kontroler nie jest elementem członkowskim dowolnego obszaru i może tylko dopasowania, gdy brak wartości parametru `area` są dostarczane przez routingu.

> [!NOTE]
> Pod względem dopasowania *żadnej wartości*, braku `area` wartość jest taka sama tak, jakby wartość `area` zostały o wartości null ani być ciągiem pustym.

Podczas wykonywania akcji wewnątrz obszaru, trasy wartość `area` będą dostępne jako *otoczenia wartość* routingu służące do generowania adresu URL. Oznacza to, że domyślnie obszarów działania *umocowany* do generowania adresu URL, jak pokazano w następującym przykładzie.

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Opis IActionConstraint

> [!NOTE]
> Ta sekcja jest rozszerzony na elementy wewnętrzne framework i jak MVC wybiera akcję do wykonania. Typowa aplikacja nie ma potrzeby niestandardowego `IActionConstraint`

Prawdopodobnie używano już `IActionConstraint` nawet, jeśli nie znasz przy użyciu interfejsu. `[HttpGet]` Atrybutu i podobnych zastosowaniach `[Http-VERB]` implementację interfejsu `IActionConstraint` w celu ograniczenia wykonywania metody akcji.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Zakładając, że domyślną trasę konwencjonalne ze ścieżką URL `/Products/Edit` dałby w efekcie wartości `{ controller = Products, action = Edit }`, który umożliwi dopasowanie **zarówno** akcji, pokazano poniżej. W `IActionConstraint` terminologii będzie mówi się, że oba te akcje są traktowane jako kandydatów — jak były ze sobą zgodne dane trasy.

Gdy `HttpGetAttribute` jest wykonywana, jest wyświetlany tekst, który *Edit()* pasuje do *UZYSKAĆ* i nie ma dopasowania dla innych zlecenie HTTP. `Edit(...)` Akcji nie ma żadnych ograniczeń zdefiniowanych i dlatego pokaże wszelkie zlecenie HTTP. Tak zakładając, że `POST` — tylko `Edit(...)` zgodny. Jednak dla `GET` obie akcje można nadal dopasować — jednak akcję z `IActionConstraint` zawsze jest uważany za *lepsze* niż akcję bez. Dlatego ponieważ `Edit()` ma `[HttpGet]` jest uważany za bardziej szczegółowe i zostanie wybrany, jeżeli może odnosić się do obu akcji.

Model `IActionConstraint` jest formą *przeciążenie*, ale zamiast przeciążenia metod o tej samej nazwie, jest przeciążenie między akcjami, które pasują do tego samego adresu URL. Routing atrybutu używa również `IActionConstraint` i może doprowadzić do działania z różnych kontrolerów oba są traktowane jako kandydatów.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Implementowanie IActionConstraint

Najprostszym sposobem wdrożenia `IActionConstraint` polega na utworzeniu klasy pochodzącej od `System.Attribute` i umieść ją w akcji i kontrolerów. MVC automatycznie wykryje, dowolny `IActionConstraint` które są stosowane jako atrybuty. Model aplikacji można użyć, aby zastosować ograniczenia i jest to prawdopodobnie najbardziej elastycznym podejściem, ponieważ umożliwia ona metaprogram, w jaki sposób są one stosowane.

W poniższym przykładzie ograniczenie wybiera akcję na podstawie *numer kierunkowy kraju* z danych trasy. [Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

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

Odpowiedzialność za wdrażanie `Accept` metody i wybierając polecenie "Order" ograniczenia do wykonania. W tym przypadku `Accept` metoda zwraca `true` do oznaczania Akcja odnosi się do dopasowania podczas `country` trasy do dopasowania wartości. To różni się od `RouteValueAttribute` , ponieważ umożliwia powrót do działania z bezatrybutowego. Przykład pokazuje, że jeśli zdefiniujesz `en-US` akcji, a następnie kod kraju, takich jak `fr-FR` powróci do kontrolera bardziej ogólnym, który nie ma `[CountrySpecific(...)]` stosowane.

`Order` Właściwość decyduje, które *etapu* ograniczenie jest częścią. Ograniczenia akcji uruchamiania w grupach na podstawie `Order`. Na przykład Framework podane atrybuty metody HTTP używać tego samego `Order` wartość, aby były one uruchamiane w tym samym etapie. Może mieć dowolną liczbę etapie, ponieważ musisz zaimplementować żądane zasady.

> [!TIP]
> Aby wybrać wartość `Order` Pomyśl o czy swoje ograniczenia powinny być stosowane przed metod HTTP. Niższych numerach uruchomiony jako pierwszy.
