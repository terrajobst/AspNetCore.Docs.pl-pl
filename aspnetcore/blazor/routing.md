---
title: ASP.NET Core Routing Blazor
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: blazor/routing
ms.openlocfilehash: 067dad657c1e89a31fac45fdfa095cce4b10798d
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238059"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="1a1e2-103">ASP.NET Core Routing Blazor</span><span class="sxs-lookup"><span data-stu-id="1a1e2-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="1a1e2-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1a1e2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1a1e2-105">Dowiedz się, jak kierować żądania oraz jak używać `NavLink` składnika do tworzenia linków nawigacji w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="1a1e2-106">ASP.NET Core integracja z routingiem punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="1a1e2-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="1a1e2-107">Blazor po stronie serwera jest zintegrowana z [routingiem punktu końcowego ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="1a1e2-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="1a1e2-108">Aplikacja ASP.NET Core jest skonfigurowana do akceptowania połączeń przychodzących dla składników interaktywnych za `MapBlazorHub` pomocą `Startup.Configure`programu w programie:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="1a1e2-109">Szablony tras</span><span class="sxs-lookup"><span data-stu-id="1a1e2-109">Route templates</span></span>

<span data-ttu-id="1a1e2-110">`Router` Składnik włącza Routing, a szablon trasy jest dostarczany do każdego dostępnego składnika.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-110">The `Router` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="1a1e2-111">Składnik pojawi się w pliku *App. Razor:* `Router`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-111">The `Router` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="1a1e2-112">W aplikacji po stronie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-112">In a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="1a1e2-113">W aplikacji po stronie klienta Blazor:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-113">In a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="1a1e2-114">Gdy plik *Razor* z `@page` dyrektywą jest kompilowany, wygenerowana Klasa jest dostarczana z <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określeniem szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-114">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="1a1e2-115">W czasie wykonywania router szuka klas składników z `RouteAttribute` i renderuje składnik przy użyciu szablonu trasy zgodnego z żądanym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="1a1e2-116">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="1a1e2-117">Poniższy składnik odpowiada na żądania dla `/BlazorRoute` i: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="1a1e2-118">Aby prawidłowo generować trasy, aplikacja `<base>` musi zawierać tag w pliku *wwwroot/index.html* (po stronie klienta Blazor) lub *strony/_Host. cshtml* (po stronie serwera) z ścieżką bazową `href` aplikacji określoną w atrybucie ( `<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="1a1e2-118">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="1a1e2-119">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-119">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="1a1e2-120">Podaj zawartość niestandardową, jeśli nie można odnaleźć zawartości</span><span class="sxs-lookup"><span data-stu-id="1a1e2-120">Provide custom content when content isn't found</span></span>

<span data-ttu-id="1a1e2-121">`Router` Składnik umożliwia aplikacji określenie zawartości niestandardowej, jeśli nie można odnaleźć zawartości dla żądanej trasy.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-121">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="1a1e2-122">W pliku *App. Razor* Ustaw zawartość niestandardową w `<NotFoundContent>` elemencie `Router` składnika:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-122">In the *App.razor* file, set custom content in the `<NotFoundContent>` element of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

