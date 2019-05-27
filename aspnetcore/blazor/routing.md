---
title: Blazor routing
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: blazor/routing
ms.openlocfilehash: b7f040292484f77c3cd12d9a0c07019782597882
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223126"
---
# <a name="blazor-routing"></a><span data-ttu-id="1c8a2-103">Blazor routing</span><span class="sxs-lookup"><span data-stu-id="1c8a2-103">Blazor routing</span></span>

<span data-ttu-id="1c8a2-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1c8a2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1c8a2-105">Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="1c8a2-106">Integracja routingu platformy ASP.NET Core punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="1c8a2-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="1c8a2-107">Blazor po stronie serwera jest zintegrowany z [routingu do Endpoint platformy ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="1c8a2-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="1c8a2-108">Aplikacji ASP.NET Core jest skonfigurowany do akceptowania połączeń przychodzących do interaktywnego składników za pomocą `MapBlazorHub` w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="1c8a2-109">Szablonów tras</span><span class="sxs-lookup"><span data-stu-id="1c8a2-109">Route templates</span></span>

<span data-ttu-id="1c8a2-110">`<Router>` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-110">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="1c8a2-111">`<Router>` Składnika, który pojawia się w *App.razor* pliku:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-111">The `<Router>` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="1c8a2-112">W aplikacji po stronie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-112">In a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="1c8a2-113">W aplikacji po stronie klienta Blazor:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-113">In a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="1c8a2-114">Gdy *.razor* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa znajduje się <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-114">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="1c8a2-115">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje składnika za pomocą szablonu trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="1c8a2-116">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="1c8a2-117">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

