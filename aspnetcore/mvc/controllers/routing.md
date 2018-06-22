---
title: Routing do akcji kontrolera w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak platformy ASP.NET Core MVC używa routingu oprogramowanie pośredniczące umożliwia dopasowanie adresów URL żądań przychodzących i zamapowania ich do akcji.
ms.author: riande
ms.date: 03/14/2017
uid: mvc/controllers/routing
ms.openlocfilehash: 795ca13674f85b6e8c1f84718c225613a7a7125a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278414"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>Routing do akcji kontrolera w ASP.NET Core

Przez [Ryan Nowak](https://github.com/rynowak) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Routing korzysta z platformy ASP.NET Core MVC [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) zgodne adresy URL żądań przychodzących i zamapowania ich do akcji. Trasy są definiowane w kod uruchomienia lub atrybutów. Trasy opisano, jak powinny być zgodne ścieżki adresu URL do akcji. Trasy są również używane do generowania adresów URL (w przypadku połączeń) wysłała w odpowiedzi. 

Akcje albo są kierowane tradycyjnie ani trasowane atrybutu. Umożliwia wprowadzania trasę do kontrolera lub akcji atrybutu routingu. Zobacz [mieszanym routingu](#routing-mixed-ref-label) Aby uzyskać więcej informacji.

Ten dokument zostanie opisano interakcje między MVC i routing i jak typowe upewnij aplikacji MVC korzystanie z funkcji routingu. Zobacz [Routing](xref:fundamentals/routing) szczegółowe informacje na temat zaawansowanego routingu.

## <a name="setting-up-routing-middleware"></a>Konfigurowanie routingu oprogramowania pośredniczącego

W Twojej *Konfiguruj* — metoda może wyświetlić kodu podobne do:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Wewnątrz wywołania `UseMvc`, `MapRoute` służy do tworzenia jedną trasę, która będzie nazywamy `default` trasy. Większość aplikacji MVC użyje trasy przy użyciu szablonu, podobnie jak `default` trasy.

Szablon trasy `"{controller=Home}/{action=Index}/{id?}"` może dopasować Ścieżka adresu URL, takie jak `/Products/Details/5` , który wyodrębnia wartości trasy `{ controller = Products, action = Details, id = 5 }` przez tokenizing ścieżki. MVC będzie podejmować próby zlokalizowania kontrolerze o nazwie `ProductsController` i Uruchom akcję `Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

Pamiętaj, że w tym przykładzie wiązania modelu używać wartości `id = 5` można ustawić `id` parametr `5` podczas wywoływania tej akcji. Zobacz [powiązanie modelu](../models/model-binding.md) więcej szczegółów.

Przy użyciu `default` trasy:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Szablon trasy:

* `{controller=Home}` definiuje `Home` jako domyślny `controller`

* `{action=Index}` definiuje `Index` jako domyślny `action`

* `{id?}` definiuje `id` jako opcjonalna

Parametry opcjonalne trasy i domyślne nie musi być obecny w ścieżkę adresu URL, pod kątem dopasowania. Zobacz [odwołania do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.

`"{controller=Home}/{action=Index}/{id?}"` można dopasować ścieżkę adresu URL `/` i spowoduje wartości trasy `{ controller = Home, action = Index }`. Wartości `controller` i `action` należy użyć wartości domyślnych `id` nie tworzy wartości, ponieważ nie nie odpowiadającym segmencie ścieżki adresu URL. MVC użyje tych wartości trasy, aby wybrać `HomeController` i `Index` akcji:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Przy użyciu tej definicji kontrolera i szablon trasy `HomeController.Index` będzie można wykonać akcji dla wszystkich następujących ścieżkach adresu URL:

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

Metoda wygody `UseMvcWithDefaultRoute`:

```csharp
app.UseMvcWithDefaultRoute();
```

Może być używany do zastąpienia:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` i `UseMvcWithDefaultRoute` dodać wystąpienia `RouterMiddleware` do potoku oprogramowania pośredniczącego. MVC nie bezpośrednią interakcję z oprogramowaniem pośredniczącym i używa routingu do obsługi żądań. MVC jest połączona z tras za pomocą wystąpienia `MvcRouteHandler`. Kod wewnątrz `UseMvc` jest podobny do następującego:

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` bezpośrednio nie definiuje żadnych tras dodaje symbolu zastępczego do kolekcji tras dla `attribute` trasy. Przeciążenie `UseMvc(Action<IRouteBuilder>)` umożliwia dodanie własnych tras i obsługuje również trasami atrybutów.  `UseMvc` i wszystkie jego zmian dodaje symbol zastępczy dla tras atrybutów — trasami atrybutów są zawsze dostępne, niezależnie od sposobu skonfigurowania `UseMvc`. `UseMvcWithDefaultRoute` definiuje domyślną trasę i obsługuje routing atrybutu. [Trasami atrybutów](#attribute-routing-ref-label) sekcja zawiera więcej informacji na temat trasami atrybutów.

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>Konwencjonalne routingu

`default` Trasy:

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Przykładem jest *routingu z konwencjonalnej*. Nazywamy ten styl *routingu z konwencjonalnej* ponieważ definiuje on *Konwencji* dla ścieżki adresu URL:

* pierwszy segment ścieżki mapuje nazwę kontrolera

* drugi mapuje nazwę akcji.

* trzeci segmentu jest używana do opcjonalny `id` służy do mapowania do modelu jednostki

Za pomocą tej `default` trasy, ścieżkę adresu URL `/Products/List` mapuje `ProductsController.List` akcji, i `/Blog/Article/17` mapuje `BlogController.Article`. To mapowanie opiera się na nazwy kontrolera i akcji **tylko** i nie jest oparte na przestrzeni nazw, lokalizacji źródłowych plików lub parametry metody.

> [!TIP]
> Korzystania z konwencjonalnej routingu z trasy domyślnej umożliwia szybkie utworzenie aplikacji bez konieczności stworzyć nowy wzorzec adres URL dla każdej akcji, którą należy zdefiniować. Dla aplikacji z akcjami CRUD styl o spójności dla adresów URL w kontrolerach może pomóc uprościć kod i wprowadzić bardziej przewidywalne interfejsu użytkownika.

> [!WARNING]
> `id` Jest zdefiniowany jako opcjonalne w szablonie trasy, co oznacza, że czynności użytkownika można wykonać bez Identyfikatora dostarczane jako część adresu URL. Zazwyczaj co się stanie, jeśli `id` pominięto w adresie URL jest, że zostanie ustawiona do `0` przez powiązanie modelu, a w rezultacie jednostki nie zostaną znalezione w odpowiadającym bazy danych `id == 0`. Atrybut routingu można zapewniają precyzyjną kontrolę, aby identyfikator wymagane w przypadku niektórych działań, a nie do innych użytkowników. Konwencja dokumentacja będzie zawierać następujące parametry opcjonalne, takich jak `id` po prawdopodobnie będą się pojawiać w odpowiednie użycie.

## <a name="multiple-routes"></a>Wiele tras

Można dodać wiele tras wewnątrz `UseMvc` przez dodanie kolejnych wywołań do `MapRoute`. Dzięki temu można zdefiniować wiele konwencje lub dodać z konwencjonalnej tras, które są przeznaczone dla określonej akcji, takich jak:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`blog` Tutaj trasa jest *dedykowanych trasy z konwencjonalnej*, co oznacza, że korzysta z konwencjonalnej systemu routingu, ale jest dedykowany do określonej akcji. Ponieważ `controller` i `action` nie są wyświetlane w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i w związku z tym zawsze przypisze tej trasy do akcji `BlogController.Article`.

Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, w których są one dodawane. Tak, w tym przykładzie `blog` trasy zostaną sprawdzone przed `default` trasy.

> [!NOTE]
> *Dedykowane tras z konwencjonalnej* często używają trasy wychwytywania parametry, takie jak `{*article}` do przechwytywania pozostała część ścieżki adresu URL. Może być zbyt intensywnie trasę co oznacza, że jest on zgodny adresów URL, które mają być dopasowywane przez innych tras. Później w tabeli tras, aby rozwiązać ten problem, należy umieścić trasy "zachłannego".

### <a name="fallback"></a>Powrotu

W ramach procesu przetwarzania żądania MVC zweryfikuje, że wartości trasy może służyć do znajdowania kontrolera i akcji w aplikacji. Jeśli akcja wartości trasy nie są zgodne. następnie trasy nie jest uznawane za zgodne i dalej trasy zostaną sprawdzone. Ta metoda jest wywoływana *rezerwowy*, i jest on przeznaczony do uprościć przypadków nakładają się tras z konwencjonalnej.

### <a name="disambiguating-actions"></a>Akcje usuwania niejednoznaczności w nazwach

Gdy dwie akcje odpowiada za pośrednictwem routingu, MVC musi odróżniania do wybierz "Najważniejsze" kandydata w przeciwnym razie Zgłoś wyjątek. Na przykład:

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

Ten kontroler definiuje dwie akcje zgodne ścieżkę adresu URL `/Products/Edit/17` i przekazywanie danych `{ controller = Products, action = Edit, id = 17 }`. Jest to typowy wzorzec kontrolerów MVC gdzie `Edit(int)` przedstawia formularz służący do edytowania produktu, i `Edit(int, Product)` przetwarza przesłanego formularza. Aby było to możliwe należy wybrać MVC `Edit(int, Product)` po żądaniu HTTP `POST` i `Edit(int)` po zlecenie HTTP jest inny.

`HttpPostAttribute` ( `[HttpPost]` ) To implementacja `IActionConstraint` umożliwiające tylko akcję, należy wybrać, jeśli polecenie HTTP jest żądaniem `POST`. Obecność `IActionConstraint` sprawia, że `Edit(int, Product)` lepiej pasuje niż `Edit(int)`, więc `Edit(int, Product)` zostaną sprawdzone najpierw.

Wystarczy do tworzenia niestandardowych `IActionConstraint` implementacje w scenariuszach specjalne, ale jego zrozumieć rolę atrybutów, takich jak `HttpPostAttribute` -podobne atrybuty są definiowane dla innych zleceń HTTP. W routingu z konwencjonalnej jest typowe dla akcji używać tej samej nazwy akcji, gdy są one częścią `show form -> submit form` przepływu pracy. Wygodę tego wzorca staną się bardziej widoczne po przejrzeniu [IActionConstraint opis](#understanding-iactionconstraint) sekcji.

Jeśli wiele tras zgodne i MVC nie można znaleźć "najlepsze" trasy, zgłosi `AmbiguousActionException`.

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

Nazwy tras udostępniania trasy nazwę logiczną, aby nazwanej trasy może służyć do generowania adresu URL. Podczas generowania adresu URL skomplikowane kolejność trasy może spowodować znacznie ułatwia tworzenie adresu URL. Nazwy tras muszą być unikatowe całej aplikacji.

Nazwy tras nie mają wpływu na adres URL dopasowania lub obsługiwanie żądań; są one używane tylko do generowania adresu URL. [Routing](xref:fundamentals/routing) przedstawiono bardziej szczegółowe informacje o tym generowania adresu URL w pomocników specyficzne dla platformy MVC generowania adresu URL.

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>Atrybut routingu

Atrybut routingu korzysta z zestawu atrybutów mapowania akcje bezpośrednio na szablonów tras. W poniższym przykładzie `app.UseMvc();` jest używany w `Configure` — metoda i trasy jest przekazywana. `HomeController` Będzie zgodny zestaw adresów URL podobny do trasy domyślnej `{controller=Home}/{action=Index}/{id?}` spełniają kryteria:

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

`HomeController.Index()` Będzie można wykonać akcji dla ścieżki adresu URL `/`, `/Home`, lub `/Home/Index`.

> [!NOTE]
> W tym przykładzie wyróżniono klucza programowania różnicę między atrybutu i konwencjonalnych routingu. Routing atrybut wymaga więcej dane wejściowe, aby określić trasy; trasy domyślnej z konwencjonalnej obsługuje więcej krótkiej formie trasy. Jednak trasami atrybutów umożliwia (i wymaga) precyzyjne dotyczą z których każda akcja szablonów tras.

Za pomocą atrybutu routingu, nazwy kontrolera i nazwy akcji odtwarzać **nie** roli wybrana akcja. W tym przykładzie będzie pasował do tych samych adresów URL co w poprzednim przykładzie.

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
> Powyżej szablonów tras nie definiują parametry trasy dla `action`, `area`, i `controller`. W rzeczywistości te parametry trasy nie są dozwolone w tras atrybutów. Ponieważ szablon trasy jest już skojarzony z akcją, go nie ma sensu można przeanalizować nazwy akcji z adresu URL.

## <a name="attribute-routing-with-httpverb-attributes"></a>Atrybut routingu z atrybutami Http [zlecenie]

Atrybut routingu, można także wprowadzić użycie `Http[Verb]` atrybutów, takich jak `HttpPostAttribute`. Wszystkie te atrybuty mogą akceptować szablonu trasy. W tym przykładzie przedstawiono dwie akcje, które pasuje do tego samego szablonu trasy:

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

Dla ścieżki adresu URL, takie jak `/products` `ProductsApi.ListProducts` będzie można wykonać akcji, gdy jest zlecenie HTTP `GET` i `ProductsApi.CreateProduct` zostaną wykonane, jeśli polecenie HTTP jest żądaniem `POST`. Atrybut routingu najpierw odpowiada adresowi URL zestawem szablony trasy zdefiniowane przez atrybuty trasy. Po zgodny szablon trasy, `IActionConstraint` ograniczenia są stosowane w celu określenia, jakie akcje mogą być wykonywane.

> [!TIP]
> Podczas konstruowania interfejsu API REST, jest rzadko, że można użyć `[Route(...)]` na metody akcji. Lepiej użyć konkretnego więcej `Http*Verb*Attributes` do dokładnego obsługuje interfejs API. Klienci interfejsów API REST powinni znać ścieżki i zleceń HTTP mapowane na operacje logiczne określone.

Ponieważ trasa atrybutu ma zastosowanie do określonej akcji, jest proste parametrów wymaganych w ramach definicji szablonu trasy. W tym przykładzie `id` jest wymagana ścieżka adresu URL.

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` Będzie można wykonać akcji dla ścieżki adresu URL, takie jak `/products/3` , ale nie ścieżka adresu URL, takie jak `/products`. Zobacz [Routing](../../fundamentals/routing.md) pełen opis szablonów tras i powiązane opcje.

## <a name="route-name"></a>Nazwa trasy

Poniższy kod definiuje *nazwy trasy* z `Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

Nazwy trasy mogą służyć do generowania adresu URL na podstawie określonej trasy. Nazwy trasy nie mają wpływu na adres URL dopasowania zachowanie routingu i służą tylko do generowania adresu URL. Nazwy tras muszą być unikatowe całej aplikacji.

> [!NOTE]
> Natomiast to z konwencjonalnej *trasy domyślnej*, który definiuje `id` parametr jako opcjonalny (`{id?}`). Tę możliwość, aby precyzyjnie określić interfejsy API ma zalet, na przykład pozwala `/products` i `/products/5` zostać przekazana do różnych działań.

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>Łączenie tras

Aby trasami atrybutów mniej powtarzane, atrybutów trasy dla kontrolera są łączone z atrybutów trasy dla poszczególnych działań. Wszystkie szablony trasy zdefiniowane na kontrolerze jest dołączany na początku w szablonach tras na akcje. Umieszczenie atrybut trasy na kontrolerze sprawia, że **wszystkie** akcji w kontrolerze korzystać z routingu atrybutu.

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

W tym przykładzie ścieżkę adresu URL `/products` może dopasować `ProductsApi.ListProducts`i Ścieżka adresu URL `/products/5` może dopasować `ProductsApi.GetProduct(int)`. Oba te akcje dopasowanie tylko HTTP `GET` ponieważ są one oznaczone `HttpGetAttribute`.

Trasy szablony zastosować do akcji, które rozpoczynają się od `/` nie łączone z szablonami trasy stosowane do kontrolera. W tym przykładzie dopasowuje zestaw ścieżki adresu URL podobny do *trasy domyślnej*.

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

### <a name="ordering-attribute-routes"></a>Porządkowanie tras atrybutów

W przeciwieństwie do konwencjonalnych tras, których wykonywany w kolejności zdefiniowanej trasami atrybutów buduje drzewo i jednocześnie dopasowuje wszystkie trasy. Zachowuje się jak — Jeśli wejścia dla trasy zostały umieszczone w kolejności idealne; specyficzny tras ma możliwość wykonania przed bardziej ogólne trasy.

Na przykład trasy, takich jak `blog/search/{topic}` jest bardziej szczegółowy niż trasy, takich jak `blog/{*article}`. Mówiąc logicznie `blog/search/{topic}` trasy "", domyślnie uruchamia, ponieważ jest to tylko za pośrednictwem porządkowania. Korzystania z konwencjonalnej routingu, dewelopera jest odpowiedzialny za umieszczenie trasy w odpowiedni sposób.

Tras atrybutów można skonfigurować kolejność przy użyciu `Order` właściwości wszystkich atrybutów trasy struktura dostępna. Trasy są przetwarzane zgodnie z rosnącym sortowania `Order` właściwości. Domyślna kolejność to `0`. Konfigurowanie tras za pomocą `Order = -1` będzie wykonywany przed tras, na których nie należy ustawiać zamówienia. Konfigurowanie tras za pomocą `Order = 1` zostanie uruchomiony po określeniu kolejności trasy domyślnej.

> [!TIP]
> Unikaj w zależności od `Order`. Jeśli adres URL obszaru wymaga wartości jawne kolejność trasy poprawnie, jest prawdopodobnie mylące również klientom. Ogólnie rzecz biorąc trasami atrybutów wybierze poprawnej trasy z pasującymi adresu URL. Jeśli domyślna kolejność używany do generowania adresu URL nie działa, używając nazwy trasy zastąpienia jest zazwyczaj łatwiejsze niż w przypadku stosowania `Order` właściwości.

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Token zastępczy w szablonach tras ([kontrolera], [action] [obszaru])

Dla wygody, obsługi tras atrybutów *token zastępczy* umieszczając tokenu w nawiasy kwadratowe (`[`, `]`). Tokeny `[action]`, `[area]`, i `[controller]` zostaną zastąpione wartościami nazwy akcji, nazwy obszaru i nazwy kontrolera z akcji, których trasa jest zdefiniowana. W tym przykładzie akcje może dopasować ścieżki adresu URL, zgodnie z opisem w komentarzach:

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Token zastępczy występuje jako ostatni etap tworzenia tras atrybutów. Powyższy przykład będą zachowywać się taka sama jak następujący kod:

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

Tras atrybutów można również łączyć z dziedziczenia. Jest to szczególnie wydajna, połączeniu z zastępujący tokenu.

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

Token zastępczy ma również zastosowanie do nazwy trasy zdefiniowane przez atrybut trasy. `[Route("[controller]/[action]", Name="[controller]_[action]")]` wygeneruje nazwę unikatową trasę dla każdej akcji.

Aby dopasować ogranicznik literału zastępujący tokenu `[` lub `]`, sekwencji ucieczki powtarzając znak (`[[` lub `]]`).

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>Wiele tras

Atrybut Obsługa routingu definiowania wiele tras, które dostarczone do tego samego działania. Użycie najbardziej typowe ma naśladować zachowanie *konwencjonalnych trasy domyślnej* jak pokazano w poniższym przykładzie:

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

Wprowadzenie różnych atrybutów trasy na kontrolerze oznacza, że każda z nich zostaną połączone z każdym z atrybutów trasy dla metody akcji.

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

Gdy różnych atrybutów trasy (które implementują `IActionConstraint`) są umieszczane w akcji, wówczas każde ograniczenie akcji łączy przy użyciu szablonu trasy z atrybut, który jest określony.

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
> Podczas za pomocą wiele tras na akcje może wydawać się zaawansowanych, lepiej jest proste i dobrze zdefiniowany aktualizowania miejsca adres URL aplikacji. Użyj wiele tras na akcje tylko wtedy, gdy jest to wymagane, na przykład do obsługi istniejących klientów.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Określenie parametrów opcjonalny atrybut trasy, wartości domyślne i ograniczenia

Tras atrybutów obsługuje takiej samej składni wbudowanego jako konwencjonalnej trasy, aby określić następujące parametry opcjonalne, wartości domyślnych i ograniczeń.

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

Zobacz [odwołania do szablonu trasy](../../fundamentals/routing.md#route-template-reference) szczegółowy opis składni szablonu trasy.

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Atrybuty niestandardowe trasy za pomocą `IRouteTemplateProvider`

Wszystkie atrybuty trasy w ramach ( `[Route(...)]`, `[HttpGet(...)]` , itd.) implementuje `IRouteTemplateProvider` interfejsu. MVC wyszukuje atrybuty klasy kontrolera i metody akcji uruchamiania i używa tych, które implementują aplikacji `IRouteTemplateProvider` do utworzenia wstępnego zestawu trasy.

Można zaimplementować `IRouteTemplateProvider` do zdefiniowania własnych atrybuty trasy. Każdy `IRouteTemplateProvider` można zdefiniować jedną trasę z szablonem tras niestandardowych kolejność i nazwy:

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

W powyższym przykładzie atrybut automatycznie ustawia `Template` do `"api/[controller]"` podczas `[MyApiController]` została zastosowana.

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>Dostosowywanie tras atrybutów za pomocą modelu aplikacji

*Model aplikacji* jest model obiektowy tworzone przy uruchamianiu ze wszystkimi metadane używane przez MVC do routingu i wykonaj czynności użytkownika. *Model aplikacji* obejmuje wszystkie dane zebrane z atrybutów trasy (za pośrednictwem `IRouteTemplateProvider`). Można napisać *konwencje* można zmodyfikować modelu aplikacji w czasie uruchamiania, aby dostosować zachowanie routingu. W tej sekcji przedstawiono prosty przykład Dostosowywanie routingu za pomocą modelu aplikacji.

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Mieszane routingu: atrybut w porównaniu z konwencjonalnej routingu

Aplikacji programu MVC, można mieszać korzystanie z konwencjonalnej i atrybut routingu. Jest to typowe tras z konwencjonalnej kontrolerów obsługujących strony HTML do przeglądarek, a atrybut routingu dla kontrolerów obsługujących interfejsów API REST.

Akcje albo są kierowane tradycyjnie ani trasowane atrybutu. Umożliwia wprowadzania trasę do kontrolera lub akcji atrybutu routingu. Akcje, które definiują tras atrybutów nie można osiągnąć za pomocą trasy z konwencjonalnej i na odwrót. **Wszelkie** atrybut trasy na kontrolerze powoduje, że wszystkie akcje w atrybucie kontrolera routingu.

> [!NOTE]
> Co odróżnia dwa typy systemów routingu jest procesem zastosowane po adres URL jest zgodna z szablonem trasy. W routingu z konwencjonalnej, wartości trasy ze dopasowania służą do tabeli odnośników wszystkich akcji routingiem z konwencjonalnej wyboru tej akcji i kontrolera. W atrybucie routingu, każdego szablonu jest już skojarzony z akcją i niezbędne jest Brak dalszych wyszukiwania.

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generowania adresu URL

MVC aplikacje mogą używać routingu w funkcji generowania adresu URL do generowania łączy URL akcji. Generowania adresów URL eliminuje hardcoding adresów URL, wprowadzenie kodu bardziej niezawodny i łatwy w obsłudze. W tej sekcji koncentruje się na funkcje generowania adresu URL, które są oferowane przez MVC i obejmuje tylko podstawowe informacje dotyczące działania generowania adresu URL. Zobacz [Routing](../../fundamentals/routing.md) szczegółowy opis generowania adresu URL.

`IUrlHelper` Interfejsu jest podstawowej infrastruktury między MVC i routing do generowania adresu URL. Można znaleźć wystąpienia `IUrlHelper` dostępne za pośrednictwem `Url` właściwości kontrolerów, widoków i składniki w widoku.

W tym przykładzie `IUrlHelper` interfejs jest używany przez `Controller.Url` właściwości do generowania adresu URL do innej akcji.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Jeśli aplikacja korzysta z konwencjonalnej domyślne trasy, wartości `url` zmienna będzie ciąg ścieżki adresu URL `/UrlGeneration/Destination`. Ta ścieżka adresu URL jest tworzony przez routingu przez połączenie wartości tras z bieżącego żądania (otoczenia wartości) z wartości przekazanych do `Url.Action` i podstawianie tych wartości do szablonu trasy:

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Każdy parametr trasy w szablonie trasy ma wartość zastąpione nazw zgodną z wartości i wartości otoczenia. Parametru trasy, który nie ma wartości można użyć wartości domyślnej, jeśli zawiera co najmniej pominięte, jeśli jest to pozycja opcjonalna (tak jak w przypadku liczby `id` w tym przykładzie). Generowania adresu URL zakończy się niepowodzeniem, jeśli parametr wszelkie wymagane trasy nie ma odpowiadającej jej wartości. Jeśli generowania adresu URL trasy nie powiedzie się, dalej trasy zostanie podjęta próba dopóki wszystkie trasy zostały wypróbowane lub Znaleziono dopasowanie.

Przykład `Url.Action` powyżej zakłada routingu z konwencjonalnej, ale adres URL generowania działa podobnie trasami atrybutów, chociaż przedstawione koncepcje mają różne. Z konwencjonalnej routingiem, wartości trasy są używane do rozwiń szablonu i wartości trasy dla `controller` i `action` zwykle pojawiają się w tym szablonie — to działa, ponieważ odpowiednia adresy URL są dopasowane wg routingu *Konwencji*. W atrybucie routingu, wartości trasy `controller` i `action` nie mogą występować w szablonie — zamiast tego są używane do wyszukiwania jaki szablon zostanie użyty.

W tym przykładzie użyto trasami atrybutów:

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC tworzy tabelę odnośników wszystkich akcji atrybutu kierowane i będzie odpowiadała `controller` i `action` wartości, aby wybrać szablon trasy do użycia podczas generowania adresu URL. W powyższym przykładowym `custom/url/to/destination` jest generowany.

### <a name="generating-urls-by-action-name"></a>Generowania adresów URL według nazwy akcji

`Url.Action` (`IUrlHelper` . `Action`) i wszystkich powiązanych przeciążenia wszystkie są oparte na pomysł, że chcesz określić, jakie łączysz przez określenie nazwy kontrolera i nazwy akcji.

> [!NOTE]
> Korzystając z `Url.Action`, wartości bieżącej trasy `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` są częścią zarówno *otoczenia wartości* **i** *wartości*. Metoda `Url.Action`, zawsze używa bieżącej wartości `action` i `controller` i wygeneruje ścieżki adresu URL, który kieruje do bieżącej akcji.

Routing próbował wartości w otaczającym wartości Wypełnij informacje nie zostały podane podczas generowania adresu URL. Za pomocą trasy, takich jak `{a}/{b}/{c}/{d}` i wartości otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma za mało informacji do generowania adresu URL bez żadnych dodatkowych wartości — ponieważ rozesłać wszystkie parametry mają wartość. Jeśli dodano wartość `{ d = Donovan }`, wartość `{ d = David }` zostałyby one zignorowane, a będzie wygenerowany ścieżkę adresu URL `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Adres URL ścieżki są hierarchiczne. W przykładzie przedstawionym powyżej należy dodać wartość `{ c = Cheryl }`, wartość `{ c = Carol, d = David }` zostałyby one zignorowane. W takim przypadku nie mamy już wartość `d` i generowania adresu URL zakończy się niepowodzeniem. Musisz określić żądaną wartość `c` i `d`.  Można oczekiwać trafień tego problemu z trasa domyślna (`{controller}/{action}/{id?}`)-, ale rzadko wystąpi ten problem, w praktyce jako `Url.Action` będzie zawsze jawnie określać `controller` i `action` wartość.

Dłużej przeciążeń `Url.Action` również podjęcia dodatkowych *wartości trasy* obiekt do innego niż Podaj wartości dla parametrów trasy `controller` i `action`. Zobaczysz najczęściej to używane z `id` jak `Url.Action("Buy", "Products", new { id = 17 })`. Konwencja *wartości trasy* obiekt jest zwykle typu anonimowego, ale można go również `IDictionary<>` lub *zwykły stary obiekt .NET*. Wartości dodatkowe trasy, które nie odpowiadają parametrów trasy są umieszczane w ciągu zapytania.

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> Aby utworzyć bezwzględny adres URL, użyj przeciążenia, które akceptuje `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generowania adresów URL przez trasy

Powyższy kod przedstawiona wygenerowania adresu URL, przekazując nazwy kontrolera i akcji. `IUrlHelper` dostępne są także `Url.RouteUrl` rodziny metod. Te metody są podobne do `Url.Action`, ale ich nie należy kopiować bieżące wartości `action` i `controller` wartości trasy. Najbardziej typowe obciążenie jest określenie nazwy trasy na potrzeby generowania adresu URL, zazwyczaj określoną trasę *bez* określania nazwy kontrolera lub akcji.

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>Generowania adresów URL w formacie HTML

`IHtmlHelper` udostępnia `HtmlHelper` metody `Html.BeginForm` i `Html.ActionLink` do generowania `<form>` i `<a>` elementy odpowiednio. Użyj tych metod `Url.Action` do generowania adresu URL i akceptują argumentów podobne. `Url.RouteUrl` Towarzyszami dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink` zawierające podobne funkcje.

TagHelpers generowania adresów URL za pośrednictwem `form` pomocnika tagów i `<a>` pomocnika tagów. Oba te użyj `IUrlHelper` ich wdrażania. Zobacz [pracy z formularzami](../views/working-with-forms.md) Aby uzyskać więcej informacji.

W widokach `IUrlHelper` jest dostępna za pośrednictwem `Url` właściwości dla dowolnego ad hoc generowania adresu URL nie pasuje do żadnego z powyższych.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>Generowania adresów URL w wynikach akcji

Powyższe przykłady wykazały, przy użyciu `IUrlHelper` w kontrolerze podczas generowania adresu URL jako część wynik akcji najbardziej typowe obciążenie w kontrolerze.

`ControllerBase` i `Controller` klas podstawowych udostępniające podręczne metody dla wyników akcji, które odwołują się do innej akcji. Jeden typowy sposób jest przekierowanie po akceptowanie danych wejściowych użytkownika.

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

Metody fabryki wyniki akcji wykonaj podobnego wzorca do metod `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Szczególnych przypadkach dla dedykowanych tras z konwencjonalnej

Routing z konwencjonalnej służy specjalny rodzaj definicji trasy o nazwie *dedykowanych trasy z konwencjonalnej*. W poniższym przykładzie trasy o nazwie `blog` jest dedykowany trasy konwencjonalnej.

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

Przy użyciu tych definicji trasy `Url.Action("Index", "Home")` wygeneruje ścieżkę adresu URL `/` z `default` trasy, ale Dlaczego? Może być odgadnąć wartości trasy `{ controller = Home, action = Index }` może być wystarczająca do generowania adresu URL za pomocą `blog`, i będą `/blog?action=Index&controller=Home`.

Dedykowany tras z konwencjonalnej polegać na specjalnego zachowania wartości domyślnych, które nie mają odpowiadającego mu parametru trasy, który uniemożliwia trasy "zbyt zachłannego" generacji adresu URL. W tym przypadku wartości domyślne są `{ controller = Blog, action = Article }`i ani `controller` ani `action` pojawia się jako parametru trasy. Gdy routingu wykonuje generowania adresu URL, podanych wartości musi być zgodna z wartościami domyślnymi. Przy użyciu generowania adresu URL `blog` zakończy się niepowodzeniem, ponieważ wartości `{ controller = Home, action = Index }` nie zgadzają się `{ controller = Blog, action = Article }`. Routing następnie powraca do spróbuj `default`, który zakończy się pomyślnie.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Obszary

[Obszary](areas.md) są funkcją MVC używane do organizowania funkcje w grupie jako osobne routingu — przestrzeń nazw (dla akcji kontrolera) i struktury folderów (dla widoków). Za pomocą obszarów umożliwia aplikacji jest wiele kontrolerów o takiej samej nazwie — tak długo, jak długo mają różne *obszarów*. Za pomocą obszarów tworzy hierarchię na potrzeby routingu, dodając inny parametru trasy, `area` do `controller` i `action`. W tej sekcji będzie omawiać routingu współdziałania z obszarów — zobacz [obszarów](areas.md) szczegółowe informacje na temat używania obszarów z widokami.

Poniższy przykład konfiguruje MVC do użycia z konwencjonalnej trasy domyślnej i *trasy obszaru* obszaru o nazwie `Blog`:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

Podczas dopasowywania Ścieżka adresu URL, takie jak `/Manage/Users/AddUser`, pierwszy trasy utworzy wartości trasy `{ area = Blog, controller = Users, action = AddUser }`. `area` Wartość trasy jest generowany przez wartość domyślną dla `area`, w rzeczywistości trasy utworzone przez `MapAreaRoute` jest odpowiednikiem następujące:

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenie dla `area` przy użyciu nazwy podanego obszaru, w tym przypadku `Blog`. Wartość domyślna zapewnia trasy zawsze daje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresu URL.

> [!TIP]
> Tradycyjnie routing jest zależny od kolejności. Ogólnie rzecz biorąc trasy z obszarami powinna zostać umieszczona wcześniej w tabeli tras, ponieważ są one bardziej szczegółowy niż trasy bez obszaru.

W powyższym przykładzie wartości trasy spowoduje dopasowanie następującej akcji:

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` Jest, co oznacza kontrolera jako część obszaru, możemy powiedzieć, że ten kontroler jest w `Blog` obszaru. Kontrolery bez `[Area]` atrybut nie są członkami żadnego obszaru i będzie **nie** zgodne, kiedy `area` wartość trasy są dostarczane przez routingu. W poniższym przykładzie, tylko pierwszy kontroler na liście można dopasować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Przestrzeń nazw każdego kontrolera jest tu aby informacje były kompletne — w przeciwnym razie kontrolery byłyby nazewnictwa konflikt i wygenerowanie błędu kompilatora. Przestrzenie nazw klasy nie mają wpływu na routingu MVC.

Pierwsze dwa kontrolery są członkami obszarów i dopasować tylko, gdy ich nazwy do odpowiedniego obszaru są dostarczane przez `area` wartości trasy. Trzeci kontrolera nie jest członkiem żadnych obszarów i może tylko dopasowania, gdy brak wartości `area` są dostarczane przez routingu.

> [!NOTE]
> Pod względem dopasowania *żadnej wartości*, braku `area` wartość jest taka sama tak, jakby wartość `area` zostały wartości null ani być pustym ciągiem.

Podczas wykonywania akcji w obszarze, wartości trasy `area` będą dostępne jako *otoczenia wartość* routingu do użycia podczas generowania adresu URL. Oznacza to, że domyślnie obszarów działania *trwałe* do generowania adresu URL, jak pokazano w poniższym przykładzie.

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>Opis IActionConstraint

> [!NOTE]
> Ta sekcja jest szczegółowo wewnętrzne framework i jak MVC wybiera akcję w celu wykonania. Typowa aplikacja nie ma potrzeby niestandardowego `IActionConstraint`

Prawdopodobnie zostały już wykorzystane `IActionConstraint` nawet, jeśli nie znasz z interfejsem. `[HttpGet]` Atrybutu i podobne `[Http-VERB]` implementuje atrybuty `IActionConstraint` w celu ograniczenia wykonywania metody akcji.

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

Zakładając, że domyślną trasę konwencjonalnej ścieżkę adresu URL `/Products/Edit` dałby w efekcie wartości `{ controller = Products, action = Edit }`, który umożliwi dopasowanie **zarówno** akcji pokazano poniżej. W `IActionConstraint` terminologii czy mówi się, że oba te akcje są traktowane jako kandydatów — jak spełniają dane trasy.

Gdy `HttpGetAttribute` wykonuje, jest wyświetlany tekst, który *Edit()* pasuje do *UZYSKAĆ* i nie ma dopasowania dla innych zlecenie HTTP. `Edit(...)` Akcji nie ma żadnych ograniczeń zdefiniowanych, a więc będzie odpowiadała żadnych zlecenie HTTP. Dlatego przy założeniu `POST` — tylko `Edit(...)` zgodny. Jednak dla `GET` zarówno akcje może nadal być zgodne z — jednak akcję z `IActionConstraint` zawsze jest uznawany za *lepsze* niż akcji bez. Dlatego ponieważ `Edit()` ma `[HttpGet]` jest uznawany za bardziej szczegółowe i należy wybrać, jeśli obie akcje może dopasować.

Koncepcyjnie `IActionConstraint` jest formą *przeładowanie*, ale zamiast przeładowanie metody o tej samej nazwie, jest przeładowanie między akcje, które spełniają tego samego adresu URL. Atrybut routingu używa również `IActionConstraint` i może spowodować akcji z różnych kontrolerów obu rozważane kandydatów.

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>Implementowanie IActionConstraint

Najprostszym sposobem, aby zaimplementować `IActionConstraint` polega na utworzeniu klasy pochodzącej od `System.Attribute` i umieść go w tej akcji i kontrolerów. MVC automatycznie wykryje żadnego `IActionConstraint` które są stosowane jako atrybuty. Model aplikacji można użyć, aby zastosować ograniczenia, a jest to prawdopodobnie największą elastyczność, ponieważ umożliwia ona metaprogram sposób ich stosowania.

W poniższym przykładzie ograniczenie wybiera akcję na podstawie *numer kierunkowy kraju* z danych trasy. [Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).

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

Jest odpowiedzialny za wdrażanie `Accept` — metoda i wybierając polecenie "Order" ograniczenia do wykonania. W takim przypadku `Accept` metoda zwraca `true` do oznaczania akcji są zgodne po `country` dopasowania wartości trasy. Różni się to od `RouteValueAttribute` , ponieważ umożliwia rezerwowy bezatrybutowego akcją. Przykład pokazuje, że jeśli zdefiniujesz `en-US` akcji, a następnie kod kraju, takich jak `fr-FR` powróci do kontrolera bardziej ogólnym, który nie ma `[CountrySpecific(...)]` zastosowane.

`Order` Właściwość decyduje, które *etap* ograniczenie jest częścią. Uruchom ograniczenia działań w grupach na podstawie `Order`. Na przykład wszystkie platformy podane atrybuty metody HTTP używać tego samego `Order` tak, aby ich działania w ramach tego samego etapu. Może mieć dowolną liczbę etapów należy wdrożyć odpowiednie zasady.

> [!TIP]
> Podjęcie decyzji na wartość `Order` Pomyśl o czy Twoje ograniczenia powinny być stosowane przed metod HTTP. Niższych numerach uruchamiany najpierw.
