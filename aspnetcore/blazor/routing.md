---
title: Blazor routing
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: blazor/routing
ms.openlocfilehash: b2a703a8dda87812fce1e52b165ad6de901393a7
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034698"
---
# <a name="blazor-routing"></a>Blazor routing

Przez [Luke Latham](https://github.com/guardrex)

Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.

## <a name="aspnet-core-endpoint-routing-integration"></a>Integracja routingu platformy ASP.NET Core punktu końcowego

Blazor po stronie serwera jest zintegrowany z [routingu do Endpoint platformy ASP.NET Core](xref:fundamentals/routing). Aplikacji ASP.NET Core jest skonfigurowany do akceptowania połączeń przychodzących do interaktywnego składników za pomocą `MapBlazorHub` w `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a>Szablonów tras

`<Router>` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny. `<Router>` Składnika, który pojawia się w *App.razor* pliku:

W aplikacji po stronie serwera Blazor:

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

W aplikacji po stronie klienta Blazor:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Gdy *.razor* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa znajduje się <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy. W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje składnika za pomocą szablonu trasy, który pasuje do żądanego adresu URL.

Wiele szablonów tras można zastosować do składnika. Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

`<Router>` obsługuje ustawienia rezerwowe składnik do renderowania po żądanej trasie nie zostanie rozwiązany. Włącz w tym scenariuszu uczestnictwo, ustawiając `FallbackComponent` parametru na typ klasy składnika rezerwowego.

W poniższym przykładzie ustawiono składnikiem, który został zdefiniowany w *Pages/MyFallbackRazorComponent.razor* jako rezerwowej składnik dla `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku (Blazor po stronie klienta) lub *stron /\_Host.cshtml* pliku (Blazor po stronie serwera) z ścieżki podstawowej aplikacji określony w `href` atrybutu (`<base href="/">`). Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/client-side#app-base-path>.

## <a name="route-parameters"></a>Parametry trasy

Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Opcjonalne parametry nie są obsługiwane w przypadku aplikacji Blazor w programie ASP.NET Core 3.0 (wersja zapoznawcza). Dwa `@page` dyrektywy są stosowane w poprzednim przykładzie. Pierwszy pozwala nawigacji do składnika bez parametrów. Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.

## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.

W poniższym przykładzie trasy do składnika użytkowników tylko pasuje, jeśli:

* `Id` Segmentu trasy jest obecny w adresie URL żądania.
* `Id` Segmentu jest liczbą całkowitą (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Ograniczenia trasy, pokazano w poniższej tabeli są dostępne. Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.

| Ograniczenia | Przykład           | Przykład dopasowań                                                                  | Niezmiennej<br>kultura<br>parowanie |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Nie                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Tak                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Tak                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Yes                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Tak                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Nie                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Tak                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Tak                              |

> [!WARNING]
> Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury. Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.

## <a name="navlink-component"></a>Składnik NavLink

Składnik NavLink zamiast HTML używany `<a>` elementów podczas tworzenia łączy nawigacji. Składnik NavLink zachowuje się jak `<a>` elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL. `active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.

Tworzy następujący składnik NavMenu [Bootstrap](https://getbootstrap.com/docs/) pasek nawigacyjny, który pokazuje, jak używać składników NavLink:

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.razor?name=snippet_NavLinks&highlight=4-6,9-11)]

Istnieją dwa `NavLinkMatch` opcje:

* `NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.
* `NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.

W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS. Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza składnika trasy Blazor (`href="BlazorRoute"`).

## <a name="uri-and-navigation-state-helpers"></a>Identyfikator URI i nawigacji pomocników stanu

Użyj `Microsoft.AspNetCore.Components.IUriHelper` do pracy z identyfikatorów URI i nawigacja w C# kodu. `IUriHelper` zapewnia zdarzenia i metody, które przedstawiono w poniższej tabeli.

| Element członkowski | Opis |
| ------ | ----------- |
| `GetAbsoluteUri` | Pobiera bieżący bezwzględny identyfikator URI. |
| `GetBaseUri` | Pobiera podstawowy identyfikator URI (z ukośnikiem końcowym), może zostać dołączony do ścieżek względnych identyfikatora URI, aby utworzyć bezwzględny identyfikator URI. Zazwyczaj `GetBaseUri` odpowiada `href` atrybutu do dokumentu `<base>` element *wwwroot/index.html* (Blazor po stronie klienta) lub *stron /\_Host.cshtml* (Blazor po stronie serwera). |
| `NavigateTo` | Powoduje przejście do określonego identyfikatora URI. Jeśli `forceLoad` jest `true`:<ul><li>Routing po stronie klienta jest pomijany.</li><li>Przeglądarki jest zmuszony do ładowania nowej strony z serwera, czy identyfikator URI zwykle odbywa się przez router po stronie klienta.</li></ul> |
| `OnLocationChanged` | Zdarzenie, które są generowane, gdy lokalizacja nawigacji została zmieniona. |
| `ToAbsoluteUri` | Konwertuje względny identyfikator URI na bezwzględny identyfikator URI. |
| `ToBaseRelativePath` | Biorąc pod uwagę podstawowy identyfikator URI (na przykład identyfikator URI wcześniej zwracany przez `GetBaseUri`), konwertuje bezwzględny identyfikator URI identyfikatora URI, względem podstawowego prefiks identyfikatora URI. |

Następujący składnik przechodzi do składnika licznika aplikacji po wybraniu przycisku:

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
