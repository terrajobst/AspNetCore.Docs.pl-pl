---
title: ASP.NET Core Blazor routingu
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/26/2019
uid: blazor/routing
ms.openlocfilehash: ddbb43f897decc94218ad950ef8dda6ea153d0d3
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406083"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="4d509-103">ASP.NET Core Blazor routingu</span><span class="sxs-lookup"><span data-stu-id="4d509-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="4d509-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4d509-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4d509-105">Dowiedz się, jak kierować żądania w aplikacjach i o `NavLink` składnika.</span><span class="sxs-lookup"><span data-stu-id="4d509-105">Learn how to route requests in apps and about the `NavLink` component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="4d509-106">Integracja routingu platformy ASP.NET Core punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="4d509-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="4d509-107">Blazor po stronie serwera jest zintegrowany z [routingu do Endpoint platformy ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="4d509-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="4d509-108">Aplikacji ASP.NET Core jest skonfigurowany do akceptowania połączeń przychodzących do interaktywnego składników za pomocą `MapBlazorHub` w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4d509-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="4d509-109">Szablonów tras</span><span class="sxs-lookup"><span data-stu-id="4d509-109">Route templates</span></span>

<span data-ttu-id="4d509-110">`Router` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny.</span><span class="sxs-lookup"><span data-stu-id="4d509-110">The `Router` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="4d509-111">`Router` Składnika, który pojawia się w *App.razor* pliku:</span><span class="sxs-lookup"><span data-stu-id="4d509-111">The `Router` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="4d509-112">W aplikacji po stronie serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="4d509-112">In a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="4d509-113">W aplikacji po stronie klienta Blazor:</span><span class="sxs-lookup"><span data-stu-id="4d509-113">In a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="4d509-114">Gdy *.razor* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa znajduje się <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="4d509-114">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="4d509-115">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje składnika za pomocą szablonu trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="4d509-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="4d509-116">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="4d509-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="4d509-117">Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="4d509-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="4d509-118">Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku (Blazor po stronie klienta) lub *Pages/_Host.cshtml* pliku (Blazor po stronie serwera) przy użyciu ścieżki podstawowej aplikacji określonych w `href` atrybutu (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="4d509-118">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="4d509-119">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="4d509-119">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="4d509-120">Podaj niestandardową zawartość, gdy zawartość nie zostanie znalezione</span><span class="sxs-lookup"><span data-stu-id="4d509-120">Provide custom content when content isn't found</span></span>

<span data-ttu-id="4d509-121">`Router` Składnik umożliwia aplikacji określić niestandardową zawartość, jeśli zawartość nie zostanie znalezione na trasie.</span><span class="sxs-lookup"><span data-stu-id="4d509-121">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="4d509-122">W *App.razor* pliku niestandardowej zawartości zestawu w `<NotFoundContent>` elementu `Router` składników:</span><span class="sxs-lookup"><span data-stu-id="4d509-122">In the *App.razor* file, set custom content in the `<NotFoundContent>` element of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

<span data-ttu-id="4d509-123">Zawartość `<NotFoundContent>` może zawierać dowolne elementy, takie jak inne składniki interaktywne.</span><span class="sxs-lookup"><span data-stu-id="4d509-123">The content of `<NotFoundContent>` can include arbitrary items, such as other interactive components.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="4d509-124">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="4d509-124">Route parameters</span></span>

<span data-ttu-id="4d509-125">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter):</span><span class="sxs-lookup"><span data-stu-id="4d509-125">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="4d509-126">Opcjonalne parametry nie są obsługiwane w przypadku aplikacji Blazor w programie ASP.NET Core 3.0 (wersja zapoznawcza).</span><span class="sxs-lookup"><span data-stu-id="4d509-126">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="4d509-127">Dwa `@page` dyrektywy są stosowane w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="4d509-127">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="4d509-128">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="4d509-128">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="4d509-129">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="4d509-129">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="4d509-130">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="4d509-130">Route constraints</span></span>

<span data-ttu-id="4d509-131">Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="4d509-131">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="4d509-132">W poniższym przykładzie trasy do `Users` składnik jest zgodny tylko, jeśli:</span><span class="sxs-lookup"><span data-stu-id="4d509-132">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="4d509-133">`Id` Segmentu trasy jest obecny w adresie URL żądania.</span><span class="sxs-lookup"><span data-stu-id="4d509-133">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="4d509-134">`Id` Segmentu jest liczbą całkowitą (`int`).</span><span class="sxs-lookup"><span data-stu-id="4d509-134">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="4d509-135">Ograniczenia trasy, pokazano w poniższej tabeli są dostępne.</span><span class="sxs-lookup"><span data-stu-id="4d509-135">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="4d509-136">Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4d509-136">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="4d509-137">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="4d509-137">Constraint</span></span> | <span data-ttu-id="4d509-138">Przykład</span><span class="sxs-lookup"><span data-stu-id="4d509-138">Example</span></span>           | <span data-ttu-id="4d509-139">Przykład dopasowań</span><span class="sxs-lookup"><span data-stu-id="4d509-139">Example Matches</span></span>                                                                  | <span data-ttu-id="4d509-140">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="4d509-140">Invariant</span></span><br><span data-ttu-id="4d509-141">kultura</span><span class="sxs-lookup"><span data-stu-id="4d509-141">culture</span></span><br><span data-ttu-id="4d509-142">parowanie</span><span class="sxs-lookup"><span data-stu-id="4d509-142">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="4d509-143">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="4d509-143">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="4d509-144">Nie</span><span class="sxs-lookup"><span data-stu-id="4d509-144">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="4d509-145">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="4d509-145">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="4d509-146">Tak</span><span class="sxs-lookup"><span data-stu-id="4d509-146">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="4d509-147">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="4d509-147">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="4d509-148">Yes</span><span class="sxs-lookup"><span data-stu-id="4d509-148">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="4d509-149">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="4d509-149">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="4d509-150">Tak</span><span class="sxs-lookup"><span data-stu-id="4d509-150">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="4d509-151">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="4d509-151">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="4d509-152">Tak</span><span class="sxs-lookup"><span data-stu-id="4d509-152">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="4d509-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="4d509-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="4d509-154">Nie</span><span class="sxs-lookup"><span data-stu-id="4d509-154">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="4d509-155">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="4d509-155">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="4d509-156">Tak</span><span class="sxs-lookup"><span data-stu-id="4d509-156">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="4d509-157">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="4d509-157">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="4d509-158">Tak</span><span class="sxs-lookup"><span data-stu-id="4d509-158">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="4d509-159">Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="4d509-159">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="4d509-160">Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.</span><span class="sxs-lookup"><span data-stu-id="4d509-160">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="4d509-161">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="4d509-161">NavLink component</span></span>

