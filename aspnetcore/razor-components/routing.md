---
title: Składniki razor routingu
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/01/2019
uid: razor-components/routing
ms.openlocfilehash: 7ea7be33cf271ba1e58e74f1cf58dae998370575
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668128"
---
# <a name="razor-components-routing"></a><span data-ttu-id="c7443-103">Składniki razor routingu</span><span class="sxs-lookup"><span data-stu-id="c7443-103">Razor Components routing</span></span>

<span data-ttu-id="c7443-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c7443-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="c7443-105">Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.</span><span class="sxs-lookup"><span data-stu-id="c7443-105">Learn how to route requests in apps and about the NavLink component.</span></span>

<span data-ttu-id="c7443-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c7443-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c7443-107">Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.</span><span class="sxs-lookup"><span data-stu-id="c7443-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="route-templates"></a><span data-ttu-id="c7443-108">Szablonów tras</span><span class="sxs-lookup"><span data-stu-id="c7443-108">Route templates</span></span>

<span data-ttu-id="c7443-109">`<Router>` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny.</span><span class="sxs-lookup"><span data-stu-id="c7443-109">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="c7443-110">`<Router>` Składnika, który pojawia się w *App.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="c7443-110">The `<Router>` component appears in the *App.cshtml* file:</span></span>

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

<span data-ttu-id="c7443-111">Gdy  *\*.cshtml* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) Określanie szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="c7443-111">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="c7443-112">W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c7443-112">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="c7443-113">Wiele szablonów tras można zastosować do składnika.</span><span class="sxs-lookup"><span data-stu-id="c7443-113">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="c7443-114">W [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="c7443-114">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), the following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="c7443-115">*Pages/BlazorRoute.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7443-115">*Pages/BlazorRoute.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

