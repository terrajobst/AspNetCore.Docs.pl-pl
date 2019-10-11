---
title: ASP.NET Core Routing Blazor
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2019
uid: blazor/routing
ms.openlocfilehash: 8f48112237e6dd3fed88404c53b8d7d9137ef6ff
ms.sourcegitcommit: 0b8a7571bf7acf85bf16118acb2435001cbe4b5d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2019
ms.locfileid: "72236528"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="ebc3c-103">ASP.NET Core Routing Blazor</span><span class="sxs-lookup"><span data-stu-id="ebc3c-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="ebc3c-104">Autor [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ebc3c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ebc3c-105">Dowiedz się, jak kierować żądania oraz jak używać składnika `NavLink` do tworzenia linków nawigacji w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="ebc3c-106">ASP.NET Core integracja z routingiem punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="ebc3c-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="ebc3c-107">Serwer Blazor jest zintegrowany z [routingiem punktu końcowego ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="ebc3c-108">Aplikacja ASP.NET Core jest skonfigurowana do akceptowania połączeń przychodzących dla składników interaktywnych z `MapBlazorHub` w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="ebc3c-109">Najbardziej typową konfiguracją jest kierowanie wszystkich żądań do strony Razor, która działa jako host dla części serwerowej aplikacji Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="ebc3c-110">Zgodnie z Konwencją strona *hosta* ma zwykle nazwę *_Host. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="ebc3c-111">Trasa określona w pliku hosta jest nazywana *trasą rezerwową* , ponieważ działa z niskim priorytetem w dopasowaniu tras.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="ebc3c-112">Trasa rezerwowa jest brana pod uwagę, gdy inne trasy nie są zgodne.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="ebc3c-113">Dzięki temu aplikacja może korzystać z innych kontrolerów i stron bez zakłócania działania aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="ebc3c-114">Szablony tras</span><span class="sxs-lookup"><span data-stu-id="ebc3c-114">Route templates</span></span>

