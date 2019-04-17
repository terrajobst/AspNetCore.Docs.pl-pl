---
title: Blazor routing
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/routing
ms.openlocfilehash: bf2a2f6218b0fb4637d60565711c08aa31cebeef
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614881"
---
# <a name="blazor-routing"></a><span data-ttu-id="9183b-103">Blazor routing</span><span class="sxs-lookup"><span data-stu-id="9183b-103">Blazor routing</span></span>

<span data-ttu-id="9183b-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9183b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9183b-105">Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.</span><span class="sxs-lookup"><span data-stu-id="9183b-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="9183b-106">Integracja routingu platformy ASP.NET Core punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="9183b-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="9183b-107">Jest zintegrowana z Blazor [platformy ASP.NET Core routingu](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="9183b-107">Blazor is integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="9183b-108">Aplikacji ASP.NET Core jest skonfigurowany do akceptowania połączeń przychodzących do interaktywnego składników za pomocą `MapComponentHub<TComponent>` w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9183b-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> <span data-ttu-id="9183b-109">`MapComponentHub` Określa, że składnik główny `App` powinien być renderowany w obrębie elementu DOM, dopasowania selektor `app`:</span><span class="sxs-lookup"><span data-stu-id="9183b-109">`MapComponentHub` specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="9183b-110">Szablonów tras</span><span class="sxs-lookup"><span data-stu-id="9183b-110">Route templates</span></span>

<span data-ttu-id="9183b-111">`<Router>` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny.</span><span class="sxs-lookup"><span data-stu-id="9183b-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="9183b-112">`<Router>` Składnika, który pojawia się w *Components/App.razor* pliku:</span><span class="sxs-lookup"><span data-stu-id="9183b-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="9183b-113">Gdy *.razor* lub *.cshtml* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="9183b-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="9183b-114">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9183b-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="9183b-115">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="9183b-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="9183b-116">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="9183b-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

<span data-ttu-id="9183b-117">`<Router>` obsługuje ustawienia rezerwowe składnika renderowanie, gdy żądanej trasie nie zostanie rozwiązany.</span><span class="sxs-lookup"><span data-stu-id="9183b-117">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="9183b-118">Włącz w tym scenariuszu uczestnictwo, ustawiając `FallbackComponent` parametru na typ klasy składnika rezerwowego.</span><span class="sxs-lookup"><span data-stu-id="9183b-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="9183b-119">W poniższym przykładzie ustawiono składnikiem, który został zdefiniowany w *Pages/MyFallbackRazorComponent.cshtml* jako rezerwowej składnik dla `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="9183b-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="9183b-120">Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku ze ścieżki podstawowej aplikacji określony w `href` atrybutu (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="9183b-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="9183b-121">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="9183b-121">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="9183b-122">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="9183b-122">Route parameters</span></span>

<span data-ttu-id="9183b-123">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter):</span><span class="sxs-lookup"><span data-stu-id="9183b-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="9183b-124">Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="9183b-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="9183b-125">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="9183b-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="9183b-126">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9183b-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="9183b-127">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="9183b-127">Route constraints</span></span>

<span data-ttu-id="9183b-128">Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="9183b-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="9183b-129">W poniższym przykładzie trasy do składnika użytkowników tylko pasuje, jeśli:</span><span class="sxs-lookup"><span data-stu-id="9183b-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="9183b-130">`Id` Segmentu trasy jest obecny w adresie URL żądania.</span><span class="sxs-lookup"><span data-stu-id="9183b-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="9183b-131">`Id` Segmentu jest liczbą całkowitą (`int`).</span><span class="sxs-lookup"><span data-stu-id="9183b-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="9183b-132">Ograniczenia trasy, pokazano w poniższej tabeli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="9183b-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="9183b-133">Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9183b-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="9183b-134">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="9183b-134">Constraint</span></span> | <span data-ttu-id="9183b-135">Przykład</span><span class="sxs-lookup"><span data-stu-id="9183b-135">Example</span></span>           | <span data-ttu-id="9183b-136">Przykład dopasowań</span><span class="sxs-lookup"><span data-stu-id="9183b-136">Example Matches</span></span>                                                                  | <span data-ttu-id="9183b-137">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="9183b-137">Invariant</span></span><br><span data-ttu-id="9183b-138">kultura</span><span class="sxs-lookup"><span data-stu-id="9183b-138">culture</span></span><br><span data-ttu-id="9183b-139">parowanie</span><span class="sxs-lookup"><span data-stu-id="9183b-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="9183b-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="9183b-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="9183b-141">Nie</span><span class="sxs-lookup"><span data-stu-id="9183b-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="9183b-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="9183b-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="9183b-143">Yes</span><span class="sxs-lookup"><span data-stu-id="9183b-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="9183b-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="9183b-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="9183b-145">Yes</span><span class="sxs-lookup"><span data-stu-id="9183b-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="9183b-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="9183b-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="9183b-147">Tak</span><span class="sxs-lookup"><span data-stu-id="9183b-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="9183b-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="9183b-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="9183b-149">Yes</span><span class="sxs-lookup"><span data-stu-id="9183b-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="9183b-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="9183b-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="9183b-151">Nie</span><span class="sxs-lookup"><span data-stu-id="9183b-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="9183b-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="9183b-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="9183b-153">Tak</span><span class="sxs-lookup"><span data-stu-id="9183b-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="9183b-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="9183b-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="9183b-155">Tak</span><span class="sxs-lookup"><span data-stu-id="9183b-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="9183b-156">Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="9183b-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="9183b-157">Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.</span><span class="sxs-lookup"><span data-stu-id="9183b-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="9183b-158">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="9183b-158">NavLink component</span></span>

<span data-ttu-id="9183b-159">Składnik NavLink zamiast HTML używany `<a>` elementów podczas tworzenia łączy nawigacji.</span><span class="sxs-lookup"><span data-stu-id="9183b-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="9183b-160">Składnik NavLink zachowuje się jak `<a>` elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="9183b-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="9183b-161">`active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.</span><span class="sxs-lookup"><span data-stu-id="9183b-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="9183b-162">Tworzy następujący składnik NavMenu [Bootstrap](https://getbootstrap.com/docs/) pasek nawigacyjny, który pokazuje, jak używać składników NavLink:</span><span class="sxs-lookup"><span data-stu-id="9183b-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="9183b-163">Istnieją dwa `NavLinkMatch` opcje:</span><span class="sxs-lookup"><span data-stu-id="9183b-163">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="9183b-164">`NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.</span><span class="sxs-lookup"><span data-stu-id="9183b-164">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="9183b-165">`NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.</span><span class="sxs-lookup"><span data-stu-id="9183b-165">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="9183b-166">W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS.</span><span class="sxs-lookup"><span data-stu-id="9183b-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="9183b-167">Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza składnika trasy Blazor (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="9183b-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