<span data-ttu-id="c7443-116">`<Router>` obsługuje ustawienia rezerwowe składnika renderowanie, gdy żądanej trasie nie zostanie rozwiązany.</span><span class="sxs-lookup"><span data-stu-id="c7443-116">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="c7443-117">Włącz w tym scenariuszu uczestnictwo, ustawiając `FallbackComponent` parametru na typ klasy składnika rezerwowego.</span><span class="sxs-lookup"><span data-stu-id="c7443-117">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="c7443-118">W poniższym przykładzie ustawiono składnikiem, który został zdefiniowany w *Pages/MyFallbackRazorComponent.cshtml* jako rezerwowej składnik dla `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="c7443-118">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="c7443-119">Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku ze ścieżki podstawowej aplikacji określony w `href` atrybutu (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="c7443-119">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="c7443-120">Aby uzyskać więcej informacji, zobacz [hosta i wdrażania: Podstawowa ścieżka aplikacji](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="c7443-120">For more information, see [Host and deploy: App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="route-parameters"></a><span data-ttu-id="c7443-121">Parametry trasy</span><span class="sxs-lookup"><span data-stu-id="c7443-121">Route parameters</span></span>

<span data-ttu-id="c7443-122">Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter).</span><span class="sxs-lookup"><span data-stu-id="c7443-122">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive).</span></span>

<span data-ttu-id="c7443-123">*Pages/RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c7443-123">*Pages/RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

<span data-ttu-id="c7443-124">Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c7443-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="c7443-125">Pierwszy pozwala nawigacji do składnika bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="c7443-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="c7443-126">Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c7443-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="c7443-127">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="c7443-127">Route constraints</span></span>

<span data-ttu-id="c7443-128">Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.</span><span class="sxs-lookup"><span data-stu-id="c7443-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="c7443-129">W poniższym przykładzie trasy do składnika użytkowników tylko pasuje, jeśli:</span><span class="sxs-lookup"><span data-stu-id="c7443-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="c7443-130">`Id` Segmentu trasy jest obecny w adresie URL żądania.</span><span class="sxs-lookup"><span data-stu-id="c7443-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="c7443-131">`Id` Segmentu jest liczbą całkowitą (`int`).</span><span class="sxs-lookup"><span data-stu-id="c7443-131">The `Id` segment is an integer (`int`).</span></span>

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

<span data-ttu-id="c7443-132">Ograniczenia trasy, pokazano w poniższej tabeli są dostępne do użycia.</span><span class="sxs-lookup"><span data-stu-id="c7443-132">The route constraints shown in the following table are available for use.</span></span> <span data-ttu-id="c7443-133">Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c7443-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="c7443-134">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="c7443-134">Constraint</span></span> | <span data-ttu-id="c7443-135">Przykład</span><span class="sxs-lookup"><span data-stu-id="c7443-135">Example</span></span>           | <span data-ttu-id="c7443-136">Przykład dopasowań</span><span class="sxs-lookup"><span data-stu-id="c7443-136">Example Matches</span></span>                                                                  | <span data-ttu-id="c7443-137">Niezmiennej</span><span class="sxs-lookup"><span data-stu-id="c7443-137">Invariant</span></span><br><span data-ttu-id="c7443-138">kultura</span><span class="sxs-lookup"><span data-stu-id="c7443-138">culture</span></span><br><span data-ttu-id="c7443-139">parowanie</span><span class="sxs-lookup"><span data-stu-id="c7443-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="c7443-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="c7443-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="c7443-141">Nie</span><span class="sxs-lookup"><span data-stu-id="c7443-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="c7443-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="c7443-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="c7443-143">Tak</span><span class="sxs-lookup"><span data-stu-id="c7443-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="c7443-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="c7443-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="c7443-145">Tak</span><span class="sxs-lookup"><span data-stu-id="c7443-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="c7443-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="c7443-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="c7443-147">Tak</span><span class="sxs-lookup"><span data-stu-id="c7443-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="c7443-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="c7443-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="c7443-149">Tak</span><span class="sxs-lookup"><span data-stu-id="c7443-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="c7443-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="c7443-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="c7443-151">Nie</span><span class="sxs-lookup"><span data-stu-id="c7443-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="c7443-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="c7443-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="c7443-153">Tak</span><span class="sxs-lookup"><span data-stu-id="c7443-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="c7443-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="c7443-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="c7443-155">Tak</span><span class="sxs-lookup"><span data-stu-id="c7443-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="c7443-156">Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury.</span><span class="sxs-lookup"><span data-stu-id="c7443-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="c7443-157">Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.</span><span class="sxs-lookup"><span data-stu-id="c7443-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="c7443-158">Składnik NavLink</span><span class="sxs-lookup"><span data-stu-id="c7443-158">NavLink component</span></span>

<span data-ttu-id="c7443-159">Składnik NavLink zamiast HTML używany  **\<>** elementów podczas tworzenia łączy nawigacji.</span><span class="sxs-lookup"><span data-stu-id="c7443-159">Use a NavLink component in place of HTML **\<a>** elements when creating navigation links.</span></span> <span data-ttu-id="c7443-160">Składnik NavLink zachowuje się jak  **\<>** elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="c7443-160">A NavLink component behaves like an **\<a>** element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="c7443-161">`active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.</span><span class="sxs-lookup"><span data-stu-id="c7443-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="c7443-162">Składnik NavMenu [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) tworzy [Bootstrap](https://getbootstrap.com/docs/) paska nawigacji, który demonstruje sposób używania składników NavLink.</span><span class="sxs-lookup"><span data-stu-id="c7443-162">The NavMenu component in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) creates a [Bootstrap](https://getbootstrap.com/docs/) nav bar that demonstrates how to use NavLink components.</span></span> <span data-ttu-id="c7443-163">Następujący kod przedstawia dwa pierwsze NavLinks w *Shared/NavMenu.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="c7443-163">The following markup shows the first two NavLinks in the *Shared/NavMenu.cshtml* file.</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

<span data-ttu-id="c7443-164">Istnieją dwa `NavLinkMatch` opcje:</span><span class="sxs-lookup"><span data-stu-id="c7443-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="c7443-165">`NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.</span><span class="sxs-lookup"><span data-stu-id="c7443-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="c7443-166">`NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.</span><span class="sxs-lookup"><span data-stu-id="c7443-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="c7443-167">W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS.</span><span class="sxs-lookup"><span data-stu-id="c7443-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="c7443-168">Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza składnika BlazorRoute (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="c7443-168">The second NavLink only receives the `active` class when the user visits the BlazorRoute component (`href="BlazorRoute"`).</span></span>
