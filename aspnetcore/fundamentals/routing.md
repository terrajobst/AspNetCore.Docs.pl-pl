---
title: Routing w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak funkcje routingu platformy ASP.NET Core jest odpowiedzialny za mapowanie przychodzące żądanie do programu obsługi trasy.
ms.author: riande
ms.date: 07/25/2018
uid: fundamentals/routing
ms.openlocfilehash: 19265ac4d19915462c50628061600b1fde04694b
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254886"
---
# <a name="routing-in-aspnet-core"></a>Routing w programie ASP.NET Core

Przez [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), i [Rick Anderson](https://twitter.com/RickAndMSFT)

Funkcje routingu jest odpowiedzialny za mapowanie przychodzące żądanie do programu obsługi trasy. Trasy są zdefiniowane w aplikacji platformy ASP.NET i skonfigurowane podczas uruchamiania aplikacji. Trasy Opcjonalnie można wyodrębnić wartości z adresu URL zawartych w żądaniu, a te wartości mogą następnie służyć do przetwarzania żądania. Korzystając z informacji o trasie z poziomu aplikacji ASP.NET, funkcje routingu jest również możliwe do generowania adresów URL, które mapują do obsługi trasy. W związku z tym routingu, można znaleźć programu obsługi trasy na podstawie adresu URL lub adres URL odpowiadający programu obsługi trasy, na podstawie informacji programu obsługi trasy.

>[!IMPORTANT]
> W tym dokumencie opisano niskopoziomowy odpowiedzialny routingu ASP.NET Core. W przypadku routingu platformy ASP.NET Core MVC, zobacz [trasy do akcji kontrolera](../mvc/controllers/routing.md)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Podstawy routingu

Używa routingu *trasy* (implementacje [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) do:

* Mapowanie żądań przychodzących do *trasy programów obsługi*

* Generowanie adresy URL używane w odpowiedzi

Ogólnie rzecz biorąc aplikacja ma jedną kolekcję tras. Gdy żądanie dociera, kolekcji tras są przetwarzane w kolejności. Żądanie przychodzące szuka trasę, która pasuje do adresu URL żądania, wywołując `RouteAsync` metody w każdej z dostępnych tras w kolekcji tras. Z drugiej strony odpowiedzi można używać routingu do generowania adresów URL (na przykład przekierowanie lub linki), w oparciu o trasach i związku z tym uniknąć trwale kodować adresów URL, który pomaga w utrzymaniu.

Routing jest podłączony do [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku przez `RouterMiddleware` klasy. [Platforma ASP.NET Core MVC](xref:mvc/overview) dodaje routingu do potoku oprogramowania pośredniczącego w ramach jego konfiguracji. Aby dowiedzieć się, przy użyciu routingu jako składnik autonomicznych, zobacz [dzięki oprogramowaniu pośredniczącemu routingu](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Dopasowanie adresu URL

Dopasowanie adresu URL jest proces, który wysyła routingu przychodzącego żądania *obsługi*. Proces ten jest zazwyczaj na podstawie danych ze ścieżki adresu URL, ale może zostać rozszerzony do należy wziąć pod uwagę wszystkie dane w żądaniu. Możliwość wysyłania żądań do rozdzielenia obsługi to klucz do skalowania, rozmiar i złożoność aplikacji.

Wprowadź żądań przychodzących `RouterMiddleware`, która wywołuje metodę `RouteAsync` metody dla każdej trasy w sekwencji. `IRouter` Wybierze wystąpienie czy *obsługi* żądania przez ustawienie `RouteContext.Handler` do innych niż null `RequestDelegate`. Jeśli trasa Ustawia program obsługi żądania, przetwarzanie zostanie zatrzymane i program obsługi trasy zostaną wywołane przetwarzania żądania. Jeśli wszystkie trasy są sprawdzane i żadna procedura obsługi nie znajduje się na żądanie, oprogramowanie pośredniczące wywołuje *dalej* i następne oprogramowanie pośredniczące w potoku żądania jest wywoływana.

Podstawowe dane wejściowe `RouteAsync` jest `RouteContext.HttpContext` skojarzone z bieżącego żądania. `RouteContext.Handler` i `RouteContext.RouteData` są dane wyjściowe, które zostaną ustawione po trasa pasuje.

Dopasowanie ciągu `RouteAsync` spowoduje także ustawienie właściwości `RouteContext.RouteData` odpowiednie wartości oparte na przetwarzanie żądań, gotowe do tej pory. Jeśli trasa pasuje do żądania `RouteContext.RouteData` będzie zawierać informacje o stanie ważne informacje *wynik*.

`RouteData.Values` jest słownik *wartości trasy* wyprodukowanych z trasy. Wartości te są zwykle określane przez tokenizowanie adres URL i może służyć do przyjmowania danych wejściowych użytkownika lub dodatkowo dispatching decyzje wewnątrz aplikacji.

`RouteData.DataTokens`  to dodatkowe dane związane z dopasowanej trasy zbiór właściwości. `DataTokens` podano obsługują kojarzenie stanu, w jakim dopasowane dane za pomocą każdej trasy, aby aplikacja podejmować decyzje później oparte na które trasy. Te wartości są definiowane przez projektanta i wykonaj **nie** mają wpływ na zachowanie routingu w dowolny sposób. Ponadto wartości przechowalni w tokeny danych może być dowolnego typu, w przeciwieństwie do wartości trasy, które muszą być łatwo konwertowany do i z ciągów.

`RouteData.Routers` znajduje się lista tras, na których uczestniczyła w pomyślnie dopasowywania żądania. Trasy mogą być zagnieżdżone wewnątrz siebie nawzajem i `Routers` właściwość odzwierciedla drogę przez drzewo logiczne tras, które spowodowały dopasowanie. Ogólnie pierwszy element `Routers` jest kolekcją tras i powinna być używana do generowania adresu URL. Ostatnim elementem w `Routers` jest programu obsługi trasy, który jest zgodny.

### <a name="url-generation"></a>Generowanie adresu URL

Generowanie adresu URL to proces, routingu, które można utworzyć ścieżki adresu URL na podstawie zestawu wartości trasy. Dzięki temu do logicznego rozdzielania między inne programy obsługi oraz w adresach URL, które do nich dostęp.

Generowanie adresu URL, który następuje podobny proces iteracyjny, ale rozpoczyna się od kodu użytkownika lub framework wywoływanie `GetVirtualPath` metoda kolekcji tras. Każdy *trasy* będzie miał jego `GetVirtualPath` metoda wywoływana w sekwencji, aż do innych niż null `VirtualPathData` jest zwracana.

Podstawowy danych wejściowych do `GetVirtualPath` są:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Trasy przede wszystkim używasz wartości trasy, dostarczone przez `Values` i `AmbientValues` zdecydować, gdzie jest możliwe do generowania adresu URL i wartości, których do uwzględnienia. `AmbientValues` Zestaw wartości trasy, które zostały utworzone z dopasowywania bieżące żądanie w systemie routingu. Z kolei `Values` są wartości trasy, które określają sposób generowania żądany adres URL dla bieżącej operacji. `HttpContext` Znajduje się w przypadku, gdy trasa serwer musi pobrać services lub dodatkowe dane skojarzone z bieżącym kontekstem.

Porada: Traktować `Values` jako zbiór zastąpienia `AmbientValues`. Generowanie adresu URL spróbuje ponownie użyć wartości trasy z bieżącego żądania, aby ułatwić do generowania adresów URL dla łącza przy użyciu tego samego trasę lub trasy wartości.

Dane wyjściowe `GetVirtualPath` jest `VirtualPathData`. `VirtualPathData` jest równolegle z `RouteData`; zawiera on `VirtualPath` dla adresu URL danych wyjściowych i niektóre dodatkowe właściwości, które powinny zostać ustawione przez trasę.

`VirtualPathData.VirtualPath` Właściwość zawiera *ścieżka wirtualna* produkowane przez trasę. W zależności od potrzeb może być konieczne ścieżkę do dalszego przetwarzania. Na przykład jeśli chcesz renderować wygenerowany adres URL w formacie HTML należy poprzedzić ścieżki podstawowej aplikacji.

`VirtualPathData.Router` Jest odwołaniem do pomyślnie wygenerowano adres URL trasy.

`VirtualPathData.DataTokens` Właściwości jest słownikiem dodatkowych danych dotyczących trasy, który wygenerował adresu URL. Jest to równoległego z `RouteData.DataTokens`.

### <a name="creating-routes"></a>Tworzenie tras

Routing zapewnia `Route` klasy jako standardowej implementacji `IRouter`. `Route` używa *szablon trasy* Składnia umożliwiająca zdefiniowanie wzorców, które zostanie dopasowany do ścieżki adresu URL po `RouteAsync` jest wywoływana. `Route` będzie używał tego samego szablonu trasy do generowania adresu URL po `GetVirtualPath` jest wywoływana.

Większość aplikacji będzie utworzyć trasy, wywołując `MapRoute` lub jednego z podobne metody rozszerzenia zdefiniowane na `IRouteBuilder`. Wszystkie te metody spowoduje utworzenie wystąpienia `Route` i dodać go do kolekcji tras.

Uwaga: `MapRoute` nie przyjmuje parametr programu obsługi trasy — tylko dodaje trasy, które będą obsługiwane przez `DefaultHandler`. Ponieważ domyślny program obsługi jest `IRouter`, mogą zdecydować, nie można obsłużyć żądania. Na przykład platformy ASP.NET MVC jest zazwyczaj skonfigurowany jako domyślny program obsługi, który obsługuje tylko żądania które pasują do dostępnych kontrolerów i akcji. Aby dowiedzieć się więcej na temat routingu do MVC, zobacz [trasy do akcji kontrolera](../mvc/controllers/routing.md).

Jest to przykład `MapRoute` wywołania używany przez typowy definicję trasy ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ten szablon będzie odpowiadał Ścieżka adresu URL, takich jak `/Products/Details/17` i Wyodrębnij wartości trasy `{ controller = Products, action = Details, id = 17 }`. Wartości trasy są określane przez podział Ścieżka adresu URL na segmenty, dopasowanie i każdy z segmentów z *kierowanie parametru* nazwy szablonu trasy. Noszą nazwy parametrów trasy. Są one definiowane, umieszczając nazwę parametru w nawiasach klamrowych `{ }`.

Powyższym szablonie może być również zgodna ze ścieżką URL `/` i spowodowało wygenerowanie wartości `{ controller = Home, action = Index }`. Wynika to z faktu `{controller}` i `{action}` parametrów trasy mają przypisane wartości domyślne, a `id` parametr trasy jest opcjonalny. Znak równości `=` logowanie następuje wartość po nazwę parametru trasa określa wartość domyślną dla parametru. Znak zapytania `?` po nazwy parametru trasy definiuje parametru jako opcjonalną. Parametry o wartości domyślne trasy *zawsze* uzyskiwania wartości trasy, jeśli trasa odpowiada — następujące parametry opcjonalne nie będzie mieć wartość trasy, jeśli nie było żadnych odpowiadającym segmencie ścieżki adresu URL.

Zobacz [dokumentacja dotycząca szablonów tras](#route-template-reference) uzyskać dokładny opis funkcji szablonu trasy i składni.

Ten przykład obejmuje *trasy ograniczenie*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ten szablon będzie odpowiadał Ścieżka adresu URL, takich jak `/Products/Details/17`, ale nie `/Products/Details/Apples`. Definicji parametru trasy `{id:int}` definiuje *trasy ograniczenie* dla `id` parametru trasy. Implementowanie ograniczenia trasy `IRouteConstraint` i badać wartości trasy, aby je zweryfikować. W tym przykładzie wartość trasy `id` musi być konwertowany na liczbę całkowitą. Zobacz [odwołanie w przypadku ograniczenia trasy](#route-constraint-reference) uzyskać bardziej szczegółowy opis ograniczenia trasy, które są dostarczane przez platformę.

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

Porada: Składnię w tekście do definiowania ograniczeń i ustawień domyślnych może być bardziej wygodne do proste trasy. Istnieją jednak funkcji, takich jak tokeny danych, które nie są obsługiwane przez składnię w tekście.

W tym przykładzie przedstawiono kilka dodatkowych funkcji:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Ten szablon będzie odpowiadał Ścieżka adresu URL, takich jak `/Blog/All-About-Routing/Introduction` , który wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Trasy domyślnej wartości `controller` i `action` są produkowane przez trasy, nawet jeśli w szablonie nie istnieją odpowiednie parametry trasy. Można określić wartości domyślnych w szablonie trasy. `article` Parametr trasy jest zdefiniowany jako *wychwytywania* za wyglądu gwiazdki `*` przed nazwą parametru trasy. Parametry trasy wychwytywania przechwytywania pozostałą część ścieżki adresu URL i można także dopasować pusty ciąg.

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

<a name="id1"></a>

### <a name="url-generation"></a>Generowanie adresu URL

`Route` Klasy można również wykonać Generowanie adresu URL, łącząc zestaw wartości trasy przy użyciu szablonu trasy. Jest to logicznie procesu zgodnych ze ścieżką URL.

Porada: Aby lepiej zrozumieć Generowanie adresu URL, Wyobraź sobie adresu URL, które chcesz wygenerować, a następnie zastanów się, jak szablon trasy będzie odpowiadać tego adresu URL. Wartości, których będzie generowany? Jest to równoważne nierównej działania Generowanie adresu URL `Route` klasy.

W tym przykładzie użyto podstawowe trasy stylu platformy ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Za pomocą wartości trasy `{ controller = Products, action = List }`, ta trasa zostanie wygenerowany adres URL `/Products/List`. Wartości trasy są zastępowane odpowiednich parametrów trasy w celu utworzenia ze ścieżką URL. Ponieważ `id` jest opcjonalny parametr trasy, to żaden problem, że nie ma wartości.

Za pomocą wartości trasy `{ controller = Home, action = Index }`, ta trasa zostanie wygenerowany adres URL `/`. Wartości trasy, które zostały dostarczone zgodne z wartościami domyślnymi, dlatego segmentów odpowiadający wartości te można bezpiecznie pominąć. Pamiętaj, że oba adresy URL generowane będzie obustronne z tą definicją trasy, a następnie generuje te same wartości trasy, które były używane do generowania adresu URL.

Porada: Należy używać w aplikacji za pomocą platformy ASP.NET MVC `UrlHelper` do generowania adresów URL zamiast wywoływać metodę do routingu bezpośrednio.

Aby uzyskać więcej szczegółów na temat procesu tworzenia adresu URL, zobacz [odwołania w przypadku generowania adresu url](#url-generation-reference).

## <a name="using-routing-middleware"></a>Dzięki oprogramowaniu Pośredniczącemu routingu

Dodaj pakiet NuGet "Microsoft.AspNetCore.Routing".

Dodaj routingu do kontenera usługi w *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Tras muszą być skonfigurowane w `Configure` method in Class metoda `Startup` klasy. Poniższy przykład używa tych interfejsów API:

* `RouteBuilder`
* `Build`
* `MapGet`  Dopasowuje tylko żądania HTTP GET
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

W poniższej tabeli przedstawiono odpowiedzi z danym identyfikatorów URI.

| Identyfikator URI | Odpowiedź  |
| ------- | -------- |
| /Package/Create/3  | Cześć! Wartości trasy: [operacji tworzenia], [id, 3] |
| / pakietu/śledzenie/3  | Cześć! Wartości trasy: [operacji, Śledź], [identyfikator -3] |
| / pakietu/śledzić/3 / | Cześć! Wartości trasy: [operacji, Śledź], [identyfikator -3]  |
| / Package/śledzenie / | \<Przechodzić niezgodności > |
| Pobierz /hello/Joe | Witaj, Jan! |
| OPUBLIKUJ /hello/Joe | \<Przechodzić, odpowiada tylko HTTP GET > |
| Pobierz /hello/Joe/Smith | \<Przechodzić niezgodności > |

Jeśli konfigurujesz jedną trasę wywołać `app.UseRouter` przekazując `IRouter` wystąpienia. Nie trzeba wywoływać `RouteBuilder`.

Framework udostępnia zestaw metod rozszerzenia tworzenie tras, na przykład:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Niektóre z tych metod, takich jak `MapGet` wymagają `RequestDelegate` należy podać. `RequestDelegate` Będzie służyć jako *programu obsługi trasy* gdy trasa odpowiada. Inne metody w tej rodzinie umożliwiają konfigurowanie potoku oprogramowania pośredniczącego, które będzie używane jako programu obsługi trasy. Jeśli *mapy* metody nie zaakceptuje program obsługi, takie jak `MapRoute`, a następnie użyje `DefaultHandler`.

`Map[Verb]` Metody używać ograniczeń w celu ograniczenia trasy do czasownik HTTP w nazwie metody. Na przykład zobacz [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) i [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Odwołanie do szablonu trasy

Tokeny w nawiasy klamrowe (`{ }`) Zdefiniuj *parametry trasy* która powiązana, jeśli trasa jest zgodny. Można zdefiniować więcej niż jeden parametr trasy w segmencie trasy, ale musi być oddzielony wartości literału. Na przykład `{controller=Home}{action=Index}` nie bylibyśmy w prawidłową trasę, ponieważ nie ma żadnych wartości literału między `{controller}` i `{action}`. Te parametry trasy musi mieć nazwę, a może określić dodatkowe atrybuty.

Tekst dosłowny niż parametrów trasy (na przykład `{id}`) i separatora ścieżki `/` musi być zgodna z tekstem w adresie URL. Dopasowywanie tekstu odbywa się bez uwzględniania wielkości liter i jest oparta na zdekodowany reprezentacja ścieżki URL. Aby dopasować Ogranicznik parametru trasy literału `{` lub `}`, zmienić jego znaczenie, powtarzając znak (`{{` lub `}}`).

Wzorce adresów URL, które próbują przechwytywania nazwę pliku z rozszerzeniem nazwy pliku opcjonalne mają dodatkowe zagadnienia. Na przykład przy użyciu szablonu `files/{filename}.{ext?}` — po obu `filename` i `ext` istnieje, obie wartości zostaną wypełnione. Jeśli tylko `filename` istnieje w adresie URL, dopasowania tras, ponieważ końcowej kropki `.` jest opcjonalne. Następujące adresy URL będzie odpowiadać tej trasy:

* `/files/myFile.txt`
* `/files/myFile`

Możesz użyć `*` znak jako prefiks do parametr trasy do powiązania w pozostałej części identyfikatora URI — jest to nazywane *wychwytywania* parametru. Na przykład `blog/{*slug}` będzie odpowiadać dowolny identyfikator URI, który uruchamiany przy użyciu `/blog` i dowolną wartość z dołączonym (może zostać przypisana do `slug` trasy wartości). Parametry przechwytującą cały mogą być również zgodna pusty ciąg.

Może mieć parametrów trasy *wartości domyślne*magazynu, określając wartość domyślna po nazwie parametru, oddzielone `=`. Na przykład `{controller=Home}` zdefiniuje `Home` jako wartość domyślna dla `controller`. Wartością domyślną jest używany, jeśli wartość nie jest obecny w adresie URL, dla parametru. Oprócz wartości domyślne parametrów trasy, może być opcjonalny (określona przez dołączenie `?` na końcu nazwy parametru, jak `id?`). Różnica między opcjonalne i "ma default" parametru trasy, z wartością domyślną będzie zawsze daje wartość opcjonalny parametr ma wartość tylko wtedy, gdy zostało ono określone.

Parametry trasy może mieć również ograniczenia, które musi odpowiadać wartości trasy, powiązany z adresu URL. Dodawanie dwukropek `:` i nazwa ograniczenia po Określa nazwę parametru trasy *ograniczenie w tekście* parametru trasy. Jeśli ograniczenie wymaga argumentów, te znajdują się w nawiasach `( )` po nazwie ograniczenia. Można określić wiele ograniczeń w tekście, dodając inny dwukropek `:` i nazwa ograniczenia. Nazwa ograniczenia jest przekazywany do `IInlineConstraintResolver` usługi w celu utworzenia wystąpienia `IRouteConstraint` do użycia podczas przetwarzania adresu URL. Na przykład szablon trasy `blog/{article:minlength(10)}` Określa `minlength` ograniczenie z argumentem `10`. Aby uzyskać więcej ograniczenia trasy opis, a lista ograniczeń dostarczanych przez szablon, zobacz [odwołanie w przypadku ograniczenia trasy](#route-constraint-reference).

Poniższa tabela przedstawia niektóre szablony trasy i ich zachowania.

| Szablon trasy | Przykładowy adres URL dopasowania | Uwagi |
| -------- | -------- | ------- |
| Cześć  | człon  | Jest zgodny tylko pojedynczą ścieżkę `/hello` |
| {Page=Home} | / | Dopasowuje i ustawia `Page` do `Home` |
| {Page=Home}  | / Contact  | Dopasowuje i ustawia `Page` do `Contact` |
| {controller} / {action} / {id}? | / / Listy produktów | Mapuje `Products` kontrolera i `List` akcji |
| {controller} / {action} / {id}? | / Produkty/szczegóły/123  |  Mapuje `Products` kontrolera i `Details` akcji.  `id` Ustaw 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  Mapuje `Home` kontrolera i `Index` metody. `id` jest ignorowana. |

Przy użyciu szablonu ogólnie jest najprostszym podejściem do obsługi routingu. Ograniczenia i ustawienia domyślne można również określić poza szablon trasy.

Porada: Włączanie [rejestrowania](xref:fundamentals/logging/index) aby zobaczyć, jak tworzone w implementacji routingu, takich jak `Route`, zgodne z żądaniami.

## <a name="reserved-routing-names"></a>Zastrzeżone nazwy routingu

Następujące słowa kluczowe są zarezerwowane nazwy i nie można użyć jako nazwy trasy i parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odwołanie do ograniczenia trasy

Ograniczenia trasy wykonania, gdy `Route` ma dopasowane składnia przychodzącego adresu URL i podzielić na tokeny Ścieżka adresu URL do wartości trasy. Ograniczenia trasy ogólnie badać wartości tras skojarzone za pośrednictwem szablon trasy i upewnij prostą tak/nie decyzji o tym, czy wartość jest dopuszczalne. Niektóre ograniczenia trasy umożliwia należy wziąć pod uwagę, czy żądania można kierować dane poza wartości trasy. Na przykład `HttpMethodRouteConstraint` można zaakceptować lub odrzucić żądanie oparte na jego zlecenie HTTP.

>[!WARNING]
> Unikaj używania ograniczenia **sprawdzania danych wejściowych**, ponieważ w ten sposób oznacza, że nieprawidłowe dane wejściowe spowoduje 404 (nie znaleziono), zamiast 400 przy użyciu odpowiedniego komunikatu o błędzie. Ograniczenia trasy powinien być używany do **odróżnić** między podobne tras, nie można sprawdzić poprawność danych wejściowych dla określonej trasy.

Poniższa tabela przedstawia niektóre ograniczenia trasy, a ich oczekiwane zachowanie.

| ograniczenie | Przykład | Przykład dopasowań | Uwagi |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Dopasowuje dowolną liczbę całkowitą |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Dopasowuje `true` lub `false` (bez uwzględniania wielkości liter) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Pasuje do prawidłowego `DateTime` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Pasuje do prawidłowego `decimal` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Pasuje do prawidłowego `double` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Pasuje do prawidłowego `float` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Pasuje do prawidłowego `Guid` wartość |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Pasuje do prawidłowego `long` wartość |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Ciąg musi być co najmniej 4 znaki |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Ciąg musi zawierać nie więcej niż 8 znaków |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Ciąg musi być dokładnie 12 znaków. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Ciąg musi zawierać co najmniej 8 i nie więcej niż 16 znaków |
| `min(value)` | `{age:min(18)}` | `19` | Wartość całkowita musi być co najmniej 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Wartość całkowita musi być nie więcej niż 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Wartość całkowita musi być co najmniej 18 lat, ale nie więcej niż 120 |
| `alpha` | `{name:alpha}` | `Rick` | Ciąg musi zawierać co najmniej jeden znak alfabetyczny (`a`-`z`, bez uwzględniania wielkości liter) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Ciąg musi być zgodny z wyrażeniem regularnym (Zobacz porady dotyczące definiując wyrażenie regularne) |
| `required`  | `{name:required}` | `Rick` |  Używane do wymuszania, że wartość parametru nie jest obecny podczas generowania adresu URL |

Wiele ograniczeń rozdzielana średnikami można zastosować do jednego parametru. Na przykład następujące ograniczenia ogranicza parametr na wartość całkowitą równą 1 lub większą:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

>[!WARNING]
> Ograniczenia trasy, które pozwalają sprawdzić adres URL można przekonwertować na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury — zakładają, że adres URL jest niemożliwe do zlokalizowania. Ograniczenia trasy dostarczone przez framework nie należy modyfikować wartości przechowywane w wartości trasy. Wszystkie wartości trasy, przeanalizować z adresu URL będą przechowywane jako ciągi. Na przykład [ograniczenia trasy Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) zostanie podjęta próba konwersji wartości trasy na format zmiennoprzecinkowy, ale przekonwertowana wartość jest używana tylko po to, aby sprawdzić, może on zostać przekonwertowany na format zmiennoprzecinkowy.

## <a name="regular-expressions"></a>Wyrażenia regularne

Dodaje platformę ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` do konstruktora wyrażenia regularnego. Zobacz [wyliczenie RegexOptions](/dotnet/api/system.text.regularexpressions.regexoptions) opis tych elementów członkowskich.

Wyrażenia regularne używać ograniczników i tokeny podobne do używanych przez usługę Routing i języka C#. Wyrażenie regularne tokeny muszą być wyjściowym. Na przykład, aby użyć wyrażenia regularnego `^\d{3}-\d{2}-\d{4}$` w routingu, musi ona mieć `\` znaki wpisane w jako `\\` w pliku źródłowym języka C# jako znak ucieczki `\` ciąg znaku ucieczki (chyba że za pomocą [verbatim Literały ciągu](/dotnet/csharp/language-reference/keywords/string). `{` , `}` , "[" I "]" znaków muszą być poprzedzone podwojenie je jako znak ucieczki znaki ogranicznika parametr routingu.  W poniższej tabeli przedstawiono wyrażeń regularnych i wersji o zmienionym znaczeniu.

| Wyrażenie               | Uwaga |
| ----------------- | ------------ |
| `^\d{3}-\d{2}-\d{4}$` | Wyrażenie regularne |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Poprzedzone znakiem zmiany znaczenia  |
| `^[a-z]{2}$` | Wyrażenie regularne |
| `^[[a-z]]{{2}}$` | Poprzedzone znakiem zmiany znaczenia  |

Wyrażenia regularne użyte w routingu często rozpocznie się za pomocą `^` znaków (dopasowanie ciągu pozycję początkową) i kończyć się `$` znak (dopasowanie kończy pozycja w ciągu). `^` i `$` znaków upewnij się, że wartość parametru trasy całe dopasowanie wyrażenia regularnego. Bez `^` i `$` znaków wyrażeń regularnych będzie odpowiadał na dowolnym podciągu wewnątrz ciągu, który jest często nie mają. W poniższej tabeli przedstawiono kilka przykładów i wyjaśnia, dlaczego zgodne lub nie odpowiada.

| Wyrażenie               | String | Dopasowanie | Komentarz |
| ----------------- | ------------ |  ------------ |  ------------ |
| `[a-z]{2}` | Cześć | Tak | podciąg dopasowania |
| `[a-z]{2}` | 123abc456 | Tak | podciąg dopasowania |
| `[a-z]{2}` | mz | Tak | zgodne z wyrażeniem |
| `[a-z]{2}` | MZ | Tak | bez uwzględniania wielkości liter |
| `^[a-z]{2}$` |  Cześć | Brak | zobacz `^` i `$` powyżej |
| `^[a-z]{2}$` |  123abc456 | Brak | zobacz `^` i `$` powyżej |

Zapoznaj się [wyrażeń regularnych programu .NET Framework](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) więcej informacji na temat składni wyrażeń regularnych.

Aby ograniczyć parametr znany zestaw możliwych wartości, należy użyć wyrażenia regularnego. Na przykład `{action:regex(^(list|get|create)$)}` jest zgodny tylko `action` trasy wartość `list`, `get`, lub `create`. Jeśli przekazywana do słownika ograniczenia, ciąg "^ (Lista | get | tworzenie) $" byłaby równoważna. Ograniczenia, które są przekazywane w słowniku ograniczenia (niewyrównane w szablonie), która nie pasuje do jednej znane ograniczenia również są traktowane jako wyrażenia regularne.

## <a name="url-generation-reference"></a>Odwołanie do generowania adresu URL

W poniższym przykładzie pokazano, jak wygenerować łącze do trasy, biorąc pod uwagę słownika wartości trasy i `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath` Jest generowany na końcu powyższego przykładu `/package/create/123`.

Drugi parametr `VirtualPathContext` Konstruktor jest kolekcją *otoczenia wartości*. Wartości otoczenia umożliwiają wygodne przez ograniczenie liczby wartości, które Deweloper należy określić w ramach kontekstu żądania. Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy. Na przykład w aplikacji ASP.NET MVC w `About` akcji `HomeController`, nie musisz określić wartość trasy kontrolera, aby połączyć `Index` akcji (otoczenia wartość `Home` będą używane).

Otoczenia wartości, które nie jest zgodny z parametrem są ignorowane, a otoczenia wartości również są ignorowane, gdy jawnie dostarczone przez wartość zastąpienia, go, przechodząc od lewej do prawej w adresie URL.

Wartości jawnie są dostarczane, ale które nie są zgodne niczego są dodawane do ciągu zapytania. W poniższej tabeli przedstawiono wyniki, korzystając z szablonu trasy `{controller}/{action}/{id?}`.

| Wartości otoczenia | Jawne wartości | Wynik |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

Jeśli ta wartość nie zostanie podany wprost trasy ma wartość domyślną, która nie jest zgodny z parametrem, musi być zgodna wartość domyślna. Na przykład:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Generowanie konsolidacji wygeneruje tylko link dla tej trasy udostępniane pasujących wartości dla kontrolerów i akcji.