<span data-ttu-id="1a1e2-123">Zawartość `<NotFoundContent>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-123">The content of `<NotFoundContent>` can include arbitrary items, such as other interactive components.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="1a1e2-124">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="1a1e2-124">Route parameters</span></span>

<span data-ttu-id="1a1e2-125">Router używa parametrów trasy do wypełniania odpowiednich parametrów składnika o tej samej nazwie (bez uwzględniania wielkości liter):</span><span class="sxs-lookup"><span data-stu-id="1a1e2-125">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="1a1e2-126">Parametry opcjonalne nie są obsługiwane w przypadku aplikacji Blazor w wersji zapoznawczej ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-126">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="1a1e2-127">W `@page` poprzednim przykładzie są stosowane dwie dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-127">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="1a1e2-128">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-128">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="1a1e2-129">Druga `@page` dyrektywa `Text` przyjmuje parametr Route i przypisuje wartość do właściwości. `{text}`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-129">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="1a1e2-130">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="1a1e2-130">Route constraints</span></span>

<span data-ttu-id="1a1e2-131">Ograniczenie trasy wymusza dopasowanie typu w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-131">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="1a1e2-132">W poniższym przykładzie trasa do `Users` składnika dopasowuje się tylko wtedy, gdy:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-132">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="1a1e2-133">Segment `Id` trasy jest obecny w adresie URL żądania.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-133">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="1a1e2-134">Segment jest liczbą całkowitą (`int`). `Id`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-134">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="1a1e2-135">Dostępne są ograniczenia trasy podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-135">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="1a1e2-136">W przypadku ograniczeń trasy, które pasują do niezmiennej kultury, zobacz ostrzeżenie poniżej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-136">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="1a1e2-137">Typu</span><span class="sxs-lookup"><span data-stu-id="1a1e2-137">Constraint</span></span> | <span data-ttu-id="1a1e2-138">Przykład</span><span class="sxs-lookup"><span data-stu-id="1a1e2-138">Example</span></span>           | <span data-ttu-id="1a1e2-139">Przykładowe dopasowania</span><span class="sxs-lookup"><span data-stu-id="1a1e2-139">Example Matches</span></span>                                                                  | <span data-ttu-id="1a1e2-140">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="1a1e2-140">Invariant</span></span><br><span data-ttu-id="1a1e2-141">kultura</span><span class="sxs-lookup"><span data-stu-id="1a1e2-141">culture</span></span><br><span data-ttu-id="1a1e2-142">parowanie</span><span class="sxs-lookup"><span data-stu-id="1a1e2-142">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="1a1e2-143">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-143">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="1a1e2-144">Nie</span><span class="sxs-lookup"><span data-stu-id="1a1e2-144">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="1a1e2-145">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-145">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="1a1e2-146">Tak</span><span class="sxs-lookup"><span data-stu-id="1a1e2-146">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="1a1e2-147">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-147">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="1a1e2-148">Tak</span><span class="sxs-lookup"><span data-stu-id="1a1e2-148">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="1a1e2-149">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-149">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="1a1e2-150">Tak</span><span class="sxs-lookup"><span data-stu-id="1a1e2-150">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="1a1e2-151">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-151">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="1a1e2-152">Tak</span><span class="sxs-lookup"><span data-stu-id="1a1e2-152">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="1a1e2-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="1a1e2-154">Nie</span><span class="sxs-lookup"><span data-stu-id="1a1e2-154">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="1a1e2-155">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-155">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="1a1e2-156">Tak</span><span class="sxs-lookup"><span data-stu-id="1a1e2-156">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="1a1e2-157">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-157">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="1a1e2-158">Tak</span><span class="sxs-lookup"><span data-stu-id="1a1e2-158">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="1a1e2-159">Ograniczenia trasy, które weryfikują adres URL i są konwertowane na typ CLR (takie `int` jak `DateTime`lub), zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-159">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="1a1e2-160">W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-160">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="1a1e2-161">Routing z adresami URL zawierającymi kropki</span><span class="sxs-lookup"><span data-stu-id="1a1e2-161">Routing with URLs that contain dots</span></span>

<span data-ttu-id="1a1e2-162">W aplikacjach po stronie serwera Blazor domyślną trasą w *_Host. cshtml* jest `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="1a1e2-162">In Blazor server-side apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="1a1e2-163">Adres URL żądania, który zawiera kropkę`.`() nie pasuje do trasy domyślnej, ponieważ adres URL wygląda na żądanie pliku.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-163">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="1a1e2-164">Aplikacja Blazor zwraca *404 — nie odnaleziono* odpowiedzi dla pliku statycznego, który nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-164">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="1a1e2-165">Aby użyć tras zawierających kropkę, skonfiguruj *_Host. cshtml* przy użyciu następującego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-165">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="1a1e2-166">`"/{**path}"` Szablon zawiera:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-166">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="1a1e2-167">Podwójna gwiazdka *catch-all* (`**`) do przechwytywania ścieżki między wieloma granicami folderów bez kodowania ukośników (`/`).</span><span class="sxs-lookup"><span data-stu-id="1a1e2-167">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="1a1e2-168">Nazwa `path` parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-168">A `path` route parameter name.</span></span>

<span data-ttu-id="1a1e2-169">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-169">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="1a1e2-170">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="1a1e2-170">NavLink component</span></span>

