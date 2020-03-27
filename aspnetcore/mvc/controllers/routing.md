---
title: Routing do akcji kontrolera w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób ASP.NET Core MVC używa programów pośredniczących routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.
ms.author: riande
ms.date: 3/25/2020
uid: mvc/controllers/routing
ms.openlocfilehash: c1c0d978714718af1de0f627e50a54f66ed391ed
ms.sourcegitcommit: 4b166b49ec557a03f99f872dd069ca5e56faa524
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/27/2020
ms.locfileid: "80362661"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>Routing do akcji kontrolera w ASP.NET Core

[Ryan Nowak](https://github.com/rynowak), [Kirka Larkin](https://twitter.com/serpent5)i [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Kontrolery ASP.NET Core używają [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) routingu do dopasowywania adresów URL żądań przychodzących i mapują je na [Akcje](#action).  Szablony tras:

* Są zdefiniowane w kodzie startowym lub atrybutów.
* Opisz, w jaki sposób ścieżki URL są dopasowywane do [akcji](#action).
* Służy do generowania adresów URL dla łączy. Wygenerowane linki są zwykle zwracane w odpowiedziach.

Akcje są albo [podkierowane do Konwencji](#cr) lub [kierowane przez atrybut](#ar). Umieszczenie trasy na kontrolerze lub [akcji](#action) sprawia, że jest ona kierowana przez atrybut. Aby uzyskać więcej informacji, zobacz [Routing mieszany](#routing-mixed-ref-label) .

Ten dokument:

* Wyjaśnia interakcje między MVC i Routing:
  * Jak typowe aplikacje MVC używają funkcji routingu.
  * Obejmuje zarówno:
    * [UNC routingu](#cr) zwykle używany z kontrolerami i widokami.
    * *Routing atrybutów* używany z interfejsami API REST. Jeśli jesteś głównie zainteresowani routingiem interfejsów API REST, przejdź do sekcji [Routing atrybutów dla interfejsów API REST](#ar) .
  * Szczegóły dotyczące routingu zaawansowanego znajdują się w temacie [Routing](xref:fundamentals/routing) .
* Odnosi się do domyślnego systemu routingu dodanego w ASP.NET Core 3,0, nazywanego routingiem punktu końcowego. Możliwe jest używanie kontrolerów z poprzednią wersją routingu w celu zapewnienia zgodności. Instrukcje można znaleźć w [przewodniku migracji 2.2-3.0](xref:migration/22-to-30) . Zapoznaj się z [wersją 2,2 tego dokumentu](xref:mvc/controllers/routing?view=aspnetcore-2.2) dotyczącą materiałów referencyjnych w starszym systemie routingu.

<a name="cr"></a>

## <a name="set-up-conventional-route"></a>Konfigurowanie trasy konwencjonalnej

w przypadku korzystania z [routingu konwencjonalnego](#crd)`Startup.Configure` zwykle ma kod podobny do poniższego:

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

Wewnątrz wywołania do <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> służy do tworzenia pojedynczej trasy. Pojedyncza trasa nosi nazwę `default` Route. Większość aplikacji z kontrolerami i widokami używa szablonu trasy podobnego do trasy `default`. Interfejsy API REST powinny używać [routingu atrybutów](#ar).

`"{controller=Home}/{action=Index}/{id?}"`szablonu trasy:

* Dopasowuje ścieżkę URL, taką jak `/Products/Details/5`
* Wyodrębnia wartości tras `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki. Wyodrębnienie wartości tras powoduje dopasowanie, jeśli aplikacja ma kontroler o nazwie `ProductsController` i akcję `Details`:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

 Metoda [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) jest dołączana do [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) i służy do wyświetlania informacji o routingu.

  * Model `/Products/Details/5` wiąże wartość `id = 5`, aby ustawić parametr `id` na `5`. Aby uzyskać więcej informacji, zobacz [powiązanie modelu](xref:mvc/models/model-binding) .
* `{controller=Home}` definiuje `Home` jako `controller`domyślny.
* `{action=Index}` definiuje `Index` jako `action`domyślny.
*  Znak `?` w `{id?}` definiuje `id` jako opcjonalne.
  * Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania. Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .
* Dopasowuje ścieżkę URL `/`.
* Tworzy wartości trasy `{ controller = Home, action = Index }`.
* Metoda [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) jest dołączana do [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) i służy do wyświetlania informacji o routingu.

Wartości `controller` i `action` używają wartości domyślnych. `id` nie produkuje wartości, ponieważ w ścieżce URL nie ma odpowiedniego segmentu. `/` dopasowuje się tylko wtedy, gdy istnieje `HomeController` i `Index` akcji:

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

Przy użyciu definicji kontrolera i szablonu trasy, Akcja `HomeController.Index` jest uruchamiana dla następujących ścieżek URL:

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

Ścieżka adresu URL `/` używa domyślnych kontrolerów szablonu trasy `Home` i akcji `Index`. Ścieżka adresu URL `/Home` używa domyślnej akcji szablonu trasy `Index`.

Wygodna <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>metody:

```csharp
endpoints.MapDefaultControllerRoute();
```

Zastępując

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

Routing jest konfigurowany przy użyciu <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*> i <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*> oprogramowania pośredniczącego. Aby użyć kontrolerów:

* Wywołaj <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> wewnątrz `UseEndpoints` w celu mapowania kontrolerów z [routingiem atrybutów](#ar) .
* Wywołaj <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> lub <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>, aby zamapować kontrolery z [Konwencją routingu](#cr) .

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a>Routing konwencjonalny

Routing konwencjonalny jest używany z kontrolerami i widokami. Trasa `default`:

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

jest przykładem *konwencjonalnego routingu*. Jest on nazywany *konwencjonalnym routingiem* , ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:

* Pierwszy segment ścieżki, `{controller=Home}`, mapuje na nazwę kontrolera.
* Drugi segment `{action=Index}`mapuje na nazwę [akcji](#action) .
* Trzeci segment `{id?}` jest używany w przypadku opcjonalnego `id`. `?` w `{id?}` sprawia, że jest to opcjonalne. `id` jest używany do mapowania na jednostkę modelu.

Korzystając z tej `default` trasy, ścieżka URL:

* `/Products/List` mapuje na akcję `ProductsController.List`.
* `/Blog/Article/17` Maps do `BlogController.Article` i zazwyczaj model wiąże `id` z 17.

To mapowanie:

* Jest oparty **tylko**na nazwach kontrolera i [akcji](#action) .
* Nie jest oparty na przestrzeniach nazw, lokalizacjach plików źródłowych ani parametrach metod.

Użycie konwencjonalnego routingu z domyślną trasą umożliwia tworzenie aplikacji bez konieczności korzystania z nowego wzorca adresu URL dla każdej akcji. W przypadku aplikacji z akcjami w stylu [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) mających spójność dla adresów URL między kontrolerami:

* Upraszcza kod.
* Sprawia, że interfejs użytkownika jest bardziej przewidywalny.

> [!WARNING]
> `id` w poprzednim kodzie jest zdefiniowany jako opcjonalny przez szablon trasy. Akcje można wykonać bez opcjonalnego identyfikatora podanego w ramach adresu URL. Ogólnie rzecz biorąc, gdy`id` zostanie pominięty z adresu URL:
>
> * `id` jest ustawiony na `0` przez powiązanie modelu.
> * Nie znaleziono jednostki w `id == 0`zgodnej z bazami danych.
>
> [Routing atrybutu](#ar) zawiera szczegółowy formant, który umożliwia określanie identyfikatora wymaganego dla niektórych akcji, a nie dla innych. Zgodnie z Konwencją, dokumentacja zawiera parametry opcjonalne, takie jak `id`, gdy prawdopodobnie będą widoczne w prawidłowym użyciu.

Większość aplikacji powinna wybrać podstawowy i opisowy schemat routingu, aby adresy URL były czytelne i zrozumiałe. Domyślna trasa konwencjonalna `{controller=Home}/{action=Index}/{id?}`:

* Obsługuje podstawowy i opisowy schemat routingu.
* Jest użytecznym punktem wyjścia dla aplikacji opartych na interfejsie użytkownika.
* Jest jedynym szablonem trasy wymaganym przez wiele aplikacji interfejsu użytkownika sieci Web. W przypadku większych aplikacji interfejsu użytkownika sieci Web inna trasa korzysta z [obszarów](#areas) , jeśli są one często używane.

<xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> i <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*>:

* Automatycznie Przypisz wartość **zamówienia** do punktów końcowych na podstawie kolejności, w której są wywoływane.

Routing punktów końcowych w ASP.NET Core 3,0 i nowszych:

* Nie ma koncepcji tras.
* Nie oferuje gwarancji porządkowania dla wykonywania rozszerzalności, wszystkie punkty końcowe są przetwarzane jednocześnie.

Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby zobaczyć, jak wbudowane implementacje routingu, takie jak <xref:Microsoft.AspNetCore.Routing.Route>, pasują do żądań.

[Routing atrybutów](#ar) został wyjaśniony w dalszej części tego dokumentu.

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a>Wiele konwencjonalnych tras

W `UseEndpoints` można dodać wiele [konwencjonalnych tras](#cr) , dodając więcej wywołań do <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*> i <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*>. Dzięki temu można definiować wiele Konwencji lub dodać do nich trasy konwencjonalne, które są przeznaczone dla konkretnej [akcji](#action), na przykład:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

Trasa `blog` w powyższym kodzie jest **dedykowaną konwencjonalną trasą**. Jest on nazywany dedykowaną trasą konwencjonalną, ponieważ:

* Używa ona [konwencjonalnego routingu](#cr).
* Jest on przeznaczony dla konkretnej [akcji](#action).

Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy `"blog/{*article}"` jako parametry:

* Mogą mieć tylko wartości domyślne `{ controller = "Blog", action = "Article" }`.
* Ta trasa zawsze jest mapowana na `BlogController.Article`akcji.

`/Blog`, `/Blog/Article`i `/Blog/{any-string}` są jedynymi ścieżkami URL, które pasują do trasy blogu.

Powyższy przykład:

* Trasa `blog` ma wyższy priorytet dla dopasowania niż trasy `default`, ponieważ jest dodawana jako pierwsza.
* Jest i przykładem routingu stylów [informacji](https://developer.mozilla.org/docs/Glossary/Slug) o sobie, w którym typowym jest nazwa artykułu w ramach adresu URL.

> [!WARNING]
> W ASP.NET Core 3,0 i nowszych routingu nie są:
> * Zdefiniuj koncepcję o nazwie *trasa*. `UseRouting` dodaje dopasowanie trasy do potoku programu pośredniczącego. Oprogramowanie pośredniczące `UseRouting` sprawdza zestaw punktów końcowych zdefiniowanych w aplikacji i wybiera najlepsze dopasowanie punktu końcowego na podstawie żądania.
> * Podaj gwarancje dotyczące kolejności wykonywania rozszerzalności, takich jak <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> lub <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.
>
>Zobacz [Routing](xref:fundamentals/routing) dla materiałów referencyjnych na trasie.

<a name="cro"></a>

### <a name="conventional-routing-order"></a>Tradycyjna kolejność routingu

Routing konwencjonalny pasuje do kombinacji akcji i kontrolera, które są zdefiniowane przez aplikację. Jest to przeznaczone do uproszczenia sytuacji, w których trasy konwencjonalne nakładają się na siebie.
Dodanie tras przy użyciu <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute*>i <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> powoduje automatyczne przypisanie wartości zamówienia do punktów końcowych na podstawie kolejności, w której są wywoływane. Dopasowania z trasy, która pojawiła się wcześniej, mają wyższy priorytet. Routowanie konwencjonalne jest zależne od kolejności. Ogólnie rzecz biorąc, trasy z obszarami powinny być umieszczone wcześniej, ponieważ są bardziej specyficzne niż trasy bez obszaru. [Dedykowane konwencjonalne trasy](#dcr) z funkcją przechwytywania wszystkie parametry tras, takie jak `{*article}`, mogą spowodować zbyt [zachłanne](xref:fundamentals/routing#greedy)trasy, co oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy. Umieść trasy zachłanne w dalszej części tabeli tras, aby zapobiec dopasowania zachłanne.

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a>Rozwiązywanie niejednoznacznych akcji

Gdy dwa punkty końcowe pasują do routingu, routing musi wykonać jedną z następujących czynności:

* Wybierz najlepszego kandydata.
* Zgłoś wyjątek.

Na przykład:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

Poprzedni kontroler definiuje dwie akcje, które są zgodne:

* Ścieżka adresu URL `/Products33/Edit/17`
* Kierowanie `{ controller = Products33, action = Edit, id = 17 }`danych.

Jest to typowy wzorzec dla kontrolerów MVC:

* `Edit(int)` wyświetla formularz służący do edytowania produktu.
* `Edit(int, Product)` przetwarza opublikowany formularz.

Aby rozwiązać poprawność trasy:

* `Edit(int, Product)` jest zaznaczone, gdy żądanie jest `POST`HTTP.
* `Edit(int)` jest wybierany, gdy [czasownik http](#verb) to coś innego. `Edit(int)` jest zazwyczaj wywoływany przez `GET`.

<xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, jest dostarczany do routingu, aby można było wybrać oparty na metodzie HTTP żądania. `HttpPostAttribute` zapewnia `Edit(int, Product)` lepszym dopasowaniem niż `Edit(int)`.

Ważne jest, aby zrozumieć rolę atrybutów, takich jak `HttpPostAttribute`. Podobne atrybuty są zdefiniowane dla innych [czasowników HTTP](#verb). W przypadku [routingu konwencjonalnego](#cr)typowe dla akcji używanie tej samej nazwy akcji, gdy są one częścią formularza Pokaż, przesyłania formularza. Na przykład zapoznaj [się z tematem badanie dwóch metod akcji edycji](xref:tutorials/first-mvc-app/controller-methods-views#get-post).

Jeśli nie można wybrać najlepszego kandydata, zostanie zgłoszony <xref:System.Reflection.AmbiguousMatchException>, zawierający wiele pasujących punktów końcowych.

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a>Nazwy tras konwencjonalnych

Ciągi `"blog"` i `"default"` w poniższych przykładach są nazwami konwencjonalnych tras:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

Nazwy tras nadają trasie nazwę logiczną. Nazwana trasa może służyć do generowania adresów URL. Użycie nazwanej trasy upraszcza tworzenie adresów URL, gdy porządkowanie tras może spowodować skomplikowane generowanie adresów URL. Nazwy tras muszą być unikatowe w całej aplikacji.

Nazwy tras:

* Nie mają wpływu na pasujące adresy URL ani obsługę żądań.
* Są używane tylko do generowania adresów URL.

Koncepcja nazwy trasy jest reprezentowana w obszarze Routing jako [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata). **Nazwa trasy** i nazwa **punktu końcowego**:

* Są zamienne.
* Który jest używany w dokumentacji i kodu zależy od opisanego interfejsu API.

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a>Routing atrybutów dla interfejsów API REST

Interfejsy API REST powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez [zlecenia http](#verb).

Funkcja routingu atrybutów używa zestawu atrybutów do mapowania akcji bezpośrednio do szablonów tras. Poniższy kod `StartUp.Configure` jest typowy dla interfejsu API REST i jest używany w następnym przykładzie:

[!code-csharp[](routing/samples/3.x/main/StartupApi.cs?name=snippet)]

W poprzednim kodzie <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> jest wywoływana wewnątrz `UseEndpoints` w celu mapowania kontrolerów z routingiem atrybutu.

W poniższym przykładzie:

* Poprzednia metoda `Configure` jest używana.
* `HomeController` dopasowuje zestaw adresów URL podobny do tego, co jest zgodne z domyślną trasą konwencjonalny `{controller=Home}/{action=Index}/{id?}`.

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

Akcja `HomeController.Index` jest uruchamiana dla dowolnej ścieżki URL `/`, `/Home`, `/Home/Index`lub `/Home/Index/3`.

W tym przykładzie przedstawiono najważniejsze różnice programistyczne między routingiem atrybutów i [routingiem konwencjonalnym](#cr). Routing atrybutów wymaga więcej danych wejściowych w celu określenia trasy. Konwencjonalne trasy domyślne obsługuje trasy bardziej zwięzłie. Jednak Routing atrybutu zezwala na ścisłą kontrolę nad tym, które szablony tras mają zastosowanie do poszczególnych [akcji](#action).

W przypadku routingu atrybutów nazwa kontrolera i nazwy akcji nie odgrywają **żadnej** roli, w której akcja jest dopasowywana. Poniższy przykład dopasowuje te same adresy URL, co w poprzednim przykładzie:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

Następujący kod używa wymiany tokenów dla `action` i `controller`:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

Następujący kod ma zastosowanie `[Route("[controller]/[action]")]` do kontrolera:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

W poprzednim kodzie szablony metod `Index` muszą być poprzedzone `/` lub `~/` do szablonów tras. Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są łączone z szablonami tras zastosowanymi do kontrolera.

Zobacz [pierwszeństwo szablonu trasy](xref:fundamentals/routing#rtp) , aby uzyskać informacje o wyborze szablonu trasy.

## <a name="reserved-routing-names"></a>Zastrzeżone nazwy routingu

Następujące słowa kluczowe są zastrzeżonymi nazwami parametrów trasy podczas korzystania z kontrolerów lub Razor Pages:

* `action`
* `area`
* `controller`
* `handler`
* `page`

Użycie `page` jako parametru trasy z routingiem atrybutu jest typowym błędem. Powoduje to niespójne i mylące zachowanie przy generowaniu adresów URL.

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

Specjalne nazwy parametrów są używane przez generowanie adresów URL w celu ustalenia, czy operacja generowania adresu URL odwołuje się do strony Razor lub do kontrolera.

<a name="verb"></a>

## <a name="http-verb-templates"></a>Szablony czasowników HTTP

ASP.NET Core ma następujące szablony czasowników HTTP:

* [Narzędzia HttpGet](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)
* [HTTPPOST](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)
* [[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)
* [[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)
* [[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)
* [[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)

<a name="rt"></a>

### <a name="route-templates"></a>Szablony tras

ASP.NET Core ma następujące szablony tras:

* Wszystkie [Szablony czasowników HTTP](#verb) są szablonami tras.
* [Szlak](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a>Routing atrybutów z atrybutami czasownik http

Rozważmy następujący kontroler:

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

W powyższym kodzie:

* Każda akcja zawiera atrybut `[HttpGet]`, który ogranicza dopasowywanie do żądań HTTP GET.
* Akcja `GetProduct` obejmuje szablon `"{id}"`, dlatego `id` jest dołączany do szablonu `"api/[controller]"` na kontrolerze. Szablon metod jest `"api/[controller]/"{id}""`. W związku z tym ta akcja dopasowuje tylko żądania GET dla formularza `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`itd.
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* Akcja `GetIntProduct` zawiera szablon `"int/{id:int}")`. Część `:int` szablonu ogranicza wartości trasy `id` do ciągów, które mogą być konwertowane na liczbę całkowitą. Żądanie GET do `/api/test2/int/abc`:
  * Nie jest zgodna z tą akcją.
  * Zwraca błąd [nie znaleziono 404](https://developer.mozilla.org/docs/Web/HTTP/Status/404) .
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* Akcja `GetInt2Product` zawiera `{id}` w szablonie, ale nie ogranicza `id` do wartości, które można przekonwertować na liczbę całkowitą. Żądanie GET do `/api/test2/int2/abc`:
  * Pasuje do tej trasy.
  * Nie można skonwertować powiązania modelu `abc` na liczbę całkowitą. `id` parametr metody jest liczbą całkowitą.
  * Zwraca [400 Nieprawidłowe żądanie](https://developer.mozilla.org/docs/Web/HTTP/Status/400) , ponieważ nie powiodło się przekonwertowanie powiązania modelu`abc` na liczbę całkowitą.
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

Funkcja routingu atrybutów może używać atrybutów <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute>, takich jak <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>i <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>. Wszystkie atrybuty [czasowników HTTP](#verb) akceptują szablon trasy. Poniższy przykład przedstawia dwie akcje, które pasują do tego samego szablonu trasy:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

Używanie `/products3`ścieżki URL:

* Akcja `MyProductsController.ListProducts` jest uruchamiana, gdy [zlecenie http](#verb) jest `GET`.
* Akcja `MyProductsController.CreateProduct` jest uruchamiana, gdy [zlecenie http](#verb) jest `POST`.

Podczas kompilowania interfejsu API REST jest to rzadko konieczne, aby użyć `[Route(...)]` na metodę akcji, ponieważ akcja akceptuje wszystkie metody HTTP. Lepiej jest używać bardziej szczegółowego [atrybutu zlecenia http](#verb) , aby precyzyjnie dowiedzieć się, co obsługuje interfejs API. Klienci interfejsów API REST powinni wiedzieć, jakie ścieżki i czasowniki HTTP mapują na określone operacje logiczne.

Interfejsy API REST powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez zlecenia HTTP. Oznacza to, że wiele operacji, na przykład Pobierz i Opublikuj dla tego samego zasobu logicznego, użyj tego samego adresu URL. Routing atrybutu zapewnia poziom kontroli, który jest wymagany do dokładnego projektowania układu publicznego punktu końcowego interfejsu API.

Ponieważ atrybut Route ma zastosowanie do określonej akcji, można łatwo wprowadzić parametry wymagane jako część definicji szablonu trasy. W poniższym przykładzie `id` jest wymagane jako część ścieżki URL:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

Akcja `Products2ApiController.GetProduct(int)`:

* Jest uruchamiany z ścieżką URL, taką jak `/products2/3`
* Nie jest uruchamiany z ścieżką URL `/products2`.

Atrybut [[](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) Requests] umożliwia akcja ograniczenia obsługiwanych typów zawartości żądania. Aby uzyskać więcej informacji, zobacz [Definiowanie obsługiwanych typów zawartości żądania przy użyciu atrybutu użycia](xref:web-api/index#consumes).

 Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](xref:fundamentals/routing) .

Aby uzyskać więcej informacji na `[ApiController]`, zobacz [atrybut ApiController](xref:web-api/index##apicontroller-attribute).

## <a name="route-name"></a>Nazwa trasy

Poniższy kod definiuje nazwę trasy `Products_List`:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

Nazwy tras mogą służyć do generowania adresów URL na podstawie określonej trasy. Nazwy tras:

* Nie ma wpływu na zachowanie w zakresie routingu zgodnego z adresem URL.
* Są używane tylko na potrzeby generowania adresów URL.

Nazwy tras muszą być unikatowe w całej aplikacji.

Należy odróżnić poprzedni kod od konwencjonalnej trasy domyślnej, która definiuje `id` parametr jako opcjonalny (`{id?}`). Możliwość precyzyjnego określania interfejsów API ma zalety, takich jak umożliwienie `/products` i `/products/5` do wysłania do różnych akcji.

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a>Łączenie tras atrybutów

Aby mniej powtarzać Routing atrybutów, atrybuty trasy na kontrolerze są łączone z atrybutami trasy dla poszczególnych akcji. Wszystkie szablony tras zdefiniowane na kontrolerze są poprzedzone w celu rozesłania szablonów w akcjach. Umieszczenie atrybutu trasy na kontrolerze powoduje, że **wszystkie** akcje w kontrolerze używają routingu atrybutów.

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

W poprzednim przykładzie:

* Ścieżka adresu URL `/products` może być zgodna `ProductsApi.ListProducts`
* Ścieżka adresu URL `/products/5` może być zgodna `ProductsApi.GetProduct(int)`.

Obie te akcje pasują do `GET` HTTP, ponieważ są oznaczone atrybutem `[HttpGet]`.

Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są łączone z szablonami tras zastosowanymi do kontrolera. Poniższy przykład dopasowuje zestaw ścieżek adresów URL podobny do trasy domyślnej.

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

W poniższej tabeli opisano atrybuty `[Route]` w poprzednim kodzie:

| Atrybut               | Łączy się z `[Route("Home")]` | Definiuje szablon trasy |
| ----------------- | ------------ | --------- |
| `[Route("")]` | Yes | `"Home"` |
| `[Route("Index")]` | Yes | `"Home/Index"` |
| `[Route("/")]` | **Nie** | `""` |
| `[Route("About")]` | Yes | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a>Porządek trasy dla atrybutu

Routing kompiluje drzewo i dopasowuje wszystkie punkty końcowe jednocześnie:

* Wpisy trasy zachowują się tak, jakby zostały umieszczone w idealnej kolejności.
* Najbardziej konkretne trasy mają możliwość wykonania przed bardziej ogólnymi trasami.

Na przykład trasa atrybutu, taka jak `blog/search/{topic}`, jest bardziej precyzyjna niż trasa atrybutu, taka jak `blog/{*article}`. Trasa `blog/search/{topic}` ma wyższy priorytet, ponieważ jest domyślnie bardziej specyficzna. Przy użyciu [konwencjonalnego routingu](#cr)deweloper jest odpowiedzialny za umieszczanie tras w odpowiedniej kolejności.

Trasy atrybutów mogą konfigurować kolejność przy użyciu właściwości <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order>. Wszystkie [atrybuty trasy](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) podane przez platformę obejmują `Order`. Trasy są przetwarzane zgodnie z rosnącym sortowaniem właściwości `Order`. Kolejność domyślna to `0`. Ustawienie trasy przy użyciu `Order = -1` jest uruchamiane przed trasami, które nie ustawiają kolejności. Ustawienie trasy przy użyciu `Order = 1` jest uruchamiane po określeniu domyślnej kolejności tras.

**Należy unikać** w zależności od `Order`. Jeśli przestrzeń adresów URL aplikacji wymaga jawnych wartości kolejności, aby można było prawidłowo kierować trasy, prawdopodobnie również jest myląca dla klientów. Ogólnie rzecz biorąc, routing atrybutów wybiera prawidłową trasę z dopasowywaniem adresów URL. Jeśli domyślna kolejność generowania adresów URL nie działa, użycie nazwy trasy jako przesłonięcia jest zwykle prostsze niż stosowanie właściwości `Order`.

Należy wziąć pod uwagę następujące dwa kontrolery, które definiują `/home`pasujące do trasy:

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

Żądanie `/home` za pomocą powyższego kodu zgłasza wyjątek podobny do następującego:

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

Dodanie `Order` do jednego z atrybutów trasy rozwiązuje niejednoznaczność:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

Za pomocą powyższego kodu `/home` uruchamia `HomeController.Index` punkt końcowy. Aby przejść do `MyDemoController.MyIndex`, zażądaj `/home/MyIndex`. **Uwaga**:

* Poprzedni kod jest przykładem lub słabym projektem routingu. Został on użyty do zilustrowania właściwości `Order`.
* Właściwość `Order` rozwiązuje tylko niejednoznaczność, ale nie można dopasować tego szablonu. Lepszym rozwiązaniem jest usunięcie szablonu `[Route("Home")]`.

Zobacz [Razor Pages trasy i konwencje aplikacji: zamówienie trasy,](xref:razor-pages/razor-pages-conventions#route-order) Aby uzyskać informacje na temat kolejności tras z Razor Pages.

W niektórych przypadkach błąd HTTP 500 jest zwracany z niejednoznacznych tras. Użyj [rejestrowania](xref:fundamentals/logging/index) , aby zobaczyć, które punkty końcowe spowodowały `AmbiguousMatchException`.

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>Zastępowanie tokenu w szablonach tras [Controller], [Action], [obszar]

Dla wygody atrybut trasy obsługują zastępowanie tokenów dla zarezerwowanych parametrów trasy przez załączanie tokenu w jednym z następujących:

* Kwadratowe nawiasy klamrowe: `[]`
* Nawiasy klamrowe: `{}`

Tokeny `[action]`, `[area]`i `[controller]` są zastępowane wartościami nazwy akcji, obszaru i nazwy kontrolera z akcji, w której zdefiniowano trasę:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

W powyższym kodzie:

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * Dopasowuje `/Products0/List`

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * Dopasowuje `/Products0/Edit/{id}`

Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów. Poprzedni przykład zachowuje się tak samo jak w poniższym kodzie:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

Trasy atrybutu można także łączyć z dziedziczeniem. Jest to zaawansowane połączenie z zastępowaniem tokenu. Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.
`[Route("[controller]/[action]", Name="[controller]_[action]")]`generuje unikatową nazwę trasy dla każdej akcji:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
generuje unikatową nazwę trasy dla każdej akcji.

Aby dopasować ogranicznik zastępczy tokenu literału `[` lub `]`, należy to zrobić, powtarzając znak (`[[` lub `]]`).

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a>Używanie transformatora parametrów do dostosowywania zastępowania tokenu

Zastępowanie tokenu można dostosować za pomocą transformatora parametrów. Transformator parametrów implementuje <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> i przekształca wartość parametrów. Na przykład niestandardowy `SlugifyParameterTransformer` przekształcania parametrów zmienia wartość trasy `SubscriptionManagement` na `subscription-management`:

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> jest konwencją modelu aplikacji, która:

* Stosuje transformator parametrów do wszystkich tras atrybutów w aplikacji.
* Dostosowuje wartości tokenów tras w miarę ich wymiany.

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

Poprzednia metoda `ListAll` pasuje do `/subscription-management/list-all`.

`RouteTokenTransformerConvention` jest zarejestrowany jako opcja w `ConfigureServices`.

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

Zobacz [powiadomienia MDN Web docs](https://developer.mozilla.org/docs/Glossary/Slug) , aby zapoznać się z definicją informacji o sobie.

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a>Wiele tras atrybutów

Routing atrybutów obsługuje definiowanie wielu tras, które docierają do tej samej akcji. Najczęstszym sposobem użycia tej metody jest naśladowanie zachowania domyślnej trasy konwencjonalnej, jak pokazano w następującym przykładzie:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

Umieszczenie wielu atrybutów trasy na kontrolerze oznacza, że każda z nich łączy się z każdym z atrybutów tras w metodach akcji:

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

Wszystkie ograniczenia trasy [zlecenia http](#verb) implementują `IActionConstraint`.

Gdy wiele atrybutów trasy, które implementują <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> są umieszczane w akcji:

* Każde ograniczenie akcji łączy się z szablonem trasy zastosowanym do kontrolera.

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

Korzystanie z wielu tras w akcjach może wydawać się użyteczne i zaawansowane, dlatego lepiej jest zachować podstawową i dobrze zdefiniowaną przestrzeń adresów URL aplikacji. Użyj wielu tras w przypadku akcji **tylko wtedy** , gdy jest to konieczne, na przykład w celu obsługi istniejących klientów.

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>Określanie opcjonalnych parametrów trasy, wartości domyślnych i ograniczeń

Trasy atrybutów obsługują tę samą składnię wbudowaną co konwencjonalne trasy, aby określić parametry opcjonalne, wartości domyślne i ograniczenia.

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

W poprzednim kodzie `[HttpPost("product/{id:int}")]` stosuje ograniczenie trasy. Akcja `ProductsController.ShowProduct` jest dopasowywana tylko przez ścieżki URL, takie jak `/product/3`. Część szablonu trasy `{id:int}` ogranicza ten segment tylko do liczb całkowitych.

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>Niestandardowe atrybuty trasy przy użyciu IRouteTemplateProvider

Wszystkie [atrybuty trasy](#rt) implementują <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>. Środowisko uruchomieniowe ASP.NET Core:

* Wyszukuje atrybuty klas kontrolera i metod akcji podczas uruchamiania aplikacji.
* Używa atrybutów, które implementują `IRouteTemplateProvider`, aby skompilować początkowy zestaw tras.

Zaimplementuj `IRouteTemplateProvider`, aby zdefiniować niestandardowe atrybuty trasy. Każda `IRouteTemplateProvider` umożliwia zdefiniowanie pojedynczej trasy z niestandardowym szablonem trasy, kolejnością i nazwą:

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

Poprzednia metoda `Get` zwraca `Order = 2, Template = api/MyTestApi`.

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a>Dostosowywanie tras atrybutów przy użyciu modelu aplikacji

Model aplikacji:

* Jest modelem obiektu utworzonym podczas uruchamiania.
* Zawiera wszystkie metadane używane przez ASP.NET Core do kierowania i wykonywania akcji w aplikacji.

Model aplikacji zawiera wszystkie dane zebrane z atrybutów tras. Atrybuty trasy są udostępniane przez implementację `IRouteTemplateProvider`. Obowiązujący

* Można napisać, aby zmodyfikować model aplikacji, aby dostosować sposób zachowania routingu.
* Są odczytywane podczas uruchamiania aplikacji.

W tej sekcji przedstawiono podstawowy przykład dostosowywania routingu przy użyciu modelu aplikacji. Poniższy kod sprawia, że trasy są w przybliżeniu zgodne ze strukturą folderów projektu.

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

Poniższy kod uniemożliwia stosowanie Konwencji `namespace` do kontrolerów, które są kierowane przez atrybut:

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

Na przykład następujący kontroler nie używa `NamespaceRoutingConvention`:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

Metoda `NamespaceRoutingConvention.Apply`:

* Nie robi nic, Jeśli kontroler ma przypisany atrybut.
* Ustawia szablon szablonu na podstawie `namespace`z usuniętym `namespace` bazowym.

`NamespaceRoutingConvention` można zastosować w `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

Rozważmy na przykład następujący kontroler:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

W powyższym kodzie:

* `namespace` podstawowe jest `My.Application`.
* Pełna nazwa poprzedniego kontrolera jest `My.Application.Admin.Controllers.UsersController`.
* `NamespaceRoutingConvention` ustawia szablon controllers na `Admin/Controllers/Users/[action]/{id?`.

`NamespaceRoutingConvention` można również zastosować jako atrybut na kontrolerze:

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Routing mieszany: Routing atrybutów a konwencjonalny Routing

Aplikacje ASP.NET Core mogą łączyć użycie konwencjonalnych routingu i routingu atrybutów. Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.

Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut. Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany. Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa. **Każdy** atrybut trasy na kontrolerze powoduje kierowanie **wszystkich** akcji w atrybucie kontrolera.

Routing atrybutów i Routing konwencjonalny używają tego samego aparatu routingu.

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a>Generowanie adresów URL i wartości otoczenia

Aplikacje mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji. Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany. Ta sekcja koncentruje się na funkcjach generowania adresów URL udostępnianych przez MVC i obejmuje jedynie podstawowe informacje na temat sposobu działania generowania adresów URL. Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](xref:fundamentals/routing) .

Interfejs <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> jest podstawowym elementem infrastruktury między MVC i routingiem na potrzeby generowania adresów URL. Wystąpienie `IUrlHelper` jest dostępne za pomocą właściwości `Url` w obszarze Kontrolery, widoki i składniki widoku.

W poniższym przykładzie interfejs `IUrlHelper` jest używany przez właściwość `Controller.Url` do generowania adresu URL dla innej akcji.

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

Jeśli aplikacja używa domyślnej trasy konwencjonalnej, wartość zmiennej `url` jest ciągiem ścieżki adresu URL `/UrlGeneration/Destination`. Ta ścieżka URL jest tworzona przez połączenie za pomocą routingu:

* Wartości trasy z bieżącego żądania, które są nazywane **wartościami otoczenia**.
* Wartości przenoszone do `Url.Action` i podstawiające te wartości do szablonu trasy:

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

Każdy parametr trasy w szablonie trasy ma swoją wartość zastępowaną przez pasujące nazwy wartościami i wartościami otoczenia. Parametr trasy, który nie ma wartości, może:

* Użyj wartości domyślnej, jeśli ma ją.
* Można pominąć, jeśli jest to opcjonalne. Na przykład `id` z szablonu trasy `{controller}/{action}/{id?}`.

Generowanie adresu URL kończy się niepowodzeniem, jeśli którykolwiek z wymaganych parametrów trasy nie ma odpowiadającej mu wartości. Jeśli generowanie adresów URL kończy się niepowodzeniem dla trasy, kolejna trasa zostanie ponowiona do momentu przetworzenia wszystkich tras lub znalezienia dopasowania.

Powyższy przykład `Url.Action` zakłada, że [konwencjonalne Routing](#cr). Generowanie adresów URL działa podobnie jak w przypadku [routingu atrybutów](#ar), chociaż koncepcje różnią się. Z routingiem konwencjonalnym:

* Wartości trasy służą do rozszerzania szablonu.
* Wartości trasy dla `controller` i `action` są zwykle wyświetlane w tym szablonie. Dzieje się tak, ponieważ adresy URL dopasowane przez Routing są zgodne z Konwencją.

W poniższym przykładzie zastosowano Routing atrybutów:

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

Akcja `Source` w poprzednim kodzie generuje `custom/url/to/destination`.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> został dodany w ASP.NET Core 3,0 jako alternatywa dla `IUrlHelper`. `LinkGenerator` oferuje podobne, ale bardziej elastyczne funkcje. Każda inna metoda na `IUrlHelper` ma również odpowiednią rodzinę metod na `LinkGenerator`.

### <a name="generating-urls-by-action-name"></a>Generowanie adresów URL według nazwy akcji

Wartość [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator. GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)i wszystkie powiązane przeciążenia są przeznaczone do generowania docelowego punktu końcowego przez określenie nazwy kontrolera i nazwy akcji.

W przypadku korzystania z `Url.Action`bieżąca wartość trasy dla `controller` i `action` jest zapewniana przez środowisko uruchomieniowe:

* Wartość `controller` i `action` są częścią [wartości otoczenia](#ambient) i wartości. Metoda `Url.Action` zawsze używa bieżących wartości `action` i `controller` i generuje ścieżkę URL, która kieruje do bieżącej akcji.

Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały dostarczone podczas generowania adresu URL. Rozważmy trasę, taką jak `{a}/{b}/{c}/{d}` z wartościami otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`:

* Routing ma wystarczającą ilość informacji do wygenerowania adresu URL bez dodatkowych wartości.
* Routing ma wystarczającą ilość informacji, ponieważ wszystkie parametry tras mają wartość.

W przypadku dodania wartości `{ d = Donovan }`:

* Wartość `{ d = David }` jest ignorowana.
* Wygenerowana Ścieżka adresu URL jest `Alice/Bob/Carol/Donovan`.

**Ostrzeżenie**: ścieżki URL są hierarchiczne. W poprzednim przykładzie, jeśli wartość `{ c = Cheryl }` zostanie dodana:

* Obie wartości `{ c = Carol, d = David }` są ignorowane.
* Nie ma już wartości `d` i generowanie adresu URL kończy się niepowodzeniem.
* Wymagane wartości `c` i `d` muszą być określone w celu wygenerowania adresu URL.  

Może się spodziewać, że ten problem zostanie osiągnięty przy użyciu trasy domyślnej `{controller}/{action}/{id?}`. Ten problem występuje rzadko, ponieważ `Url.Action` zawsze jawnie określa `controller` i `action` wartość.

Kilka przeciążeń [adresu URL. akcja](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) przyjmuje obiekt wartości trasy, aby podać wartości parametrów trasy innych niż `controller` i `action`. Obiekt wartości trasy jest często używany z `id`. Na przykład `Url.Action("Buy", "Products", new { id = 17 })`. Obiekt wartości trasy:

* Według Konwencji jest zwykle obiektem typu anonimowego.
* Może to być `IDictionary<>` lub [poco](https://wikipedia.org/wiki/Plain_old_CLR_object)).

Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

Poprzedni kod generuje `/Products/Buy/17?color=red`.

Poniższy kod generuje bezwzględny adres URL:

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

Aby utworzyć bezwzględny adres URL, użyj jednego z następujących elementów:

* Przeciążenie, które akceptuje `protocol`. Na przykład poprzedzający kod.
* [LinkGenerator. GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), który domyślnie generuje bezwzględne identyfikatory URI.

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a>Generuj adresy URL według trasy

Poprzedni kod wykazał wygenerowanie adresu URL przez przekazanie go do kontrolera i nazwy akcji. `IUrlHelper` udostępnia również [RouteUrl z adresem URL.](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) Te metody są podobne do [adresu URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy. Najczęstsze użycie `Url.RouteUrl`:

* Określa nazwę trasy do wygenerowania adresu URL.
* Zwykle nie określa kontrolera ani nazwy akcji.

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

Następujący plik Razor generuje link HTML do `Destination_Route`:

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a>Generuj adresy URL w formacie HTML i Razor

<xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> udostępnia metody <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> [HTML. BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) i [HTML. ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) w celu wygenerowania odpowiednio elementów `<form>` i `<a>`. Metody te używają metody [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) do generowania adresu URL i akceptują podobne argumenty. `Url.RouteUrl` pomocników dla `HtmlHelper` są `Html.BeginRouteForm` i `Html.RouteLink`, które mają podobną funkcjonalność.

TagHelpers Generuj adresy URL za pomocą `form` TagHelper i `<a>` TagHelper. Oba te zastosowania `IUrlHelper` do ich implementacji. Aby uzyskać więcej informacji, zobacz [pomocników tagów w formularzach](xref:mvc/views/working-with-forms) .

W widokach, `IUrlHelper` jest dostępna za pomocą właściwości `Url` dla dowolnej generacji adresów URL ad hoc, które nie są objęte powyższym.

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a>Generowanie adresów URL w wynikach akcji

Poprzednie przykłady pokazano przy użyciu `IUrlHelper` w kontrolerze. Najczęstszym sposobem użycia na kontrolerze jest wygenerowanie adresu URL jako części wyniku akcji.

Klasy bazowe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> i <xref:Microsoft.AspNetCore.Mvc.Controller> zapewniają wygodne metody dla wyników akcji, które odwołują się do innej akcji. Jednym z typowych zastosowań jest przekierowanie po zaakceptowaniu danych wejściowych użytkownika:

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

Metody fabryki wyników działania, takie jak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction*> i <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>, są zgodne z podobnym wzorcem do metod w `IUrlHelper`.

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>Specjalny przypadek dla dedykowanych tras konwencjonalnych

Funkcja [routingu konwencjonalnego](#cr) może używać specjalnego rodzaju definicji trasy o nazwie [dedykowanej, konwencjonalnej trasy](#dcr). W poniższym przykładzie trasa o nazwie `blog` jest dedykowaną konwencjonalną trasą:

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

Przy użyciu poprzednich definicji trasy `Url.Action("Index", "Home")` generuje ścieżkę URL `/` przy użyciu trasy `default`, ale dlaczego? Możesz odgadnąć wartości tras `{ controller = Home, action = Index }` wystarcza do wygenerowania adresu URL przy użyciu `blog`, a wynik zostanie `/blog?action=Index&controller=Home`.

[Dedykowane konwencjonalne trasy](#dcr) polegają na specjalnym zachowaniu wartości domyślnych, które nie mają odpowiadającego parametru trasy, który uniemożliwia zbyt [zachłanne](xref:fundamentals/routing#greedy) trasy z generowaniem adresów URL. W takim przypadku wartości domyślne są `{ controller = Blog, action = Article }`, a ani `controller`, ani `action` nie pojawiają się jako parametr trasy. Gdy Routing wykonuje generowanie adresów URL, podane wartości muszą być zgodne z wartościami domyślnymi. Generowanie adresu URL przy użyciu `blog` nie powiodło się, ponieważ wartości `{ controller = Home, action = Index }` nie pasują do `{ controller = Blog, action = Article }`. Następnie usługa routingu wraca do wypróbowania `default`, która kończy się powodzeniem.

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>Obszary

[Obszary](xref:mvc/controllers/areas) są funkcją MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej:

* Przestrzeń nazw routingu dla akcji kontrolera.
* Struktura folderów dla widoków.

Użycie obszarów umożliwia aplikacji posiadanie wielu kontrolerów o tej samej nazwie, o ile mają one różne obszary. Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie innego parametru trasy, `area` do `controller` i `action`. W tej sekcji omówiono sposób, w jaki Routing współdziała z obszarami. Zobacz [obszary](xref:mvc/controllers/areas) , aby uzyskać szczegółowe informacje na temat sposobu używania obszarów w widokach.

Poniższy przykład konfiguruje MVC do używania domyślnej trasy konwencjonalnej i trasy `area` dla `area` o nazwie `Blog`:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

W poprzednim kodzie <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> jest wywoływana w celu utworzenia `"blog_route"`. Drugi parametr, `"Blog"`, jest nazwą obszaru.

W przypadku dopasowania ścieżki URL, takiej jak `/Manage/Users/AddUser`, trasa `"blog_route"` generuje wartości trasy `{ area = Blog, controller = Users, action = AddUser }`. `area` wartość trasy jest generowana przez wartość domyślną dla `area`. Trasa utworzona przez `MapAreaControllerRoute` jest równoważna następującym:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

`MapAreaControllerRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dla `area` przy użyciu podanej nazwy obszaru, w tym przypadku `Blog`. Wartość domyślna zapewnia, że trasa zawsze generuje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresów URL.

Routowanie konwencjonalne jest zależne od kolejności. Ogólnie rzecz biorąc, trasy z obszarami powinny być umieszczone wcześniej, ponieważ są bardziej specyficzne niż trasy bez obszaru.

Korzystając z powyższego przykładu, wartości trasy `{ area = Blog, controller = Users, action = AddUser }` są zgodne z następującą akcją:

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

Atrybut [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) wskazuje, co oznacza kontroler w ramach obszaru. Ten kontroler znajduje się w obszarze `Blog`. Kontrolery bez atrybutu `[Area]` nie są członkami żadnego obszaru i **nie pasują** , gdy wartość trasy `area` jest udostępniana przez usługę Routing. W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu na potrzeby kompletności. Jeśli powyższe kontrolery używają tego samego obszaru nazw, wygenerowany zostanie błąd kompilatora. Przestrzenie nazw klas nie mają wpływu na Routing MVC.

Pierwsze dwa kontrolery są członkami obszarów i pasują tylko wtedy, gdy ich nazwa obszaru jest podana przez wartość `area` trasy. Trzeci kontroler nie jest członkiem żadnego obszaru i może pasować tylko wtedy, gdy żadna wartość `area` nie jest udostępniana przez Routing.

<a name="aa"></a>

W warunkach pasujących do *nie ma żadnej wartości*, brak wartości `area` jest taka sama jak wtedy, gdy wartość `area` była zerowa lub ciągiem pustym.

Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` jest dostępna jako [wartość otoczenia](#ambient) dla routingu do użycia na potrzeby generowania adresów URL. Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

Poniższy kod generuje adres URL `/Zebra/Users/AddUser`:

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a>Definicja akcji

Metody publiczne na kontrolerze, z wyjątkiem tych z atrybutem nie będącym [akcją](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) , są akcjami.

## <a name="sample-code"></a>Przykładowy kod

 * Metoda [MyDisplayRouteInfo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x/main/Extensions/ControllerContextExtensions.cs) jest dołączana do [pobranego przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) i służy do wyświetlania informacji o routingu.
* [Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/routing/samples/3.x) ([jak pobrać](xref:index#how-to-download-a-sample))

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania. Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .

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

Poprzedni kod jest przykładem konwencjonalnego routingu. Ten styl jest nazywany routingiem konwencjonalnym, ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:

* Pierwszy segment ścieżki jest mapowany na nazwę kontrolera.
* Druga mapowanie na nazwę akcji.
* Trzeci segment jest używany w przypadku opcjonalnego `id`. `id` mapuje do jednostki modelu.

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
> *Dedykowane konwencjonalne trasy* często korzystają z parametrów trasy **catch-all** , takich jak `{*article}`, do przechwytywania pozostałej części ścieżki URL. Może to spowodować, że trasa "zbyt zachłanne" oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy. Umieść trasy "zachłanne" później w tabeli tras, aby rozwiązać ten problem.

### <a name="fallback"></a>Istnie

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

Akcja `ProductsApi.GetProduct(int)` zostanie wykonana dla ścieżki URL, takiej jak `/products/3`, ale nie dla ścieżki URL, takiej jak `/products`. Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](xref:fundamentals/routing) .

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

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów. Powyższy przykład będzie zachowywać się tak samo jak poniższy kod:

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

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

::: moniker-end

::: moniker range="= aspnetcore-2.2"

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


::: moniker range="< aspnetcore-3.0"
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

Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .

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

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>Routing mieszany: Routing atrybutów a konwencjonalny Routing

Aplikacje MVC mogą łączyć użycie konwencjonalnego routingu i routingu atrybutów. Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.

Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut. Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany. Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa. **Każdy** atrybut trasy na kontrolerze powoduje kierowanie wszystkich akcji w atrybucie kontrolera.

> [!NOTE]
> Co odróżnia dwa typy systemów routingu, proces stosowany po adresie URL pasuje do szablonu trasy. W przypadku routingu konwencjonalnego wartości trasy z dopasowania są używane do wybierania akcji i kontrolera z tabeli odnośników wszystkich konwencjonalnych akcji kierowanych. W obszarze Routing atrybutów każdy szablon jest już skojarzony z akcją i nie jest wymagany dalsze wyszukiwanie.

## <a name="complex-segments"></a>Złożone segmenty

Złożone segmenty (na przykład `[Route("/dog{token}cat")]`) są przetwarzane przez dopasowanie literałów od prawej do lewej w sposób niezachłanney. Sprawdź [kod źródłowy](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) opisu. Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/8197).

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>Generowanie adresu URL

Aplikacje MVC mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji. Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany. Ta sekcja koncentruje się na funkcjach generowania adresów URL dostarczonych przez MVC i obejmuje tylko podstawowe informacje na temat sposobu działania generowania adresów URL. Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](xref:fundamentals/routing) .

Interfejs `IUrlHelper` jest podstawową częścią infrastruktury między MVC i routingiem na potrzeby generowania adresów URL. Wystąpienie `IUrlHelper` dostępne za pomocą właściwości `Url` w obszarze Kontrolery, widoki i składniki widoku.

W tym przykładzie interfejs `IUrlHelper` jest używany przez właściwość `Controller.Url` do generowania adresu URL dla innej akcji.

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

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

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC kompiluje tabelę odnośników wszystkich akcji przypisanych do atrybutu i dopasowuje wartości `controller` i `action`, aby wybrać szablon trasy do użycia na potrzeby generowania adresów URL. W przykładzie powyżej jest generowana `custom/url/to/destination`.

### <a name="generating-urls-by-action-name"></a>Generowanie adresów URL według nazwy akcji

`Url.Action` (`IUrlHelper`. `Action`) i wszystkie powiązane przeciążenia są oparte na tym pomysłie, aby określić, do czego chcesz utworzyć łącze, określając nazwę kontrolera i nazwę akcji.

> [!NOTE]
> W przypadku korzystania z `Url.Action`, bieżące wartości trasy dla `controller` i `action` są określone dla Ciebie — wartość `controller` i `action` są częścią *wartości otoczenia* **i** *wartości*. Metoda `Url.Action`, zawsze używa bieżących wartości `action` i `controller` i wygeneruje ścieżkę URL, która kieruje do bieżącej akcji.

Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały wprowadzone podczas generowania adresu URL. Przy użyciu trasy, takiej jak `{a}/{b}/{c}/{d}` i wartości otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }`, routing ma wystarczającą ilość informacji do wygenerowania adresu URL bez dodatkowych wartości — ponieważ wszystkie parametry trasy mają wartość. W przypadku dodania wartości `{ d = Donovan }`wartość `{ d = David }` zostanie zignorowana, a wygenerowana Ścieżka adresu URL zostanie `Alice/Bob/Carol/Donovan`.

> [!WARNING]
> Ścieżki URL są hierarchiczne. W powyższym przykładzie, jeśli dodano wartość `{ c = Cheryl }`, obie wartości `{ c = Carol, d = David }` zostałyby zignorowane. W takim przypadku nie ma już wartości `d` a generowanie adresów URL zakończy się niepowodzeniem. Należy określić pożądaną wartość `c` i `d`.  Może wystąpić potrzeba napotkania tego problemu przy użyciu trasy domyślnej (`{controller}/{action}/{id?}`), ale w takim przypadku rzadko wystąpi to zachowanie, ponieważ `Url.Action` będzie zawsze jawnie określać `controller` i `action` wartość.

Dłuższe przeciążenia `Url.Action` również pobierają dodatkowy obiekt *wartości trasy* , aby zapewnić wartości parametrów trasy innych niż `controller` i `action`. Najczęściej zobaczysz ten sposób, który jest używany z `id`, takich jak `Url.Action("Buy", "Products", new { id = 17 })`. Według Konwencji obiekt *wartości trasy* jest zwykle obiektem typu anonimowego, ale może również być `IDictionary<>` lub *zwykłym starym obiektem platformy .NET*. Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> Aby utworzyć bezwzględny adres URL, Użyj przeciążenia, które akceptuje `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>Generowanie adresów URL według trasy

Powyższy kod wygeneruje adres URL przez przekazanie go do kontrolera i nazwy akcji. `IUrlHelper` zapewnia także `Url.RouteUrl` rodziny metod. Te metody są podobne do `Url.Action`, ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy. Najbardziej typowym zastosowaniem jest określenie nazwy trasy do użycia określonej trasy do wygenerowania adresu URL, na ogół *bez* określania kontrolera lub nazwy akcji.

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

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

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

W przypadku dopasowania ścieżki URL, takiej jak `/Manage/Users/AddUser`, Pierwsza trasa będzie generować wartości trasy `{ area = Blog, controller = Users, action = AddUser }`. `area` wartość trasy jest generowana przez wartość domyślną dla `area`, w rzeczywistości trasa utworzona przez `MapAreaRoute` jest równoważna następującym:

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dla `area` przy użyciu podanej nazwy obszaru, w tym przypadku `Blog`. Wartość domyślna zapewnia, że trasa zawsze generuje `{ area = Blog, ... }`, ograniczenie wymaga wartości `{ area = Blog, ... }` do generowania adresów URL.

> [!TIP]
> Routowanie konwencjonalne jest zależne od kolejności. Ogólnie rzecz biorąc, trasy z obszarami należy umieścić wcześniej w tabeli tras, ponieważ są one bardziej specyficzne niż trasy bez obszaru.

Korzystając z powyższego przykładu, wartości trasy będą zgodne z następującą akcją:

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` to oznacza, że w ramach obszaru znajduje się kontroler. Załóżmy, że ten kontroler znajduje się w obszarze `Blog`. Kontrolery bez atrybutu `[Area]` nie są członkami żadnego obszaru i **nie** będą zgodne, gdy wartość trasy `area` zostanie udostępniona przez Routing. W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }`.

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu w celu zapewnienia kompletności — w przeciwnym razie kontrolery będą mieć konflikt nazw i generują błąd kompilatora. Przestrzenie nazw klas nie mają wpływu na Routing MVC.

Pierwsze dwa kontrolery są członkami obszarów i pasują tylko wtedy, gdy ich nazwa obszaru jest podana przez wartość `area` trasy. Trzeci kontroler nie jest członkiem żadnego obszaru i może pasować tylko wtedy, gdy żadna wartość `area` nie jest udostępniana przez Routing.

> [!NOTE]
> W warunkach pasujących do *nie ma żadnej wartości*, brak wartości `area` jest taka sama jak wtedy, gdy wartość `area` była zerowa lub ciągiem pustym.

Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` będzie dostępna jako *wartość otoczenia* dla routingu do użycia na potrzeby generowania adresów URL. Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

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

::: moniker-end