<span data-ttu-id="ebc3c-115">Składnik `Router` umożliwia routing do każdego składnika z określoną trasą.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="ebc3c-116">Składnik `Router` pojawia się w pliku *App. Razor* :</span><span class="sxs-lookup"><span data-stu-id="ebc3c-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="ebc3c-117">Gdy zostanie skompilowany plik *Razor* z dyrektywą `@page`, wygenerowana klasa ma <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określający szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="ebc3c-118">W środowisku uruchomieniowym składnik `RouteView`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="ebc3c-119">Odbiera `RouteData` z `Router` wraz z dowolnymi żądanymi parametrami.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="ebc3c-120">Renderuje określony składnik za pomocą układu (lub opcjonalnego układu domyślnego) przy użyciu określonych parametrów.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="ebc3c-121">Opcjonalnie można określić parametr `DefaultLayout` z klasą układu, która ma być używana dla składników, które nie określają układu.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="ebc3c-122">Domyślne szablony Blazor określają składnik `MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="ebc3c-123">*MainLayout. Razor* znajduje się w folderze *udostępnionym* projektu szablonu.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="ebc3c-124">Aby uzyskać więcej informacji na temat układów, zobacz <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="ebc3c-125">Do składnika można zastosować wiele szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="ebc3c-126">Poniższy składnik odpowiada na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="ebc3c-127">Aby adresy URL zostały poprawnie rozpoznane, aplikacja musi zawierać tag `<base>` w pliku *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server) z ścieżką bazową aplikacji określoną w atrybucie `href` (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="ebc3c-128">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="ebc3c-129">Podaj zawartość niestandardową, jeśli nie można odnaleźć zawartości</span><span class="sxs-lookup"><span data-stu-id="ebc3c-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="ebc3c-130">Składnik `Router` umożliwia aplikacji określenie zawartości niestandardowej, jeśli nie można odnaleźć zawartości dla żądanej trasy.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="ebc3c-131">W pliku *App. Razor* Ustaw zawartość niestandardową w parametrze szablonu `NotFound` składnika `Router`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="ebc3c-132">Zawartość tagów `<NotFound>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="ebc3c-133">Aby zastosować domyślny układ do `NotFound` zawartości, zobacz <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="ebc3c-134">Kierowanie do składników z wielu zestawów</span><span class="sxs-lookup"><span data-stu-id="ebc3c-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="ebc3c-135">Użyj parametru `AdditionalAssemblies`, aby określić dodatkowe zestawy dla składnika `Router`, które mają być brane pod uwagę podczas wyszukiwania składników routingu.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="ebc3c-136">Określone zestawy są uznawane za uzupełnieniem @no__t określonego zestawu -0.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="ebc3c-137">W poniższym przykładzie `Component1` to składnik rutowany zdefiniowany w bibliotece klas, do której się odwołuje.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="ebc3c-138">Poniższy `AdditionalAssemblies` przykład powoduje obsługę routingu dla `Component1`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }>
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="ebc3c-139">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="ebc3c-139">Route parameters</span></span>

<span data-ttu-id="ebc3c-140">Router używa parametrów trasy do wypełniania odpowiednich parametrów składnika o tej samej nazwie (bez uwzględniania wielkości liter):</span><span class="sxs-lookup"><span data-stu-id="ebc3c-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="ebc3c-141">Parametry opcjonalne nie są obsługiwane w przypadku aplikacji Blazor w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-141">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0.</span></span> <span data-ttu-id="ebc3c-142">W poprzednim przykładzie zastosowano dwie dyrektywy `@page`.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="ebc3c-143">Pierwszy zezwala na nawigowanie do składnika bez parametru.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="ebc3c-144">Druga dyrektywa `@page` przyjmuje parametr trasy `{text}` i przypisuje wartość do właściwości `Text`.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="ebc3c-145">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="ebc3c-145">Route constraints</span></span>

<span data-ttu-id="ebc3c-146">Ograniczenie trasy wymusza dopasowanie typu w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="ebc3c-147">W poniższym przykładzie trasy do składnika `Users` dopasowuje się tylko wtedy, gdy:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="ebc3c-148">W adresie URL żądania występuje segment trasy `Id`.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="ebc3c-149">Segment `Id` jest liczbą całkowitą (`int`).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-149">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="ebc3c-150">Dostępne są ograniczenia trasy podane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="ebc3c-151">W przypadku ograniczeń trasy, które pasują do niezmiennej kultury, zobacz ostrzeżenie poniżej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="ebc3c-152">Typu</span><span class="sxs-lookup"><span data-stu-id="ebc3c-152">Constraint</span></span> | <span data-ttu-id="ebc3c-153">Przykład</span><span class="sxs-lookup"><span data-stu-id="ebc3c-153">Example</span></span>           | <span data-ttu-id="ebc3c-154">Przykładowe dopasowania</span><span class="sxs-lookup"><span data-stu-id="ebc3c-154">Example Matches</span></span>                                                                  | <span data-ttu-id="ebc3c-155">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="ebc3c-155">Invariant</span></span><br><span data-ttu-id="ebc3c-156">dziedzinie</span><span class="sxs-lookup"><span data-stu-id="ebc3c-156">culture</span></span><br><span data-ttu-id="ebc3c-157">dopasowania</span><span class="sxs-lookup"><span data-stu-id="ebc3c-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="ebc3c-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="ebc3c-159">Nie</span><span class="sxs-lookup"><span data-stu-id="ebc3c-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="ebc3c-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="ebc3c-161">Tak</span><span class="sxs-lookup"><span data-stu-id="ebc3c-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="ebc3c-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="ebc3c-163">Tak</span><span class="sxs-lookup"><span data-stu-id="ebc3c-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="ebc3c-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="ebc3c-165">Tak</span><span class="sxs-lookup"><span data-stu-id="ebc3c-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="ebc3c-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="ebc3c-167">Tak</span><span class="sxs-lookup"><span data-stu-id="ebc3c-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="ebc3c-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="ebc3c-169">Nie</span><span class="sxs-lookup"><span data-stu-id="ebc3c-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="ebc3c-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="ebc3c-171">Tak</span><span class="sxs-lookup"><span data-stu-id="ebc3c-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="ebc3c-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ebc3c-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="ebc3c-173">Tak</span><span class="sxs-lookup"><span data-stu-id="ebc3c-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="ebc3c-174">Ograniczenia trasy weryfikujące adres URL i konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="ebc3c-175">W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="ebc3c-176">Routing z adresami URL zawierającymi kropki</span><span class="sxs-lookup"><span data-stu-id="ebc3c-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="ebc3c-177">W aplikacjach serwera Blazor domyślną trasą w *_Host. cshtml* jest `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="ebc3c-178">Adres URL żądania, który zawiera kropkę (`.`) nie pasuje do trasy domyślnej, ponieważ adres URL wygląda na żądanie pliku.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="ebc3c-179">Aplikacja Blazor zwraca *404 — nie odnaleziono* odpowiedzi dla pliku statycznego, który nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="ebc3c-180">Aby użyć tras zawierających kropkę, skonfiguruj *_Host. cshtml* przy użyciu następującego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="ebc3c-181">Szablon `"/{**path}"` obejmuje:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="ebc3c-182">Podwójna gwiazdka *catch-all* (`**`), aby przechwycić ścieżkę między wieloma granicami folderów bez kodowania ukośników (`/`).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="ebc3c-183">Nazwa parametru trasy `path`.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-183">A `path` route parameter name.</span></span>

