---
title: Routing w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak funkcje routingu platformy ASP.NET Core jest odpowiedzialny za mapowanie przychodzące żądanie do programu obsługi trasy.
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2018
uid: fundamentals/routing
ms.openlocfilehash: a014782ba503bc8bd0fdefb4cb4f382aa8fde4cd
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244973"
---
# <a name="routing-in-aspnet-core"></a>Routing w programie ASP.NET Core

Przez [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), i [Rick Anderson](https://twitter.com/RickAndMSFT)

Funkcje routingu jest odpowiedzialny za mapowanie przychodzące żądanie do programu obsługi trasy. Trasy są zdefiniowane w aplikacji i skonfigurowane, po uruchomieniu aplikacji. Trasy Opcjonalnie można wyodrębnić wartości z adresu URL zawartych w żądaniu, a te wartości mogą następnie służyć do przetwarzania żądania. Korzystając z informacji o trasie z aplikacji, funkcji routingu jest również możliwe do generowania adresów URL, które mapują do obsługi trasy. W związku z tym routing można znaleźć programu obsługi trasy na podstawie adresu URL lub znaleźć adres URL odpowiadający programu obsługi trasy, na podstawie informacji programu obsługi trasy.

Większość aplikacji należy wybrać podstawowe i opisowe schemat routingu, aby adresy URL są czytelne i zrozumiałe. Domyślną trasę konwencjonalne `{controller=Home}/{action=Index}/{id?}`:

* Obsługuje podstawowe i opisowe schemat routingu:
* To dobry punkt początkowy dla aplikacji sieci web przeznaczony do użytku przez przeglądarki.

Często dodania dodatkowych tras zwięzła do obszarów dużego ruchu aplikacji w sytuacjach wyspecjalizowanego (na przykład, blogów, handlu elektronicznego) przy użyciu [trasowanie atrybutów](xref:mvc/controllers/routing#attribute-routing) lub trasy są konwencjonalne funkcje w wersji dedykowanej.

Interfejsy API sieci Web należy używać trasowanie atrybutów do modelu funkcjonalność aplikacji jako zestaw zasobów, której operacje są reprezentowane przez zleceń HTTP. Oznacza to, że wiele operacji (na przykład GET, POST) na ten sam zasób logicznej będzie używać tego samego adresu URL. Routing atrybutów zapewnia poziom kontroli, który jest potrzebny do uważnie projektować przestrzeni URL interfejsu API.

Obsługa generowania adresu URL MVC umożliwia aplikacji można rozwijać bez twardych kodowania adresy URL, aby połączyć aplikację. Dzięki temu począwszy od podstawowej konfiguracji routingu i modyfikowania trasy, po kształt aplikacji jest określony.

> [!IMPORTANT]
> W tym dokumencie opisano niskiego poziomu routingu platformy ASP.NET Core. Aby uzyskać informacji na temat routingu platformy ASP.NET Core MVC, zobacz <xref:mvc/controllers/routing>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Podstawy routingu

Używa routingu *trasy* (implementacje <xref:Microsoft.AspNetCore.Routing.IRouter>) do:

* Mapowanie żądań przychodzących do *trasy obsługi*.
* Generowanie adresy URL używane w odpowiedzi.

Ogólnie rzecz biorąc aplikacja ma jedną kolekcję tras. Gdy żądanie dociera, kolekcji tras są przetwarzane w kolejności. Żądanie przychodzące szuka trasę, która pasuje do adresu URL żądania, wywołując <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> metody w każdej z dostępnych tras w kolekcji tras. Z drugiej strony odpowiedzi, można używać routingu do generowania adresów URL (na przykład przekierowanie lub linki), w oparciu o trasach i dlatego uniknąć trwale kodować adresów URL, która pomaga w utrzymaniu.

Routing jest podłączony do [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku przez <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> klasy. [Platforma ASP.NET Core MVC](xref:mvc/overview) dodaje routingu do potoku oprogramowania pośredniczącego w ramach jego konfiguracji. Aby dowiedzieć się, przy użyciu routingu jako składnik autonomicznych, zobacz [użycia routingu oprogramowania pośredniczącego](#use-routing-middleware) sekcji.

### <a name="url-matching"></a>Dopasowanie adresu URL

Dopasowanie adresu URL jest proces, który wysyła routingu przychodzącego żądania *obsługi*. Ten proces jest na podstawie danych ze ścieżki adresu URL, ale może zostać rozszerzony do należy wziąć pod uwagę wszystkie dane w żądaniu. Możliwość wysyłania żądań do rozdzielenia obsługi to klucz do skalowania, rozmiar i złożoność aplikacji.

Wprowadź żądań przychodzących `RouterMiddleware`, która wywołuje metodę <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> metody dla każdej trasy w sekwencji. <xref:Microsoft.AspNetCore.Routing.IRouter> Wybierze wystąpienie czy *obsługi* żądania przez ustawienie [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) do innych niż null <xref:Microsoft.AspNetCore.Http.RequestDelegate>. Jeśli trasa Ustawia program obsługi żądania, route zatrzymanie przetwarzania i program obsługi jest wywoływane w celu przetworzenia żądania. Jeśli wszystkie trasy są sprawdzane i żadna procedura obsługi nie znajduje się na żądanie, oprogramowanie pośredniczące wywołuje *dalej*, a następne oprogramowanie pośredniczące w potoku żądania jest wywoływana.

Podstawowe dane wejściowe `RouteAsync` jest [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) skojarzone z bieżącego żądania. `RouteContext.Handler` i [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) są dane wyjściowe, gdy trasa pasuje.

Dopasowanie ciągu `RouteAsync` spowoduje także ustawienie właściwości `RouteContext.RouteData` odpowiednie wartości oparte na przetwarzanie żądań wykonywane do tej pory. Jeśli trasa pasuje do żądania `RouteContext.RouteData` zawiera informacje o stanie ważne informacje *wynik*.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) jest słownikiem *wartości trasy* wyprodukowanych z trasy. Wartości te są zwykle określane przez tokenizowanie adres URL i może służyć do przyjmowania danych wejściowych użytkownika lub dodatkowo dispatching decyzje wewnątrz aplikacji.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) to zbiór właściwości dodatkowych danych związanych z dopasowanej trasy. `DataTokens` dostarczone dane o pomocy technicznej kojarzenie stanu z każdej trasy tak, aby aplikacja może wykonać później na podstawie decyzji w trasie, która pasuje. Te wartości są definiowane przez projektanta i wykonaj **nie** mają wpływ na zachowanie routingu w dowolny sposób. Ponadto przechowalni wartości w `RouteData.DataTokens` może być dowolnego typu, w przeciwieństwie do `RouteData.Values`, które muszą być łatwo konwertowany do i z ciągów.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) znajduje się lista tras, na których uczestniczyła w pomyślnie dopasowywania żądania. Trasy mogą być zagnieżdżone wewnątrz siebie nawzajem. `Routers` Właściwość odzwierciedla drogę przez drzewo logiczne tras, które spowodowały dopasowanie. Ogólnie rzecz biorąc, pierwszy element `Routers` jest kolekcją tras i powinny być używane do generowania adresu URL. Ostatnim elementem w `Routers` jest programu obsługi trasy, który jest zgodny.

### <a name="url-generation"></a>Generowanie adresu URL

Generowanie adresu URL to proces, routingu, które można utworzyć ścieżki adresu URL na podstawie zestawu wartości trasy. Dzięki temu do logicznego rozdzielania między inne programy obsługi oraz w adresach URL, które do nich dostęp.

Generowanie adresu URL następuje podobny proces iteracyjny, ale zaczyna się od kodu użytkownika lub framework wywoływanie <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> metoda kolekcji tras. Każdy *trasy* uzyska jego `GetVirtualPath` metoda wywoływana w sekwencji, aż do innych niż null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> jest zwracana.

Podstawowy danych wejściowych do `GetVirtualPath` są:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Trasy przede wszystkim używasz wartości trasy, dostarczone przez `Values` i `AmbientValues` zdecydować, czy jest możliwe do generowania adresu URL i wartości, których do uwzględnienia. `AmbientValues` Zestaw wartości trasy, które zostały utworzone z dopasowywania bieżące żądanie w systemie routingu. Z kolei `Values` są wartości trasy, które określają sposób generowania żądany adres URL dla bieżącej operacji. `HttpContext` Znajduje się w przypadku, gdy trasa musi uzyskać services lub dodatkowe dane skojarzone z bieżącym kontekstem.

> [!TIP]
> Traktować [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) jako zbiór zastąpienia [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). Generowanie adresu URL spróbuje ponownie użyć wartości trasy z bieżącego żądania, aby ułatwić do generowania adresów URL dla łącza przy użyciu tego samego trasę lub trasy wartości.

Dane wyjściowe `GetVirtualPath` jest `VirtualPathData`. `VirtualPathData` jest równolegle z `RouteData`. `VirtualPathData` zawiera `VirtualPath` dla adresu URL danych wyjściowych i niektóre dodatkowe właściwości, które powinny zostać ustawione przez trasę.

[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) właściwość zawiera *ścieżka wirtualna* produkowane przez trasę. W zależności od potrzeb może być konieczne ścieżkę do dalszego przetwarzania. Jeśli chcesz renderować wygenerowany adres URL w formacie HTML, należy poprzedzić ścieżki podstawowej aplikacji.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) jest odwołaniem do pomyślnie wygenerowano adres URL trasy.

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) właściwości jest słownikiem dodatkowych danych dotyczących trasy, który wygenerował adresu URL. Jest to równoległego z [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="creating-routes"></a>Tworzenie tras

Routing zapewnia <xref:Microsoft.AspNetCore.Routing.Route> klasy jako standardowej implementacji <xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` używa *szablon trasy* Składnia umożliwiająca zdefiniowanie wzorców, które dopasowywania ścieżki adresu URL po <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> jest wywoływana. `Route` używa tego samego szablonu trasy do wygenerowania adresu URL po `GetVirtualPath` jest wywoływana.

Większość aplikacji utworzyć trasy, wywołując <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> lub jednego z podobne metody rozszerzenia zdefiniowane na <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Wszystkie te metody Utwórz wystąpienie obiektu <xref:Microsoft.AspNetCore.Routing.Route> i dodać go do kolekcji tras.

`MapRoute` nie przyjmuje parametrów programu obsługi trasy. `MapRoute` tylko dodaje trasy, które są obsługiwane przez <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Ponieważ domyślny program obsługi jest `IRouter`, mogą zdecydować, nie można obsłużyć żądania. Na przykład ASP.NET Core MVC jest zazwyczaj skonfigurowany jako domyślny program obsługi, który obsługuje tylko żądania które pasują do dostępnych kontrolerów i akcji. Aby dowiedzieć się więcej na temat routingu do MVC, zobacz <xref:mvc/controllers/routing>.

Poniższy przykład kodu jest przykładem `MapRoute` wywołania używany przez typowy definicję trasy ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ten szablon jest zgodny Ścieżka adresu URL, takich jak `/Products/Details/17` i wyodrębnia wartości trasy `{ controller = Products, action = Details, id = 17 }`. Wartości trasy są określane przez dopasowanie każdego segmentu i dzielenia Ścieżka adresu URL na segmenty *kierowanie parametru* nazwy szablonu trasy. Noszą nazwy parametrów trasy. Są one definiowane, umieszczając nazwę parametru w nawiasach klamrowych `{ ... }`.

Powyższy szablon może być również zgodna ze ścieżką URL `/` i spowodowało wygenerowanie wartości `{ controller = Home, action = Index }`. Dzieje się tak dlatego `{controller}` i `{action}` parametrów trasy mają przypisane wartości domyślne i `id` parametr trasy jest opcjonalny. Znak równości `=` logowanie następuje wartość po nazwę parametru trasa określa wartość domyślną dla parametru. Znak zapytania `?` po nazwy parametru trasy definiuje parametru jako opcjonalną. Parametry o wartości domyślne trasy *zawsze* uzyskiwania wartości trasy, gdy jest zgodna z trasy. Parametry opcjonalne nie tworzy wartości trasy, jeśli nie było żadnych odpowiadającym segmencie ścieżki adresu URL.

Zobacz [dokumentacja dotycząca szablonów tras](#route-template-reference) uzyskać dokładny opis funkcji szablonu trasy i składni.

Ten przykład obejmuje *trasy ograniczenie*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ten szablon jest zgodny Ścieżka adresu URL, takich jak `/Products/Details/17` , ale nie `/Products/Details/Apples`. Definicji parametru trasy `{id:int}` definiuje [trasy ograniczenie](#route-constraint-reference) dla `id` parametru trasy. Implementowanie ograniczenia trasy <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> i badać wartości trasy, aby je zweryfikować. W tym przykładzie wartość trasy `id` musi być konwertowany na liczbę całkowitą. Zobacz [odwołanie w przypadku ograniczenia trasy](#route-constraint-reference) uzyskać bardziej szczegółowy opis ograniczenia trasy, które są dostarczane przez platformę.

Dodatkowe przeciążenia `MapRoute` akceptowanych wartości `constraints`, `dataTokens`, i `defaults`. Te dodatkowe parametry `MapRoute` są zdefiniowane jako typ `object`. Jest typowy tych parametrów do przekazania obiektu anonimowo wpisane, gdzie nazwy właściwości typu anonimowego dopasowania trasy nazwy parametrów.

Dwa poniższe przykłady tworzenia tras równoważne:

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
> Składnię w tekście do definiowania ograniczeń i ustawień domyślnych może być wygodną proste trasy. Istnieją jednak funkcji, takich jak tokeny danych, które nie są obsługiwane przez składnię w tekście.

W poniższym przykładzie pokazano kilka scenariuszy:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Ten szablon jest zgodny Ścieżka adresu URL, takich jak `/Blog/All-About-Routing/Introduction` i wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Trasy domyślnej wartości `controller` i `action` są produkowane przez trasy, nawet jeśli w szablonie nie istnieją odpowiednie parametry trasy. Można określić wartości domyślnych w szablonie trasy. `article` Parametr trasy jest zdefiniowany jako *wychwytywania* za wyglądu gwiazdki `*` przed nazwą parametru trasy. Parametry trasy wychwytywania Przechwytywanie pozostałą część ścieżki adresu URL i można także dopasować pusty ciąg.

Ten przykład dodaje tokeny ograniczeń i dane trasy:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Ten szablon jest zgodny Ścieżka adresu URL, takich jak `/en-US/Products/5` i wyodrębnia wartości `{ controller = Products, action = Details, id = 5 }` i tokeny danych `{ locale = en-US }`.

![Tokeny Windows zmiennych lokalnych](routing/_static/tokens.png)

### <a name="url-generation"></a>Generowanie adresu URL

`Route` Klasy można również wykonać Generowanie adresu URL, łącząc zestaw wartości trasy przy użyciu szablonu trasy. Jest to logicznie procesu zgodnych ze ścieżką URL.

> [!TIP]
> Aby lepiej zrozumieć Generowanie adresu URL, Wyobraź sobie adresu URL, które chcesz wygenerować, a następnie zastanów się, jak szablon trasy będzie odpowiadać tego adresu URL. Wartości, których będzie generowany? Jest to równoważne nierównej działania Generowanie adresu URL `Route` klasy.

W tym przykładzie użyto podstawowe trasy ASP.NET Core MVC styl:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Za pomocą wartości trasy `{ controller = Products, action = List }`, ta trasa generuje adres URL `/Products/List`. Wartości trasy są zastępowane odpowiednich parametrów trasy w celu utworzenia ze ścieżką URL. Ponieważ `id` jest opcjonalny parametr trasy, to żaden problem, że nie ma wartości.

Za pomocą wartości trasy `{ controller = Home, action = Index }`, ta trasa generuje adres URL `/`. Wartości trasy, które zostały dostarczone zgodne z wartościami domyślnymi, tak aby segmenty odpowiadający wartości te można bezpiecznie pominąć. Oba adresy URL generowane dwustronnej konwersji z tą definicją trasy i wygenerować tej samej wartości trasy, które były używane do generowania adresu URL.

> [!TIP]
> Skorzystaj z aplikacji za pomocą platformy ASP.NET Core MVC <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> do generowania adresów URL zamiast wywoływać metodę do routingu bezpośrednio.

Aby uzyskać więcej informacji na temat generowania adresu URL, zobacz [odwołania w przypadku generowania adresu url](#url-generation-reference).

## <a name="use-routing-middleware"></a>Użyj oprogramowania pośredniczącego routingu

::: moniker range=">= aspnetcore-2.1"

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) w pliku projektu aplikacji.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Odwołanie [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) w pliku projektu aplikacji.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Odwołanie [Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/) w pliku projektu aplikacji.

::: moniker-end

Dodaj routingu do kontenera usługi w `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

Tras muszą być skonfigurowane w `Startup.Configure` metody. Przykładowa aplikacja korzysta z tych interfejsów API:

* `RouteBuilder`
* `Build`
* `MapGet` &ndash; Dopasowuje tylko żądania HTTP GET.
* `UseRouter`

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

W poniższej tabeli przedstawiono odpowiedzi z danym identyfikatorów URI.

| Identyfikator URI                  | Odpowiedź                                          |
| -------------------- | ------------------------------------------------- |
| /Package/Create/3    | Cześć! Wartości trasy: [operacji tworzenia], [id, 3] |
| / pakietu/śledzenie/3    | Cześć! Wartości trasy: [operacji, Śledź], [identyfikator -3] |
| / pakietu/śledzić/3 /   | Cześć! Wartości trasy: [operacji, Śledź], [identyfikator -3] |
| / Package/śledzenie /      | &lt;Przechodzić niezgodności&gt;                    |
| Pobierz /hello/Joe       | Witaj, Jan!                                          |
| OPUBLIKUJ /hello/Joe      | &lt;Przechodzić, odpowiada tylko HTTP GET&gt;       |
| Pobierz /hello/Joe/Smith | &lt;Przechodzić niezgodności&gt;                    |

Jeśli konfigurujesz jedną trasę wywołać <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> przekazując `IRouter` wystąpienia. Nie będzie konieczne użycie <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

Framework udostępnia zestaw metod rozszerzenia do tworzenia tras, takich jak:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Niektóre z tych metod, takich jak `MapGet`, wymagają `RequestDelegate` należy podać. `RequestDelegate` Służy jako *programu obsługi trasy* gdy trasa odpowiada. Inne metody w tej rodzinie umożliwiają konfigurowanie potoku oprogramowania pośredniczącego do użytku jako program obsługi trasy. Jeśli *mapy* metody nie zaakceptuje program obsługi, takie jak `MapRoute`, a następnie używa <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

`Map[Verb]` Metody używać ograniczeń w celu ograniczenia trasy do czasownik HTTP w nazwie metody. Na przykład zobacz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> i <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Odwołanie do szablonu trasy

Tokeny w nawiasy klamrowe (`{ ... }`) Zdefiniuj *parametry trasy* , są powiązane, jeśli trasa jest zgodny. Można zdefiniować więcej niż jeden parametr trasy w segmencie trasy, ale musi być oddzielony wartości literału. Na przykład `{controller=Home}{action=Index}` nie jest prawidłową trasę, ponieważ nie ma żadnych wartości literału między `{controller}` i `{action}`. Te parametry trasy musi mieć nazwę i może określić dodatkowe atrybuty.

Tekst dosłowny niż parametrów trasy (na przykład `{id}`) i separatora ścieżki `/` musi być zgodna z tekstem w adresie URL. Dopasowywanie tekstu odbywa się bez uwzględniania wielkości liter i jest oparta na zdekodowany reprezentacja ścieżki URL. Aby dopasować Ogranicznik parametru trasy literału `{` lub `}`, zmienić jego znaczenie, powtarzając znak (`{{` lub `}}`).

Wzorce adresów URL, które próbują przechwytywania nazwę pliku z rozszerzeniem nazwy pliku opcjonalne mają dodatkowe zagadnienia. Rozważmy na przykład szablon `files/{filename}.{ext?}`. Gdy oba `filename` i `ext` istnieje, obie wartości są wypełnione. Jeśli tylko `filename` istnieje w adresie URL, dopasowania tras, ponieważ końcowej kropki `.` jest opcjonalne. Następujące adresy URL dopasowania tej trasy:

* `/files/myFile.txt`
* `/files/myFile`

Możesz użyć `*` znak jako prefiks parametru trasy, aby powiązać pozostałą część identyfikatora URI. Jest to nazywane *wychwytywania* parametru. Na przykład `blog/{*slug}` dopasowuje dowolny identyfikator URI, który rozpoczyna się od `/blog` i dowolną wartość z dołączonym (która jest przypisana do `slug` trasy wartość). Parametry przechwytującą cały mogą być również zgodna pusty ciąg.

::: moniker range=">= aspnetcore-2.2"

Parametrze wychwytywania odpowiednie znaki specjalne, gdy trasa służy do generowania adresu URL, w tym separatora ścieżki (`/`) znaków. Na przykład tras `foo/{*path}` wartościami trasy `{ path = "my/path" }` generuje `foo/my%2Fpath`. Należy pamiętać o zmienionym znaczeniu ukośnikiem. Aby znakach separatora ścieżki Rundy, należy użyć `**` prefiks parametru trasy. Trasa `foo/{**path}` z `{ path = "my/path" }` generuje `foo/my/path`.

::: moniker-end

Może mieć parametrów trasy *wartości domyślne*magazynu, określając wartość domyślna po nazwie parametru, rozdzielone znakiem równości (`=`). Na przykład `{controller=Home}` definiuje `Home` jako wartość domyślna dla `controller`. Wartością domyślną jest używany, jeśli wartość nie jest obecny w adresie URL, dla parametru. Oprócz wartości domyślne parametrów trasy może być opcjonalnie, określony przez dodanie znaku zapytania (`?`) na końcu nazwy parametru, jak `id?`. Różnica między wartościami opcjonalne i domyślne parametry trasy jest parametr trasy z wartością domyślną będzie zawsze daje wartość opcjonalny parametr ma wartość tylko wtedy, gdy wartość zostanie podana w adresie URL żądania.

::: moniker range=">= aspnetcore-2.2"

Parametry trasy może mieć ograniczeń, które musi odpowiadać wartości trasy, powiązany z adresu URL. Dodawanie dwukropka (`:`) i nazwa ograniczenia po Określa nazwę parametru trasy *ograniczenie w tekście* parametru trasy. Jeśli ograniczenie wymaga argumentów, są ujęte w nawiasy `( )` po nazwie ograniczenia. Można określić wiele ograniczeń w tekście, dodając inny dwukropka (`:`) i nazwa ograniczenia. Nazwa ograniczenia i argumenty są przekazywane do <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> usługi w celu utworzenia wystąpienia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> do użycia podczas przetwarzania adresu URL. Jeśli Konstruktor ograniczenie wymaga usług, są one rozwiązane z iniekcji zależności aplikacji usług. Na przykład szablon trasy `blog/{article:minlength(10)}` Określa `minlength` ograniczenie z argumentem `10`. Aby uzyskać więcej informacji na temat ograniczenia trasy i lista ograniczeń dostarczanych przez szablon, zobacz [przekierować odwołanie do ograniczenia](#route-constraint-reference) sekcji.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Parametry trasy może mieć ograniczeń, które musi odpowiadać wartości trasy, powiązany z adresu URL. Dodawanie dwukropka (`:`) i nazwa ograniczenia po Określa nazwę parametru trasy *ograniczenie w tekście* parametru trasy. Jeśli ograniczenie wymaga argumentów, są ujęte w nawiasy `( )` po nazwie ograniczenia. Można określić wiele ograniczeń w tekście, dodając inny dwukropka (`:`) i nazwa ograniczenia. Nazwa ograniczenia i argumenty są przekazywane do <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> usługi w celu utworzenia wystąpienia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> do użycia podczas przetwarzania adresu URL. Na przykład szablon trasy `blog/{article:minlength(10)}` Określa `minlength` ograniczenie z argumentem `10`. Aby uzyskać więcej informacji na temat ograniczenia trasy i lista ograniczeń dostarczanych przez szablon, zobacz [przekierować odwołanie do ograniczenia](#route-constraint-reference) sekcji.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Parametry trasy może mieć również transformatory parametrów, które przekształcania wartości parametru podczas generowania łączy pasującego działania i stron na identyfikatory URI. Ograniczenia, np. parametr transformatory można dodać wbudowane do parametru trasy, dodając dwukropek (`:`) i nazwa transformatora po nazwie parametru trasy. Na przykład szablon trasy `blog/{article:slugify}` Określa `slugify` przekształcania.

::: moniker-end

Poniższa tabela przedstawia niektóre szablony trasy i ich zachowania.

| Szablon trasy                         | Przykładowy adres URL dopasowania  | Uwagi                                                                  |
| -------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| Cześć                                  | człon                | Jest zgodny tylko pojedynczą ścieżkę `/hello`                                  |
| {Page=Home}                            | /                     | Dopasowuje i ustawia `Page` do `Home`                                      |
| {Page=Home}                            | / Contact              | Dopasowuje i ustawia `Page` do `Contact`                                   |
| {controller} / {action} / {id}?            | / / Listy produktów        | Mapuje `Products` kontrolera i `List` akcji                       |
| {controller} / {action} / {id}?            | / Produkty/szczegóły/123 |  Mapuje `Products` kontrolera i `Details` akcji.  `id` Ustaw 123 |
| {controller=Home}/{action=Index}/{id?} | /                     |  Mapuje `Home` kontrolera i `Index` metody. `id` jest ignorowana.       |

Przy użyciu szablonu ogólnie jest najprostszym podejściem do obsługi routingu. Ograniczenia i ustawienia domyślne można również określić poza szablon trasy.

> [!TIP]
> Włącz [rejestrowania](xref:fundamentals/logging/index) aby zobaczyć, jak tworzone w implementacji routingu, takich jak `Route`, zgodne z żądaniami.

## <a name="reserved-routing-names"></a>Zastrzeżone nazwy routingu

Następujące słowa kluczowe są zarezerwowane nazwy i nie można użyć jako nazwy trasy i parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odwołanie do ograniczenia trasy

Ograniczenia trasy wykonania, gdy `Route` ma dopasowane składnia przychodzącego adresu URL i podzielić na tokeny Ścieżka adresu URL do wartości trasy. Ograniczenia trasy ogólnie badać wartości tras skojarzone za pośrednictwem szablon trasy i wprowadzić tak, / nie decyzję o tym, czy wartość jest dopuszczalne. Niektóre ograniczenia trasy umożliwia należy wziąć pod uwagę, czy żądania można kierować dane poza wartości trasy. Na przykład <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> można zaakceptować lub odrzucić żądanie oparte na jego zlecenie HTTP.

> [!WARNING]
> Należy unikać stosowania ograniczeń dla **sprawdzania danych wejściowych** ponieważ w ten sposób oznacza, że nieprawidłowe dane wejściowe skutkuje *404 — Nie można odnaleźć* odpowiedzi zamiast *400 — Nieprawidłowe żądanie* z odpowiedni komunikat o błędzie. Ograniczenia trasy są używane do **odróżnić** między podobne tras, nie można sprawdzić poprawność danych wejściowych dla określonej trasy.

Poniższa tabela przedstawia niektóre ograniczenia trasy, a ich oczekiwane zachowanie.

| ograniczenie | Przykład | Przykład dopasowań | Uwagi |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Dopasowuje dowolną liczbę całkowitą |
| `bool` | `{active:bool}` | `true`, `FALSE` | Dopasowuje `true` lub `false` (bez uwzględniania wielkości liter) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Pasuje do prawidłowego `DateTime` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Pasuje do prawidłowego `decimal` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Pasuje do prawidłowego `double` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Pasuje do prawidłowego `float` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Pasuje do prawidłowego `Guid` wartość |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Pasuje do prawidłowego `long` wartość |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Ciąg musi być co najmniej 4 znaki |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Ciąg musi zawierać nie więcej niż 8 znaków |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Ciąg musi być dokładnie 12 znaków. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Ciąg musi zawierać co najmniej 8 i nie więcej niż 16 znaków |
| `min(value)` | `{age:min(18)}` | `19` | Wartość całkowita musi być co najmniej 18 |
| `max(value)` | `{age:max(120)}` | `91` | Wartość całkowita musi być nie więcej niż 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Wartość całkowita musi być co najmniej 18 lat, ale nie więcej niż 120 |
| `alpha` | `{name:alpha}` | `Rick` | Ciąg musi zawierać co najmniej jeden znak alfabetyczny (`a`-`z`, bez uwzględniania wielkości liter) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Ciąg musi być zgodny z wyrażeniem regularnym (Zobacz porady dotyczące definiując wyrażenie regularne) |
| `required` | `{name:required}` | `Rick` |  Używane do wymuszania, że wartość parametru nie jest obecny podczas generowania adresu URL |

Wiele ograniczeń rozdzielana średnikami można zastosować do jednego parametru. Na przykład następujące ograniczenia ogranicza parametr na wartość całkowitą równą 1 lub większą:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury. Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania. Ograniczenia trasy dostarczone przez framework nie należy modyfikować wartości przechowywane w wartości trasy. Wszystkie wartości trasy, pochodzącą z adresu URL analizy są przechowywane jako ciągi. Na przykład `float` ograniczenie podejmuje próbę przekonwertowania na format zmiennoprzecinkowy wartości trasy, ale przekonwertowana wartość jest używana tylko po to, aby sprawdzić, może on zostać przekonwertowany na format zmiennoprzecinkowy.

## <a name="regular-expressions"></a>Wyrażenia regularne

Dodaje platformę ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` do konstruktora wyrażenia regularnego. Zobacz <xref:System.Text.RegularExpressions.RegexOptions> opis tych elementów członkowskich.

Wyrażenia regularne używać ograniczników i tokeny podobne do używanych przez usługę Routing i języka C#. Wyrażenie regularne tokeny muszą być wyjściowym. Można użyć wyrażenia regularnego `^\d{3}-\d{2}-\d{4}$` routingu, wyrażenie musi mieć `\` znaki wpisane w jako `\\` w pliku źródłowym języka C# jako znak ucieczki `\` ciąg znaku ucieczki (chyba że za pomocą [ciąg verbatim literały](/dotnet/csharp/language-reference/keywords/string). `{`, `}`, `[`, I `]` znaków muszą być poprzedzone podwojenie je jako znak ucieczki znaki ogranicznika parametr routingu. W poniższej tabeli przedstawiono wyrażeń regularnych i wersji o zmienionym znaczeniu.

| Wyrażenie            | Poprzedzone znakiem zmiany znaczenia                        |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Wyrażenia regularne użyte w routingu często rozpoczynać `^` znaków (dopasowanie ciągu pozycję początkową) i kończyć się `$` znak (dopasowanie kończy pozycja w ciągu). `^` i `$` znaków upewnij się, że wartość parametru trasy całe dopasowanie wyrażenia regularnego. Bez `^` i `$` znaków, wyrażenie regularne odpowiada wszelkich podciągu wewnątrz ciągu, który jest często niepożądane. W poniższej tabeli przedstawiono kilka przykładów i wyjaśnia, dlaczego zgodne lub nie odpowiada.

| Wyrażenie   | String    | Dopasowanie | Komentarz               |
| ------------ | --------- |  ---- |  -------------------- |
| `[a-z]{2}`   | Cześć     | Tak   | podciąg dopasowania     |
| `[a-z]{2}`   | 123abc456 | Tak   | podciąg dopasowania     |
| `[a-z]{2}`   | mz        | Tak   | zgodne z wyrażeniem    |
| `[a-z]{2}`   | MZ        | Tak   | bez uwzględniania wielkości liter    |
| `^[a-z]{2}$` |  Cześć    | Nie    | zobacz `^` i `$` powyżej |
| `^[a-z]{2}$` | 123abc456 | Nie    | zobacz `^` i `$` powyżej |

Aby uzyskać więcej informacji na temat składni wyrażeń regularnych, zobacz [wyrażeń regularnych programu .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Aby ograniczyć parametr znany zestaw możliwych wartości, należy użyć wyrażenia regularnego. Na przykład `{action:regex(^(list|get|create)$)}` jest zgodny tylko `action` trasy wartość `list`, `get`, lub `create`. Jeśli przekazywana do słownika ograniczenia, ciąg `^(list|get|create)$` odpowiada. Ograniczenia, które są przekazywane w słowniku ograniczenia (niewyrównane w szablonie), która nie pasuje do jednej znane ograniczenia również są traktowane jako wyrażenia regularne.

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Odwołania do parametru transformatora

Parametr transformatory:

* Wykonania podczas generowania łącza dla `Route`.
* Implementowanie `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Zostały skonfigurowane za pomocą <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Pobrać parametru, wartości trasy i przekształć go w nową wartość ciągu.
* Po przekształceniu wartość jest używana w wygenerowane łącze.

Na przykład niestandardowy `slugify` transformatora parametr we wzorcu trasy `blog\{article:slugify}` z `Url.Action(new { article = "MyTestArticle" })` generuje `blog\my-test-article`.

Transformatory parametru są również używane przez struktury do przekształcania identyfikatora URI, do którego jest rozpoznawany jako punkt końcowy. Na przykład ASP.NET Core MVC używa parametru transformatory do przekształcania wartości trasy, używany do dopasowywania `area`, `controller`, `action`, i `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home:slugify}/{action=Index:slugify}/{id?}");
```

Trasa poprzedniej akcji `SubscriptionManagementController.GetAll()` jest dopasowany do identyfikatora URI `/subscription-management/get-all`. Transformer parametru nie zmienia wartości trasy, używanego do generowania łącza. `Url.Action("GetAll", "SubscriptionManagement")` dane wyjściowe `/subscription-management/get-all`.

ASP.NET Core zawiera konwencje interfejsu API transformatory parametru przy użyciu wygenerowanego trasy:

* ASP.NET Core MVC ma `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` Konwencji interfejsu API. Ta Konwencja dotyczy transformatora określony parametr wszystkich tras atrybutów w aplikacji. Transformer parametr przekształca tokeny trasy atrybutu, ponieważ zostaną zastąpione. Aby uzyskać więcej informacji, zobacz [Użyj transformatora parametru, aby dostosować zastępowania tokenu](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Strony razor ma `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` Konwencji interfejsu API. Ta Konwencja dotyczy transformatora określony parametr wszystkie automatycznie odnalezione stron Razor. Transformer parametr przekształca folderu i pliku segmentów nazwy tras strony Razor. Aby uzyskać więcej informacji, zobacz [Użyj transformatora parametru, aby dostosować stronę trasy](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>Odwołanie do generowania adresu URL

Poniższy przykład pokazuje, jak wygenerować łącze do trasy, biorąc pod uwagę słownika wartości trasy i <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

`VirtualPath` Generowane na końcu poprzedniego przykładowe dane stanowią `/package/create/123`. Słownik dostarcza `operation` i `id` wartości szablonu "Śledzenie trasy pakietu" trasy `package/{operation}/{id}`. Aby uzyskać szczegółowe informacje, zobacz przykładowy kod [użycia routingu w oprogramowaniu pośredniczącym](#use-routing-middleware) sekcji lub [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Drugi parametr `VirtualPathContext` Konstruktor jest kolekcją *otoczenia wartości*. Wartości otoczenia umożliwiają wygodne przez ograniczenie liczby wartości, które Deweloper należy określić w ramach kontekstu żądania. Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy. W aplikacji ASP.NET Core MVC, jeśli znajdują się w `About` akcji `HomeController`, nie musisz określić wartość trasy kontrolera, aby połączyć `Index` akcji&mdash;otoczenia wartość `Home` jest używany.

Otoczenia wartości, które nie jest zgodny z parametrem są ignorowane, a otoczenia wartości również są ignorowane, gdy jawnie podana jest wartość zastępuje, go, przechodząc od lewej do prawej w adresie URL.

Wartości jawnie są dostarczane, ale które nie są zgodne niczego są dodawane do ciągu zapytania. W poniższej tabeli przedstawiono wyniki, korzystając z szablonu trasy `{controller}/{action}/{id?}`.

| Wartości otoczenia                | Jawne wartości                   | Wynik                  |
| ----------------------------- | --------------------------------- | ----------------------- |
| controller="Home"             | action="About"                    | `/Home/About`           |
| controller="Home"             | controller="Order",action="About" | `/Order/About`          |
| controller="Home",color="Red" | action="About"                    | `/Home/About`           |
| controller="Home"             | action="About",color="Red"        | `/Home/About?color=Red` |

Jeśli ta wartość nie zostanie podany wprost trasy ma wartość domyślną, która nie jest zgodny z parametrem, musi być zgodna wartość domyślna:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Generowanie konsolidacji tylko generuje link dla tej trasy, gdy znajdują się pasujących wartości dla kontrolerów i akcji.
