---
title: Routing w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób Routing ASP.NET Core jest odpowiedzialny za mapowanie identyfikatorów URI żądań na selektory punktów końcowych i wysyłanie żądań przychodzących do punktów końcowych.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2019
uid: fundamentals/routing
ms.openlocfilehash: 5e3ff65420b3c6769d52f8b96c216043cb1fdc1a
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727004"
---
# <a name="routing-in-aspnet-core"></a>Routing w ASP.NET Core

[Ryan Nowak](https://github.com/rynowak), [Steve Kowalski](https://ardalis.com/)i [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Routing jest odpowiedzialny za mapowanie identyfikatorów URI żądań na punkty końcowe i wysyłanie żądań przychodzących do tych punktów końcowych. Trasy są zdefiniowane w aplikacji i konfigurowane podczas uruchamiania aplikacji. Trasa może opcjonalnie wyodrębnić wartości z adresu URL zawartego w żądaniu. te wartości mogą być następnie używane do przetwarzania żądań. Przy użyciu informacji o trasie z aplikacji Routing jest również w stanie generować adresy URL mapowane na punkty końcowe. Wiele aplikacji nie musi dodawać tras wykraczających poza elementy oferowane przez szablony. Szablony ASP.NET Core dla kontrolerów i stron Razor konfigurują punkty końcowe tras. Jeśli musisz dodać niestandardowe punkty końcowe tras, niestandardowe punkty końcowe można skonfigurować obok punktów końcowych trasy generowanych przez szablon.

> [!IMPORTANT]
> Ten dokument obejmuje Routing ASP.NET Core niskiego poziomu. Aby uzyskać informacje na temat ASP.NET Core routingu MVC, zobacz <xref:mvc/controllers/routing>. Aby uzyskać informacje na temat Konwencji routingu w Razor Pages, zobacz <xref:razor-pages/razor-pages-conventions>.

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Podstawy routingu

Większość aplikacji powinna wybrać podstawowy i opisowy schemat routingu, aby adresy URL były czytelne i zrozumiałe. Domyślna trasa konwencjonalna `{controller=Home}/{action=Index}/{id?}`:

* Obsługuje podstawowy i opisowy schemat routingu.
* Jest użytecznym punktem wyjścia dla aplikacji opartych na interfejsie użytkownika.

Deweloperzy często dodają dodatkowe trasy zwięzła do obszarów o dużym ruchu w aplikacji w wyspecjalizowanych sytuacjach (na przykład blog i punkty końcowe handlu elektronicznego) przy użyciu [routingu atrybutów](xref:mvc/controllers/routing#attribute-routing) lub dedykowanych tras konwencjonalnych.

Interfejsy API sieci Web powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez zlecenia HTTP. Oznacza to, że wiele operacji (na przykład GET, POST) dla tego samego zasobu logicznego będzie używać tego samego adresu URL. Routing atrybutu zapewnia poziom kontroli, który jest wymagany do dokładnego projektowania układu publicznego punktu końcowego interfejsu API.

Aplikacje Razor Pages używają domyślnego routingu konwencjonalnego do obsłużenia nazwanych zasobów w folderze *strony* aplikacji. Dostępne są dodatkowe konwencje, które umożliwiają dostosowanie Razor Pages zachowaniem routingu. Aby uzyskać więcej informacji, zobacz <xref:razor-pages/index> i <xref:razor-pages/razor-pages-conventions>.

Obsługa generowania adresów URL umożliwia tworzenie aplikacji bez adresów URL, które mają być połączone ze sobą. Ta obsługa pozwala rozpocząć od podstawowej konfiguracji routingu i zmodyfikować trasy po ustaleniu układu zasobów aplikacji.

Funkcja routingu używa *punktów końcowych* (`Endpoint`) do reprezentowania logicznych punktów końcowych w aplikacji.

Punkt końcowy definiuje delegata do przetwarzania żądań i kolekcji dowolnych metadanych. Metadane służą do implementowania zagadnień związanych z wycinaniem w oparciu o zasady i konfigurację dołączone do każdego punktu końcowego.

System routingu ma następujące cechy:

* Składnia szablonu trasy służy do definiowania tras z parametrami trasy z tokenami.
* Dozwolona jest konfiguracja języka końcowego w stylu konwencjonalnym i stylu atrybutu.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> służy do określenia, czy parametr adresu URL zawiera prawidłową wartość dla danego ograniczenia punktu końcowego.
* Modele aplikacji, takie jak MVC/Razor Pages, rejestrują wszystkie punkty końcowe, które mają przewidywalne implementację scenariuszy routingu.
* Implementacja routingu podejmuje decyzje dotyczące routingu w dowolnym miejscu w potoku programu pośredniczącego.
* Oprogramowanie pośredniczące, które pojawia się po utworzeniu oprogramowania pośredniczącego, może sprawdzić wynik decyzji punktu końcowego usługi routingu dla danego identyfikatora URI żądania.
* Można wyliczyć wszystkie punkty końcowe w aplikacji w dowolnym miejscu w potoku programu pośredniczącego.
* Aplikacja może używać routingu do generowania adresów URL (na przykład w przypadku przekierowania lub linków) na podstawie informacji o punkcie końcowym, a tym samym unikania zakodowanych adresów URL, co ułatwia łatwość utrzymania.
* Generowanie adresów URL jest oparte na adresach, które obsługują arbitralną rozszerzalność:

  * Interfejs API generatora linków (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) można rozpoznać w dowolnym miejscu przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection) do generowania adresów URL.
  * Gdy interfejs API generatora linków nie jest dostępny za pośrednictwem programu DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> oferuje metody do kompilowania adresów URL.

> [!NOTE]
> Łączenie punktów końcowych jest ograniczone do akcji i stron programu MVC/Razor Pages. Rozszerzenia punktów końcowych konsolidacji są planowane dla przyszłych wersji.

Routing jest połączony z potokiem [pośredniczącym](xref:fundamentals/middleware/index) przez klasę <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) dodaje Routing do potoku oprogramowania pośredniczącego w ramach swojej konfiguracji i obsługuje routing w aplikacjach MVC i Razor Pages. Aby dowiedzieć się, jak używać routingu jako składnika autonomicznego, zapoznaj się z sekcją [Korzystanie z oprogramowania do routingu](#use-routing-middleware) .

### <a name="url-matching"></a>Dopasowanie adresu URL

Dopasowywanie adresów URL to proces, za pomocą którego Routing wysyła żądanie przychodzące do *punktu końcowego*. Ten proces jest oparty na danych w ścieżce URL, ale można go rozszerzyć w celu uwzględnienia wszelkich danych w żądaniu. Możliwość wysyłania żądań do oddzielnych programów obsługi jest kluczem do skalowania rozmiaru i złożoności aplikacji.

Po zakończeniu routingu oprogramowanie pośredniczące ustawia punkt końcowy (`Endpoint`) i kieruje wartości do funkcji na <xref:Microsoft.AspNetCore.Http.HttpContext>. Dla bieżącego żądania:

* Wywołanie `HttpContext.GetEndpoint` Pobiera punkt końcowy.
* `HttpRequest.RouteValues` Pobiera kolekcję wartości tras.

Oprogramowanie pośredniczące działające po stronie routingu pośredniczącego może zobaczyć punkt końcowy i podjąć odpowiednie działania. Na przykład, oprogramowanie pośredniczące autoryzacji może przejrzeć kolekcję metadanych punktu końcowego dla zasad autoryzacji. Gdy zostanie wykonane wszystkie oprogramowanie pośredniczące w potoku przetwarzania żądań, zostanie wywołany delegat wybranego punktu końcowego.

System routingu w ramach routingu punktów końcowych jest odpowiedzialny za wszystkie decyzje dotyczące wysyłania. Ponieważ oprogramowanie pośredniczące stosuje zasady oparte na wybranym punkcie końcowym, ważne jest, aby wszystkie decyzje, które mogą mieć wpływ na wysyłanie lub stosowanie zasad zabezpieczeń, zostały wprowadzone w systemie routingu.

Po wykonaniu delegata punktu końcowego właściwości [RouteContext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) są ustawiane na odpowiednie wartości na podstawie przetwarzania żądania wykonanego do tej pory.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) to słownik *wartości tras* uzyskanych z trasy. Te wartości są zwykle określane przez tokenizowanie jako adres URL i mogą służyć do akceptowania danych wejściowych użytkownika lub do dalszej akceptacji decyzji w aplikacji.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) to zbiór właściwości dodatkowych danych dotyczących dopasowanej trasy. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> zapewnia obsługę kojarzenia danych stanu z każdą trasą, dzięki czemu aplikacja może podejmować decyzje na podstawie dopasowanej trasy. Te wartości są zdefiniowane przez dewelopera i **nie** mają wpływu na zachowanie routingu. Ponadto wartości umieszczane w [RouteData. Datatokeny](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) mogą być dowolnego typu, w przeciwieństwie do [RouteData. wartości](xref:Microsoft.AspNetCore.Routing.RouteData.Values), które muszą być konwertowane do i z ciągów.

[RouteData. routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) to lista tras, które brały udział w pomyślnie dopasowane do żądania. Trasy mogą być zagnieżdżone wewnątrz siebie. Właściwość <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> odzwierciedla ścieżkę przez logiczne drzewo tras, które spowodowały dopasowanie. Ogólnie rzecz biorąc, pierwszy element w <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> jest kolekcją tras i powinien być używany do generowania adresów URL. Ostatnim elementem w <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> jest program obsługi trasy, który został dopasowany.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>Generowanie adresu URL za pomocą LinkGenerator

Generowanie adresu URL to proces, za pomocą którego Routing może utworzyć ścieżkę URL na podstawie zestawu wartości trasy. Pozwala to na logiczne rozdzielenie między punktami końcowymi i adresami URL, które uzyskują do nich dostęp.

Routing punktów końcowych obejmuje interfejs API generatora linków (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>). <xref:Microsoft.AspNetCore.Routing.LinkGenerator> to pojedyncza usługa, którą można pobrać z programu DI. Interfejsu API można używać poza kontekstem żądania wykonania. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> i scenariusze MVC, które opierają się na <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, takich jak [pomocników tagów](xref:mvc/views/tag-helpers/intro), pomocników HTML i [wynikach akcji](xref:mvc/controllers/actions), używają generatora linków, aby zapewnić możliwości generowania linków.

Generator łącza jest objęty koncepcją i *schematami* *adresów.* Schemat adresów jest sposobem określania punktów końcowych, które należy wziąć pod uwagę podczas generowania łącza. Na przykład nazwa trasy i wartości trasy scenariusze wielu użytkowników są znane z programu z MVC/Razor Pages są implementowane jako schemat adresów.

Generator łącza może łączyć się z akcjami i stronami MVC/Razor Pages za pośrednictwem następujących metod rozszerzających:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Przeciążenie tych metod przyjmuje argumenty, które zawierają `HttpContext`. Metody te są funkcjonalnie równoważne `Url.Action` i `Url.Page` ale oferują dodatkową elastyczność i opcje.

Metody `GetPath*` są najbardziej podobne do `Url.Action` i `Url.Page` w tym, że generują identyfikator URI zawierający ścieżkę bezwzględną. Metody `GetUri*` zawsze generują bezwzględny identyfikator URI zawierający schemat i hosta. Metody akceptujące `HttpContext` generują identyfikator URI w kontekście żądania wykonania. Użycie wartości tras w otoczeniu, ścieżki podstawowej adresu URL, schematu i hosta z żądania wykonania jest używane, chyba że zostaną zastąpione.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> jest wywoływana przy użyciu adresu. Generowanie identyfikatora URI występuje w dwóch krokach:

1. Adres jest powiązany z listą punktów końcowych, które pasują do adresu.
1. `RoutePattern` poszczególnych punktów końcowych są oceniane do momentu znalezienia wzorca tras pasującego do dostarczonych wartości. Wynikowe dane wyjściowe są łączone z innymi częściami identyfikatora URI dostarczanymi do generatora linków i zwracanymi.

Metody zapewniane przez <xref:Microsoft.AspNetCore.Routing.LinkGenerator> obsługują funkcje generowania linków standardowych dla dowolnego typu adresu. Najbardziej wygodnym sposobem korzystania z generatora łączy są metody rozszerzające, które wykonują operacje dla określonego typu adresu.

| Metoda rozszerzenia | Opis |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Generuje identyfikator URI z ścieżką bezwzględną na podstawie podanych wartości. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Generuje bezwzględny identyfikator URI na podstawie podanych wartości.             |

> [!WARNING]
> Zwróć uwagę na następujące konsekwencje wywołania <xref:Microsoft.AspNetCore.Routing.LinkGenerator> metod:
>
> * Użyj `GetUri*` metod rozszerzenia z zachowaniem ostrożności w konfiguracji aplikacji, która nie weryfikuje `Host` nagłówka żądań przychodzących. Jeśli nagłówek `Host` żądań przychodzących nie jest zweryfikowany, dane wejściowe żądania niezaufanego mogą być wysyłane z powrotem do klienta w identyfikatorach URI w widoku/stronie. Zalecamy, aby wszystkie aplikacje produkcyjne skonfigurowali swój serwer do sprawdzania poprawności nagłówka `Host` pod kątem znanych prawidłowych wartości.
>
> * Używaj <xref:Microsoft.AspNetCore.Routing.LinkGenerator> z przestrogą w oprogramowaniu pośredniczącym w połączeniu z `Map` lub `MapWhen`. `Map*` zmienia ścieżkę podstawową żądania wykonania, która ma wpływ na dane wyjściowe generowania łącza. Wszystkie <xref:Microsoft.AspNetCore.Routing.LinkGenerator> interfejsy API umożliwiają określanie ścieżki podstawowej. Zawsze określaj pustą ścieżkę bazową do cofnięcia `Map*`ma wpływ na generowanie linków.

## <a name="endpoint-routing"></a>Routing punktów końcowych

