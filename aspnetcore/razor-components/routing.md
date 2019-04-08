---
title: Składniki razor routingu
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: 8f917c94300bd21dde92d4533bee856adee98478
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068108"
---
# <a name="razor-components-routing"></a>Składniki razor routingu

Przez [Luke Latham](https://github.com/guardrex)

Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.

## <a name="aspnet-core-endpoint-routing-integration"></a>Integracja routingu platformy ASP.NET Core punktu końcowego

Składniki razor są zintegrowane z [platformy ASP.NET Core routingu](xref:fundamentals/routing). Aplikacji ASP.NET Core jest skonfigurowany do akceptowania połączeń przychodzących, interaktywne składników Razor za pomocą `MapComponentHub<TComponent>` w `Startup.Configure`. `MapComponentHub` Określa, że składnik główny `App` powinien być renderowany w obrębie elementu DOM, dopasowania selektor `app`:

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a>Szablonów tras

`<Router>` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny. `<Router>` Składnika, który pojawia się w *Components/App.razor* pliku:

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

Gdy *.razor* lub *.cshtml* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> Określanie szablonu trasy. W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.

Wiele szablonów tras można zastosować do składnika. Następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` obsługuje ustawienia rezerwowe składnika renderowanie, gdy żądanej trasie nie zostanie rozwiązany. Włącz w tym scenariuszu uczestnictwo, ustawiając `FallbackComponent` parametru na typ klasy składnika rezerwowego.

W poniższym przykładzie ustawiono składnikiem, który został zdefiniowany w *Pages/MyFallbackRazorComponent.cshtml* jako rezerwowej składnik dla `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku ze ścieżki podstawowej aplikacji określony w `href` atrybutu (`<base href="/" />`). Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.

## <a name="route-parameters"></a>Parametry trasy

Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie. Pierwszy pozwala nawigacji do składnika bez parametrów. Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.

## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.

W poniższym przykładzie trasy do składnika użytkowników tylko pasuje, jeśli:

* `Id` Segmentu trasy jest obecny w adresie URL żądania.
* `Id` Segmentu jest liczbą całkowitą (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

Ograniczenia trasy, pokazano w poniższej tabeli są dostępne. Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.

| Ograniczenia | Przykład           | Przykład dopasowań                                                                  | Niezmiennej<br>kultura<br>parowanie |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Nie                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Tak                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Tak                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Tak                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Tak                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Nie                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Yes                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Yes                              |

> [!WARNING]
> Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury. Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.

## <a name="navlink-component"></a>Składnik NavLink

Składnik NavLink zamiast HTML używany `<a>` elementów podczas tworzenia łączy nawigacji. Składnik NavLink zachowuje się jak `<a>` elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL. `active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.

Tworzy następujący składnik NavMenu [Bootstrap](https://getbootstrap.com/docs/) pasek nawigacyjny, który pokazuje, jak używać składników NavLink:

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

Istnieją dwa `NavLinkMatch` opcje:

* `NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.
* `NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.

W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS. Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza składnika trasy Blazor (`href="BlazorRoute"`).
