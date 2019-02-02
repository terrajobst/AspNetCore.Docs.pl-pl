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
# <a name="razor-components-routing"></a>Składniki razor routingu

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)). Zobacz [wprowadzenie](xref:razor-components/get-started) tematu wstępnie wymaganych składników.

## <a name="route-templates"></a>Szablonów tras

`<Router>` Składnika umożliwia routing i szablon trasy znajduje się na każdej części dostępny. `<Router>` Składnika, który pojawia się w *App.cshtml* pliku:

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

Gdy  *\*.cshtml* plik z `@page` dyrektywa jest kompilowany, wygenerowana klasa otrzymuje [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) Określanie szablonu trasy. W czasie wykonywania, router szuka klasy składników za pomocą `RouteAttribute` i renderuje, niezależnie od składnik ma szablon trasy, który pasuje do żądanego adresu URL.

Wiele szablonów tras można zastosować do składnika. W [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), następujący składnik, który będzie odpowiadał na żądania `/BlazorRoute` i `/DifferentBlazorRoute`.

*Pages/BlazorRoute.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

`<Router>` obsługuje ustawienia rezerwowe składnika renderowanie, gdy żądanej trasie nie zostanie rozwiązany. Włącz w tym scenariuszu uczestnictwo, ustawiając `FallbackComponent` parametru na typ klasy składnika rezerwowego.

W poniższym przykładzie ustawiono składnikiem, który został zdefiniowany w *Pages/MyFallbackRazorComponent.cshtml* jako rezerwowej składnik dla `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Aby prawidłowo wygenerować trasy, aplikacja musi zawierać `<base>` tagów w jego *wwwroot/index.html* pliku ze ścieżki podstawowej aplikacji określony w `href` atrybutu (`<base href="/" />`). Aby uzyskać więcej informacji, zobacz [hosta i wdrażania: Podstawowa ścieżka aplikacji](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="route-parameters"></a>Parametry trasy

Router używa parametrów trasy do wypełnienia odpowiednich parametrów składnika o takiej samej nazwie (wielkich liter).

*Pages/RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

Opcjonalne parametry nie są obsługiwane, dlatego dwie `@page` dyrektywy są stosowane w powyższym przykładzie. Pierwszy pozwala nawigacji do składnika bez parametrów. Drugi `@page` przyjmuje dyrektywy `{text}` kierowanie parametru, a następnie przypisuje wartość do `Text` właściwości.

## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenia trasy wymusza typ zgodny w segmencie trasy do składnika.

W poniższym przykładzie trasy do składnika użytkowników tylko pasuje, jeśli:

* `Id` Segmentu trasy jest obecny w adresie URL żądania.
* `Id` Segmentu jest liczbą całkowitą (`int`).

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

Ograniczenia trasy, pokazano w poniższej tabeli są dostępne do użycia. Dla ograniczenia trasy, które odpowiadają przy użyciu niezmiennej kultury Zobacz ostrzeżenie w poniższej tabeli, aby uzyskać więcej informacji.

| Ograniczenia | Przykład           | Przykład dopasowań                                                                  | Niezmiennej<br>kultura<br>parowanie |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Nie                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Tak                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Tak                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Tak                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Tak                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Nie                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Tak                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Tak                              |

> [!WARNING]
> Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury. Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania.

## <a name="navlink-component"></a>Składnik NavLink

Składnik NavLink zamiast HTML używany  **\<>** elementów podczas tworzenia łączy nawigacji. Składnik NavLink zachowuje się jak  **\<>** elementu, z wyjątkiem go Włącza/wyłącza `active` klasy CSS na ich podstawie jego `href` zgodny z bieżącym adresem URL. `active` Pomaga użytkownikom zrozumieć stronę, która jest stroną aktywną między łącza nawigacji wyświetlane, klasy.

Składnik NavMenu [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) tworzy [Bootstrap](https://getbootstrap.com/docs/) paska nawigacji, który demonstruje sposób używania składników NavLink. Następujący kod przedstawia dwa pierwsze NavLinks w *Shared/NavMenu.cshtml* pliku.

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

Istnieją dwa `NavLinkMatch` opcje:

* `NavLinkMatch.All` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z bieżącą cały adres URL.
* `NavLinkMatch.Prefix` &ndash; Określa, że NavLink powinien być aktywny, gdy są one zgodne z dowolnego prefiksu bieżący adres URL.

W powyższym przykładzie Home NavLink (`href=""`) dopasowuje wszystkie adresy URL i zawsze będzie otrzymywał `active` klasę CSS. Drugi NavLink tylko odbiera `active` klasy, gdy użytkownik odwiedza składnika BlazorRoute (`href="BlazorRoute"`).