<span data-ttu-id="ebc3c-184">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-184">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="ebc3c-185">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="ebc3c-185">NavLink component</span></span>

<span data-ttu-id="ebc3c-186">Podczas tworzenia linków nawigacji używaj składnika `NavLink` zamiast elementów hiperlinków (`<a>`).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-186">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="ebc3c-187">Składnik `NavLink` działa jak element `<a>`, z wyjątkiem przełączenia klasy CSS `active` w zależności od tego, czy `href` jest zgodny z bieżącym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-187">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="ebc3c-188">Klasa `active` pomaga użytkownikowi zrozumieć, która strona jest aktywną stroną między wyświetlonymi łączami nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-188">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="ebc3c-189">Poniższy składnik `NavMenu` tworzy pasek nawigacyjny [Bootstrap](https://getbootstrap.com/docs/) , który pokazuje, jak używać składników `NavLink`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-189">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="ebc3c-190">Dostępne są dwie opcje `NavLinkMatch`, które można przypisać do atrybutu `Match` elementu `<NavLink>`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-190">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="ebc3c-191">`NavLinkMatch.All` &ndash; `NavLink` jest aktywny, gdy jest zgodny z całym bieżącym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-191">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="ebc3c-192">`NavLinkMatch.Prefix` (*wartość domyślna*) &ndash; `NavLink` jest aktywny, gdy dopasowuje dowolny prefiks bieżącego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-192">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="ebc3c-193">W poprzednim przykładzie @no__t Home-0 `href=""` dopasowuje adres URL strony głównej i odbiera tylko klasę CSS `active` przy domyślnym adresie URL ścieżki podstawowej aplikacji (na przykład `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-193">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="ebc3c-194">Sekunda `NavLink` otrzymuje klasę `active`, gdy użytkownik odwiedzi dowolny adres URL z prefiksem `MyComponent` (na przykład `https://localhost:5001/MyComponent` i `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-194">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="ebc3c-195">Dodatkowe atrybuty składnika `NavLink` są przenoszone do renderowanego tagu zakotwiczenia.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-195">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="ebc3c-196">W poniższym przykładzie składnik `NavLink` zawiera atrybut `target`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-196">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="ebc3c-197">Renderuje następujący znacznik HTML:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-197">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="ebc3c-198">Pomoc dotycząca stanu identyfikatora URI i nawigacji</span><span class="sxs-lookup"><span data-stu-id="ebc3c-198">URI and navigation state helpers</span></span>

<span data-ttu-id="ebc3c-199">Użyj `Microsoft.AspNetCore.Components.NavigationManager` do pracy z identyfikatorami URI i C# nawigacją w kodzie.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-199">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="ebc3c-200">`NavigationManager` zawiera zdarzenie i metody przedstawione w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-200">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="ebc3c-201">Członek</span><span class="sxs-lookup"><span data-stu-id="ebc3c-201">Member</span></span> | <span data-ttu-id="ebc3c-202">Opis</span><span class="sxs-lookup"><span data-stu-id="ebc3c-202">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="ebc3c-203">Pobiera bieżący bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-203">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="ebc3c-204">Pobiera podstawowy identyfikator URI (z końcowym ukośnikiem), który można dołączać do względnych ścieżek URI w celu utworzenia bezwzględnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-204">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="ebc3c-205">Zazwyczaj `BaseUri` odpowiada atrybutowi `href` w elemencie `<base>` dokumentu w *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="ebc3c-205">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="ebc3c-206">Przechodzi do określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-206">Navigates to the specified URI.</span></span> <span data-ttu-id="ebc3c-207">Jeśli `forceLoad` jest `true`:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-207">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="ebc3c-208">Routing po stronie klienta jest pomijany.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-208">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="ebc3c-209">W przeglądarce wymuszone jest załadowanie nowej strony z serwera, niezależnie od tego, czy identyfikator URI jest zwykle obsługiwany przez router po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-209">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="ebc3c-210">Zdarzenie, które jest wyzwalane po zmianie lokalizacji nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-210">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="ebc3c-211">Konwertuje względny identyfikator URI na bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-211">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="ebc3c-212">Przy użyciu podstawowego identyfikatora URI (na przykład identyfikatora URI wcześniej zwróconego przez `GetBaseUri`) program konwertuje bezwzględny identyfikator URI na identyfikator URI względem podstawowego prefiksu URI.</span><span class="sxs-lookup"><span data-stu-id="ebc3c-212">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="ebc3c-213">Poniższy składnik przechodzi do składnika `Counter` aplikacji po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="ebc3c-213">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