<span data-ttu-id="4d509-162">Użyj `NavLink` składnika zamiast HTML `<a>` elementów podczas tworzenia łączy nawigacji.</span><span class="sxs-lookup"><span data-stu-id="4d509-162">Use a `NavLink` component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="4d509-163">A `NavLink` składnik zachowuje się jak `<a>` elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="4d509-163">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="4d509-164">`active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.</span><span class="sxs-lookup"><span data-stu-id="4d509-164">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="4d509-165">Następujące `NavMenu` składnik tworzy [Bootstrap](https://getbootstrap.com/docs/) pasek nawigacyjny, który demonstruje sposób skorzystania `NavLink` składników:</span><span class="sxs-lookup"><span data-stu-id="4d509-165">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.razor?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="4d509-166">Istnieją dwa `NavLinkMatch` opcje:</span><span class="sxs-lookup"><span data-stu-id="4d509-166">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="4d509-167">`NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.</span><span class="sxs-lookup"><span data-stu-id="4d509-167">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="4d509-168">`NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.</span><span class="sxs-lookup"><span data-stu-id="4d509-168">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="4d509-169">W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS.</span><span class="sxs-lookup"><span data-stu-id="4d509-169">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="4d509-170">Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza `BlazorRoute` składnika (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="4d509-170">The second NavLink only receives the `active` class when the user visits the `BlazorRoute` component (`href="BlazorRoute"`).</span></span>

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="4d509-171">Identyfikator URI i nawigacji pomocników stanu</span><span class="sxs-lookup"><span data-stu-id="4d509-171">URI and navigation state helpers</span></span>

<span data-ttu-id="4d509-172">Użyj `Microsoft.AspNetCore.Components.IUriHelper` do pracy z identyfikatorów URI i nawigacja w C# kodu.</span><span class="sxs-lookup"><span data-stu-id="4d509-172">Use `Microsoft.AspNetCore.Components.IUriHelper` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="4d509-173">`IUriHelper` zapewnia zdarzenia i metody, które przedstawiono w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="4d509-173">`IUriHelper` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="4d509-174">Element członkowski</span><span class="sxs-lookup"><span data-stu-id="4d509-174">Member</span></span> | <span data-ttu-id="4d509-175">Opis</span><span class="sxs-lookup"><span data-stu-id="4d509-175">Description</span></span> |
| ------ | ----------- |
| `GetAbsoluteUri` | <span data-ttu-id="4d509-176">Pobiera bieżący bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="4d509-176">Gets the current absolute URI.</span></span> |
| `GetBaseUri` | <span data-ttu-id="4d509-177">Pobiera podstawowy identyfikator URI (z ukośnikiem końcowym), może zostać dołączony do ścieżek względnych identyfikatora URI, aby utworzyć bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="4d509-177">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="4d509-178">Zazwyczaj `GetBaseUri` odnosi się do `href` atrybutu w dokumencie `<base>` element *wwwroot/index.html* (Blazor po stronie klienta) lub *Pages/_Host.cshtml* () Blazor po stronie serwera).</span><span class="sxs-lookup"><span data-stu-id="4d509-178">Typically, `GetBaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="4d509-179">Powoduje przejście do określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="4d509-179">Navigates to the specified URI.</span></span> <span data-ttu-id="4d509-180">Jeśli `forceLoad` jest `true`:</span><span class="sxs-lookup"><span data-stu-id="4d509-180">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="4d509-181">Routing po stronie klienta jest pomijany.</span><span class="sxs-lookup"><span data-stu-id="4d509-181">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="4d509-182">Przeglądarki jest zmuszony do ładowania nowej strony z serwera, czy identyfikator URI zwykle odbywa się przez router po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="4d509-182">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `OnLocationChanged` | <span data-ttu-id="4d509-183">Zdarzenie, które są generowane, gdy lokalizacja nawigacji została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="4d509-183">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="4d509-184">Konwertuje względny identyfikator URI na bezwzględny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="4d509-184">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="4d509-185">Biorąc pod uwagę podstawowy identyfikator URI (na przykład identyfikator URI wcześniej zwracany przez `GetBaseUri`), konwertuje bezwzględny identyfikator URI identyfikatora URI, względem podstawowego prefiks identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="4d509-185">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="4d509-186">Następujący składnik przechodzi do składnika licznika aplikacji po wybraniu przycisku:</span><span class="sxs-lookup"><span data-stu-id="4d509-186">The following component navigates to the app's Counter component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="@NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```
