---
title: Blazor routing
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/06/2019
uid: blazor/routing
ms.openlocfilehash: fc61b8998682d519f7b936d95645c6311ffa5c09
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086135"
---
# <a name="blazor-routing"></a><span data-ttu-id="58e0b-103">Blazor routing</span><span class="sxs-lookup"><span data-stu-id="58e0b-103">Blazor routing</span></span>

<span data-ttu-id="58e0b-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="58e0b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="58e0b-105">Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.</span><span class="sxs-lookup"><span data-stu-id="58e0b-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="58e0b-106">Integracja routingu platformy ASP.NET Core punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="58e0b-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="58e0b-107">Blazor po stronie serwera jest zintegrowany z [routingu do Endpoint platformy ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="58e0b-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="58e0b-108">Aplikacji ASP.NET Core jest skonfigurowany do akceptowania połączeń przychodzących do interaktywnego składników za pomocą `MapBlazorHub` w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="58e0b-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="58e0b-109">Szablonów tras</span><span class="sxs-lookup"><span data-stu-id="58e0b-109">Route templates</span></span>

<span data-ttu-id="58e0b-110">`<Router>` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny.</span><span class="sxs-lookup"><span data-stu-id="58e0b-110">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="58e0b-111">`<Router>` Składnika, który pojawia się w *App.razor* pliku:</span><span class="sxs-lookup"><span data-stu-id="58e0b-111">The `<Router>` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="58e0b-112">W aplikacji po stronie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="58e0b-112">In a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="58e0b-113">W aplikacji po stronie klienta Blazor:</span><span class="sxs-lookup"><span data-stu-id="58e0b-113">In a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="58e0b-114">Gdy *.razor* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa znajduje się <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="58e0b-114">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="58e0b-115">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje składnika za pomocą szablonu trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="58e0b-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="58e0b-116">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="58e0b-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="58e0b-117">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="58e0b-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

<span data-ttu-id="58e0b-118">`<Router>` obsługuje ustawienia rezerwowe składnik do renderowania po żądanej trasie nie zostanie rozwiązany.</span><span class="sxs-lookup"><span data-stu-id="58e0b-118">`<Router>` supports setting a fallback component to render when a requested route isn't resolved.</span></span> <span data-ttu-id="58e0b-119">Włącz w tym scenariuszu uczestnictwo, ustawiając `FallbackComponent` parametru na typ klasy składnika rezerwowego.</span><span class="sxs-lookup"><span data-stu-id="58e0b-119">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="58e0b-120">W poniższym przykładzie ustawiono składnikiem, który został zdefiniowany w *Pages/MyFallbackRazorComponent.razor* jako rezerwowej składnik dla `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="58e0b-120">The following example sets a component defined in *Pages/MyFallbackRazorComponent.razor* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="58e0b-121">Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku ze ścieżki podstawowej aplikacji określony w `href` atrybutu (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="58e0b-121">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="58e0b-122">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="58e0b-122">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="58e0b-123">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="58e0b-123">Route parameters</span></span>

<span data-ttu-id="58e0b-124">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter):</span><span class="sxs-lookup"><span data-stu-id="58e0b-124">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="58e0b-125">Opcjonalne parametry nie są obsługiwane w przypadku aplikacji Blazor w programie ASP.NET Core 3.0 (wersja zapoznawcza).</span><span class="sxs-lookup"><span data-stu-id="58e0b-125">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="58e0b-126">Dwa `@page` dyrektywy są stosowane w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="58e0b-126">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="58e0b-127">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="58e0b-127">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="58e0b-128">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="58e0b-128">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="58e0b-129">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="58e0b-129">Route constraints</span></span>