<span data-ttu-id="1c8a2-118">`<Router>` obsługuje ustawienia rezerwowe składnik do renderowania po żądanej trasie nie zostanie rozwiązany.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-118">`<Router>` supports setting a fallback component to render when a requested route isn't resolved.</span></span> <span data-ttu-id="1c8a2-119">Włącz w tym scenariuszu uczestnictwo, ustawiając `FallbackComponent` parametru na typ klasy składnika rezerwowego.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-119">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="1c8a2-120">W poniższym przykładzie ustawiono składnikiem, który został zdefiniowany w *Pages/MyFallbackRazorComponent.razor* jako rezerwowej składnik dla `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-120">The following example sets a component defined in *Pages/MyFallbackRazorComponent.razor* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="1c8a2-121">Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku (Blazor po stronie klienta) lub *stron /\_Host.cshtml* pliku (Blazor po stronie serwera) z ścieżki podstawowej aplikacji określony w `href` atrybutu (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="1c8a2-121">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/\_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="1c8a2-122">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-122">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="1c8a2-123">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="1c8a2-123">Route parameters</span></span>

<span data-ttu-id="1c8a2-124">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter):</span><span class="sxs-lookup"><span data-stu-id="1c8a2-124">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="1c8a2-125">Opcjonalne parametry nie są obsługiwane w przypadku aplikacji Blazor w programie ASP.NET Core 3.0 (wersja zapoznawcza).</span><span class="sxs-lookup"><span data-stu-id="1c8a2-125">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="1c8a2-126">Dwa `@page` dyrektywy są stosowane w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-126">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="1c8a2-127">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-127">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="1c8a2-128">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-128">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="1c8a2-129">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="1c8a2-129">Route constraints</span></span>

<span data-ttu-id="1c8a2-130">Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-130">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="1c8a2-131">W poniższym przykładzie trasy do składnika użytkowników tylko pasuje, jeśli:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-131">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="1c8a2-132">`Id` Segmentu trasy jest obecny w adresie URL żądania.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-132">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="1c8a2-133">`Id` Segmentu jest liczbą całkowitą (`int`).</span><span class="sxs-lookup"><span data-stu-id="1c8a2-133">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="1c8a2-134">Ograniczenia trasy, pokazano w poniższej tabeli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-134">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="1c8a2-135">Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-135">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="1c8a2-136">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="1c8a2-136">Constraint</span></span> | <span data-ttu-id="1c8a2-137">Przykład</span><span class="sxs-lookup"><span data-stu-id="1c8a2-137">Example</span></span>           | <span data-ttu-id="1c8a2-138">Przykład dopasowań</span><span class="sxs-lookup"><span data-stu-id="1c8a2-138">Example Matches</span></span>                                                                  | <span data-ttu-id="1c8a2-139">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="1c8a2-139">Invariant</span></span><br><span data-ttu-id="1c8a2-140">kultura</span><span class="sxs-lookup"><span data-stu-id="1c8a2-140">culture</span></span><br><span data-ttu-id="1c8a2-141">parowanie</span><span class="sxs-lookup"><span data-stu-id="1c8a2-141">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="1c8a2-142">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-142">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="1c8a2-143">Nie</span><span class="sxs-lookup"><span data-stu-id="1c8a2-143">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="1c8a2-144">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-144">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="1c8a2-145">Tak</span><span class="sxs-lookup"><span data-stu-id="1c8a2-145">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="1c8a2-146">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-146">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="1c8a2-147">Yes</span><span class="sxs-lookup"><span data-stu-id="1c8a2-147">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="1c8a2-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="1c8a2-149">Tak</span><span class="sxs-lookup"><span data-stu-id="1c8a2-149">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="1c8a2-150">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-150">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="1c8a2-151">Yes</span><span class="sxs-lookup"><span data-stu-id="1c8a2-151">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="1c8a2-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="1c8a2-153">Nie</span><span class="sxs-lookup"><span data-stu-id="1c8a2-153">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="1c8a2-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="1c8a2-155">Yes</span><span class="sxs-lookup"><span data-stu-id="1c8a2-155">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="1c8a2-156">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="1c8a2-156">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="1c8a2-157">Yes</span><span class="sxs-lookup"><span data-stu-id="1c8a2-157">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="1c8a2-158">Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-158">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="1c8a2-159">Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-159">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="1c8a2-160">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="1c8a2-160">NavLink component</span></span>

<span data-ttu-id="1c8a2-161">Składnik NavLink zamiast HTML używany `<a>` elementów podczas tworzenia łączy nawigacji.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-161">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="1c8a2-162">Składnik NavLink zachowuje się jak `<a>` elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-162">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="1c8a2-163">`active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-163">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="1c8a2-164">Tworzy następujący składnik NavMenu [Bootstrap](https://getbootstrap.com/docs/) pasek nawigacyjny, który pokazuje, jak używać składników NavLink:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-164">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.razor?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="1c8a2-165">Istnieją dwa `NavLinkMatch` opcje:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-165">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="1c8a2-166">`NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-166">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="1c8a2-167">`NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-167">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="1c8a2-168">W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-168">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="1c8a2-169">Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza składnika trasy Blazor (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="1c8a2-169">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="1c8a2-170">Identyfikator URI i nawigacji pomocników stanu</span><span class="sxs-lookup"><span data-stu-id="1c8a2-170">URI and navigation state helpers</span></span>

<span data-ttu-id="1c8a2-171">Użyj `Microsoft.AspNetCore.Components.IUriHelper` do pracy z identyfikatorów URI i nawigacja w C# kodu.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-171">Use `Microsoft.AspNetCore.Components.IUriHelper` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="1c8a2-172">`IUriHelper` zapewnia zdarzenia i metody, które przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-172">`IUriHelper` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="1c8a2-173">Element członkowski</span><span class="sxs-lookup"><span data-stu-id="1c8a2-173">Member</span></span> | <span data-ttu-id="1c8a2-174">Opis</span><span class="sxs-lookup"><span data-stu-id="1c8a2-174">Description</span></span> |
| ------ | ----------- |
| `GetAbsoluteUri` | <span data-ttu-id="1c8a2-175">Pobiera bieżący bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-175">Gets the current absolute URI.</span></span> |
| `GetBaseUri` | <span data-ttu-id="1c8a2-176">Pobiera podstawowy identyfikator URI (z ukośnikiem końcowym), może zostać dołączony do ścieżek względnych identyfikatora URI, aby utworzyć bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-176">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="1c8a2-177">Zazwyczaj `GetBaseUri` odpowiada `href` atrybutu do dokumentu `<base>` element *wwwroot/index.html* (Blazor po stronie klienta) lub *stron /\_Host.cshtml* (Blazor po stronie serwera).</span><span class="sxs-lookup"><span data-stu-id="1c8a2-177">Typically, `GetBaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/\_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="1c8a2-178">Powoduje przejście do określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-178">Navigates to the specified URI.</span></span> <span data-ttu-id="1c8a2-179">Jeśli `forceLoad` jest `true`:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-179">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="1c8a2-180">Routing po stronie klienta jest pomijany.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-180">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="1c8a2-181">Przeglądarki jest zmuszony do ładowania nowej strony z serwera, czy identyfikator URI zwykle odbywa się przez router po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-181">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `OnLocationChanged` | <span data-ttu-id="1c8a2-182">Zdarzenie, które są generowane, gdy lokalizacja nawigacji została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-182">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="1c8a2-183">Konwertuje względny identyfikator URI na bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-183">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="1c8a2-184">Biorąc pod uwagę podstawowy identyfikator URI (na przykład identyfikator URI wcześniej zwracany przez `GetBaseUri`), konwertuje bezwzględny identyfikator URI identyfikatora URI, względem podstawowego prefiks identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="1c8a2-184">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="1c8a2-185">Następujący składnik przechodzi do składnika licznika aplikacji po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="1c8a2-185">The following component navigates to the app's Counter component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" onclick="@NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@functions {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```