<span data-ttu-id="1a1e2-171">Podczas tworzenia linków nawigacji Użyj`<a>` składnikazamiastelementówhiperlinkówHTML().`NavLink`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-171">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="1a1e2-172">Składnik zachowuje się `<a>` jak element, `active` z wyjątkiem przełączenia klasy CSS w zależności od tego, czy `href` jest on zgodny z bieżącym adresem URL. `NavLink`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-172">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="1a1e2-173">`active` Klasa pomaga użytkownikowi zrozumieć, która strona jest aktywną stroną między wyświetlanymi łączami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-173">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="1a1e2-174">Poniższy `NavMenu` składnik tworzy pasek nawigacyjny [Bootstrap](https://getbootstrap.com/docs/) , który pokazuje, jak używać `NavLink` składników:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-174">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="1a1e2-175">Dostępne są dwie `NavLinkMatch` opcje, które można przypisać `Match` do atrybutu `<NavLink>` elementu:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-175">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="1a1e2-176">`NavLinkMatch.All`Jest aktywny, `NavLink` gdy jest zgodny z całym bieżącym adresem URL. &ndash;</span><span class="sxs-lookup"><span data-stu-id="1a1e2-176">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="1a1e2-177">`NavLinkMatch.Prefix`(*ustawienie domyślne*) Jest aktywny, `NavLink` gdy pasuje do dowolnego prefiksu bieżącego adresu URL. &ndash;</span><span class="sxs-lookup"><span data-stu-id="1a1e2-177">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="1a1e2-178">W `NavLink` poprzednim przykładzie Strona główna `href=""` odpowiada głównemu `active` adresowi URL i odbiera tylko klasy CSS przy `https://localhost:5001/`domyślnym adresie URL ścieżki podstawowej aplikacji (na przykład).</span><span class="sxs-lookup"><span data-stu-id="1a1e2-178">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="1a1e2-179">Druga `NavLink` otrzymuje klasę, `active` gdy użytkownik `MyComponent` odwiedzi dowolny adres URL z prefiksem (na przykład `https://localhost:5001/MyComponent` i `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="1a1e2-179">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="1a1e2-180">Dodatkowe `NavLink` atrybuty składników są przenoszone do renderowanego tagu zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-180">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="1a1e2-181">W poniższym przykładzie `NavLink` składnik `target` zawiera atrybut:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-181">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="1a1e2-182">Renderuje następujący znacznik HTML:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-182">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="1a1e2-183">Pomoc dotycząca stanu identyfikatora URI i nawigacji</span><span class="sxs-lookup"><span data-stu-id="1a1e2-183">URI and navigation state helpers</span></span>

<span data-ttu-id="1a1e2-184">Służy `Microsoft.AspNetCore.Components.IUriHelper` do pracy z identyfikatorami URI i C# nawigacją w kodzie.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-184">Use `Microsoft.AspNetCore.Components.IUriHelper` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="1a1e2-185">`IUriHelper`zawiera zdarzenie i metody przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-185">`IUriHelper` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="1a1e2-186">Element członkowski</span><span class="sxs-lookup"><span data-stu-id="1a1e2-186">Member</span></span> | <span data-ttu-id="1a1e2-187">Opis</span><span class="sxs-lookup"><span data-stu-id="1a1e2-187">Description</span></span> |
| ------ | ----------- |
| `GetAbsoluteUri` | <span data-ttu-id="1a1e2-188">Pobiera bieżący bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-188">Gets the current absolute URI.</span></span> |
| `GetBaseUri` | <span data-ttu-id="1a1e2-189">Pobiera podstawowy identyfikator URI (z końcowym ukośnikiem), który można dołączać do względnych ścieżek URI w celu utworzenia bezwzględnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-189">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="1a1e2-190">`GetBaseUri` Zazwyczaj odpowiada `href` *atrybutowi* elementu dokumentu w wwwroot/index.html (Blazor po stronie klienta) lub *stron/_Host. cshtml* (Blazor po stronie serwera). `<base>`</span><span class="sxs-lookup"><span data-stu-id="1a1e2-190">Typically, `GetBaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="1a1e2-191">Przechodzi do określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-191">Navigates to the specified URI.</span></span> <span data-ttu-id="1a1e2-192">Jeśli `forceLoad` jest `true`:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-192">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="1a1e2-193">Routing po stronie klienta jest pomijany.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-193">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="1a1e2-194">W przeglądarce wymuszone jest załadowanie nowej strony z serwera, niezależnie od tego, czy identyfikator URI jest zwykle obsługiwany przez router po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-194">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `OnLocationChanged` | <span data-ttu-id="1a1e2-195">Zdarzenie, które jest wyzwalane po zmianie lokalizacji nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-195">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="1a1e2-196">Konwertuje względny identyfikator URI na bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-196">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="1a1e2-197">Przy użyciu podstawowego identyfikatora URI (na przykład identyfikatora URI zwróconego wcześniej `GetBaseUri`przez), program konwertuje bezwzględny identyfikator URI na identyfikator URI względem podstawowego prefiksu URI.</span><span class="sxs-lookup"><span data-stu-id="1a1e2-197">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="1a1e2-198">Poniższy składnik przechodzi do `Counter` składnika aplikacji po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="1a1e2-198">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```