<span data-ttu-id="58e0b-130">Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="58e0b-130">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="58e0b-131">W poniższym przykładzie trasy do składnika użytkowników tylko pasuje, jeśli:</span><span class="sxs-lookup"><span data-stu-id="58e0b-131">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="58e0b-132">`Id` Segmentu trasy jest obecny w adresie URL żądania.</span><span class="sxs-lookup"><span data-stu-id="58e0b-132">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="58e0b-133">`Id` Segmentu jest liczbą całkowitą (`int`).</span><span class="sxs-lookup"><span data-stu-id="58e0b-133">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="58e0b-134">Ograniczenia trasy, pokazano w poniższej tabeli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="58e0b-134">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="58e0b-135">Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="58e0b-135">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="58e0b-136">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="58e0b-136">Constraint</span></span> | <span data-ttu-id="58e0b-137">Przykład</span><span class="sxs-lookup"><span data-stu-id="58e0b-137">Example</span></span>           | <span data-ttu-id="58e0b-138">Przykład dopasowań</span><span class="sxs-lookup"><span data-stu-id="58e0b-138">Example Matches</span></span>                                                                  | <span data-ttu-id="58e0b-139">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="58e0b-139">Invariant</span></span><br><span data-ttu-id="58e0b-140">kultura</span><span class="sxs-lookup"><span data-stu-id="58e0b-140">culture</span></span><br><span data-ttu-id="58e0b-141">parowanie</span><span class="sxs-lookup"><span data-stu-id="58e0b-141">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="58e0b-142">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="58e0b-142">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="58e0b-143">Nie</span><span class="sxs-lookup"><span data-stu-id="58e0b-143">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="58e0b-144">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="58e0b-144">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="58e0b-145">Tak</span><span class="sxs-lookup"><span data-stu-id="58e0b-145">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="58e0b-146">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="58e0b-146">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="58e0b-147">Tak</span><span class="sxs-lookup"><span data-stu-id="58e0b-147">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="58e0b-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="58e0b-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="58e0b-149">Tak</span><span class="sxs-lookup"><span data-stu-id="58e0b-149">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="58e0b-150">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="58e0b-150">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="58e0b-151">Tak</span><span class="sxs-lookup"><span data-stu-id="58e0b-151">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="58e0b-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="58e0b-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="58e0b-153">Nie</span><span class="sxs-lookup"><span data-stu-id="58e0b-153">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="58e0b-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="58e0b-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="58e0b-155">Tak</span><span class="sxs-lookup"><span data-stu-id="58e0b-155">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="58e0b-156">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="58e0b-156">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="58e0b-157">Tak</span><span class="sxs-lookup"><span data-stu-id="58e0b-157">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="58e0b-158">Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="58e0b-158">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="58e0b-159">Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.</span><span class="sxs-lookup"><span data-stu-id="58e0b-159">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="58e0b-160">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="58e0b-160">NavLink component</span></span>

<span data-ttu-id="58e0b-161">Składnik NavLink zamiast HTML używany `<a>` elementów podczas tworzenia łączy nawigacji.</span><span class="sxs-lookup"><span data-stu-id="58e0b-161">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="58e0b-162">Składnik NavLink zachowuje się jak `<a>` elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="58e0b-162">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="58e0b-163">`active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.</span><span class="sxs-lookup"><span data-stu-id="58e0b-163">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="58e0b-164">Tworzy następujący składnik NavMenu [Bootstrap](https://getbootstrap.com/docs/) pasek nawigacyjny, który pokazuje, jak używać składników NavLink:</span><span class="sxs-lookup"><span data-stu-id="58e0b-164">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.razor?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="58e0b-165">Istnieją dwa `NavLinkMatch` opcje:</span><span class="sxs-lookup"><span data-stu-id="58e0b-165">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="58e0b-166">`NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.</span><span class="sxs-lookup"><span data-stu-id="58e0b-166">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="58e0b-167">`NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.</span><span class="sxs-lookup"><span data-stu-id="58e0b-167">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="58e0b-168">W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS.</span><span class="sxs-lookup"><span data-stu-id="58e0b-168">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="58e0b-169">Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza składnika trasy Blazor (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="58e0b-169">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
