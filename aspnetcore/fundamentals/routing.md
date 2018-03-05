---
title: Routing w platformy ASP.NET Core
author: ardalis
description: "Odkryj, jak funkcji routingu platformy ASP.NET Core jest odpowiedzialny za mapowania przychodzącego żądania do obsługi trasy."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/routing
ms.openlocfilehash: 1ff08ee6389ce7b12d74b162b990ddaaadc05ea8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="routing-in-aspnet-core"></a>Routing w platformy ASP.NET Core

Przez [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), i [Rick Anderson](https://twitter.com/RickAndMSFT)

Funkcje routingu jest odpowiedzialny za mapowania przychodzącego żądania do obsługi trasy. Trasy są zdefiniowane w aplikacji ASP.NET i skonfigurowane podczas uruchamiania aplikacji. Trasy opcjonalnie może wyodrębnić wartości z adresu URL zawarty w żądaniu, a następnie można używać tych wartości do przetworzenia żądania. Korzystając z informacji trasy z aplikacji ASP.NET, funkcji routingu może także do generowania adresów URL mapowane na programy obsługi trasy. W związku z tym routingu można znaleźć obsługi trasy, na podstawie adresu URL lub adres URL odpowiadający obsługi danego trasy, na podstawie informacji programu obsługi trasy.

>[!IMPORTANT]
> W tym dokumencie opisano niewielkie platformy ASP.NET Core routingu. Dla routingu platformy ASP.NET Core MVC, zobacz [routingu do akcji kontrolera](../mvc/controllers/routing.md)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Podstawowe informacje dotyczące routingu

Używa routingu *tras* (implementacje [IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) do:

* Mapuj przychodzące żądania do *trasy programów obsługi*

* Generowanie adresy URL używane w odpowiedzi

Ogólnie rzecz biorąc aplikacja ma jednej kolekcji tras. Gdy żądanie dociera, kolekcji tras są przetwarzane w kolejności. Przychodzące żądanie wyszukują trasę, która odpowiada adresowi URL żądania przez wywołanie metody `RouteAsync` metody dla każdej trasy dostępne w kolekcji tras. Z kolei odpowiedzi można korzystać z routingu do generowania adresów URL (na przykład dla przekierowania lub łącza) na podstawie informacji trasy i w związku z tym uniknąć kodowane adresów URL, co ułatwia utrzymanie.

Routing jest podłączony do [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) potoku przez `RouterMiddleware` klasy. [Platformy ASP.NET Core MVC](xref:mvc/overview) dodaje routingu do potoku oprogramowania pośredniczącego w ramach jego konfiguracji. Aby dowiedzieć się więcej o użyciu routingu jako składnik autonomicznych, zobacz [za pomocą oprogramowania pośredniczącego routingu](#using-routing-middleware).

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>Adres URL dopasowania

Dopasowywania adresu URL to proces, przez które routingu wysyła żądanie przychodzące *obsługi*. Ten proces zwykle są oparte na danych w ścieżkę adresu URL, ale może zostać rozszerzony do należy wziąć pod uwagę wszystkie dane w żądaniu. Możliwość wysyłania żądań do rozdzielenia obsługi jest kluczem do skalowania, rozmiar i złożoność aplikacji.

Wprowadź żądań przychodzących `RouterMiddleware`, które wywołuje `RouteAsync` metody dla każdej trasy w sekwencji. `IRouter` Wybierze wystąpienie czy *obsługi* żądania przez ustawienie `RouteContext.Handler` z systemem innym niż null `RequestDelegate`. Jeśli trasa Ustawia program obsługi żądania, zostanie wywołany przetwarzanie zostanie zatrzymane i program obsługi trasy do przetworzenia żądania. Wszystkie trasy są sprawdzane nie odnaleźć programu obsługi jest żądanie, oprogramowanie pośredniczące wywołuje *dalej* i następne oprogramowanie pośredniczące w potoku żądania jest wywoływana.

Podstawowe dane wejściowe `RouteAsync` jest `RouteContext.HttpContext` skojarzone z bieżącego żądania. `RouteContext.Handler` i `RouteContext.RouteData` są dane wyjściowe, które można ustawić po trasa pasuje.

Dopasowanie podczas `RouteAsync` będzie także ustawić właściwości `RouteContext.RouteData` odpowiednie wartości oparte na przetwarzanie żądań gotowe do tej pory. Jeśli trasa pasuje do żądania `RouteContext.RouteData` będzie zawierać informacje o stanie ważne informacje *wynik*.

`RouteData.Values` jest słownikiem *wartości trasy* utworzone na podstawie trasy. Wartości te zwykle są określane przez tokenizing adres URL i może służyć do przyjmowania danych wejściowych użytkownika lub do podejmowania dalszych wysyłania decyzji wewnątrz aplikacji.

`RouteData.DataTokens`  jest zbiorem właściwości dodatkowych danych dotyczących dopasowanej trasy. `DataTokens` są przekazywane do obsługi kojarzenia stanu, który danych z każdej trasy tak aplikacji podjęcie decyzji dotyczących później oparte na która trasa pasuje. Te wartości są definiowane przez deweloperów i wykonaj **nie** wpływają na zachowanie routingu w dowolny sposób. Ponadto wartości zwiniętych w tokeny danych mogą być dowolnego typu, w przeciwieństwie do wartości trasy, które muszą być łatwe do przekonwertowania do i z ciągów.

`RouteData.Routers` znajduje się lista tras, na których uczestniczyła w pomyślnie dopasowywania żądania. Trasy mogą być zagnieżdżone wewnątrz, a `Routers` właściwość odzwierciedla ścieżkę za pośrednictwem drzewa logicznego tras, które spowodowało dopasowanie. Zazwyczaj pierwszy element `Routers` jest kolekcji tras i powinna być używana do generowania adresu URL. Ostatni element `Routers` jest programu obsługi trasy, który jest zgodny.

### <a name="url-generation"></a>Generowania adresu URL

Generowania adresu URL to proces, za które routingu można utworzyć ścieżki adresu URL na podstawie zestawu wartości trasy. Umożliwia to logiczne rozdzielenie programu obsługi oraz w adresach URL, które uzyskiwać do nich dostęp.

Generowania adresu URL następuje podobne procesem iteracyjnym, ale rozpoczyna się od kodu użytkownika lub framework wywoływanie `GetVirtualPath` metody kolekcji tras. Każdy *trasy* będzie miał jego `GetVirtualPath` metoda wywoływana w sekwencji do innych niż null `VirtualPathData` jest zwracany.

Podstawowy danych wejściowych do `GetVirtualPath` są:

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

Trasy przede wszystkim użyj wartości trasy udostępniane przez `Values` i `AmbientValues` decyzji o tym, jeśli jest to możliwe do generowania adresu URL i jakie wartości do uwzględnienia. `AmbientValues` Zestaw wartości trasy, które zostały utworzone z dopasowywania bieżące żądanie w systemie routingu. Z kolei `Values` są wartości trasy, które określają sposób generowania żądany adres URL dla bieżącej operacji. `HttpContext` Jest dostępne w przypadku trasy trzeba uzyskać usług lub dodatkowe dane skojarzone z bieżącym kontekście.

Porada: Należy traktować `Values` jako zbiór zastąpienia `AmbientValues`. Generowania adresu URL spróbuje ponownie użyć wartości tras z bieżącego żądania umożliwia łatwe do generowania adresów URL dla łącza za pomocą tego samego trasy i wartości trasy.

Dane wyjściowe `GetVirtualPath` jest `VirtualPathData`. `VirtualPathData` jest równolegle z `RouteData`; zawiera `VirtualPath` dla adresu URL danych wyjściowych, a także pewne dodatkowe właściwości, które powinien być ustawiony przez trasę.

`VirtualPathData.VirtualPath` Właściwość zawiera *ścieżki wirtualnej* utworzonego przez trasy. W zależności od potrzeb konieczne może przetwarzać dalszych ścieżki. Na przykład jeśli ma być renderowany wygenerowany adres URL w formacie HTML konieczne dołączenie wartości podstawowa ścieżka aplikacji.

`VirtualPathData.Router` Jest odwołaniem do pomyślnie wygenerowany adres URL trasy.

`VirtualPathData.DataTokens` Właściwości jest słownikiem dodatkowe dane dotyczące wygenerowany adres URL trasy. Jest to równolegle z `RouteData.DataTokens`.

### <a name="creating-routes"></a>Tworzenie trasy

Routing zapewnia `Route` klasy jako standardowa implementacja elementu `IRouter`. `Route` używa *szablon trasy* składni wzorce, które zostanie dopasowany ścieżkę adresu URL po `RouteAsync` jest wywoływana. `Route` będzie używał tego samego szablonu trasy do generowania adresu URL po `GetVirtualPath` jest wywoływana.

Większość aplikacji spowoduje utworzenie trasy przez wywołanie metody `MapRoute` lub jednej z metod rozszerzenia podobne zdefiniowane na `IRouteBuilder`. Wszystkie te metody spowoduje utworzenie wystąpienia `Route` i dodaj go do kolekcji tras.

Uwaga: `MapRoute` nie przyjmuje parametr programu obsługi trasy — tylko dodaje tras, na których będzie obsługiwany przez `DefaultHandler`. Ponieważ jest domyślny program obsługi `IRouter`, mogą zdecydować, nie można obsłużyć żądania. Na przykład ASP.NET MVC jest zazwyczaj skonfigurowany jako domyślny program obsługi, który obsługuje tylko żądania zgodnych dostępnych kontrolerów i akcji. Aby dowiedzieć się więcej na temat routingu do MVC, zobacz [routingu do akcji kontrolera](../mvc/controllers/routing.md).

To jest przykład `MapRoute` połączenie używane przez definicję typowej tras platformy ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ten szablon jest zgodny Ścieżka adresu URL, takie jak `/Products/Details/17` i Wyodrębnij wartości trasy `{ controller = Products, action = Details, id = 17 }`. Wartości trasy są określane przez dzielenia na segmenty ścieżki adresu URL i dopasowywanie każdy z segmentów z *trasy parametru* nazwie w szablonie trasy. Noszą nazwy parametrów trasy. Definiowania umieszczając nazwę parametru w nawiasach klamrowych `{ }`.

Powyżej szablonu można także dopasować ścieżkę adresu URL `/` i zwróci wartości `{ controller = Home, action = Index }`. Zdarza się to `{controller}` i `{action}` parametrów trasy mają przypisane wartości domyślne, a `id` parametr trasy jest opcjonalny. Equals `=` znak następuje wartość po Nazwa parametru trasy definiuje wartości domyślnej dla parametru. Znak zapytania `?` po Nazwa parametru trasy definiuje parametr jako opcjonalną. Parametry o wartości domyślne trasy *zawsze* tworzy wartości trasy, jeśli trasa odpowiada — następujące parametry opcjonalne nie tworzy wartości trasy, jeśli nie było żadnych odpowiadającym segmencie ścieżki adresu URL.

Zobacz [trasy szablonu odwołanie](#route-template-reference) dokładnego opis funkcji szablon trasy i składni.

W tym przykładzie dołączono *ograniczenia trasy*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ten szablon jest zgodny Ścieżka adresu URL, takie jak `/Products/Details/17`, ale nie `/Products/Details/Apples`. Definicja parametru trasy `{id:int}` definiuje *ograniczenia trasy* dla `id` parametru trasy. Ograniczenia trasy zaimplementować `IRouteConstraint` i sprawdzić wartości trasy w celu weryfikacji. W tym przykładzie wartość trasy `id` musi być możliwe do przekonwertowania na liczbę całkowitą. Zobacz [trasy ograniczenia odwołanie](#route-constraint-reference) dla bardziej szczegółowy opis ograniczenia trasy, które są dostarczane przez platformę.

Dodatkowe przeciążeń `MapRoute` zaakceptować wartości `constraints`, `dataTokens`, i `defaults`. Te dodatkowe parametry `MapRoute` są zdefiniowane jako typ `object`. Jest typowy sposób użycia tych parametrów do przekazania anonimowo typu obiektu, których nazwy właściwości typu anonimowego dopasowania trasy nazwy parametrów.

Następujące dwa przykłady tworzenia równoważne:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Porada: Składnia wbudowanego definiowanie ograniczeń i ustawień domyślnych może być wygodniejsze proste trasy. Istnieją jednak funkcji, takich jak tokeny danych, które nie są obsługiwane przez wbudowany składni.

W tym przykładzie przedstawiono kilka dodatkowych funkcji:

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

Ten szablon jest zgodny Ścieżka adresu URL, takie jak `/Blog/All-About-Routing/Introduction` , który wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Trasy domyślnej wartości dla `controller` i `action` są produkowane przez trasy, nawet jeśli w szablonie nie istnieją odpowiednie parametry trasy. Można określić wartości domyślne w szablonie trasy. `article` Parametr trasy jest zdefiniowany jako *wychwytywania* za wyglądu gwiazdkę `*` przed nazwą parametru trasy. Parametry trasy wychwytywania przechwytywania pozostała część ścieżki adresu URL i mogą być również zgodna pusty ciąg.

W tym przykładzie dodano tokeny ograniczenia i dane trasy:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Ten szablon jest zgodny Ścieżka adresu URL, takie jak `/Products/5` , który wyodrębnia wartości `{ controller = Products, action = Details, id = 5 }` i tokeny danych `{ locale = en-US }`.

![Zmienne lokalne tokenów systemu Windows](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>Generowania adresu URL

`Route` Klasy można również wykonywać generowania adresu URL, łącząc zestaw wartości tras z jej szablon trasy. Jest to logicznie procesu zgodnych ścieżkę adresu URL.

Porada: Aby lepiej zrozumieć generowania adresu URL, załóżmy adres URL, które chcesz wygenerować, a następnie zastanowić, jak szablon trasy spowoduje dopasowanie tego adresu URL. Czy będą tworzone co wartości? Jest to równoważne nierównej działania generowania adresu URL `Route` klasy.

W tym przykładzie użyto podstawowe trasy stylu platformy ASP.NET MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Przy użyciu wartości trasy `{ controller = Products, action = List }`, generuje adres URL tej trasy `/Products/List`. Wartości trasy są zastępowane dla odpowiednich parametrów trasy do utworzenia ścieżkę adresu URL. Ponieważ `id` jest opcjonalny parametr trasy, jest on żadnych problemów, że nie ma wartości.

Przy użyciu wartości trasy `{ controller = Home, action = Index }`, generuje adres URL tej trasy `/`. Wartości trasy, które zostały podane zgodne z wartościami domyślnymi, można bezpiecznie pominąć segmentów odpowiadającej jej wartości. Należy pamiętać, że oba adresy URL wygenerowany będzie wykonywać Rundy z tej definicji trasy i utworzyć takie same wartości trasy, które były używane do generowania adresu URL.

Porada: Należy użyć aplikacji za pomocą platformy ASP.NET MVC `UrlHelper` do generowania adresów URL zamiast wywoływać metodę do routingu bezpośredniego.

Aby uzyskać więcej informacji o procesie generowania adresu URL, zobacz [adres url generowania](#url-generation-reference).

## <a name="using-routing-middleware"></a>Za pomocą oprogramowania pośredniczącego routingu

Dodaj pakiet NuGet "Microsoft.AspNetCore.Routing".

Dodaj routingu do kontenera usług w *Startup.cs*:

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

Trasy muszą być skonfigurowane w `Configure` metoda `Startup` klasy. Poniższy przykład korzysta z poniższych interfejsów API:

* `RouteBuilder`
* `Build`
* `MapGet`  Jest zgodna tylko żądania HTTP GET
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
| /Package/Create/3  | Cześć! Wartości trasy: [operacji tworzenia], [identyfikator, 3] |
| / pakietu/ścieżka/3  | Cześć! Wartości trasy: [operacja, Śledź], [identyfikator, -3] |
| / pakietu/ścieżka/3 / | Cześć! Wartości trasy: [operacja, Śledź], [identyfikator, -3]  |
| / Package/ścieżka / | \<Przechodzić Brak dopasowania > |
| Pobierz /hello/Joe | Cześć, Jan! |
| POST /hello/Joe | \<Przechodzić, tylko odpowiada HTTP GET > |
| Pobierz /hello/Joe/Smith | \<Przechodzić Brak dopasowania > |

Jeśli konfigurujesz trasa wywołanie `app.UseRouter` przekazując `IRouter` wystąpienia. Nie trzeba wywołać `RouteBuilder`.

Platformę udostępnia zestaw metod rozszerzeń do tworzenia tras, takich jak:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Niektóre z tych metod, takich jak `MapGet` wymagają `RequestDelegate` mają zostać podane. `RequestDelegate` Będzie służyć jako *obsługi trasy* gdy trasa odpowiada. Inne metody w tej rodzinie umożliwiają konfigurowanie potoku oprogramowania pośredniczącego, które będzie służyć jako program obsługi trasy. Jeśli *mapy* — metoda nie obsługuje programu obsługi, takie jak `MapRoute`, a następnie użyje `DefaultHandler`.

`Map[Verb]` Metody używać ograniczenia w celu ograniczenia trasy do zlecenie HTTP w nazwie metody. Na przykład, zobacz [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) i [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).

## <a name="route-template-reference"></a>Odwołanie do szablonu trasy

Tokeny w nawiasy klamrowe (`{ }`) zdefiniowanie *parametry trasy* która powiązana, jeśli trasa jest zgodny. Można zdefiniować więcej niż jeden parametr trasy w segmencie trasy, ale muszą one być oddzielone wartość literału. Na przykład `{controller=Home}{action=Index}` byłoby prawidłowej trasy, ponieważ nie istnieje wartość literału między `{controller}` i `{action}`. Te parametry trasy musi mieć nazwę i może określić dodatkowe atrybuty.

Literały tekstowe niż parametrów trasy (na przykład `{id}`) i separatora ścieżki `/` musi być zgodna z tekstem w adresie URL. Dopasowywanie tekstu odbywa się bez uwzględniania wielkości liter i jest oparta na dekodowane reprezentacja ścieżki adresów URL. Aby dopasować Ogranicznik parametru trasy literału `{` lub `}`, sekwencji ucieczki powtarzając znak (`{{` lub `}}`).

Wzorce adresu URL, które próbują przechwytywania opcjonalny plik z rozszerzeniem nazwy pliku ma dodatkowe zagadnienia. Na przykład przy użyciu szablonu `files/{filename}.{ext?}` — po obu `filename` i `ext` istnieje, obie wartości zostaną wypełnione. Jeśli tylko `filename` istnieje w adresie URL dopasowań trasy, ponieważ kropki końcowej `.` jest opcjonalna. Następujące adresy URL spowoduje dopasowanie tej trasy:

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

Można użyć `*` znak jako prefiksu do parametr trasy do powiązania do pozostałej części identyfikatora URI — jest to *wychwytywania* parametru. Na przykład `blog/{*slug}` spowoduje dopasowanie dowolnej identyfikator URI, który wprowadzenie `/blog` i nie miał wartości następnego (może zostać przypisana do `slug` trasy wartości). Parametry wychwytywania można również zgodne pusty ciąg.

Może mieć parametrów trasy *wartości domyślne*, wyznaczonych, określając wartość domyślna po nazwę parametru, oddzielając `=`. Na przykład `{controller=Home}` zdefiniowane `Home` jako wartość domyślną dla `controller`. Jeśli wartość nie jest obecny w adresie URL dla parametru, zostanie użyta domyślna wartość. Oprócz wartości domyślne parametrów trasy może być opcjonalny (określone przez dołączenie `?` na końcu nazwy parametru, jak w `id?`). Różnica między opcjonalne i "ma domyślne" jest parametrem trasy z wartością domyślną będzie zawsze daje wartość; opcjonalny parametr ma wartość tylko wtedy, gdy zostało ono określone.

Parametry trasy mogą także mieć ograniczenia, które musi odpowiadać wartości trasy, powiązany z adresu URL. Dodawanie dwukropkiem `:` i nazwa ograniczenia po Określa nazwę parametru trasy *ograniczenie w tekście* parametru trasy. Jeśli ograniczenie wymaga argumentów, te są udostępniane w nawiasach `( )` po nazwie ograniczenia. Można określić wiele ograniczeń w tekście przez dołączenie innego dwukropek `:` i nazwa ograniczenia. Nazwa ograniczenia jest przekazywana do `IInlineConstraintResolver` usługi można utworzyć wystąpienia `IRouteConstraint` do użycia podczas przetwarzania adresu URL. Na przykład szablon trasy `blog/{article:minlength(10)}` Określa `minlength` ograniczenia z argumentem `10`. Aby uzyskać więcej ograniczenia trasy opis, a lista ograniczenia podana przez platformę, zobacz [trasy ograniczenia odwołanie](#route-constraint-reference).

Poniższa tabela przedstawia niektóre szablony trasy i ich zachowanie.

| Szablon trasy | Przykładowy adres URL dopasowania | Uwagi |
| -------- | -------- | ------- |
| Cześć  | /hello  | Zgodny tylko pojedynczą ścieżkę `/hello` |
| {Page=Home} | / | Dopasowuje i ustawia `Page` do `Home` |
| {Page=Home}  | / Skontaktuj się z  | Dopasowuje i ustawia `Page` do `Contact` |
| {controller} / {action} / {id}? | / / Listy produktów | Mapuje `Products` kontrolera i `List` akcji |
| {controller} / {action} / {id}? | / Produkty/szczegóły/123  |  Mapuje `Products` kontrolera i `Details` akcji.  `id` Ustaw 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  Mapuje `Home` kontrolera i `Index` metody; `id` jest ignorowana. |

Przy użyciu szablonu zwykle jest najprostsza metoda routingu. Ograniczenia i ustawienia domyślne można również określić poza szablon trasy.

Porada: Włączanie [rejestrowanie](xref:fundamentals/logging/index) aby zobaczyć sposób tworzone w implementacjach routingu, takich jak `Route`, odpowiada na żądania.

## <a name="route-constraint-reference"></a>Odwołanie do ograniczenia trasy

Ograniczenia trasy wykonania, gdy `Route` ma dopasowane składnia przychodzącego adresu URL i stokenizowanego ścieżkę adresu URL do wartości trasy. Ograniczenia trasy zazwyczaj sprawdzenie wartości trasy skojarzone za pośrednictwem szablon trasy i upewnij prostą tak/nie decyzja o tym, czy wartość jest dopuszczalne. Niektóre ograniczenia trasy umożliwia należy wziąć pod uwagę, czy można przekierować żądania danych poza wartości trasy. Na przykład `HttpMethodRouteConstraint` można akceptować lub odrzucać żądania na podstawie jego zleceń HTTP.

>[!WARNING]
> Unikaj używania ograniczenia **sprawdzania danych wejściowych**, ponieważ ten sposób oznacza, że nieprawidłowe dane wejściowe spowoduje 404 (nie znaleziono) zamiast 400 odpowiedni komunikat o błędzie. Ograniczenia trasy powinny być używane do **odróżniania** między podobne tras, nie można sprawdzić poprawności danych wejściowych dla określonej trasy.

Poniższa tabela przedstawia niektóre ograniczenia trasy, a ich oczekiwane zachowanie.

| ograniczenie | Przykład | Przykład dopasowań | Uwagi |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Dopasowuje dowolną liczbę całkowitą |
| `bool`  | `{active:bool}` | `true`, `FALSE` | Dopasowań `true` lub `false` (bez uwzględniania wielkości liter) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Zgodny z prawidłowym `DateTime` wartość (Niezmienna kultura — Zobacz ostrzeżenia) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Zgodny z prawidłowym `decimal` wartość (Niezmienna kultura — Zobacz ostrzeżenia) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | Zgodny z prawidłowym `double` wartość (Niezmienna kultura — Zobacz ostrzeżenia) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | Zgodny z prawidłowym `float` wartość (Niezmienna kultura — Zobacz ostrzeżenia) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Zgodny z prawidłowym `Guid` wartość |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Zgodny z prawidłowym `long` wartość |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Ciąg musi być co najmniej 4 znaków |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Ciąg musi mieć nie więcej niż 8 znaków |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Ciąg musi mieć dokładnie 12 znaków. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Ciąg musi zawierać co najmniej 8 i nie więcej niż 16 znaków |
| `min(value)` | `{age:min(18)}` | `19` | Wartość całkowita musi być co najmniej 18 |
| `max(value)` | `{age:max(120)}` |  `91` | Wartość całkowita musi być nie może przekraczać 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Wartość całkowita musi być co najmniej 18, ale nie może przekraczać 120 |
| `alpha` | `{name:alpha}` | `Rick` | Ciąg musi składać się z jednego lub kilku znaków alfabetycznych (`a`-`z`, bez uwzględniania wielkości liter) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Ciąg musi być zgodny z wyrażeniem regularnym (Zobacz porady na temat definiowania wyrażenie regularne) |
| `required`  | `{name:required}` | `Rick` |  Używany w celu wymuszenia, że wartość parametru nie jest obecny podczas generowania adresu URL |

>[!WARNING]
> Ograniczenia trasy, sprawdź adres URL, które mogą być konwertowane na typ CLR (takich jak `int` lub `DateTime`) zawsze używaj Niezmienna kultura — zakładają, że adres URL jest niemożliwe do zlokalizowania. Ograniczenia trasy dostarczane przez framework nie należy modyfikować wartości przechowywanych w wartości trasy. Wszystkie wartości trasy przeanalizować z adresu URL będą przechowywane jako ciągi. Na przykład [ograniczenia trasy Float](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) podejmie próbę przekonwertowania na format zmiennoprzecinkowy wartości trasy, ale skonwertowana wartość jest używana tylko do weryfikacji, aby można było przekonwertować na typ float.

## <a name="regular-expressions"></a>Wyrażenia regularne 

Dodaje platformę ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` do konstruktora wyrażenia regularnego. Zobacz [wyliczenie RegexOptions](https://docs.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions) opis tych elementów członkowskich.

Wyrażenia regularne używać ograniczników i tokeny podobne do tych używanych przez usługę Routing i języka C#. Należy użyć znaków ucieczki tokeny wyrażenia regularnego. Na przykład, aby użyć wyrażenia regularnego `^\d{3}-\d{2}-\d{4}$` routingu, musi mieć `\` znaków wpisanych jako `\\` w pliku źródłowym C# ucieczki `\` ciągu znak ucieczki (chyba że przy użyciu [dosłownego wyrażenia Literały ciągu](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string). `{` , `}` , "[" I "]" znaki należy wstawić podwajając im Usuń znaki Ogranicznik parametru routingu.  Poniższa tabela zawiera wyrażenie regularne i zmienionym wersji.

| Wyrażenie               | Uwaga |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | Wyrażenie regularne |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | Znaki ucieczki  |
| `^[a-z]{2}$` | Wyrażenie regularne |
| `^[[a-z]]{{2}}$` | Znaki ucieczki  |

Wyrażenia regularne użyte w routingu często zacznie działanie od `^` znaków (zaczynający się pozycja ciągu) i kończyć się `$` znaków (dopasowanie końcowy pozycji ciągu). `^` i `$` znaków Sprawdź, czy wartość parametru trasy całego dopasowania wyrażenia regularnego. Bez `^` i `$` znaków wyrażenia regularnego będzie dopasowania do dowolnego ciągu podrzędne w ciągu, która jest często nie ma. W poniższej tabeli przedstawiono kilka przykładów i wyjaśnia, dlaczego odpowiada lub nie odpowiada.

| Wyrażenie               | String | Dopasowanie | Komentarz |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | Cześć | Tak | dopasowań podciągów |
| `[a-z]{2}` | 123abc456 | Tak | dopasowań podciągów |
| `[a-z]{2}` | mz | Tak | pasuje do wyrażenia |
| `[a-z]{2}` | MZ | Tak | bez rozróżniania wielkości liter |
| `^[a-z]{2}$` |  Cześć | Brak | zobacz `^` i `$` powyżej |
| `^[a-z]{2}$` |  123abc456 | Brak | zobacz `^` i `$` powyżej |

Zapoznaj się [wyrażeń regularnych programu .NET Framework](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) uzyskać więcej informacji o składni wyrażeń regularnych.

Aby ograniczyć parametr znane zestaw możliwych wartości, należy użyć wyrażenie regularnego. Na przykład `{action:regex(^(list|get|create)$)}` zgodny tylko `action` trasy wartość `list`, `get`, lub `create`. Jeśli przekazany do słownika ograniczenia, ciąg "^ (listy | get | utworzyć) $" może być taka sama. Ograniczenia, które są przekazywane w słowniku ograniczenia (niewyrównane w ramach szablonu), które nie odpowiada żadnemu z znane ograniczenia są także traktowane jak wyrażenia regularne.

## <a name="url-generation-reference"></a>Odwołanie do generowania adresu URL

W poniższym przykładzie pokazano, jak wygenerować łącze do trasy podane słownika wartości trasy i `RouteCollection`.

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath` Jest generowany na końcu powyższego przykładu `/package/create/123`.

Drugi parametr `VirtualPathContext` Konstruktor jest kolekcją *otoczenia wartości*. Wygodniejszego otoczenia wartości przez ograniczenie liczby wartości, które deweloper musi określać w ramach kontekstu żądania. Bieżące wartości trasy bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy. Na przykład w aplikacji ASP.NET MVC w `About` akcji `HomeController`, nie trzeba określać wartości trasy kontrolera do odesłania do `Index` akcji (otoczenia wartość `Home` będą używane).

Otoczenia wartości, które nie jest zgodny z parametrem są ignorowane, a wartości otoczenia również są ignorowane, gdy wartość jawnie dostarczone przesłania, przechodząc od lewej do prawej w adresie URL.

Wartości są dostarczane bezpośrednio, ale która nie odpowiada niczego są dodawane do ciągu zapytania. W poniższej tabeli przedstawiono wynik przy użyciu szablonu trasy `{controller}/{action}/{id?}`.

| Wartości otoczenia | Jawne wartości | Wynik |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

Jeśli ta wartość nie zostanie podany wprost trasy ma wartość domyślną, który nie jest zgodny z parametrem, musi być zgodna wartość domyślną. Na przykład:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

Generowanie konsolidacji będzie generować tylko łącze dla tej trasy podano pasujących wartości dla kontrolerów i akcji.