* Punkt końcowy trasy ma szablon, metadane i delegat żądania, który obsługuje odpowiedź punktu końcowego. Metadane służą do implementowania zagadnień związanych z wycinaniem w oparciu o zasady i konfigurację dołączone do każdego punktu końcowego. Na przykład, oprogramowanie pośredniczące autoryzacji może przejrzeć kolekcję metadanych punktu końcowego dla [zasad autoryzacji](xref:security/authorization/policies#applying-policies-to-mvc-controllers).
* Routing punktów końcowych integruje się z oprogramowanie pośredniczące przy użyciu dwóch metod rozszerzających:
  * [UseRouting](xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting*) dodaje dopasowanie trasy do potoku programu pośredniczącego. Musi ona występować przed wszelkimi wykorzystanymi przez trasę programem pośredniczącym, takim jak autoryzacja, wykonywaniem punktów końcowych itd.
  * [UseEndpoints](xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints*) dodaje wykonywanie punktu końcowego do potoku programu pośredniczącego. Uruchamia delegata żądania, który obsługuje odpowiedź punktu końcowego.
  `UseEndpoints` jest również miejscem, w którym skonfigurowano punkty końcowe trasy, które mogą być dopasowane i wykonywane przez aplikację. Na przykład <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>i <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>.
* Aplikacje używają metod pomocnika ASP.NET Core, aby skonfigurować ich trasy. Struktury ASP.NET Core zapewniają metody pomocnika, takie jak <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers*> i `MapHub<THub>`. Istnieją również metody pomocnika do konfigurowania własnych niestandardowych punktów końcowych tras: <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapGet*>, <xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions.MapPost*>i [MapVerb](xref:Microsoft.AspNetCore.Builder.EndpointRouteBuilderExtensions). 
* Routing punktu końcowego obsługuje również punkty końcowe zmieniające się po uruchomieniu aplikacji. Aby można było obsługiwać ten element w aplikacji lub ASP.NET Core Framework, należy utworzyć i zarejestrować niestandardowy <xref:Microsoft.AspNetCore.Routing.EndpointDataSource>. Jest to funkcja zaawansowana i zazwyczaj nie jest wymagana. Punkty końcowe są zwykle konfigurowane podczas uruchamiania i są statyczne przez okres istnienia aplikacji. Ładowanie konfiguracji trasy z pliku lub bazy danych podczas uruchamiania nie jest dynamiczne.

Poniższy kod przedstawia podstawowy przykład routingu punktów końcowych:

[!code-csharp[](routing/samples/3.x/Startup.cs?name=snippet)]

Aby uzyskać więcej informacji na temat routingu punktów końcowych, zobacz [dopasowanie adresów URL](#url-matching) w tym dokumencie.

## <a name="endpoint-routing-differences-from-earlier-versions-of-routing"></a>Różnice w kierowaniu punktów końcowych z wcześniejszych wersji routingu

Istnieje kilka różnic między routingiem punktu końcowego i wersjami routingu wcześniejszego niż w ASP.NET Core 2,2:

* System routingu punktu końcowego nie obsługuje rozszerzalności opartej na <xref:Microsoft.AspNetCore.Routing.IRouter>, w tym dziedziczenie z <xref:Microsoft.AspNetCore.Routing.Route>.

* Routing punktów końcowych nie obsługuje [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Użyj [wersji zgodności](xref:mvc/compatibility-version) 2,1 (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`), aby nadal korzystać z podkładki zgodności.

* Routing punktów końcowych ma inne zachowanie dla wielkości liter wygenerowanych identyfikatorów URI podczas korzystania z konwencjonalnych tras.

  Rozważmy następujący szablon trasy domyślnej:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Załóżmy, że generujesz link do akcji przy użyciu następującej trasy:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  W przypadku routingu opartego na <xref:Microsoft.AspNetCore.Routing.IRouter>ten kod generuje identyfikator URI `/blog/ReadPost/17`, co uwzględnia wielkość liter podanej trasy. Routing punktów końcowych w ASP.NET Core 2,2 lub późniejszych produkuje `/Blog/ReadPost/17` ("blog" jest pisane wielkimi literami). Routing punktów końcowych udostępnia interfejs `IOutboundParameterTransformer`, którego można użyć do dostosowania tego zachowania globalnie lub w celu zastosowania różnych konwencji do mapowania adresów URL.

  Aby uzyskać więcej informacji, zobacz sekcję [informacje dotyczące transformatora parametrów](#parameter-transformer-reference) .

* Generowanie linków używane przez MVC/Razor Pages z trasami konwencjonalnymi działa inaczej podczas próby połączenia z kontrolerem/akcją lub stroną, która nie istnieje.

  Rozważmy następujący szablon trasy domyślnej:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Załóżmy, że generowany jest link do akcji przy użyciu szablonu domyślnego z następującymi:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  W przypadku routingu opartego na `IRouter`wynik jest zawsze `/Blog/ReadPost/17`, nawet jeśli `BlogController` nie istnieje lub nie ma metody akcji `ReadPost`. Zgodnie z oczekiwaniami Routing punktów końcowych w ASP.NET Core 2,2 lub nowszej tworzy `/Blog/ReadPost/17`, jeśli istnieje metoda akcji. *Jednak w przypadku, gdy akcja nie istnieje, routing punktu końcowego tworzy pusty ciąg.* Koncepcyjnie, routing punktu końcowego nie zakłada, że punkt końcowy istnieje, jeśli akcja nie istnieje.

* *Algorytm unieważniania wartości otoczenia* generacji linku działa inaczej, gdy jest używany z routingiem punktów końcowych.

  *Nieprawidłowa wartość otoczenia* , która decyduje o tym, które wartości trasy z aktualnie wykonywanego żądania (wartości otoczenia) mogą być używane w operacjach generowania linków. Routowanie konwencjonalne zawsze unieważnia dodatkowe wartości trasy podczas łączenia z inną akcją. Routing atrybutów nie miał tego zachowania przed wydaniem ASP.NET Core 2,2. We wcześniejszych wersjach ASP.NET Core linki do innej akcji, która używa tych samych nazw parametrów trasy, spowodowały błędy generacji łącza. W ASP.NET Core 2,2 lub nowszej, obie formy routingu unieważnią wartości podczas łączenia z inną akcją.

  Rozważmy następujący przykład w ASP.NET Core 2,1 lub starszej. W przypadku łączenia z inną akcją (lub inną stroną) wartości trasy mogą być ponownie używane na różne sposoby.

  W */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  W */Pages/login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  Jeśli identyfikator URI jest `/Store/Product/18` w ASP.NET Core 2,1 lub starszej, link wygenerowany na stronie Sklep/informacje przez `@Url.Page("/Login")` jest `/Login/18`. Wartość 18 zostanie ponownie użyta, mimo że lokalizacja docelowa linku jest zupełnie inna częścią aplikacji. `id` `id` wartość trasy w kontekście strony `/Login` jest prawdopodobnie wartością identyfikatora użytkownika, a nie wartością identyfikatora produktu w sklepie.

  W przypadku routingu punktów końcowych z ASP.NET Core 2,2 lub nowszym wynik jest `/Login`. Wartości otoczenia nie są ponownie używane, gdy połączone miejsce docelowe jest inną akcją lub stroną.

* Składnia parametru trasy okrężnej: ukośniki nie są kodowane przy użyciu podwójnej gwiazdki (`**`) składni parametrów.

  Podczas generowania łącza system routingu koduje wartość przechwyconą w dwugwiazdkowym (`**`) parametrem catch-all (na przykład `{**myparametername}`), z wyjątkiem ukośników. Podwójna gwiazdka "catch-all" jest obsługiwana w przypadku routingu opartego na `IRouter`w ASP.NET Core 2,2 lub nowszym.

  Jedyna gwiazdka "catch-all" w poprzednich wersjach ASP.NET Core (`{*myparametername}`) pozostaje obsługiwana, a ukośniki są zakodowane.

  | Trasa              | Wygenerowano łącze<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (ukośnik zostanie zakodowany)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Przykład oprogramowania pośredniczącego

W poniższym przykładzie oprogramowanie pośredniczące używa interfejsu API <xref:Microsoft.AspNetCore.Routing.LinkGenerator>, aby utworzyć łącze do metody akcji, która wyświetla listę produktów ze sklepu. Przy użyciu generatora linków poprzez wstrzyknięcie go do klasy i wywołanie `GenerateLink` jest dostępny dla każdej klasy w aplikacji.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a>Tworzenie tras

Większość aplikacji tworzy trasy przez wywołanie <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> lub jednej z podobnych metod rozszerzających zdefiniowanych w <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Dowolna z metod rozszerzenia <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> tworzy wystąpienie <xref:Microsoft.AspNetCore.Routing.Route> i dodaje je do kolekcji tras.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> nie akceptuje parametru procedury obsługi trasy. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> dodaje tylko trasy, które są obsługiwane przez <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Aby dowiedzieć się więcej na temat routingu w MVC, zobacz <xref:mvc/controllers/routing>.

Poniższy przykład kodu jest przykładem wywołania <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> używanego przez typową definicję trasy ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ten szablon dopasowuje ścieżkę URL i wyodrębnia wartości tras. Na przykład ścieżka `/Products/Details/17` generuje następujące wartości trasy: `{ controller = Products, action = Details, id = 17 }`.

Wartości tras są określane przez podzielenie ścieżki URL na segmenty i dopasowanie do każdego segmentu przy użyciu nazwy *parametru trasy* w szablonie trasy. Parametry trasy są nazywane. Parametry zdefiniowane przez ujęcie nazwy parametru w nawiasach klamrowych `{ ... }`.

Poprzedni szablon może być również zgodny z ścieżką URL `/` i generować wartości `{ controller = Home, action = Index }`. Dzieje się tak, ponieważ parametry `{controller}` i `{action}` trasy mają wartości domyślne, a parametr trasy `id` jest opcjonalny. Znak równości (`=`), po którym następuje wartość po nazwie parametru trasy definiuje wartość domyślną dla parametru. Znak zapytania (`?`) po nazwie parametru trasy definiuje opcjonalny parametr.

Parametry trasy z wartością domyślną *zawsze* generują wartość trasy w przypadku dopasowania trasy. Parametry opcjonalne nie generują wartości trasy, jeśli nie ma odpowiedniego segmentu ścieżki adresu URL. Szczegółowe opisy scenariuszy i składni szablonów tras można znaleźć w sekcji [Dokumentacja dotycząca szablonu trasy](#route-template-reference) .

W poniższym przykładzie definicja parametru trasy `{id:int}` definiuje [ograniczenie trasy](#route-constraint-reference) dla parametru trasy `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ten szablon dopasowuje ścieżkę URL, taką jak `/Products/Details/17`, ale nie `/Products/Details/Apples`. Ograniczenia trasy implementują <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> i sprawdzają wartości routingu w celu ich zweryfikowania. W tym przykładzie wartość trasy `id` musi być możliwa do przekonwertowania na liczbę całkowitą. Aby uzyskać wyjaśnienie ograniczeń trasy dostarczonych przez platformę, zobacz temat [ograniczenia trasy](#route-constraint-reference) .

Dodatkowe przeciążenia <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> akceptują wartości `constraints`, `dataTokens`i `defaults`. Typowym użyciem tych parametrów jest przekazanie anonimowo wpisanego obiektu, gdzie nazwy właściwości typu anonimowego są zgodne z nazwami parametrów trasy.

Poniższe <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> przykłady tworzą równoważne trasy:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> Wbudowana Składnia służąca do definiowania ograniczeń i wartości domyślnych może być wygodna dla prostych tras. Istnieją jednak scenariusze, takie jak tokeny danych, które nie są obsługiwane przez składnię wbudowaną.

Poniższy przykład ilustruje kilka dodatkowych scenariuszy:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Poprzedni szablon pasuje do ścieżki URL, takiej jak `/Blog/All-About-Routing/Introduction`, i wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Domyślne wartości trasy dla `controller` i `action` są generowane przez trasę, nawet jeśli w szablonie nie ma odpowiednich parametrów trasy. Wartości domyślne można określić w szablonie trasy. Parametr trasy `article` jest zdefiniowany jako *przechwycenie* przez wygląd podwójnej gwiazdki (`**`) przed nazwą parametru trasy. Catch-wszystkie parametry tras przechwytują resztę ścieżki URL i mogą również pasować do pustego ciągu.

Poniższy przykład dodaje ograniczenia trasy i tokeny danych:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Poprzedni szablon pasuje do ścieżki URL, takiej jak `/en-US/Products/5`, i wyodrębnia wartości `{ controller = Products, action = Details, id = 5 }` i `{ locale = en-US }`tokenów danych.

![Lokalne tokeny systemu Windows](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generowanie adresu URL klasy trasy

Klasa <xref:Microsoft.AspNetCore.Routing.Route> może również wykonywać generowanie adresów URL przez połączenie zestawu wartości tras z jego szablonem trasy. Jest to logicznie proces odwrotny pasujący do ścieżki URL.

> [!TIP]
> Aby lepiej zrozumieć generowanie adresów URL, Załóżmy, jaki adres URL ma zostać wygenerowany, a następnie pomyśl o sposobie dopasowania szablonu trasy do tego adresu URL. Jakie wartości zostałyby wygenerowane? Jest to sztywny odpowiednik działania generowania adresów URL w klasie <xref:Microsoft.AspNetCore.Routing.Route>.

Poniższy przykład używa ogólnej trasy domyślnej ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Po `{ controller = Products, action = List }`wartości trasy zostanie wygenerowany adres URL `/Products/List`. Wartości trasy są zastępowane odpowiednimi parametrami trasy w celu utworzenia ścieżki URL. Ponieważ `id` jest opcjonalnym parametrem trasy, adres URL został pomyślnie wygenerowany bez wartości dla `id`.

Po `{ controller = Home, action = Index }`wartości trasy zostanie wygenerowany adres URL `/`. Podane wartości trasy są zgodne z wartościami domyślnymi, a segmenty odpowiadające wartościom domyślnym są bezpiecznie pomijane.

Oba adresy URL wygenerowały rundę z następującą definicją trasy (`/Home/Index` i `/`) tworzą te same wartości trasy, które zostały użyte do wygenerowania adresu URL.

> [!NOTE]
> Aplikacja używająca ASP.NET Core MVC powinna używać <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> do generowania adresów URL zamiast bezpośredniego wywoływania do routingu.

Aby uzyskać więcej informacji na temat generowania adresów URL, zobacz sekcję [informacje dotyczące generowania adresów URL](#url-generation-reference) .

## <a name="use-routing-middleware"></a>Korzystanie z oprogramowania pośredniczącego routingu

Odwołuje się do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) w pliku projektu aplikacji.

Dodawanie routingu do kontenera usługi w `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Trasy muszą być skonfigurowane w metodzie `Startup.Configure`. Przykładowa aplikacja używa następujących interfejsów API:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; dopasowuje tylko żądania HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

W poniższej tabeli przedstawiono odpowiedzi z podanym identyfikatorem URI.

| Identyfikator URI                    | Odpowiedź                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Cześć! Wartości trasy: [Operation, Create], [ID, 3] |
| `/package/track/-3`    | Cześć! Wartości trasy: [Operation, Track], [ID,-3] |
| `/package/track/-3/`   | Cześć! Wartości trasy: [Operation, Track], [ID,-3] |
| `/package/track/`      | Żądanie przepada w, brak dopasowania.              |
| `GET /hello/Joe`       | Witaj, Jan!                                          |
| `POST /hello/Joe`      | Żądanie przepada w, dopasowuje tylko HTTP GET. |
| `GET /hello/Joe/Smith` | Żądanie przepada w, brak dopasowania.              |

Struktura zawiera zestaw metod rozszerzających do tworzenia tras (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

Metody `Map[Verb]` używają ograniczeń do ograniczenia trasy do zlecenia HTTP w nazwie metody. Na przykład zobacz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> i <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Odwołanie do szablonu trasy

Tokeny w nawiasach klamrowych (`{ ... }`) definiują *Parametry trasy* , które są powiązane w przypadku dopasowania trasy. Można zdefiniować więcej niż jeden parametr trasy w segmencie trasy, ale muszą one być oddzielone wartością literału. Na przykład `{controller=Home}{action=Index}` nie jest prawidłową trasą, ponieważ nie ma żadnej wartości literału między `{controller}` i `{action}`. Te parametry trasy muszą mieć nazwę i mogą mieć określone dodatkowe atrybuty.

Tekst literału inny niż parametry trasy (na przykład `{id}`) i separator ścieżki `/` musi być zgodny z tekstem w adresie URL. W dopasowaniu do tekstu nie jest rozróżniana wielkość liter i w oparciu o zdekodowaną reprezentację ścieżki URL. Aby dopasować Ogranicznik parametru trasy literału (`{` lub `}`), należy wprowadzić ogranicznik przez powtórzenie znaku (`{{` lub `}}`).

Wzorce adresów URL, które próbują przechwycić nazwę pliku z opcjonalnym rozszerzeniem pliku, mają dodatkowe uwagi. Rozważmy na przykład szablon `files/{filename}.{ext?}`. Gdy istnieją wartości dla obu `filename` i `ext`, są wypełniane obie wartości. Jeśli w adresie URL istnieje tylko wartość `filename`, trasa pasuje do, ponieważ końcowy okres (`.`) jest opcjonalny. Następujące adresy URL pasują do tej trasy:

* `/files/myFile.txt`
* `/files/myFile`

Możesz użyć gwiazdki (`*`) lub podwójnej gwiazdki (`**`) jako prefiksu do parametru trasy, aby powiązać z pozostałą częścią identyfikatora URI. Są one nazywane parametrami *przechwycenia* . Na przykład `blog/{**slug}` dopasowuje dowolny identyfikator URI, który rozpoczyna się od `/blog` i ma dowolną wartość, która jest przypisana do wartości `slug` Route. Catch-wszystkie parametry można także dopasować do pustego ciągu.

Parametr catch-all wyprowadza odpowiednie znaki, gdy trasa jest używana do generowania adresu URL, w tym znaków separatora ścieżki (`/`). Na przykład `foo/{*path}` trasy z wartościami trasy `{ path = "my/path" }` generuje `foo/my%2Fpath`. Zwróć uwagę na odwrócony ukośnik. Aby zaokrąglić znaki separatora ścieżki, użyj prefiksu parametru `**` Route. `foo/{**path}` trasy z `{ path = "my/path" }` generuje `foo/my/path`.

Parametry trasy mogą mieć *wartości domyślne* , określając wartość domyślną po nazwie parametru oddzielone znakiem równości (`=`). Na przykład `{controller=Home}` definiuje `Home` jako wartość domyślną dla `controller`. Wartość domyślna jest używana, jeśli żadna wartość nie jest obecna w adresie URL dla parametru. Parametry tras są opcjonalne przez dołączenie znaku zapytania (`?`) na końcu nazwy parametru, jak w `id?`. Różnica między wartościami opcjonalnymi i domyślnymi parametrami trasy polega na tym, że parametr trasy z wartością domyślną zawsze generuje wartość&mdash;opcjonalny parametr ma wartość tylko wtedy, gdy wartość jest podana przez adres URL żądania.

Parametry trasy mogą mieć ograniczenia, które muszą być zgodne z wartością trasy powiązaną z adresem URL. Dodanie dwukropek (`:`) i nazwy ograniczenia po nazwie parametru trasy określa *ograniczenie wbudowane* dla parametru trasy. Jeśli ograniczenie wymaga argumentów, są one ujęte w nawiasy (`(...)`) po nazwie ograniczenia. Można określić wiele ograniczeń wbudowanych, dołączając inną dwukropek (`:`) i nazwę ograniczenia.

Nazwa i argumenty ograniczenia są przesyłane do usługi <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> w celu utworzenia wystąpienia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> do użycia w przetwarzaniu adresów URL. Na przykład szablon trasy `blog/{article:minlength(10)}` określa ograniczenie `minlength` z argumentem `10`. Aby uzyskać więcej informacji na temat ograniczeń trasy i listę ograniczeń zapewnianych przez platformę, zobacz sekcję [odwołanie do ograniczenia trasy](#route-constraint-reference) .

Parametry trasy mogą także mieć Transformatory parametrów, które przekształcają wartość parametru podczas generowania linków i dopasowywania stron do adresów URL. Podobnie jak ograniczenia, transformatory parametrów mogą być dodawane wewnętrznie do parametru trasy przez dodanie dwukropka (`:`) i nazwy transformatora po nazwie parametru trasy. Na przykład szablon trasy `blog/{article:slugify}` określa `slugify` transformator. Aby uzyskać więcej informacji na temat transformatorów parametrów, zobacz sekcję [informacje dotyczące transformatora parametrów](#parameter-transformer-reference) .

W poniższej tabeli przedstawiono przykładowe szablony tras i ich zachowanie.

| Szablon trasy                           | Przykładowy pasujący identyfikator URI    | &hellip; identyfikatora URI żądania                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Dopasowuje tylko jedną ścieżkę `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Dopasowuje i ustawia `Page`, aby `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Dopasowuje i ustawia `Page`, aby `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Mapuje na kontroler `Products` i akcję `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Mapuje na kontroler `Products` i akcję `Details` (`id` ustawioną na 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Mapuje na kontroler `Home` i metodę `Index` (`id` jest ignorowana).        |

Użycie szablonu jest ogólnie najprostszym podejściem do routingu. Ograniczenia i wartości domyślne można także określić poza szablonem trasy.

> [!TIP]
> Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby zobaczyć, jak wbudowane implementacje routingu, takie jak <xref:Microsoft.AspNetCore.Routing.Route>, pasują do żądań.

## <a name="reserved-routing-names"></a>Zastrzeżone nazwy routingu

Następujące słowa kluczowe są nazwami zarezerwowanymi i nie mogą być używane jako nazwy tras lub parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odwołanie do ograniczenia trasy

Ograniczenia trasy są wykonywane, gdy nastąpiło dopasowanie do przychodzącego adresu URL, a Ścieżka adresu URL jest zgodna z wartościami trasy. Ograniczenia trasy zwykle sprawdzają wartość trasy skojarzoną za pośrednictwem szablonu trasy i podejmują decyzję o tym, czy wartość jest akceptowalna. Niektóre ograniczenia trasy używają danych poza wartością trasy, aby uwzględnić, czy żądanie może być kierowane. Na przykład <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> może zaakceptować lub odrzucić żądanie na podstawie jego zlecenia HTTP. Ograniczenia są używane w żądaniach routingu i generowania linków.

> [!WARNING]
> Nie używaj ograniczeń **sprawdzania poprawności danych wejściowych**. Jeśli ograniczenia są używane do **sprawdzania poprawności danych wejściowych**, nieprawidłowe dane wejściowe w odpowiedzi *404 — nie znaleziono* , zamiast *żądania 400-złe* z odpowiednim komunikatem o błędzie. Ograniczenia trasy są **używane do** odróżniania podobnych tras, a nie do sprawdzania danych wejściowych dla określonej trasy.

W poniższej tabeli przedstawiono przykładowe ograniczenia trasy i ich oczekiwane zachowanie.

| ograniczenie | Przykład | Przykładowe dopasowania | Uwagi |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Dopasowuje dowolną liczbę całkowitą |
| `bool` | `{active:bool}` | `true`, `FALSE` | Dopasowuje `true` lub `false` (bez uwzględniania wielkości liter) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Dopasowuje prawidłową wartość `DateTime` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Dopasowuje prawidłową wartość `decimal` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Dopasowuje prawidłową wartość `double` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Dopasowuje prawidłową wartość `float` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Dopasowuje prawidłową wartość `Guid` |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Dopasowuje prawidłową wartość `long` |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Ciąg musi składać się z co najmniej 4 znaków |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Ciąg nie może zawierać więcej niż 8 znaków |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Ciąg musi zawierać dokładnie 12 znaków |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Ciąg musi mieć długość co najmniej 8 znaków |
| `min(value)` | `{age:min(18)}` | `19` | Wartość całkowita musi być równa co najmniej 18 |
| `max(value)` | `{age:max(120)}` | `91` | Wartość całkowita nie może być większa niż 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Wartość całkowita musi być równa co najmniej 18, ale nie więcej niż 120 |
| `alpha` | `{name:alpha}` | `Rick` | Ciąg musi zawierać co najmniej jeden znak alfabetyczny (`a`-`z`, bez uwzględniania wielkości liter) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Ciąg musi być zgodny z wyrażeniem regularnym (zobacz Porady dotyczące definiowania wyrażenia regularnego) |
| `required` | `{name:required}` | `Rick` | Służy do wymuszania, że podczas generowania adresu URL jest obecna wartość, która nie jest wartością parametru |

Wielokrotne ograniczenia rozdzielane średnikami można zastosować do jednego parametru. Na przykład następujące ograniczenie ogranicza parametr do wartości całkowitej 1 lub wyższej:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Ograniczenia trasy, które weryfikują adres URL i są konwertowane na typ CLR (takie jak `int` lub `DateTime`), zawsze używają niezmiennej kultury. W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny. Ograniczenia trasy dostarczone przez platformę nie modyfikują wartości przechowywanych w wartościach trasy. Wszystkie wartości tras analizowane na podstawie adresu URL są przechowywane jako ciągi. Na przykład, ograniczenie `float` próbuje skonwertować wartość trasy na float, ale przekonwertowana wartość jest używana tylko w celu sprawdzenia, czy można ją przekonwertować na wartość zmiennoprzecinkową.

## <a name="regular-expressions"></a>Wyrażenia regularne

ASP.NET Core Framework dodaje `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` do konstruktora wyrażeń regularnych. Aby uzyskać opis tych elementów członkowskich, zobacz <xref:System.Text.RegularExpressions.RegexOptions>.

Wyrażenia regularne używają ograniczników i tokenów podobnych do tych używanych w ramach routingu i C# języka. Tokeny wyrażenia regularnego muszą być zmienione. Aby użyć wyrażenia regularnego `^\d{3}-\d{2}-\d{4}$` w routingu, wyrażenie musi mieć `\` (pojedynczy ukośnik odwrotny) podany w ciągu jako `\\` (podwójny ukośnik odwrotny) w pliku C# źródłowym w celu wypróbowania znaku ucieczki `\` ciągu znaków (chyba że jest używany [literały ciągu Verbatim](/dotnet/csharp/language-reference/keywords/string)). Na znaki ogranicznika parametru routingu ucieczki (`{`, `}`, `[`, `]`), podwójne znaki w wyrażeniu (`{{`, `}`, `[[`, `]]`). W poniższej tabeli przedstawiono wyrażenie regularne i wersja z ucieczką.

| Wyrażenie regularne    | Wyrażenie regularne o zmienionym znaczeniu     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Wyrażenia regularne używane w routingu często zaczynają się od znaku daszka (`^`) i dopasowują pozycję początkową ciągu. Wyrażenia często kończą się znakiem dolara (`$`) i końcem ciągu. Znaki `^` i `$` zapewniają, że wyrażenie regularne dopasowuje całą wartość parametru trasy. Bez znaków `^` i `$` wyrażenie regularne dopasowuje dowolny podciąg w ciągu, co jest często niepożądane. W poniższej tabeli przedstawiono przykłady i wyjaśniono, dlaczego są one zgodne lub niezgodne.

| Wyrażenie   | String    | Dopasowanie | Komentarz               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Cześć     | Tak   | Dopasowania podciągów     |
| `[a-z]{2}`   | 123abc456 | Tak   | Dopasowania podciągów     |
| `[a-z]{2}`   | mz        | Tak   | Wyrażenie dopasowania    |
| `[a-z]{2}`   | MZ        | Tak   | Bez uwzględniania wielkości liter    |
| `^[a-z]{2}$` | Cześć     | Nie    | Zobacz `^` i `$` powyżej |
| `^[a-z]{2}$` | 123abc456 | Nie    | Zobacz `^` i `$` powyżej |

Aby uzyskać więcej informacji na temat składni wyrażeń regularnych, zobacz [.NET Framework wyrażeń regularnych](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Aby ograniczyć parametr do znanego zestawu możliwych wartości, użyj wyrażenia regularnego. Na przykład `{action:regex(^(list|get|create)$)}` dopasowuje wartość trasy `action` do `list`, `get`lub `create`. Jeśli przeszedł do słownika ograniczeń, ciąg `^(list|get|create)$` jest równoważny. Ograniczenia, które są przesyłane w słowniku ograniczenia (nie wbudowane w szablon), które nie pasują do jednego ze znanych ograniczeń, są również traktowane jako wyrażenia regularne.

## <a name="custom-route-constraints"></a>Niestandardowe ograniczenia trasy

Oprócz wbudowanych ograniczeń trasy niestandardowe ograniczenia trasy mogą być tworzone przez implementację interfejsu <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. Interfejs <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> zawiera pojedynczą metodę, `Match`, która zwraca `true` w przypadku spełnienia ograniczenia i `false` w inny sposób.

Aby można było użyć niestandardowego <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, typ ograniczenia trasy musi być zarejestrowany w <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> aplikacji w kontenerze usługi aplikacji. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> jest słownikiem, który mapuje klucze ograniczeń trasy na implementacje <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, które weryfikują te ograniczenia. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> aplikacji można zaktualizować w `Startup.ConfigureServices` w ramach [usług. Wywołanie addrouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) lub przez skonfigurowanie <xref:Microsoft.AspNetCore.Routing.RouteOptions> bezpośrednio z `services.Configure<RouteOptions>`. Na przykład:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Ograniczenie można następnie zastosować do tras w zwykły sposób, przy użyciu nazwy określonej podczas rejestrowania typu ograniczenia. Na przykład:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>Odwołanie do transformatora parametrów

Transformatory parametrów:

* Wykonaj podczas generowania linku do <xref:Microsoft.AspNetCore.Routing.Route>.
* Zaimplementuj `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Konfiguruje się za pomocą <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Wypełnij wartość trasy parametru i Przekształć ją na nową wartość ciągu.
* Wynikiem jest użycie przekształconej wartości w wygenerowanym łączu.

Na przykład niestandardowy `slugify` przekształcania parametrów w wzorcu trasy `blog\{article:slugify}` z `Url.Action(new { article = "MyTestArticle" })` generuje `blog\my-test-article`.

Aby użyć transformatora parametrów w wzorcu trasy, skonfiguruj go najpierw przy użyciu <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> w `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Transformatory parametrów są używane przez platformę do przekształcania identyfikatora URI, w którym jest rozpoznawany punkt końcowy. Na przykład ASP.NET Core MVC używa transformatorów parametrów do przekształcenia wartości trasy używanej w celu dopasowania do `area`, `controller`, `action`i `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

W przypadku powyższej trasy `SubscriptionManagementController.GetAll()` akcji jest dopasowywany do `/subscription-management/get-all`URI. Transformator parametrów nie zmienia wartości trasy użytych do wygenerowania linku. Na przykład `Url.Action("GetAll", "SubscriptionManagement")` dane wyjściowe `/subscription-management/get-all`.

ASP.NET Core udostępnia konwencje interfejsu API do używania transformatorów parametrów z wygenerowanymi trasami:

* ASP.NET Core MVC ma konwencję interfejsu API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Ta Konwencja stosuje określony transformator parametrów do wszystkich tras atrybutów w aplikacji. Transformator parametrów przekształca tokeny trasy atrybutów po ich wymianie. Aby uzyskać więcej informacji, zobacz [używanie transformatora parametrów do dostosowywania zastępowania tokenu](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages ma konwencję interfejsu API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Ta Konwencja stosuje określony transformator parametrów do wszystkich automatycznie odnalezionych Razor Pages. Transformator parametrów przekształca segmenty i nazwy plików Razor Pages tras. Aby uzyskać więcej informacji, zobacz [używanie transformatora parametrów do dostosowywania tras stron](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>Odwołanie do generacji adresów URL

Poniższy przykład pokazuje, jak wygenerować link do trasy, używając słownika wartości tras i <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> wygenerowany na końcu powyższego przykładu jest `/package/create/123`. Słownik dostarcza wartości `operation` i `id` trasy szablonu "śledzenie trasy pakietów", `package/{operation}/{id}`. Aby uzyskać szczegółowe informacje, zapoznaj się z przykładowym kodem w sekcji [Używanie oprogramowania pośredniczącego usługi routingu](#use-routing-middleware) lub [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Drugim parametrem konstruktora <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> jest zbiór *wartości otoczenia*. Wartości otoczenia są wygodne do użycia, ponieważ ograniczają liczbę wartości, które Deweloper musi określić w kontekście żądania. Bieżące wartości trasy bieżącego żądania są uznawane za wartości otoczenia dla generacji łącza. W `About` akcji ASP.NET Core aplikacji MVC `HomeController`nie trzeba określać wartości trasy kontrolera do łączenia z akcją `Index`,&mdash;zostanie użyta wartość otoczenia `Home`.

Wartości otoczenia, które nie pasują do parametru, są ignorowane. Wartości otoczenia są również ignorowane, gdy jawnie podana wartość przesłania wartość otoczenia. Dopasowanie występuje od lewej do prawej w adresie URL.

Wartości jawnie podane, ale które nie pasują do segmentu trasy, są dodawane do ciągu zapytania. W poniższej tabeli przedstawiono wyniki przy użyciu szablonu trasy `{controller}/{action}/{id?}`.

| Wartości otoczenia                     | Wartości jawne                        | Wynik                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| Controller = "Strona główna"                | Action = "informacje"                       | `/Home/About`           |
| Controller = "Strona główna"                | Controller = "Order", Action = "informacje" | `/Order/About`          |
| Controller = "Home", Color = "Red" | Action = "informacje"                       | `/Home/About`           |
| Controller = "Strona główna"                | Action = "informacje", Color = "Red"        | `/Home/About?color=Red` |

Jeśli trasa ma wartość domyślną, która nie odpowiada parametrowi, a ta wartość jest jawnie określona, musi być zgodna z wartością domyślną:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Generowanie linku generuje tylko łącze do tej trasy, gdy zostaną podane pasujące wartości dla `controller` i `action`.

## <a name="complex-segments"></a>Złożone segmenty

Złożone segmenty (na przykład `[Route("/x{token}y")]`) są przetwarzane przez dopasowanie literałów z prawej strony do lewej w sposób niezachłanney. [Ten kod](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) zawiera szczegółowy opis sposobu dopasowywania segmentów złożonych. [Przykład kodu](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) nie jest używany przez ASP.NET Core, ale zapewnia dobre wyjaśnienie złożonych segmentów.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

## <a name="configuring-endpoint-metadata"></a>Konfigurowanie metadanych punktu końcowego

Poniższe linki zawierają informacje dotyczące konfigurowania metadanych punktu końcowego:

* [Włączanie mechanizmu CORS przy użyciu routingu punktu końcowego](xref:security/cors#enable-cors-with-endpoint-routing)
* [Przykład IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) przy użyciu niestandardowego atrybutu `[MinimumAgeAuthorize]`
* [Testowanie uwierzytelniania przy użyciu atrybutu [autoryzuje]](xref:security/authentication/identity#test-identity)
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* [Wybieranie schematu z atrybutem [autoryzuje]](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)
* [Stosowanie zasad przy użyciu atrybutu [autoryzuje]](xref:security/authorization/policies#applying-policies-to-mvc-controllers)
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a>Pasujące hosty w trasach z RequireHost

`RequireHost` stosuje ograniczenie do trasy wymagającej określonego hosta. Parametr `RequireHost` lub `[Host]` może być:

* Host: `www.domain.com` (dopasowuje `www.domain.com` z dowolnym portem)
* Host z symbolem wieloznacznym: `*.domain.com` (dopasowuje `www.domain.com`, `subdomain.domain.com`lub `www.subdomain.domain.com` na dowolnym porcie)
* Port: `*:5000` (dopasowuje port 5000 z dowolnym hostem)
* Host i port: `www.domain.com:5000`, `*.domain.com:5000` (dopasowuje hosta i portu)

Za pomocą `RequireHost` lub `[Host]`można określić wiele parametrów. Ograniczenie będzie zgodne z hostami prawidłowymi dla któregokolwiek z parametrów. Na przykład `[Host("domain.com", "*.domain.com")]` będzie pasować do `domain.com`, `www.domain.com`lub `subdomain.domain.com`.

Poniższy kod używa `RequireHost`, aby wymagać określonego hosta dla trasy:

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi Contoso!"))
            .RequireHost("contoso.com");
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi AdventureWorks!"))
            .RequireHost("adventure-works.com");
        endpoints.MapHealthChecks("/healthz").RequireHost("*:8080");
    });
}
```

Poniższy kod używa atrybutu `[Host]`, aby wymagać określonego hosta na kontrolerze:

```csharp
[Host("contoso.com", "adventure-works.com")]
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        return View();
    }

    [Host("example.com:8080")]
    public IActionResult Privacy()
    {
        return View();
    }

}
```

Gdy atrybut `[Host]` jest stosowany do obu metod:

* Atrybut akcji jest używany.
* Atrybut Controller jest ignorowany.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Routing jest odpowiedzialny za mapowanie identyfikatorów URI żądań na punkty końcowe i wysyłanie żądań przychodzących do tych punktów końcowych. Trasy są zdefiniowane w aplikacji i konfigurowane podczas uruchamiania aplikacji. Trasa może opcjonalnie wyodrębnić wartości z adresu URL zawartego w żądaniu. te wartości mogą być następnie używane do przetwarzania żądań. Przy użyciu informacji o trasie z aplikacji Routing jest również w stanie generować adresy URL mapowane na punkty końcowe.

Aby użyć najnowszych scenariuszy routingu w ASP.NET Core 2,2, określ [wersję zgodności](xref:mvc/compatibility-version) do rejestracji usług MVC w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Opcja <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> określa, czy funkcja routingu powinna wewnętrznie używać logiki opartej na punkcie końcowym lub logiki opartej na <xref:Microsoft.AspNetCore.Routing.IRouter>ASP.NET Core 2,1 lub starszej. Gdy wersja zgodności jest ustawiona na 2,2 lub nowszej, wartość domyślna to `true`. Ustaw wartość na `false`, aby użyć poprzedniej logiki routingu:

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Więcej informacji na temat routingu opartego na <xref:Microsoft.AspNetCore.Routing.IRouter>można znaleźć w [wersji ASP.NET Core 2,1 tego tematu](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).

> [!IMPORTANT]
> Ten dokument obejmuje Routing ASP.NET Core niskiego poziomu. Aby uzyskać informacje na temat ASP.NET Core routingu MVC, zobacz <xref:mvc/controllers/routing>. Aby uzyskać informacje na temat Konwencji routingu w Razor Pages, zobacz <xref:razor-pages/razor-pages-conventions>.

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Podstawy routingu

Większość aplikacji powinna wybrać podstawowy i opisowy schemat routingu, aby adresy URL były czytelne i zrozumiałe. Domyślna trasa konwencjonalna `{controller=Home}/{action=Index}/{id?}`:

* Obsługuje podstawowy i opisowy schemat routingu.
* Jest użytecznym punktem wyjścia dla aplikacji opartych na interfejsie użytkownika.

Deweloperzy często dodają dodatkowe trasy zwięzła do obszarów o dużym ruchu w aplikacji w wyspecjalizowanych sytuacjach (na przykład blog i punkty końcowe handlu elektronicznego) przy użyciu [routingu atrybutów](xref:mvc/controllers/routing#attribute-routing) lub dedykowanych tras konwencjonalnych.

Interfejsy API sieci Web powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez zlecenia HTTP. Oznacza to, że wiele operacji (na przykład GET, POST) dla tego samego zasobu logicznego będzie używać tego samego adresu URL. Routing atrybutu zapewnia poziom kontroli, który jest wymagany do dokładnego projektowania układu publicznego punktu końcowego interfejsu API.

Aplikacje Razor Pages używają domyślnego routingu konwencjonalnego do obsłużenia nazwanych zasobów w folderze *strony* aplikacji. Dostępne są dodatkowe konwencje, które umożliwiają dostosowanie Razor Pages zachowaniem routingu. Aby uzyskać więcej informacji, zobacz <xref:razor-pages/index> i <xref:razor-pages/razor-pages-conventions>.

Obsługa generowania adresów URL umożliwia tworzenie aplikacji bez adresów URL, które mają być połączone ze sobą. Ta obsługa pozwala rozpocząć od podstawowej konfiguracji routingu i zmodyfikować trasy po ustaleniu układu zasobów aplikacji.

Funkcja routingu używa *punktów końcowych* (`Endpoint`) do reprezentowania logicznych punktów końcowych w aplikacji.

Punkt końcowy definiuje delegata do przetwarzania żądań i kolekcji dowolnych metadanych. Metadane są używane w celu zaimplementowania zagadnień związanych z wycinaniem w oparciu o zasady i konfigurację dołączone do każdego punktu końcowego.

System routingu ma następujące cechy:

* Składnia szablonu trasy służy do definiowania tras z parametrami trasy z tokenami.
* Dozwolona jest konfiguracja języka końcowego w stylu konwencjonalnym i stylu atrybutu.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> służy do określenia, czy parametr adresu URL zawiera prawidłową wartość dla danego ograniczenia punktu końcowego.
* Modele aplikacji, takie jak MVC/Razor Pages, rejestrują wszystkie punkty końcowe, które mają przewidywalne implementację scenariuszy routingu.
* Implementacja routingu podejmuje decyzje dotyczące routingu w dowolnym miejscu w potoku programu pośredniczącego.
* Oprogramowanie pośredniczące, które pojawia się po utworzeniu oprogramowania pośredniczącego, może sprawdzić wynik decyzji punktu końcowego usługi routingu dla danego identyfikatora URI żądania.
* Można wyliczyć wszystkie punkty końcowe w aplikacji w dowolnym miejscu w potoku programu pośredniczącego.
* Aplikacja może używać routingu do generowania adresów URL (na przykład w przypadku przekierowania lub linków) na podstawie informacji o punkcie końcowym, a tym samym unikania zakodowanych adresów URL, co ułatwia łatwość utrzymania.
* Generowanie adresów URL jest oparte na adresach, które obsługują arbitralną rozszerzalność:

  * Interfejs API generatora linków (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) można rozpoznać w dowolnym miejscu przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection) do generowania adresów URL.
  * Gdy interfejs API generatora linków nie jest dostępny za pośrednictwem programu DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> oferuje metody do kompilowania adresów URL.

> [!NOTE]
> Dzięki wydaniu routingu punktów końcowych w ASP.NET Core 2,2, łączenie punktów końcowych jest ograniczone do akcji i stron programu MVC/Razor Pages. Rozszerzenia punktów końcowych konsolidacji są planowane dla przyszłych wersji.

Routing jest połączony z potokiem [pośredniczącym](xref:fundamentals/middleware/index) przez klasę <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) dodaje Routing do potoku oprogramowania pośredniczącego w ramach swojej konfiguracji i obsługuje routing w aplikacjach MVC i Razor Pages. Aby dowiedzieć się, jak używać routingu jako składnika autonomicznego, zapoznaj się z sekcją [Korzystanie z oprogramowania do routingu](#use-routing-middleware) .

### <a name="url-matching"></a>Dopasowanie adresu URL

Dopasowywanie adresów URL to proces, za pomocą którego Routing wysyła żądanie przychodzące do *punktu końcowego*. Ten proces jest oparty na danych w ścieżce URL, ale można go rozszerzyć w celu uwzględnienia wszelkich danych w żądaniu. Możliwość wysyłania żądań do oddzielnych programów obsługi jest kluczem do skalowania rozmiaru i złożoności aplikacji.

System routingu w ramach routingu punktów końcowych jest odpowiedzialny za wszystkie decyzje dotyczące wysyłania. Ponieważ oprogramowanie pośredniczące stosuje zasady oparte na wybranym punkcie końcowym, ważne jest, aby wszystkie decyzje, które mogą mieć wpływ na wysyłanie lub stosowanie zasad zabezpieczeń, zostały wprowadzone w systemie routingu.

Po wykonaniu delegata punktu końcowego właściwości [RouteContext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) są ustawiane na odpowiednie wartości na podstawie przetwarzania żądania wykonanego do tej pory.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) to słownik *wartości tras* uzyskanych z trasy. Te wartości są zwykle określane przez tokenizowanie jako adres URL i mogą służyć do akceptowania danych wejściowych użytkownika lub do dalszej akceptacji decyzji w aplikacji.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) to zbiór właściwości dodatkowych danych dotyczących dopasowanej trasy. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> zapewnia obsługę kojarzenia danych stanu z każdą trasą, dzięki czemu aplikacja może podejmować decyzje na podstawie dopasowanej trasy. Te wartości są zdefiniowane przez dewelopera i **nie** mają wpływu na zachowanie routingu. Ponadto wartości umieszczane w [RouteData. Datatokeny](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) mogą być dowolnego typu, w przeciwieństwie do [RouteData. wartości](xref:Microsoft.AspNetCore.Routing.RouteData.Values), które muszą być konwertowane do i z ciągów.

[RouteData. routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) to lista tras, które brały udział w pomyślnie dopasowane do żądania. Trasy mogą być zagnieżdżone wewnątrz siebie. Właściwość <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> odzwierciedla ścieżkę przez logiczne drzewo tras, które spowodowały dopasowanie. Ogólnie rzecz biorąc, pierwszy element w <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> jest kolekcją tras i powinien być używany do generowania adresów URL. Ostatnim elementem w <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> jest program obsługi trasy, który został dopasowany.

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a>Generowanie adresu URL za pomocą LinkGenerator

Generowanie adresu URL to proces, za pomocą którego Routing może utworzyć ścieżkę URL na podstawie zestawu wartości trasy. Pozwala to na logiczne rozdzielenie między punktami końcowymi i adresami URL, które uzyskują do nich dostęp.

Routing punktów końcowych obejmuje interfejs API generatora linków (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>). <xref:Microsoft.AspNetCore.Routing.LinkGenerator> to pojedyncza usługa, którą można pobrać z programu DI. Interfejsu API można używać poza kontekstem żądania wykonania. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> i scenariusze MVC, które opierają się na <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, takich jak [pomocników tagów](xref:mvc/views/tag-helpers/intro), pomocników HTML i [wynikach akcji](xref:mvc/controllers/actions), używają generatora linków, aby zapewnić możliwości generowania linków.

Generator łącza jest objęty koncepcją i *schematami* *adresów.* Schemat adresów jest sposobem określania punktów końcowych, które należy wziąć pod uwagę podczas generowania łącza. Na przykład nazwa trasy i wartości trasy scenariusze wielu użytkowników są znane z programu z MVC/Razor Pages są implementowane jako schemat adresów.

Generator łącza może łączyć się z akcjami i stronami MVC/Razor Pages za pośrednictwem następujących metod rozszerzających:

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

Przeciążenie tych metod przyjmuje argumenty, które zawierają `HttpContext`. Metody te są funkcjonalnie równoważne `Url.Action` i `Url.Page` ale oferują dodatkową elastyczność i opcje.

Metody `GetPath*` są najbardziej podobne do `Url.Action` i `Url.Page` w tym, że generują identyfikator URI zawierający ścieżkę bezwzględną. Metody `GetUri*` zawsze generują bezwzględny identyfikator URI zawierający schemat i hosta. Metody akceptujące `HttpContext` generują identyfikator URI w kontekście żądania wykonania. Użycie wartości tras w otoczeniu, ścieżki podstawowej adresu URL, schematu i hosta z żądania wykonania jest używane, chyba że zostaną zastąpione.

<xref:Microsoft.AspNetCore.Routing.LinkGenerator> jest wywoływana przy użyciu adresu. Generowanie identyfikatora URI występuje w dwóch krokach:

1. Adres jest powiązany z listą punktów końcowych, które pasują do adresu.
1. `RoutePattern` poszczególnych punktów końcowych są oceniane do momentu znalezienia wzorca tras pasującego do dostarczonych wartości. Wynikowe dane wyjściowe są łączone z innymi częściami identyfikatora URI dostarczanymi do generatora linków i zwracanymi.

Metody zapewniane przez <xref:Microsoft.AspNetCore.Routing.LinkGenerator> obsługują funkcje generowania linków standardowych dla dowolnego typu adresu. Najbardziej wygodnym sposobem korzystania z generatora łączy są metody rozszerzające, które wykonują operacje dla określonego typu adresu.

| Metoda rozszerzenia   | Opis                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | Generuje identyfikator URI z ścieżką bezwzględną na podstawie podanych wartości. |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | Generuje bezwzględny identyfikator URI na podstawie podanych wartości.             |

> [!WARNING]
> Zwróć uwagę na następujące konsekwencje wywołania <xref:Microsoft.AspNetCore.Routing.LinkGenerator> metod:
>
> * Użyj `GetUri*` metod rozszerzenia z zachowaniem ostrożności w konfiguracji aplikacji, która nie weryfikuje `Host` nagłówka żądań przychodzących. Jeśli nagłówek `Host` żądań przychodzących nie jest zweryfikowany, dane wejściowe żądania niezaufanego mogą być wysyłane z powrotem do klienta w identyfikatorach URI w widoku/stronie. Zalecamy, aby wszystkie aplikacje produkcyjne skonfigurowali swój serwer do sprawdzania poprawności nagłówka `Host` pod kątem znanych prawidłowych wartości.
>
> * Używaj <xref:Microsoft.AspNetCore.Routing.LinkGenerator> z przestrogą w oprogramowaniu pośredniczącym w połączeniu z `Map` lub `MapWhen`. `Map*` zmienia ścieżkę podstawową żądania wykonania, która ma wpływ na dane wyjściowe generowania łącza. Wszystkie <xref:Microsoft.AspNetCore.Routing.LinkGenerator> interfejsy API umożliwiają określanie ścieżki podstawowej. Zawsze określaj pustą ścieżkę bazową do cofnięcia `Map*`ma wpływ na generowanie linków.

## <a name="differences-from-earlier-versions-of-routing"></a>Różnice wynikające z wcześniejszych wersji usługi Routing

Istnieje kilka różnic między routingiem punktu końcowego w ASP.NET Core 2,2 lub nowszym i wcześniejszymi wersjami routingu w programie ASP.NET Core:

* System routingu punktu końcowego nie obsługuje rozszerzalności opartej na <xref:Microsoft.AspNetCore.Routing.IRouter>, w tym dziedziczenie z <xref:Microsoft.AspNetCore.Routing.Route>.

* Routing punktów końcowych nie obsługuje [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Użyj [wersji zgodności](xref:mvc/compatibility-version) 2,1 (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`), aby nadal korzystać z podkładki zgodności.

* Routing punktów końcowych ma inne zachowanie dla wielkości liter wygenerowanych identyfikatorów URI podczas korzystania z konwencjonalnych tras.

  Rozważmy następujący szablon trasy domyślnej:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Załóżmy, że generujesz link do akcji przy użyciu następującej trasy:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  W przypadku routingu opartego na <xref:Microsoft.AspNetCore.Routing.IRouter>ten kod generuje identyfikator URI `/blog/ReadPost/17`, co uwzględnia wielkość liter podanej trasy. Routing punktów końcowych w ASP.NET Core 2,2 lub późniejszych produkuje `/Blog/ReadPost/17` ("blog" jest pisane wielkimi literami). Routing punktów końcowych udostępnia interfejs `IOutboundParameterTransformer`, którego można użyć do dostosowania tego zachowania globalnie lub w celu zastosowania różnych konwencji do mapowania adresów URL.

  Aby uzyskać więcej informacji, zobacz sekcję [informacje dotyczące transformatora parametrów](#parameter-transformer-reference) .

* Generowanie linków używane przez MVC/Razor Pages z trasami konwencjonalnymi działa inaczej podczas próby połączenia z kontrolerem/akcją lub stroną, która nie istnieje.

  Rozważmy następujący szablon trasy domyślnej:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Załóżmy, że generowany jest link do akcji przy użyciu szablonu domyślnego z następującymi:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  W przypadku routingu opartego na `IRouter`wynik jest zawsze `/Blog/ReadPost/17`, nawet jeśli `BlogController` nie istnieje lub nie ma metody akcji `ReadPost`. Zgodnie z oczekiwaniami Routing punktów końcowych w ASP.NET Core 2,2 lub nowszej tworzy `/Blog/ReadPost/17`, jeśli istnieje metoda akcji. *Jednak w przypadku, gdy akcja nie istnieje, routing punktu końcowego tworzy pusty ciąg.* Koncepcyjnie, routing punktu końcowego nie zakłada, że punkt końcowy istnieje, jeśli akcja nie istnieje.

* *Algorytm unieważniania wartości otoczenia* generacji linku działa inaczej, gdy jest używany z routingiem punktów końcowych.

  *Nieprawidłowa wartość otoczenia* , która decyduje o tym, które wartości trasy z aktualnie wykonywanego żądania (wartości otoczenia) mogą być używane w operacjach generowania linków. Routowanie konwencjonalne zawsze unieważnia dodatkowe wartości trasy podczas łączenia z inną akcją. Routing atrybutów nie miał tego zachowania przed wydaniem ASP.NET Core 2,2. We wcześniejszych wersjach ASP.NET Core linki do innej akcji, która używa tych samych nazw parametrów trasy, spowodowały błędy generacji łącza. W ASP.NET Core 2,2 lub nowszej, obie formy routingu unieważnią wartości podczas łączenia z inną akcją.

  Rozważmy następujący przykład w ASP.NET Core 2,1 lub starszej. W przypadku łączenia z inną akcją (lub inną stroną) wartości trasy mogą być ponownie używane na różne sposoby.

  W */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  W */Pages/login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  Jeśli identyfikator URI jest `/Store/Product/18` w ASP.NET Core 2,1 lub starszej, link wygenerowany na stronie Sklep/informacje przez `@Url.Page("/Login")` jest `/Login/18`. Wartość 18 zostanie ponownie użyta, mimo że lokalizacja docelowa linku jest zupełnie inna częścią aplikacji. `id` `id` wartość trasy w kontekście strony `/Login` jest prawdopodobnie wartością identyfikatora użytkownika, a nie wartością identyfikatora produktu w sklepie.

  W przypadku routingu punktów końcowych z ASP.NET Core 2,2 lub nowszym wynik jest `/Login`. Wartości otoczenia nie są ponownie używane, gdy połączone miejsce docelowe jest inną akcją lub stroną.

* Składnia parametru trasy okrężnej: ukośniki nie są kodowane przy użyciu podwójnej gwiazdki (`**`) składni parametrów.

  Podczas generowania łącza system routingu koduje wartość przechwyconą w dwugwiazdkowym (`**`) parametrem catch-all (na przykład `{**myparametername}`), z wyjątkiem ukośników. Podwójna gwiazdka "catch-all" jest obsługiwana w przypadku routingu opartego na `IRouter`w ASP.NET Core 2,2 lub nowszym.

  Jedyna gwiazdka "catch-all" w poprzednich wersjach ASP.NET Core (`{*myparametername}`) pozostaje obsługiwana, a ukośniki są zakodowane.

  | Trasa              | Wygenerowano łącze<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (ukośnik zostanie zakodowany)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Przykład oprogramowania pośredniczącego

W poniższym przykładzie oprogramowanie pośredniczące używa interfejsu API <xref:Microsoft.AspNetCore.Routing.LinkGenerator>, aby utworzyć łącze do metody akcji, która wyświetla listę produktów ze sklepu. Przy użyciu generatora linków poprzez wstrzyknięcie go do klasy i wywołanie `GenerateLink` jest dostępny dla każdej klasy w aplikacji.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a>Tworzenie tras

Większość aplikacji tworzy trasy przez wywołanie <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> lub jednej z podobnych metod rozszerzających zdefiniowanych w <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Dowolna z metod rozszerzenia <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> tworzy wystąpienie <xref:Microsoft.AspNetCore.Routing.Route> i dodaje je do kolekcji tras.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> nie akceptuje parametru procedury obsługi trasy. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> dodaje tylko trasy, które są obsługiwane przez <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Aby dowiedzieć się więcej na temat routingu w MVC, zobacz <xref:mvc/controllers/routing>.

Poniższy przykład kodu jest przykładem wywołania <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> używanego przez typową definicję trasy ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ten szablon dopasowuje ścieżkę URL i wyodrębnia wartości tras. Na przykład ścieżka `/Products/Details/17` generuje następujące wartości trasy: `{ controller = Products, action = Details, id = 17 }`.

Wartości tras są określane przez podzielenie ścieżki URL na segmenty i dopasowanie do każdego segmentu przy użyciu nazwy *parametru trasy* w szablonie trasy. Parametry trasy są nazywane. Parametry zdefiniowane przez ujęcie nazwy parametru w nawiasach klamrowych `{ ... }`.

Poprzedni szablon może być również zgodny z ścieżką URL `/` i generować wartości `{ controller = Home, action = Index }`. Dzieje się tak, ponieważ parametry `{controller}` i `{action}` trasy mają wartości domyślne, a parametr trasy `id` jest opcjonalny. Znak równości (`=`), po którym następuje wartość po nazwie parametru trasy definiuje wartość domyślną dla parametru. Znak zapytania (`?`) po nazwie parametru trasy definiuje opcjonalny parametr.

Parametry trasy z wartością domyślną *zawsze* generują wartość trasy w przypadku dopasowania trasy. Parametry opcjonalne nie generują wartości trasy, jeśli nie ma odpowiedniego segmentu ścieżki adresu URL. Szczegółowe opisy scenariuszy i składni szablonów tras można znaleźć w sekcji [Dokumentacja dotycząca szablonu trasy](#route-template-reference) .

W poniższym przykładzie definicja parametru trasy `{id:int}` definiuje [ograniczenie trasy](#route-constraint-reference) dla parametru trasy `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ten szablon dopasowuje ścieżkę URL, taką jak `/Products/Details/17`, ale nie `/Products/Details/Apples`. Ograniczenia trasy implementują <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> i sprawdzają wartości routingu w celu ich zweryfikowania. W tym przykładzie wartość trasy `id` musi być możliwa do przekonwertowania na liczbę całkowitą. Aby uzyskać wyjaśnienie ograniczeń trasy dostarczonych przez platformę, zobacz temat [ograniczenia trasy](#route-constraint-reference) .

Dodatkowe przeciążenia <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> akceptują wartości `constraints`, `dataTokens`i `defaults`. Typowym użyciem tych parametrów jest przekazanie anonimowo wpisanego obiektu, gdzie nazwy właściwości typu anonimowego są zgodne z nazwami parametrów trasy.

Poniższe <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> przykłady tworzą równoważne trasy:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> Wbudowana Składnia służąca do definiowania ograniczeń i wartości domyślnych może być wygodna dla prostych tras. Istnieją jednak scenariusze, takie jak tokeny danych, które nie są obsługiwane przez składnię wbudowaną.

Poniższy przykład ilustruje kilka dodatkowych scenariuszy:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Poprzedni szablon pasuje do ścieżki URL, takiej jak `/Blog/All-About-Routing/Introduction`, i wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Domyślne wartości trasy dla `controller` i `action` są generowane przez trasę, nawet jeśli w szablonie nie ma odpowiednich parametrów trasy. Wartości domyślne można określić w szablonie trasy. Parametr trasy `article` jest zdefiniowany jako *przechwycenie* przez wygląd podwójnej gwiazdki (`**`) przed nazwą parametru trasy. Catch-wszystkie parametry tras przechwytują resztę ścieżki URL i mogą również pasować do pustego ciągu.

Poniższy przykład dodaje ograniczenia trasy i tokeny danych:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Poprzedni szablon pasuje do ścieżki URL, takiej jak `/en-US/Products/5`, i wyodrębnia wartości `{ controller = Products, action = Details, id = 5 }` i `{ locale = en-US }`tokenów danych.

![Lokalne tokeny systemu Windows](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generowanie adresu URL klasy trasy

Klasa <xref:Microsoft.AspNetCore.Routing.Route> może również wykonywać generowanie adresów URL przez połączenie zestawu wartości tras z jego szablonem trasy. Jest to logicznie proces odwrotny pasujący do ścieżki URL.

> [!TIP]
> Aby lepiej zrozumieć generowanie adresów URL, Załóżmy, jaki adres URL ma zostać wygenerowany, a następnie pomyśl o sposobie dopasowania szablonu trasy do tego adresu URL. Jakie wartości zostałyby wygenerowane? Jest to sztywny odpowiednik działania generowania adresów URL w klasie <xref:Microsoft.AspNetCore.Routing.Route>.

Poniższy przykład używa ogólnej trasy domyślnej ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Po `{ controller = Products, action = List }`wartości trasy zostanie wygenerowany adres URL `/Products/List`. Wartości trasy są zastępowane odpowiednimi parametrami trasy w celu utworzenia ścieżki URL. Ponieważ `id` jest opcjonalnym parametrem trasy, adres URL został pomyślnie wygenerowany bez wartości dla `id`.

Po `{ controller = Home, action = Index }`wartości trasy zostanie wygenerowany adres URL `/`. Podane wartości trasy są zgodne z wartościami domyślnymi, a segmenty odpowiadające wartościom domyślnym są bezpiecznie pomijane.

Oba adresy URL wygenerowały rundę z następującą definicją trasy (`/Home/Index` i `/`) tworzą te same wartości trasy, które zostały użyte do wygenerowania adresu URL.

> [!NOTE]
> Aplikacja używająca ASP.NET Core MVC powinna używać <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> do generowania adresów URL zamiast bezpośredniego wywoływania do routingu.

Aby uzyskać więcej informacji na temat generowania adresów URL, zobacz sekcję [informacje dotyczące generowania adresów URL](#url-generation-reference) .

## <a name="use-routing-middleware"></a>Korzystanie z oprogramowania pośredniczącego routingu

Odwołuje się do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) w pliku projektu aplikacji.

Dodawanie routingu do kontenera usługi w `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Trasy muszą być skonfigurowane w metodzie `Startup.Configure`. Przykładowa aplikacja używa następujących interfejsów API:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; dopasowuje tylko żądania HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

W poniższej tabeli przedstawiono odpowiedzi z podanym identyfikatorem URI.

| Identyfikator URI                    | Odpowiedź                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Cześć! Wartości trasy: [Operation, Create], [ID, 3] |
| `/package/track/-3`    | Cześć! Wartości trasy: [Operation, Track], [ID,-3] |
| `/package/track/-3/`   | Cześć! Wartości trasy: [Operation, Track], [ID,-3] |
| `/package/track/`      | Żądanie przepada w, brak dopasowania.              |
| `GET /hello/Joe`       | Witaj, Jan!                                          |
| `POST /hello/Joe`      | Żądanie przepada w, dopasowuje tylko HTTP GET. |
| `GET /hello/Joe/Smith` | Żądanie przepada w, brak dopasowania.              |

Struktura zawiera zestaw metod rozszerzających do tworzenia tras (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

Metody `Map[Verb]` używają ograniczeń do ograniczenia trasy do zlecenia HTTP w nazwie metody. Na przykład zobacz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> i <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Odwołanie do szablonu trasy

Tokeny w nawiasach klamrowych (`{ ... }`) definiują *Parametry trasy* , które są powiązane w przypadku dopasowania trasy. Można zdefiniować więcej niż jeden parametr trasy w segmencie trasy, ale muszą one być oddzielone wartością literału. Na przykład `{controller=Home}{action=Index}` nie jest prawidłową trasą, ponieważ nie ma żadnej wartości literału między `{controller}` i `{action}`. Te parametry trasy muszą mieć nazwę i mogą mieć określone dodatkowe atrybuty.

Tekst literału inny niż parametry trasy (na przykład `{id}`) i separator ścieżki `/` musi być zgodny z tekstem w adresie URL. W dopasowaniu do tekstu nie jest rozróżniana wielkość liter i w oparciu o zdekodowaną reprezentację ścieżki URL. Aby dopasować Ogranicznik parametru trasy literału (`{` lub `}`), należy wprowadzić ogranicznik przez powtórzenie znaku (`{{` lub `}}`).

Wzorce adresów URL, które próbują przechwycić nazwę pliku z opcjonalnym rozszerzeniem pliku, mają dodatkowe uwagi. Rozważmy na przykład szablon `files/{filename}.{ext?}`. Gdy istnieją wartości dla obu `filename` i `ext`, są wypełniane obie wartości. Jeśli w adresie URL istnieje tylko wartość `filename`, trasa pasuje do, ponieważ końcowy okres (`.`) jest opcjonalny. Następujące adresy URL pasują do tej trasy:

* `/files/myFile.txt`
* `/files/myFile`

Możesz użyć gwiazdki (`*`) lub podwójnej gwiazdki (`**`) jako prefiksu do parametru trasy, aby powiązać z pozostałą częścią identyfikatora URI. Są one nazywane parametrami *przechwycenia* . Na przykład `blog/{**slug}` dopasowuje dowolny identyfikator URI, który rozpoczyna się od `/blog` i ma dowolną wartość, która jest przypisana do wartości `slug` Route. Catch-wszystkie parametry można także dopasować do pustego ciągu.

Parametr catch-all wyprowadza odpowiednie znaki, gdy trasa jest używana do generowania adresu URL, w tym znaków separatora ścieżki (`/`). Na przykład `foo/{*path}` trasy z wartościami trasy `{ path = "my/path" }` generuje `foo/my%2Fpath`. Zwróć uwagę na odwrócony ukośnik. Aby zaokrąglić znaki separatora ścieżki, użyj prefiksu parametru `**` Route. `foo/{**path}` trasy z `{ path = "my/path" }` generuje `foo/my/path`.

Parametry trasy mogą mieć *wartości domyślne* , określając wartość domyślną po nazwie parametru oddzielone znakiem równości (`=`). Na przykład `{controller=Home}` definiuje `Home` jako wartość domyślną dla `controller`. Wartość domyślna jest używana, jeśli żadna wartość nie jest obecna w adresie URL dla parametru. Parametry tras są opcjonalne przez dołączenie znaku zapytania (`?`) na końcu nazwy parametru, jak w `id?`. Różnica między wartościami opcjonalnymi i domyślnymi parametrami trasy polega na tym, że parametr trasy z wartością domyślną zawsze generuje wartość&mdash;opcjonalny parametr ma wartość tylko wtedy, gdy wartość jest podana przez adres URL żądania.

Parametry trasy mogą mieć ograniczenia, które muszą być zgodne z wartością trasy powiązaną z adresem URL. Dodanie dwukropek (`:`) i nazwy ograniczenia po nazwie parametru trasy określa *ograniczenie wbudowane* dla parametru trasy. Jeśli ograniczenie wymaga argumentów, są one ujęte w nawiasy (`(...)`) po nazwie ograniczenia. Można określić wiele ograniczeń wbudowanych, dołączając inną dwukropek (`:`) i nazwę ograniczenia.

Nazwa i argumenty ograniczenia są przesyłane do usługi <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> w celu utworzenia wystąpienia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> do użycia w przetwarzaniu adresów URL. Na przykład szablon trasy `blog/{article:minlength(10)}` określa ograniczenie `minlength` z argumentem `10`. Aby uzyskać więcej informacji na temat ograniczeń trasy i listę ograniczeń zapewnianych przez platformę, zobacz sekcję [odwołanie do ograniczenia trasy](#route-constraint-reference) .

Parametry trasy mogą także mieć Transformatory parametrów, które przekształcają wartość parametru podczas generowania linków i dopasowywania stron do adresów URL. Podobnie jak ograniczenia, transformatory parametrów mogą być dodawane wewnętrznie do parametru trasy przez dodanie dwukropka (`:`) i nazwy transformatora po nazwie parametru trasy. Na przykład szablon trasy `blog/{article:slugify}` określa `slugify` transformator. Aby uzyskać więcej informacji na temat transformatorów parametrów, zobacz sekcję [informacje dotyczące transformatora parametrów](#parameter-transformer-reference) .

W poniższej tabeli przedstawiono przykładowe szablony tras i ich zachowanie.

| Szablon trasy                           | Przykładowy pasujący identyfikator URI    | &hellip; identyfikatora URI żądania                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Dopasowuje tylko jedną ścieżkę `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Dopasowuje i ustawia `Page`, aby `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Dopasowuje i ustawia `Page`, aby `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Mapuje na kontroler `Products` i akcję `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Mapuje na kontroler `Products` i akcję `Details` (`id` ustawioną na 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Mapuje na kontroler `Home` i metodę `Index` (`id` jest ignorowana).        |

Użycie szablonu jest ogólnie najprostszym podejściem do routingu. Ograniczenia i wartości domyślne można także określić poza szablonem trasy.

> [!TIP]
> Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby zobaczyć, jak wbudowane implementacje routingu, takie jak <xref:Microsoft.AspNetCore.Routing.Route>, pasują do żądań.

## <a name="reserved-routing-names"></a>Zastrzeżone nazwy routingu

Następujące słowa kluczowe są nazwami zarezerwowanymi i nie mogą być używane jako nazwy tras lub parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odwołanie do ograniczenia trasy

Ograniczenia trasy są wykonywane, gdy nastąpiło dopasowanie do przychodzącego adresu URL, a Ścieżka adresu URL jest zgodna z wartościami trasy. Ograniczenia trasy zwykle sprawdzają wartość trasy skojarzoną za pośrednictwem szablonu trasy i podejmują decyzję o tym, czy wartość jest akceptowalna. Niektóre ograniczenia trasy używają danych poza wartością trasy, aby uwzględnić, czy żądanie może być kierowane. Na przykład <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> może zaakceptować lub odrzucić żądanie na podstawie jego zlecenia HTTP. Ograniczenia są używane w żądaniach routingu i generowania linków.

> [!WARNING]
> Nie używaj ograniczeń **sprawdzania poprawności danych wejściowych**. Jeśli ograniczenia są używane do **sprawdzania poprawności danych wejściowych**, nieprawidłowe dane wejściowe w odpowiedzi *404 — nie znaleziono* , zamiast *żądania 400-złe* z odpowiednim komunikatem o błędzie. Ograniczenia trasy są **używane do** odróżniania podobnych tras, a nie do sprawdzania danych wejściowych dla określonej trasy.

W poniższej tabeli przedstawiono przykładowe ograniczenia trasy i ich oczekiwane zachowanie.

| ograniczenie | Przykład | Przykładowe dopasowania | Uwagi |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Dopasowuje dowolną liczbę całkowitą |
| `bool` | `{active:bool}` | `true`, `FALSE` | Dopasowuje `true` lub `false` (bez uwzględniania wielkości liter) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Dopasowuje prawidłową wartość `DateTime` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Dopasowuje prawidłową wartość `decimal` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Dopasowuje prawidłową wartość `double` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Dopasowuje prawidłową wartość `float` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Dopasowuje prawidłową wartość `Guid` |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Dopasowuje prawidłową wartość `long` |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Ciąg musi składać się z co najmniej 4 znaków |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Ciąg nie może zawierać więcej niż 8 znaków |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Ciąg musi zawierać dokładnie 12 znaków |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Ciąg musi mieć długość co najmniej 8 znaków |
| `min(value)` | `{age:min(18)}` | `19` | Wartość całkowita musi być równa co najmniej 18 |
| `max(value)` | `{age:max(120)}` | `91` | Wartość całkowita nie może być większa niż 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Wartość całkowita musi być równa co najmniej 18, ale nie więcej niż 120 |
| `alpha` | `{name:alpha}` | `Rick` | Ciąg musi zawierać co najmniej jeden znak alfabetyczny (`a`-`z`, bez uwzględniania wielkości liter) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Ciąg musi być zgodny z wyrażeniem regularnym (zobacz Porady dotyczące definiowania wyrażenia regularnego) |
| `required` | `{name:required}` | `Rick` | Służy do wymuszania, że podczas generowania adresu URL jest obecna wartość, która nie jest wartością parametru |

Wielokrotne ograniczenia rozdzielane średnikami można zastosować do jednego parametru. Na przykład następujące ograniczenie ogranicza parametr do wartości całkowitej 1 lub wyższej:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Ograniczenia trasy, które weryfikują adres URL i są konwertowane na typ CLR (takie jak `int` lub `DateTime`), zawsze używają niezmiennej kultury. W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny. Ograniczenia trasy dostarczone przez platformę nie modyfikują wartości przechowywanych w wartościach trasy. Wszystkie wartości tras analizowane na podstawie adresu URL są przechowywane jako ciągi. Na przykład, ograniczenie `float` próbuje skonwertować wartość trasy na float, ale przekonwertowana wartość jest używana tylko w celu sprawdzenia, czy można ją przekonwertować na wartość zmiennoprzecinkową.

## <a name="regular-expressions"></a>Wyrażenia regularne

ASP.NET Core Framework dodaje `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` do konstruktora wyrażeń regularnych. Aby uzyskać opis tych elementów członkowskich, zobacz <xref:System.Text.RegularExpressions.RegexOptions>.

Wyrażenia regularne używają ograniczników i tokenów podobnych do tych używanych w ramach routingu i C# języka. Tokeny wyrażenia regularnego muszą być zmienione. Aby użyć wyrażenia regularnego `^\d{3}-\d{2}-\d{4}$` w routingu, wyrażenie musi mieć `\` (pojedynczy ukośnik odwrotny) podany w ciągu jako `\\` (podwójny ukośnik odwrotny) w pliku C# źródłowym w celu wypróbowania znaku ucieczki `\` ciągu znaków (chyba że jest używany [literały ciągu Verbatim](/dotnet/csharp/language-reference/keywords/string)). Na znaki ogranicznika parametru routingu ucieczki (`{`, `}`, `[`, `]`), podwójne znaki w wyrażeniu (`{{`, `}`, `[[`, `]]`). W poniższej tabeli przedstawiono wyrażenie regularne i wersja z ucieczką.

| Wyrażenie regularne    | Wyrażenie regularne o zmienionym znaczeniu     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Wyrażenia regularne używane w routingu często zaczynają się od znaku daszka (`^`) i dopasowują pozycję początkową ciągu. Wyrażenia często kończą się znakiem dolara (`$`) i końcem ciągu. Znaki `^` i `$` zapewniają, że wyrażenie regularne dopasowuje całą wartość parametru trasy. Bez znaków `^` i `$` wyrażenie regularne dopasowuje dowolny podciąg w ciągu, co jest często niepożądane. W poniższej tabeli przedstawiono przykłady i wyjaśniono, dlaczego są one zgodne lub niezgodne.

| Wyrażenie   | String    | Dopasowanie | Komentarz               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Cześć     | Tak   | Dopasowania podciągów     |
| `[a-z]{2}`   | 123abc456 | Tak   | Dopasowania podciągów     |
| `[a-z]{2}`   | mz        | Tak   | Wyrażenie dopasowania    |
| `[a-z]{2}`   | MZ        | Tak   | Bez uwzględniania wielkości liter    |
| `^[a-z]{2}$` | Cześć     | Nie    | Zobacz `^` i `$` powyżej |
| `^[a-z]{2}$` | 123abc456 | Nie    | Zobacz `^` i `$` powyżej |

Aby uzyskać więcej informacji na temat składni wyrażeń regularnych, zobacz [.NET Framework wyrażeń regularnych](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Aby ograniczyć parametr do znanego zestawu możliwych wartości, użyj wyrażenia regularnego. Na przykład `{action:regex(^(list|get|create)$)}` dopasowuje wartość trasy `action` do `list`, `get`lub `create`. Jeśli przeszedł do słownika ograniczeń, ciąg `^(list|get|create)$` jest równoważny. Ograniczenia, które są przesyłane w słowniku ograniczenia (nie wbudowane w szablon), które nie pasują do jednego ze znanych ograniczeń, są również traktowane jako wyrażenia regularne.

## <a name="custom-route-constraints"></a>Niestandardowe ograniczenia trasy

Oprócz wbudowanych ograniczeń trasy niestandardowe ograniczenia trasy mogą być tworzone przez implementację interfejsu <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. Interfejs <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> zawiera pojedynczą metodę, `Match`, która zwraca `true` w przypadku spełnienia ograniczenia i `false` w inny sposób.

Aby można było użyć niestandardowego <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, typ ograniczenia trasy musi być zarejestrowany w <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> aplikacji w kontenerze usługi aplikacji. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> jest słownikiem, który mapuje klucze ograniczeń trasy na implementacje <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, które weryfikują te ograniczenia. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> aplikacji można zaktualizować w `Startup.ConfigureServices` w ramach [usług. Wywołanie addrouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) lub przez skonfigurowanie <xref:Microsoft.AspNetCore.Routing.RouteOptions> bezpośrednio z `services.Configure<RouteOptions>`. Na przykład:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Ograniczenie można następnie zastosować do tras w zwykły sposób, przy użyciu nazwy określonej podczas rejestrowania typu ograniczenia. Na przykład:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a>Odwołanie do transformatora parametrów

Transformatory parametrów:

* Wykonaj podczas generowania linku do <xref:Microsoft.AspNetCore.Routing.Route>.
* Zaimplementuj `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Konfiguruje się za pomocą <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Wypełnij wartość trasy parametru i Przekształć ją na nową wartość ciągu.
* Wynikiem jest użycie przekształconej wartości w wygenerowanym łączu.

Na przykład niestandardowy `slugify` przekształcania parametrów w wzorcu trasy `blog\{article:slugify}` z `Url.Action(new { article = "MyTestArticle" })` generuje `blog\my-test-article`.

Aby użyć transformatora parametrów w wzorcu trasy, skonfiguruj go najpierw przy użyciu <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> w `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Transformatory parametrów są używane przez platformę do przekształcania identyfikatora URI, w którym jest rozpoznawany punkt końcowy. Na przykład ASP.NET Core MVC używa transformatorów parametrów do przekształcenia wartości trasy używanej w celu dopasowania do `area`, `controller`, `action`i `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

W przypadku powyższej trasy `SubscriptionManagementController.GetAll()` akcji jest dopasowywany do `/subscription-management/get-all`URI. Transformator parametrów nie zmienia wartości trasy użytych do wygenerowania linku. Na przykład `Url.Action("GetAll", "SubscriptionManagement")` dane wyjściowe `/subscription-management/get-all`.

ASP.NET Core udostępnia konwencje interfejsu API do używania transformatorów parametrów z wygenerowanymi trasami:

* ASP.NET Core MVC ma konwencję interfejsu API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention`. Ta Konwencja stosuje określony transformator parametrów do wszystkich tras atrybutów w aplikacji. Transformator parametrów przekształca tokeny trasy atrybutów po ich wymianie. Aby uzyskać więcej informacji, zobacz [używanie transformatora parametrów do dostosowywania zastępowania tokenu](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages ma konwencję interfejsu API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Ta Konwencja stosuje określony transformator parametrów do wszystkich automatycznie odnalezionych Razor Pages. Transformator parametrów przekształca segmenty i nazwy plików Razor Pages tras. Aby uzyskać więcej informacji, zobacz [używanie transformatora parametrów do dostosowywania tras stron](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

## <a name="url-generation-reference"></a>Odwołanie do generacji adresów URL

Poniższy przykład pokazuje, jak wygenerować link do trasy, używając słownika wartości tras i <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> wygenerowany na końcu powyższego przykładu jest `/package/create/123`. Słownik dostarcza wartości `operation` i `id` trasy szablonu "śledzenie trasy pakietów", `package/{operation}/{id}`. Aby uzyskać szczegółowe informacje, zapoznaj się z przykładowym kodem w sekcji [Używanie oprogramowania pośredniczącego usługi routingu](#use-routing-middleware) lub [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Drugim parametrem konstruktora <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> jest zbiór *wartości otoczenia*. Wartości otoczenia są wygodne do użycia, ponieważ ograniczają liczbę wartości, które Deweloper musi określić w kontekście żądania. Bieżące wartości trasy bieżącego żądania są uznawane za wartości otoczenia dla generacji łącza. W `About` akcji ASP.NET Core aplikacji MVC `HomeController`nie trzeba określać wartości trasy kontrolera do łączenia z akcją `Index`,&mdash;zostanie użyta wartość otoczenia `Home`.

Wartości otoczenia, które nie pasują do parametru, są ignorowane. Wartości otoczenia są również ignorowane, gdy jawnie podana wartość przesłania wartość otoczenia. Dopasowanie występuje od lewej do prawej w adresie URL.

Wartości jawnie podane, ale które nie pasują do segmentu trasy, są dodawane do ciągu zapytania. W poniższej tabeli przedstawiono wyniki przy użyciu szablonu trasy `{controller}/{action}/{id?}`.

| Wartości otoczenia                     | Wartości jawne                        | Wynik                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| Controller = "Strona główna"                | Action = "informacje"                       | `/Home/About`           |
| Controller = "Strona główna"                | Controller = "Order", Action = "informacje" | `/Order/About`          |
| Controller = "Home", Color = "Red" | Action = "informacje"                       | `/Home/About`           |
| Controller = "Strona główna"                | Action = "informacje", Color = "Red"        | `/Home/About?color=Red` |

Jeśli trasa ma wartość domyślną, która nie odpowiada parametrowi, a ta wartość jest jawnie określona, musi być zgodna z wartością domyślną:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Generowanie linku generuje tylko łącze do tej trasy, gdy zostaną podane pasujące wartości dla `controller` i `action`.

## <a name="complex-segments"></a>Złożone segmenty

Złożone segmenty (na przykład `[Route("/x{token}y")]`) są przetwarzane przez dopasowanie literałów z prawej strony do lewej w sposób niezachłanney. [Ten kod](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) zawiera szczegółowy opis sposobu dopasowywania segmentów złożonych. [Przykład kodu](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) nie jest używany przez ASP.NET Core, ale zapewnia dobre wyjaśnienie złożonych segmentów.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Routing jest odpowiedzialny za mapowanie identyfikatorów URI żądań w celu rozesłania procedur obsługi i wysyłania żądań przychodzących. Trasy są zdefiniowane w aplikacji i konfigurowane podczas uruchamiania aplikacji. Trasa może opcjonalnie wyodrębnić wartości z adresu URL zawartego w żądaniu. te wartości mogą być następnie używane do przetwarzania żądań. Przy użyciu skonfigurowanych tras z aplikacji Routing jest w stanie generować adresy URL mapowane na procedury obsługi tras.

Aby użyć najnowszych scenariuszy routingu w ASP.NET Core 2,1, określ [wersję zgodności](xref:mvc/compatibility-version) do rejestracji usług MVC w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> Ten dokument obejmuje Routing ASP.NET Core niskiego poziomu. Aby uzyskać informacje na temat ASP.NET Core routingu MVC, zobacz <xref:mvc/controllers/routing>. Aby uzyskać informacje na temat Konwencji routingu w Razor Pages, zobacz <xref:razor-pages/razor-pages-conventions>.

[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Podstawy routingu

Większość aplikacji powinna wybrać podstawowy i opisowy schemat routingu, aby adresy URL były czytelne i zrozumiałe. Domyślna trasa konwencjonalna `{controller=Home}/{action=Index}/{id?}`:

* Obsługuje podstawowy i opisowy schemat routingu.
* Jest użytecznym punktem wyjścia dla aplikacji opartych na interfejsie użytkownika.

Deweloperzy często dodają dodatkowe trasy zwięzła do obszarów o dużym ruchu w aplikacji w wyspecjalizowanych sytuacjach (na przykład blog i punkty końcowe handlu elektronicznego) przy użyciu [routingu atrybutów](xref:mvc/controllers/routing#attribute-routing) lub dedykowanych tras konwencjonalnych.

Interfejsy API sieci Web powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez zlecenia HTTP. Oznacza to, że wiele operacji (na przykład GET, POST) dla tego samego zasobu logicznego będzie używać tego samego adresu URL. Routing atrybutu zapewnia poziom kontroli, który jest wymagany do dokładnego projektowania układu publicznego punktu końcowego interfejsu API.

Aplikacje Razor Pages używają domyślnego routingu konwencjonalnego do obsłużenia nazwanych zasobów w folderze *strony* aplikacji. Dostępne są dodatkowe konwencje, które umożliwiają dostosowanie Razor Pages zachowaniem routingu. Aby uzyskać więcej informacji, zobacz <xref:razor-pages/index> i <xref:razor-pages/razor-pages-conventions>.

Obsługa generowania adresów URL umożliwia tworzenie aplikacji bez adresów URL, które mają być połączone ze sobą. Ta obsługa pozwala rozpocząć od podstawowej konfiguracji routingu i zmodyfikować trasy po ustaleniu układu zasobów aplikacji.

Routing używa *tras* (implementacje <xref:Microsoft.AspNetCore.Routing.IRouter>) do:

* Mapuj przychodzące żądania do *obsługi tras*.
* Generuj adresy URL używane w odpowiedziach.

Domyślnie aplikacja ma jedną kolekcję tras. Po nadejściu żądania trasy w kolekcji są przetwarzane w kolejności, w jakiej istnieją w kolekcji. Platforma próbuje dopasować przychodzący adres URL żądania do trasy w kolekcji, wywołując metodę <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> dla każdej trasy w kolekcji. Odpowiedź może używać routingu do generowania adresów URL (na przykład w przypadku przekierowania lub linków) na podstawie informacji o trasach i w ten sposób unikania zakodowanych adresów URL, co ułatwia łatwość utrzymania.

System routingu ma następujące cechy:

* Składnia szablonu trasy służy do definiowania tras z parametrami trasy z tokenami.
* Dozwolona jest konfiguracja języka końcowego w stylu konwencjonalnym i stylu atrybutu.
* <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> służy do określenia, czy parametr adresu URL zawiera prawidłową wartość dla danego ograniczenia punktu końcowego.
* Modele aplikacji, takie jak MVC/Razor Pages, rejestrują wszystkie trasy z przewidywalną implementacją scenariuszy routingu.
* Odpowiedź może używać routingu do generowania adresów URL (na przykład w przypadku przekierowania lub linków) na podstawie informacji o trasach i w ten sposób unikania zakodowanych adresów URL, co ułatwia łatwość utrzymania.
* Generacja adresów URL jest oparta na trasach, które obsługują arbitralną rozszerzalność. <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> oferuje metody do kompilowania adresów URL.

Routing jest połączony z potokiem [pośredniczącym](xref:fundamentals/middleware/index) przez klasę <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) dodaje Routing do potoku oprogramowania pośredniczącego w ramach swojej konfiguracji i obsługuje routing w aplikacjach MVC i Razor Pages. Aby dowiedzieć się, jak używać routingu jako składnika autonomicznego, zapoznaj się z sekcją [Korzystanie z oprogramowania do routingu](#use-routing-middleware) .

### <a name="url-matching"></a>Dopasowanie adresu URL

Dopasowywanie adresów URL to proces, za pomocą którego Routing wysyła żądanie przychodzące do *procedury obsługi*. Ten proces jest oparty na danych w ścieżce URL, ale można go rozszerzyć w celu uwzględnienia wszelkich danych w żądaniu. Możliwość wysyłania żądań do oddzielnych programów obsługi jest kluczem do skalowania rozmiaru i złożoności aplikacji.

Żądania przychodzące wprowadzają <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, która wywołuje metodę <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> dla każdej trasy w sekwencji. Wystąpienie <xref:Microsoft.AspNetCore.Routing.IRouter> określa, czy *obsłużyć* żądanie przez ustawienie [RouteContext. Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) do <xref:Microsoft.AspNetCore.Http.RequestDelegate>o wartości innej niż null. Jeśli trasa ustawi procedurę obsługi dla żądania, przetwarzanie trasy zostanie zatrzymane, a procedura obsługi zostanie wywołana w celu przetworzenia żądania. Jeśli nie znaleziono procedury obsługi trasy do przetworzenia żądania, oprogramowanie pośredniczące przekazuje żądanie do następnego oprogramowania pośredniczącego w potoku żądania.

Podstawowym wejściem do <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> jest [RouteContext. HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) skojarzony z bieżącym żądaniem. [RouteContext. Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) i [RouteContext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) są ustawiane jako wyjście po dopasowaniu trasy.

Dopasowanie, które wywołuje <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> również ustawia właściwości [RouteContext. RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) na odpowiednie wartości na podstawie przetwarzania żądania wykonanego do tej pory.

[RouteData. Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) to słownik *wartości tras* uzyskanych z trasy. Te wartości są zwykle określane przez tokenizowanie jako adres URL i mogą służyć do akceptowania danych wejściowych użytkownika lub do dalszej akceptacji decyzji w aplikacji.

[RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) to zbiór właściwości dodatkowych danych dotyczących dopasowanej trasy. <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> zapewnia obsługę kojarzenia danych stanu z każdą trasą, dzięki czemu aplikacja może podejmować decyzje na podstawie dopasowanej trasy. Te wartości są zdefiniowane przez dewelopera i **nie** mają wpływu na zachowanie routingu. Ponadto wartości umieszczane w [RouteData. Datatokeny](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) mogą być dowolnego typu, w przeciwieństwie do [RouteData. wartości](xref:Microsoft.AspNetCore.Routing.RouteData.Values), które muszą być konwertowane do i z ciągów.

[RouteData. routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) to lista tras, które brały udział w pomyślnie dopasowane do żądania. Trasy mogą być zagnieżdżone wewnątrz siebie. Właściwość <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> odzwierciedla ścieżkę przez logiczne drzewo tras, które spowodowały dopasowanie. Ogólnie rzecz biorąc, pierwszy element w <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> jest kolekcją tras i powinien być używany do generowania adresów URL. Ostatnim elementem w <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> jest program obsługi trasy, który został dopasowany.

<a name="lg"></a>

### <a name="url-generation"></a>Generowanie adresu URL

Generowanie adresu URL to proces, za pomocą którego Routing może utworzyć ścieżkę URL na podstawie zestawu wartości trasy. Pozwala to na logiczne rozdzielenie między programami obsługi tras i adresami URL, które uzyskują do nich dostęp.

Generowanie adresu URL następuje w podobnym procesie iteracyjnym, ale rozpoczyna się od kodu użytkownika lub struktury wywołującego metodę <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> kolekcji tras. Każda *trasa* ma metodę <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> wywoływana w sekwencji do momentu zwrócenia <xref:Microsoft.AspNetCore.Routing.VirtualPathData> o wartości innej niż null.

Podstawowe dane wejściowe do <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> są następujące:

* [VirtualPathContext. HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [VirtualPathContext. wartości](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

Trasy korzystają głównie z wartości tras dostarczonych przez <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> i <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> do decydowania, czy możliwe jest wygenerowanie adresu URL i wartości do uwzględnienia. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> to zbiór wartości tras, które zostały wygenerowane na podstawie pasującego do bieżącego żądania. Z kolei <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> są wartościami tras, które określają sposób generowania żądanego adresu URL dla bieżącej operacji. <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> jest udostępniana w przypadku, gdy trasa powinna uzyskać usługi lub dodatkowe dane skojarzone z bieżącym kontekstem.

> [!TIP]
> Pomyśl o [VirtualPathContext. wartości](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) jako zestawu zastąpień dla [VirtualPathContext. AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). Generowanie adresów URL próbuje ponownie użyć wartości trasy z bieżącego żądania w celu wygenerowania adresów URL dla linków przy użyciu tych samych wartości trasy lub trasy.

Dane wyjściowe <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> to <xref:Microsoft.AspNetCore.Routing.VirtualPathData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> jest równoległa <xref:Microsoft.AspNetCore.Routing.RouteData>. <xref:Microsoft.AspNetCore.Routing.VirtualPathData> zawiera <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> dla wyjściowego adresu URL oraz kilka dodatkowych właściwości, które powinny być ustawiane przez trasę.

Właściwość [VirtualPathData. VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) zawiera *ścieżkę wirtualną* wygenerowaną przez trasę. W zależności od potrzeb może być konieczne dalsze przetworzenie ścieżki. Jeśli chcesz renderować wygenerowany adres URL w formacie HTML, poprzedź ścieżkę podstawową aplikacji.

[VirtualPathData. router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) jest odwołaniem do trasy, która pomyślnie wygenerowała adres URL.

Właściwości [VirtualPathData. DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) to słownik dodatkowych danych związanych z trasą, która wygenerowała adres URL. Jest to równoległy [RouteData. DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="create-routes"></a>Tworzenie tras

Routing udostępnia klasę <xref:Microsoft.AspNetCore.Routing.Route> jako standardową implementację <xref:Microsoft.AspNetCore.Routing.IRouter>. <xref:Microsoft.AspNetCore.Routing.Route> używa składni *szablonu trasy* do definiowania wzorców do dopasowania względem ścieżki URL, gdy <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> jest wywoływana. <xref:Microsoft.AspNetCore.Routing.Route> używa tego samego szablonu trasy do wygenerowania adresu URL po wywołaniu <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*>.

Większość aplikacji tworzy trasy przez wywołanie <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> lub jednej z podobnych metod rozszerzających zdefiniowanych w <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Dowolna z metod rozszerzenia <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> tworzy wystąpienie <xref:Microsoft.AspNetCore.Routing.Route> i dodaje je do kolekcji tras.

<xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> nie akceptuje parametru procedury obsługi trasy. <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> dodaje tylko trasy, które są obsługiwane przez <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Domyślna procedura obsługi to `IRouter`, a program obsługi może nie obsłużyć żądania. Na przykład ASP.NET Core MVC jest zwykle skonfigurowany jako domyślny program obsługi, który obsługuje tylko żądania zgodne z dostępnym kontrolerem i akcją. Aby dowiedzieć się więcej na temat routingu w MVC, zobacz <xref:mvc/controllers/routing>.

Poniższy przykład kodu jest przykładem wywołania <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> używanego przez typową definicję trasy ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ten szablon dopasowuje ścieżkę URL i wyodrębnia wartości tras. Na przykład ścieżka `/Products/Details/17` generuje następujące wartości trasy: `{ controller = Products, action = Details, id = 17 }`.

Wartości tras są określane przez podzielenie ścieżki URL na segmenty i dopasowanie do każdego segmentu przy użyciu nazwy *parametru trasy* w szablonie trasy. Parametry trasy są nazywane. Parametry zdefiniowane przez ujęcie nazwy parametru w nawiasach klamrowych `{ ... }`.

Poprzedni szablon może być również zgodny z ścieżką URL `/` i generować wartości `{ controller = Home, action = Index }`. Dzieje się tak, ponieważ parametry `{controller}` i `{action}` trasy mają wartości domyślne, a parametr trasy `id` jest opcjonalny. Znak równości (`=`), po którym następuje wartość po nazwie parametru trasy definiuje wartość domyślną dla parametru. Znak zapytania (`?`) po nazwie parametru trasy definiuje opcjonalny parametr.

Parametry trasy z wartością domyślną *zawsze* generują wartość trasy w przypadku dopasowania trasy. Parametry opcjonalne nie generują wartości trasy, jeśli nie ma odpowiedniego segmentu ścieżki adresu URL. Szczegółowe opisy scenariuszy i składni szablonów tras można znaleźć w sekcji [Dokumentacja dotycząca szablonu trasy](#route-template-reference) .

W poniższym przykładzie definicja parametru trasy `{id:int}` definiuje [ograniczenie trasy](#route-constraint-reference) dla parametru trasy `id`:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ten szablon dopasowuje ścieżkę URL, taką jak `/Products/Details/17`, ale nie `/Products/Details/Apples`. Ograniczenia trasy implementują <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> i sprawdzają wartości routingu w celu ich zweryfikowania. W tym przykładzie wartość trasy `id` musi być możliwa do przekonwertowania na liczbę całkowitą. Aby uzyskać wyjaśnienie ograniczeń trasy dostarczonych przez platformę, zobacz temat [ograniczenia trasy](#route-constraint-reference) .

Dodatkowe przeciążenia <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> akceptują wartości `constraints`, `dataTokens`i `defaults`. Typowym użyciem tych parametrów jest przekazanie anonimowo wpisanego obiektu, gdzie nazwy właściwości typu anonimowego są zgodne z nazwami parametrów trasy.

Poniższe <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> przykłady tworzą równoważne trasy:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> Wbudowana Składnia służąca do definiowania ograniczeń i wartości domyślnych może być wygodna dla prostych tras. Istnieją jednak scenariusze, takie jak tokeny danych, które nie są obsługiwane przez składnię wbudowaną.

Poniższy przykład ilustruje kilka dodatkowych scenariuszy:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Poprzedni szablon pasuje do ścieżki URL, takiej jak `/Blog/All-About-Routing/Introduction`, i wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Domyślne wartości trasy dla `controller` i `action` są generowane przez trasę, nawet jeśli w szablonie nie ma odpowiednich parametrów trasy. Wartości domyślne można określić w szablonie trasy. Parametr trasy `article` jest zdefiniowany jako *przechwycenie* przez wygląd gwiazdki (`*`) przed nazwą parametru trasy. Catch-wszystkie parametry tras przechwytują resztę ścieżki URL i mogą również pasować do pustego ciągu.

Poniższy przykład dodaje ograniczenia trasy i tokeny danych:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Poprzedni szablon pasuje do ścieżki URL, takiej jak `/en-US/Products/5`, i wyodrębnia wartości `{ controller = Products, action = Details, id = 5 }` i `{ locale = en-US }`tokenów danych.

![Lokalne tokeny systemu Windows](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generowanie adresu URL klasy trasy

Klasa <xref:Microsoft.AspNetCore.Routing.Route> może również wykonywać generowanie adresów URL przez połączenie zestawu wartości tras z jego szablonem trasy. Jest to logicznie proces odwrotny pasujący do ścieżki URL.

> [!TIP]
> Aby lepiej zrozumieć generowanie adresów URL, Załóżmy, jaki adres URL ma zostać wygenerowany, a następnie pomyśl o sposobie dopasowania szablonu trasy do tego adresu URL. Jakie wartości zostałyby wygenerowane? Jest to sztywny odpowiednik działania generowania adresów URL w klasie <xref:Microsoft.AspNetCore.Routing.Route>.

Poniższy przykład używa ogólnej trasy domyślnej ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Po `{ controller = Products, action = List }`wartości trasy zostanie wygenerowany adres URL `/Products/List`. Wartości trasy są zastępowane odpowiednimi parametrami trasy w celu utworzenia ścieżki URL. Ponieważ `id` jest opcjonalnym parametrem trasy, adres URL został pomyślnie wygenerowany bez wartości dla `id`.

Po `{ controller = Home, action = Index }`wartości trasy zostanie wygenerowany adres URL `/`. Podane wartości trasy są zgodne z wartościami domyślnymi, a segmenty odpowiadające wartościom domyślnym są bezpiecznie pomijane.

Oba adresy URL wygenerowały rundę z następującą definicją trasy (`/Home/Index` i `/`) tworzą te same wartości trasy, które zostały użyte do wygenerowania adresu URL.

> [!NOTE]
> Aplikacja używająca ASP.NET Core MVC powinna używać <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> do generowania adresów URL zamiast bezpośredniego wywoływania do routingu.

Aby uzyskać więcej informacji na temat generowania adresów URL, zobacz sekcję [informacje dotyczące generowania adresów URL](#url-generation-reference) .

## <a name="use-routing-middleware"></a>Korzystanie z oprogramowania pośredniczącego routingu

Odwołuje się do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) w pliku projektu aplikacji.

Dodawanie routingu do kontenera usługi w `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Trasy muszą być skonfigurowane w metodzie `Startup.Configure`. Przykładowa aplikacja używa następujących interfejsów API:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; dopasowuje tylko żądania HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

W poniższej tabeli przedstawiono odpowiedzi z podanym identyfikatorem URI.

| Identyfikator URI                    | Odpowiedź                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Cześć! Wartości trasy: [Operation, Create], [ID, 3] |
| `/package/track/-3`    | Cześć! Wartości trasy: [Operation, Track], [ID,-3] |
| `/package/track/-3/`   | Cześć! Wartości trasy: [Operation, Track], [ID,-3] |
| `/package/track/`      | Żądanie przepada w, brak dopasowania.              |
| `GET /hello/Joe`       | Witaj, Jan!                                          |
| `POST /hello/Joe`      | Żądanie przepada w, dopasowuje tylko HTTP GET. |
| `GET /hello/Joe/Smith` | Żądanie przepada w, brak dopasowania.              |

W przypadku konfigurowania pojedynczej trasy Wywołaj <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> przekazywanie w wystąpieniu `IRouter`. Nie musisz używać <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

Struktura zawiera zestaw metod rozszerzających do tworzenia tras (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

Niektóre z wymienionych metod, takie jak <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, wymagają <xref:Microsoft.AspNetCore.Http.RequestDelegate>. <xref:Microsoft.AspNetCore.Http.RequestDelegate> jest używany jako *program obsługi trasy* , gdy trasa pasuje. Inne metody w tej rodzinie umożliwiają skonfigurowanie potoku oprogramowania pośredniczącego, które ma być używane jako procedura obsługi trasy. Jeśli metoda `Map*` nie akceptuje procedury obsługi, takiej jak <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, używa <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

Metody `Map[Verb]` używają ograniczeń do ograniczenia trasy do zlecenia HTTP w nazwie metody. Na przykład zobacz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> i <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Odwołanie do szablonu trasy

Tokeny w nawiasach klamrowych (`{ ... }`) definiują *Parametry trasy* , które są powiązane w przypadku dopasowania trasy. Można zdefiniować więcej niż jeden parametr trasy w segmencie trasy, ale muszą one być oddzielone wartością literału. Na przykład `{controller=Home}{action=Index}` nie jest prawidłową trasą, ponieważ nie ma żadnej wartości literału między `{controller}` i `{action}`. Te parametry trasy muszą mieć nazwę i mogą mieć określone dodatkowe atrybuty.

Tekst literału inny niż parametry trasy (na przykład `{id}`) i separator ścieżki `/` musi być zgodny z tekstem w adresie URL. W dopasowaniu do tekstu nie jest rozróżniana wielkość liter i w oparciu o zdekodowaną reprezentację ścieżki URL. Aby dopasować Ogranicznik parametru trasy literału (`{` lub `}`), należy wprowadzić ogranicznik przez powtórzenie znaku (`{{` lub `}}`).

Wzorce adresów URL, które próbują przechwycić nazwę pliku z opcjonalnym rozszerzeniem pliku, mają dodatkowe uwagi. Rozważmy na przykład szablon `files/{filename}.{ext?}`. Gdy istnieją wartości dla obu `filename` i `ext`, są wypełniane obie wartości. Jeśli w adresie URL istnieje tylko wartość `filename`, trasa pasuje do, ponieważ końcowy okres (`.`) jest opcjonalny. Następujące adresy URL pasują do tej trasy:

* `/files/myFile.txt`
* `/files/myFile`

Możesz użyć gwiazdki (`*`) jako prefiksu do parametru trasy, aby powiązać z resztą identyfikatora URI. Jest to nazywane parametrem *"catch-all"* . Na przykład `blog/{*slug}` dopasowuje dowolny identyfikator URI, który rozpoczyna się od `/blog` i ma dowolną wartość, która jest przypisana do wartości `slug` Route. Catch-wszystkie parametry można także dopasować do pustego ciągu.

Parametr catch-all wyprowadza odpowiednie znaki, gdy trasa jest używana do generowania adresu URL, w tym znaków separatora ścieżki (`/`). Na przykład `foo/{*path}` trasy z wartościami trasy `{ path = "my/path" }` generuje `foo/my%2Fpath`. Zwróć uwagę na odwrócony ukośnik.

Parametry trasy mogą mieć *wartości domyślne* , określając wartość domyślną po nazwie parametru oddzielone znakiem równości (`=`). Na przykład `{controller=Home}` definiuje `Home` jako wartość domyślną dla `controller`. Wartość domyślna jest używana, jeśli żadna wartość nie jest obecna w adresie URL dla parametru. Parametry tras są opcjonalne przez dołączenie znaku zapytania (`?`) na końcu nazwy parametru, jak w `id?`. Różnica między wartościami opcjonalnymi i domyślnymi parametrami trasy polega na tym, że parametr trasy z wartością domyślną zawsze generuje wartość&mdash;opcjonalny parametr ma wartość tylko wtedy, gdy wartość jest podana przez adres URL żądania.

Parametry trasy mogą mieć ograniczenia, które muszą być zgodne z wartością trasy powiązaną z adresem URL. Dodanie dwukropek (`:`) i nazwy ograniczenia po nazwie parametru trasy określa *ograniczenie wbudowane* dla parametru trasy. Jeśli ograniczenie wymaga argumentów, są one ujęte w nawiasy (`(...)`) po nazwie ograniczenia. Można określić wiele ograniczeń wbudowanych, dołączając inną dwukropek (`:`) i nazwę ograniczenia.

Nazwa i argumenty ograniczenia są przesyłane do usługi <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> w celu utworzenia wystąpienia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> do użycia w przetwarzaniu adresów URL. Na przykład szablon trasy `blog/{article:minlength(10)}` określa ograniczenie `minlength` z argumentem `10`. Aby uzyskać więcej informacji na temat ograniczeń trasy i listę ograniczeń zapewnianych przez platformę, zobacz sekcję [odwołanie do ograniczenia trasy](#route-constraint-reference) .

W poniższej tabeli przedstawiono przykładowe szablony tras i ich zachowanie.

| Szablon trasy                           | Przykładowy pasujący identyfikator URI    | &hellip; identyfikatora URI żądania                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Dopasowuje tylko jedną ścieżkę `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Dopasowuje i ustawia `Page`, aby `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Dopasowuje i ustawia `Page`, aby `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Mapuje na kontroler `Products` i akcję `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Mapuje na kontroler `Products` i akcję `Details` (`id` ustawioną na 123). |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | Mapuje na kontroler `Home` i metodę `Index` (`id` jest ignorowana).        |

Użycie szablonu jest ogólnie najprostszym podejściem do routingu. Ograniczenia i wartości domyślne można także określić poza szablonem trasy.

> [!TIP]
> Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby zobaczyć, jak wbudowane implementacje routingu, takie jak <xref:Microsoft.AspNetCore.Routing.Route>, pasują do żądań.

## <a name="reserved-routing-names"></a>Zastrzeżone nazwy routingu

Następujące słowa kluczowe są nazwami zarezerwowanymi i nie mogą być używane jako nazwy tras lub parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odwołanie do ograniczenia trasy

Ograniczenia trasy są wykonywane, gdy nastąpiło dopasowanie do przychodzącego adresu URL, a Ścieżka adresu URL jest zgodna z wartościami trasy. Ograniczenia trasy zwykle sprawdzają wartość trasy skojarzoną za pośrednictwem szablonu trasy i podejmują decyzję o tym, czy wartość jest akceptowalna. Niektóre ograniczenia trasy używają danych poza wartością trasy, aby uwzględnić, czy żądanie może być kierowane. Na przykład <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> może zaakceptować lub odrzucić żądanie na podstawie jego zlecenia HTTP. Ograniczenia są używane w żądaniach routingu i generowania linków.

> [!WARNING]
> Nie używaj ograniczeń **sprawdzania poprawności danych wejściowych**. Jeśli ograniczenia są używane do **sprawdzania poprawności danych wejściowych**, nieprawidłowe dane wejściowe w odpowiedzi *404 — nie znaleziono* , zamiast *żądania 400-złe* z odpowiednim komunikatem o błędzie. Ograniczenia trasy są **używane do** odróżniania podobnych tras, a nie do sprawdzania danych wejściowych dla określonej trasy.

W poniższej tabeli przedstawiono przykładowe ograniczenia trasy i ich oczekiwane zachowanie.

| ograniczenie | Przykład | Przykładowe dopasowania | Uwagi |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Dopasowuje dowolną liczbę całkowitą |
| `bool` | `{active:bool}` | `true`, `FALSE` | Dopasowuje `true` lub `false` (bez uwzględniania wielkości liter) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Dopasowuje prawidłową wartość `DateTime` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Dopasowuje prawidłową wartość `decimal` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Dopasowuje prawidłową wartość `double` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Dopasowuje prawidłową wartość `float` (w niezmiennej kulturze — Zobacz ostrzeżenie) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Dopasowuje prawidłową wartość `Guid` |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Dopasowuje prawidłową wartość `long` |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Ciąg musi składać się z co najmniej 4 znaków |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Ciąg nie może zawierać więcej niż 8 znaków |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Ciąg musi zawierać dokładnie 12 znaków |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Ciąg musi mieć długość co najmniej 8 znaków |
| `min(value)` | `{age:min(18)}` | `19` | Wartość całkowita musi być równa co najmniej 18 |
| `max(value)` | `{age:max(120)}` | `91` | Wartość całkowita nie może być większa niż 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Wartość całkowita musi być równa co najmniej 18, ale nie więcej niż 120 |
| `alpha` | `{name:alpha}` | `Rick` | Ciąg musi zawierać co najmniej jeden znak alfabetyczny (`a`-`z`, bez uwzględniania wielkości liter) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Ciąg musi być zgodny z wyrażeniem regularnym (zobacz Porady dotyczące definiowania wyrażenia regularnego) |
| `required` | `{name:required}` | `Rick` | Służy do wymuszania, że podczas generowania adresu URL jest obecna wartość, która nie jest wartością parametru |

Wielokrotne ograniczenia rozdzielane średnikami można zastosować do jednego parametru. Na przykład następujące ograniczenie ogranicza parametr do wartości całkowitej 1 lub wyższej:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Ograniczenia trasy, które weryfikują adres URL i są konwertowane na typ CLR (takie jak `int` lub `DateTime`), zawsze używają niezmiennej kultury. W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny. Ograniczenia trasy dostarczone przez platformę nie modyfikują wartości przechowywanych w wartościach trasy. Wszystkie wartości tras analizowane na podstawie adresu URL są przechowywane jako ciągi. Na przykład, ograniczenie `float` próbuje skonwertować wartość trasy na float, ale przekonwertowana wartość jest używana tylko w celu sprawdzenia, czy można ją przekonwertować na wartość zmiennoprzecinkową.

## <a name="regular-expressions"></a>Wyrażenia regularne

ASP.NET Core Framework dodaje `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` do konstruktora wyrażeń regularnych. Aby uzyskać opis tych elementów członkowskich, zobacz <xref:System.Text.RegularExpressions.RegexOptions>.

Wyrażenia regularne używają ograniczników i tokenów podobnych do tych używanych w ramach routingu i C# języka. Tokeny wyrażenia regularnego muszą być zmienione. Aby użyć wyrażenia regularnego `^\d{3}-\d{2}-\d{4}$` w routingu, wyrażenie musi mieć `\` (pojedynczy ukośnik odwrotny) podany w ciągu jako `\\` (podwójny ukośnik odwrotny) w pliku C# źródłowym w celu wypróbowania znaku ucieczki `\` ciągu znaków (chyba że jest używany [literały ciągu Verbatim](/dotnet/csharp/language-reference/keywords/string)). Na znaki ogranicznika parametru routingu ucieczki (`{`, `}`, `[`, `]`), podwójne znaki w wyrażeniu (`{{`, `}`, `[[`, `]]`). W poniższej tabeli przedstawiono wyrażenie regularne i wersja z ucieczką.

| Wyrażenie regularne    | Wyrażenie regularne o zmienionym znaczeniu     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Wyrażenia regularne używane w routingu często zaczynają się od znaku daszka (`^`) i dopasowują pozycję początkową ciągu. Wyrażenia często kończą się znakiem dolara (`$`) i końcem ciągu. Znaki `^` i `$` zapewniają, że wyrażenie regularne dopasowuje całą wartość parametru trasy. Bez znaków `^` i `$` wyrażenie regularne dopasowuje dowolny podciąg w ciągu, co jest często niepożądane. W poniższej tabeli przedstawiono przykłady i wyjaśniono, dlaczego są one zgodne lub niezgodne.

| Wyrażenie   | String    | Dopasowanie | Komentarz               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Cześć     | Tak   | Dopasowania podciągów     |
| `[a-z]{2}`   | 123abc456 | Tak   | Dopasowania podciągów     |
| `[a-z]{2}`   | mz        | Tak   | Wyrażenie dopasowania    |
| `[a-z]{2}`   | MZ        | Tak   | Bez uwzględniania wielkości liter    |
| `^[a-z]{2}$` | Cześć     | Nie    | Zobacz `^` i `$` powyżej |
| `^[a-z]{2}$` | 123abc456 | Nie    | Zobacz `^` i `$` powyżej |

Aby uzyskać więcej informacji na temat składni wyrażeń regularnych, zobacz [.NET Framework wyrażeń regularnych](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Aby ograniczyć parametr do znanego zestawu możliwych wartości, użyj wyrażenia regularnego. Na przykład `{action:regex(^(list|get|create)$)}` dopasowuje wartość trasy `action` do `list`, `get`lub `create`. Jeśli przeszedł do słownika ograniczeń, ciąg `^(list|get|create)$` jest równoważny. Ograniczenia, które są przesyłane w słowniku ograniczenia (nie wbudowane w szablon), które nie pasują do jednego ze znanych ograniczeń, są również traktowane jako wyrażenia regularne.

## <a name="custom-route-constraints"></a>Niestandardowe ograniczenia trasy

Oprócz wbudowanych ograniczeń trasy niestandardowe ograniczenia trasy mogą być tworzone przez implementację interfejsu <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. Interfejs <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> zawiera pojedynczą metodę, `Match`, która zwraca `true` w przypadku spełnienia ograniczenia i `false` w inny sposób.

Aby można było użyć niestandardowego <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, typ ograniczenia trasy musi być zarejestrowany w <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> aplikacji w kontenerze usługi aplikacji. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> jest słownikiem, który mapuje klucze ograniczeń trasy na implementacje <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, które weryfikują te ograniczenia. <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> aplikacji można zaktualizować w `Startup.ConfigureServices` w ramach [usług. Wywołanie addrouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) lub przez skonfigurowanie <xref:Microsoft.AspNetCore.Routing.RouteOptions> bezpośrednio z `services.Configure<RouteOptions>`. Na przykład:

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

Ograniczenie można następnie zastosować do tras w zwykły sposób, przy użyciu nazwy określonej podczas rejestrowania typu ograniczenia. Na przykład:

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a>Odwołanie do generacji adresów URL

Poniższy przykład pokazuje, jak wygenerować link do trasy, używając słownika wartości tras i <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> wygenerowany na końcu powyższego przykładu jest `/package/create/123`. Słownik dostarcza wartości `operation` i `id` trasy szablonu "śledzenie trasy pakietów", `package/{operation}/{id}`. Aby uzyskać szczegółowe informacje, zapoznaj się z przykładowym kodem w sekcji [Używanie oprogramowania pośredniczącego usługi routingu](#use-routing-middleware) lub [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Drugim parametrem konstruktora <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> jest zbiór *wartości otoczenia*. Wartości otoczenia są wygodne do użycia, ponieważ ograniczają liczbę wartości, które Deweloper musi określić w kontekście żądania. Bieżące wartości trasy bieżącego żądania są uznawane za wartości otoczenia dla generacji łącza. W `About` akcji ASP.NET Core aplikacji MVC `HomeController`nie trzeba określać wartości trasy kontrolera do łączenia z akcją `Index`,&mdash;zostanie użyta wartość otoczenia `Home`.

Wartości otoczenia, które nie pasują do parametru, są ignorowane. Wartości otoczenia są również ignorowane, gdy jawnie podana wartość przesłania wartość otoczenia. Dopasowanie występuje od lewej do prawej w adresie URL.

Wartości jawnie podane, ale które nie pasują do segmentu trasy, są dodawane do ciągu zapytania. W poniższej tabeli przedstawiono wyniki przy użyciu szablonu trasy `{controller}/{action}/{id?}`.

| Wartości otoczenia                     | Wartości jawne                        | Wynik                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| Controller = "Strona główna"                | Action = "informacje"                       | `/Home/About`           |
| Controller = "Strona główna"                | Controller = "Order", Action = "informacje" | `/Order/About`          |
| Controller = "Home", Color = "Red" | Action = "informacje"                       | `/Home/About`           |
| Controller = "Strona główna"                | Action = "informacje", Color = "Red"        | `/Home/About?color=Red` |

Jeśli trasa ma wartość domyślną, która nie odpowiada parametrowi, a ta wartość jest jawnie określona, musi być zgodna z wartością domyślną:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Generowanie linku generuje tylko łącze do tej trasy, gdy zostaną podane pasujące wartości dla `controller` i `action`.

## <a name="complex-segments"></a>Złożone segmenty

Złożone segmenty (na przykład `[Route("/x{token}y")]`) są przetwarzane przez dopasowanie literałów z prawej strony do lewej w sposób niezachłanney. [Ten kod](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) zawiera szczegółowy opis sposobu dopasowywania segmentów złożonych. [Przykład kodu](https://github.com/dotnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) nie jest używany przez ASP.NET Core, ale zapewnia dobre wyjaśnienie złożonych segmentów.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/dotnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end
