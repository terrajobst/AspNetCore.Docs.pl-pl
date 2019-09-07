---
title: ASP.NET Core Routing Blazor
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/routing
ms.openlocfilehash: d348908261c51b477aa698a407266d05c0df5a33
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800343"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="9bdf5-103">ASP.NET Core Routing Blazor</span><span class="sxs-lookup"><span data-stu-id="9bdf5-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="9bdf5-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9bdf5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9bdf5-105">Dowiedz się, jak kierować żądania oraz jak używać `NavLink` składnika do tworzenia linków nawigacji w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="9bdf5-106">ASP.NET Core integracja z routingiem punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="9bdf5-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="9bdf5-107">Blazor po stronie serwera jest zintegrowana z [routingiem punktu końcowego ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="9bdf5-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="9bdf5-108">Aplikacja ASP.NET Core jest skonfigurowana do akceptowania połączeń przychodzących dla składników interaktywnych za `MapBlazorHub` pomocą `Startup.Configure`programu w programie:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="9bdf5-109">Szablony tras</span><span class="sxs-lookup"><span data-stu-id="9bdf5-109">Route templates</span></span>

<span data-ttu-id="9bdf5-110">`Router` Składnik umożliwia kierowanie do każdego składnika z określoną trasą.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-110">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="9bdf5-111">Składnik pojawi się w pliku *App. Razor:* `Router`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-111">The `Router` component appears in the *App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="9bdf5-112">Gdy plik *Razor* z `@page` dyrektywą jest kompilowany, wygenerowana Klasa jest dostarczana z <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określeniem szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-112">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="9bdf5-113">W środowisku uruchomieniowym `RouteView` składnik:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-113">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="9bdf5-114">`RouteData` Odbiera`Router` od siebie wraz z dowolnymi żądanymi parametrami.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-114">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="9bdf5-115">Renderuje określony składnik za pomocą układu (lub opcjonalnego układu domyślnego) przy użyciu określonych parametrów.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-115">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="9bdf5-116">Opcjonalnie można określić `DefaultLayout` parametr z klasą układu, która ma być używana dla składników, które nie określają układu.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-116">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="9bdf5-117">Domyślne szablony Blazor określają `MainLayout` składnik.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-117">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="9bdf5-118">*MainLayout. Razor* znajduje się w folderze *udostępnionym* projektu szablonu.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-118">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="9bdf5-119">Aby uzyskać więcej informacji na temat układów <xref:blazor/layouts>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-119">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="9bdf5-120">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-120">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="9bdf5-121">Poniższy składnik odpowiada na żądania dla `/BlazorRoute` i: `/DifferentBlazorRoute`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-121">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="9bdf5-122">Aby adresy URL zostały poprawnie rozpoznane, aplikacja `<base>` musi zawierać tag w pliku *wwwroot/index.html* (po stronie klienta Blazor) lub *strony/_Host. cshtml* (po stronie serwera) z ścieżką bazową `href` aplikacji określoną w atrybucie ( `<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="9bdf5-122">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="9bdf5-123">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-123">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="9bdf5-124">Podaj zawartość niestandardową, jeśli nie można odnaleźć zawartości</span><span class="sxs-lookup"><span data-stu-id="9bdf5-124">Provide custom content when content isn't found</span></span>

<span data-ttu-id="9bdf5-125">`Router` Składnik umożliwia aplikacji określenie zawartości niestandardowej, jeśli nie można odnaleźć zawartości dla żądanej trasy.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-125">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="9bdf5-126">W pliku *App. Razor* Ustaw zawartość niestandardową w `NotFound` parametrze `Router` szablonu składnika:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-126">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

<span data-ttu-id="9bdf5-127">Zawartość `<NotFound>` tagów może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-127">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="9bdf5-128">Aby zastosować domyślny układ do `NotFound` zawartości, zobacz. <xref:blazor/layouts></span><span class="sxs-lookup"><span data-stu-id="9bdf5-128">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="9bdf5-129">Kierowanie do składników z wielu zestawów</span><span class="sxs-lookup"><span data-stu-id="9bdf5-129">Route to components from multiple assemblies</span></span>

<span data-ttu-id="9bdf5-130">Użyj parametru `AdditionalAssemblies` , aby określić dodatkowe zestawy `Router` dla składnika do uwzględnienia podczas wyszukiwania składników routingu.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-130">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="9bdf5-131">Określone zestawy są traktowane jako uzupełnienie `AppAssembly`określonego zestawu.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-131">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="9bdf5-132">W poniższym przykładzie `Component1` jest składnikiem rutowanym zdefiniowanym w bibliotece klas, do której się odwołuje.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-132">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="9bdf5-133">W poniższym `AdditionalAssemblies` przykładzie przedstawiono obsługę routingu dla: `Component1`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-133">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="9bdf5-134">< router AppAssembly = "typeof (program). Zestaw "AdditionalAssemblies =" New [] {typeof (Component1). Zestaw} >...</Router></span><span class="sxs-lookup"><span data-stu-id="9bdf5-134"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="9bdf5-135">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="9bdf5-135">Route parameters</span></span>

<span data-ttu-id="9bdf5-136">Router używa parametrów trasy do wypełniania odpowiednich parametrów składnika o tej samej nazwie (bez uwzględniania wielkości liter):</span><span class="sxs-lookup"><span data-stu-id="9bdf5-136">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="9bdf5-137">Parametry opcjonalne nie są obsługiwane w przypadku aplikacji Blazor w wersji zapoznawczej ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-137">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="9bdf5-138">W `@page` poprzednim przykładzie są stosowane dwie dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-138">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="9bdf5-139">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-139">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="9bdf5-140">Druga `@page` dyrektywa `Text` przyjmuje parametr Route i przypisuje wartość do właściwości. `{text}`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-140">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="9bdf5-141">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="9bdf5-141">Route constraints</span></span>

<span data-ttu-id="9bdf5-142">Ograniczenie trasy wymusza dopasowanie typu w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-142">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="9bdf5-143">W poniższym przykładzie trasa do `Users` składnika dopasowuje się tylko wtedy, gdy:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-143">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="9bdf5-144">Segment `Id` trasy jest obecny w adresie URL żądania.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-144">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="9bdf5-145">Segment jest liczbą całkowitą (`int`). `Id`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-145">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="9bdf5-146">Dostępne są ograniczenia trasy podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-146">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="9bdf5-147">W przypadku ograniczeń trasy, które pasują do niezmiennej kultury, zobacz ostrzeżenie poniżej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-147">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="9bdf5-148">Typu</span><span class="sxs-lookup"><span data-stu-id="9bdf5-148">Constraint</span></span> | <span data-ttu-id="9bdf5-149">Przykład</span><span class="sxs-lookup"><span data-stu-id="9bdf5-149">Example</span></span>           | <span data-ttu-id="9bdf5-150">Przykładowe dopasowania</span><span class="sxs-lookup"><span data-stu-id="9bdf5-150">Example Matches</span></span>                                                                  | <span data-ttu-id="9bdf5-151">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="9bdf5-151">Invariant</span></span><br><span data-ttu-id="9bdf5-152">kultura</span><span class="sxs-lookup"><span data-stu-id="9bdf5-152">culture</span></span><br><span data-ttu-id="9bdf5-153">parowanie</span><span class="sxs-lookup"><span data-stu-id="9bdf5-153">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="9bdf5-154">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-154">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="9bdf5-155">Nie</span><span class="sxs-lookup"><span data-stu-id="9bdf5-155">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="9bdf5-156">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-156">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="9bdf5-157">Tak</span><span class="sxs-lookup"><span data-stu-id="9bdf5-157">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="9bdf5-158">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-158">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="9bdf5-159">Tak</span><span class="sxs-lookup"><span data-stu-id="9bdf5-159">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="9bdf5-160">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-160">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="9bdf5-161">Tak</span><span class="sxs-lookup"><span data-stu-id="9bdf5-161">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="9bdf5-162">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-162">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="9bdf5-163">Tak</span><span class="sxs-lookup"><span data-stu-id="9bdf5-163">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="9bdf5-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="9bdf5-165">Nie</span><span class="sxs-lookup"><span data-stu-id="9bdf5-165">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="9bdf5-166">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-166">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="9bdf5-167">Tak</span><span class="sxs-lookup"><span data-stu-id="9bdf5-167">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="9bdf5-168">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-168">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="9bdf5-169">Tak</span><span class="sxs-lookup"><span data-stu-id="9bdf5-169">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="9bdf5-170">Ograniczenia trasy, które weryfikują adres URL i są konwertowane na typ CLR (takie `int` jak `DateTime`lub), zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-170">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="9bdf5-171">W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-171">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="9bdf5-172">Routing z adresami URL zawierającymi kropki</span><span class="sxs-lookup"><span data-stu-id="9bdf5-172">Routing with URLs that contain dots</span></span>

<span data-ttu-id="9bdf5-173">W aplikacjach po stronie serwera Blazor domyślną trasą w *_Host. cshtml* jest `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="9bdf5-173">In Blazor server-side apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="9bdf5-174">Adres URL żądania, który zawiera kropkę`.`() nie pasuje do trasy domyślnej, ponieważ adres URL wygląda na żądanie pliku.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-174">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="9bdf5-175">Aplikacja Blazor zwraca *404 — nie odnaleziono* odpowiedzi dla pliku statycznego, który nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-175">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="9bdf5-176">Aby użyć tras zawierających kropkę, skonfiguruj *_Host. cshtml* przy użyciu następującego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-176">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="9bdf5-177">`"/{**path}"` Szablon zawiera:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-177">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="9bdf5-178">Podwójna gwiazdka *catch-all* (`**`) do przechwytywania ścieżki między wieloma granicami folderów bez kodowania ukośników (`/`).</span><span class="sxs-lookup"><span data-stu-id="9bdf5-178">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="9bdf5-179">Nazwa `path` parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-179">A `path` route parameter name.</span></span>

<span data-ttu-id="9bdf5-180">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="9bdf5-181">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="9bdf5-181">NavLink component</span></span>

<span data-ttu-id="9bdf5-182">Podczas tworzenia linków nawigacji Użyj`<a>` składnikazamiastelementówhiperlinkówHTML().`NavLink`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-182">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="9bdf5-183">Składnik zachowuje się `<a>` jak element, `active` z wyjątkiem przełączenia klasy CSS w zależności od tego, czy `href` jest on zgodny z bieżącym adresem URL. `NavLink`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-183">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="9bdf5-184">`active` Klasa pomaga użytkownikowi zrozumieć, która strona jest aktywną stroną między wyświetlanymi łączami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-184">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="9bdf5-185">Poniższy `NavMenu` składnik tworzy pasek nawigacyjny [Bootstrap](https://getbootstrap.com/docs/) , który pokazuje, jak używać `NavLink` składników:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-185">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="9bdf5-186">Dostępne są dwie `NavLinkMatch` opcje, które można przypisać `Match` do atrybutu `<NavLink>` elementu:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-186">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="9bdf5-187">`NavLinkMatch.All`Jest aktywny, `NavLink` gdy jest zgodny z całym bieżącym adresem URL. &ndash;</span><span class="sxs-lookup"><span data-stu-id="9bdf5-187">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="9bdf5-188">`NavLinkMatch.Prefix`(*ustawienie domyślne*) Jest aktywny, `NavLink` gdy pasuje do dowolnego prefiksu bieżącego adresu URL. &ndash;</span><span class="sxs-lookup"><span data-stu-id="9bdf5-188">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="9bdf5-189">W `NavLink` poprzednim przykładzie Strona główna `href=""` odpowiada głównemu `active` adresowi URL i odbiera tylko klasy CSS przy `https://localhost:5001/`domyślnym adresie URL ścieżki podstawowej aplikacji (na przykład).</span><span class="sxs-lookup"><span data-stu-id="9bdf5-189">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="9bdf5-190">Druga `NavLink` otrzymuje klasę, `active` gdy użytkownik `MyComponent` odwiedzi dowolny adres URL z prefiksem (na przykład `https://localhost:5001/MyComponent` i `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="9bdf5-190">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="9bdf5-191">Dodatkowe `NavLink` atrybuty składników są przenoszone do renderowanego tagu zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-191">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="9bdf5-192">W poniższym przykładzie `NavLink` składnik `target` zawiera atrybut:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-192">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="9bdf5-193">Renderuje następujący znacznik HTML:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-193">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="9bdf5-194">Pomoc dotycząca stanu identyfikatora URI i nawigacji</span><span class="sxs-lookup"><span data-stu-id="9bdf5-194">URI and navigation state helpers</span></span>

<span data-ttu-id="9bdf5-195">Służy `Microsoft.AspNetCore.Components.NavigationManager` do pracy z identyfikatorami URI i C# nawigacją w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-195">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="9bdf5-196">`NavigationManager`zawiera zdarzenie i metody przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-196">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="9bdf5-197">Element członkowski</span><span class="sxs-lookup"><span data-stu-id="9bdf5-197">Member</span></span> | <span data-ttu-id="9bdf5-198">Opis</span><span class="sxs-lookup"><span data-stu-id="9bdf5-198">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="9bdf5-199">Pobiera bieżący bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-199">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="9bdf5-200">Pobiera podstawowy identyfikator URI (z końcowym ukośnikiem), który można dołączać do względnych ścieżek URI w celu utworzenia bezwzględnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-200">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="9bdf5-201">`BaseUri` Zazwyczaj odpowiada `href` *atrybutowi* elementu dokumentu w wwwroot/index.html (Blazor po stronie klienta) lub *stron/_Host. cshtml* (Blazor po stronie serwera). `<base>`</span><span class="sxs-lookup"><span data-stu-id="9bdf5-201">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="9bdf5-202">Przechodzi do określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-202">Navigates to the specified URI.</span></span> <span data-ttu-id="9bdf5-203">Jeśli `forceLoad` jest `true`:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-203">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="9bdf5-204">Routing po stronie klienta jest pomijany.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-204">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="9bdf5-205">W przeglądarce wymuszone jest załadowanie nowej strony z serwera, niezależnie od tego, czy identyfikator URI jest zwykle obsługiwany przez router po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-205">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="9bdf5-206">Zdarzenie, które jest wyzwalane po zmianie lokalizacji nawigacji.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-206">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="9bdf5-207">Konwertuje względny identyfikator URI na bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-207">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="9bdf5-208">Przy użyciu podstawowego identyfikatora URI (na przykład identyfikatora URI zwróconego wcześniej `GetBaseUri`przez), program konwertuje bezwzględny identyfikator URI na identyfikator URI względem podstawowego prefiksu URI.</span><span class="sxs-lookup"><span data-stu-id="9bdf5-208">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="9bdf5-209">Poniższy składnik przechodzi do `Counter` składnika aplikacji po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="9bdf5-209">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```
