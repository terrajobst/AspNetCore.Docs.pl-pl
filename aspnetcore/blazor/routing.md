---
title: Routing Blazor ASP.NET Core
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/routing
ms.openlocfilehash: d4b76c00f79f333884fa7e30b27eadc6e36de287
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589941"
---
# <a name="aspnet-core-opno-locblazor-routing"></a>Routing Blazor ASP.NET Core

Autor [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Dowiedz się, jak kierować żądania i jak używać składnika `NavLink` do tworzenia linków nawigacji w aplikacjach Blazor.

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core integracja z routingiem punktu końcowego

Serwer Blazor jest zintegrowany z [ASP.NET Core routingiem punktu końcowego](xref:fundamentals/routing). Aplikacja ASP.NET Core jest skonfigurowana do akceptowania połączeń przychodzących dla składników interaktywnych z `MapBlazorHub` w `Startup.Configure`:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

Najbardziej typową konfiguracją jest kierowanie wszystkich żądań do strony Razor, która działa jako host dla części serwera Blazor aplikacji. Zgodnie z Konwencją strona *hosta* ma zwykle nazwę *_Host. cshtml*. Trasa określona w pliku hosta jest nazywana *trasą rezerwową* , ponieważ działa z niskim priorytetem w dopasowaniu tras. Trasa rezerwowa jest brana pod uwagę, gdy inne trasy nie są zgodne. Dzięki temu aplikacja może korzystać z innych kontrolerów i stron bez zakłócania działania aplikacji serwera Blazor.

## <a name="route-templates"></a>Szablony tras

Składnik `Router` umożliwia routing do każdego składnika z określoną trasą. Składnik `Router` pojawia się w pliku *App. Razor* :

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

Gdy zostanie skompilowany plik *Razor* z dyrektywą `@page`, wygenerowana klasa ma <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określający szablon trasy.

W środowisku uruchomieniowym składnik `RouteView`:

* Odbiera `RouteData` z `Router` wraz z dowolnymi żądanymi parametrami.
* Renderuje określony składnik za pomocą układu (lub opcjonalnego układu domyślnego) przy użyciu określonych parametrów.

Opcjonalnie można określić parametr `DefaultLayout` z klasą układu, która ma być używana dla składników, które nie określają układu. Domyślne szablony Blazor określają składnik `MainLayout`. *MainLayout. Razor* znajduje się w folderze *udostępnionym* projektu szablonu. Aby uzyskać więcej informacji na temat układów, zobacz <xref:blazor/layouts>.

Do składnika można zastosować wiele szablonów tras. Poniższy składnik odpowiada na żądania `/BlazorRoute` i `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Aby adresy URL zostały poprawnie rozpoznane, aplikacja musi zawierać tag `<base>` w pliku *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (serwer Blazor) z ścieżką bazową aplikacji określoną w atrybucie `href` (`<base href="/">`). Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Podaj zawartość niestandardową, jeśli nie można odnaleźć zawartości

Składnik `Router` umożliwia aplikacji określenie zawartości niestandardowej, jeśli nie można odnaleźć zawartości dla żądanej trasy.

W pliku *App. Razor* Ustaw zawartość niestandardową w parametrze szablonu `NotFound` składnika `Router`:

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

Zawartość tagów `<NotFound>` może zawierać dowolne elementy, takie jak inne składniki interaktywne. Aby zastosować domyślny układ do `NotFound` zawartości, zobacz <xref:blazor/layouts>.

## <a name="route-to-components-from-multiple-assemblies"></a>Kierowanie do składników z wielu zestawów

Użyj parametru `AdditionalAssemblies`, aby określić dodatkowe zestawy dla składnika `Router`, które mają być brane pod uwagę podczas wyszukiwania składników routingu. Określone zestawy są traktowane jako uzupełnienie zestawu określonego `AppAssembly`. W poniższym przykładzie `Component1` to składnik rutowany zdefiniowany w bibliotece klas, do której się odwołuje. Poniższy `AdditionalAssemblies` przykład powoduje obsługę routingu dla `Component1`:

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a>Parametry trasy

Router używa parametrów trasy do wypełniania odpowiednich parametrów składnika o tej samej nazwie (bez uwzględniania wielkości liter):

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Parametry opcjonalne nie są obsługiwane w przypadku aplikacji Blazor w ASP.NET Core 3,0. W poprzednim przykładzie zastosowano dwie dyrektywy `@page`. Pierwszy zezwala na nawigowanie do składnika bez parametru. Druga dyrektywa `@page` przyjmuje parametr trasy `{text}` i przypisuje wartość do właściwości `Text`.

## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenie trasy wymusza dopasowanie typu w segmencie trasy do składnika.

W poniższym przykładzie trasy do składnika `Users` dopasowuje się tylko wtedy, gdy:

* W adresie URL żądania występuje segment trasy `Id`.
* Segment `Id` jest liczbą całkowitą (`int`).

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

Dostępne są ograniczenia trasy podane w poniższej tabeli. W przypadku ograniczeń trasy, które pasują do niezmiennej kultury, zobacz ostrzeżenie poniżej tabeli, aby uzyskać więcej informacji.

| Typu | Przykład           | Przykładowe dopasowania                                                                  | Niezmiennej<br>kultura<br>parowanie |
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
> Ograniczenia trasy weryfikujące adres URL i konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury. W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny.

### <a name="routing-with-urls-that-contain-dots"></a>Routing z adresami URL zawierającymi kropki

W aplikacjach Blazor Server domyślna trasa w *_Host. cshtml* jest `/` (`@page "/"`). Adres URL żądania, który zawiera kropkę (`.`) nie pasuje do trasy domyślnej, ponieważ adres URL wygląda na żądanie pliku. Aplikacja Blazor zwraca *404 — nie odnaleziono* odpowiedzi dla pliku statycznego, który nie istnieje. Aby użyć tras zawierających kropkę, skonfiguruj *_Host. cshtml* przy użyciu następującego szablonu trasy:

```cshtml
@page "/{**path}"
```

Szablon `"/{**path}"` obejmuje:

* Podwójna gwiazdka *catch-all* (`**`), aby przechwycić ścieżkę między wieloma granicami folderów bez kodowania ukośników (`/`).
* Nazwa parametru trasy `path`.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Składnik NavLink

Podczas tworzenia linków nawigacji używaj składnika `NavLink` zamiast elementów hiperlinków (`<a>`). Składnik `NavLink` działa jak element `<a>`, z wyjątkiem przełączenia klasy CSS `active` w zależności od tego, czy `href` jest zgodny z bieżącym adresem URL. Klasa `active` pomaga użytkownikowi zrozumieć, która strona jest aktywną stroną między wyświetlonymi łączami nawigacji.

Poniższy składnik `NavMenu` tworzy pasek nawigacyjny [Bootstrap](https://getbootstrap.com/docs/) , który pokazuje, jak używać składników `NavLink`:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Dostępne są dwie opcje `NavLinkMatch`, które można przypisać do atrybutu `Match` elementu `<NavLink>`:

* `NavLinkMatch.All` &ndash; `NavLink` jest aktywny, gdy jest zgodny z całym bieżącym adresem URL.
* `NavLinkMatch.Prefix` (*wartość domyślna*) &ndash; `NavLink` jest aktywny, gdy dopasowuje dowolny prefiks bieżącego adresu URL.

W powyższym przykładzie `NavLink` Home `href=""` dopasowuje główny adres URL i odbiera `active`j klasy CSS tylko w domyślnym adresie URL ścieżki podstawowej aplikacji (na przykład `https://localhost:5001/`). Sekunda `NavLink` otrzymuje klasę `active`, gdy użytkownik odwiedzi dowolny adres URL z prefiksem `MyComponent` (na przykład `https://localhost:5001/MyComponent` i `https://localhost:5001/MyComponent/AnotherSegment`).

Dodatkowe atrybuty składnika `NavLink` są przenoszone do renderowanego tagu zakotwiczenia. W poniższym przykładzie składnik `NavLink` zawiera atrybut `target`:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Renderuje następujący znacznik HTML:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>Pomoc dotycząca stanu identyfikatora URI i nawigacji

Użyj `Microsoft.AspNetCore.Components.NavigationManager` do pracy z identyfikatorami URI i C# nawigacją w kodzie. `NavigationManager` zawiera zdarzenie i metody przedstawione w poniższej tabeli.

| Element członkowski | Opis |
| ------ | ----------- |
| `Uri` | Pobiera bieżący bezwzględny identyfikator URI. |
| `BaseUri` | Pobiera podstawowy identyfikator URI (z końcowym ukośnikiem), który można dołączać do względnych ścieżek URI w celu utworzenia bezwzględnego identyfikatora URI. Zwykle `BaseUri` odpowiada atrybutowi `href` w elemencie `<base>` dokumentu w *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server). |
| `NavigateTo` | Przechodzi do określonego identyfikatora URI. Jeśli `forceLoad` jest `true`:<ul><li>Routing po stronie klienta jest pomijany.</li><li>W przeglądarce wymuszone jest załadowanie nowej strony z serwera, niezależnie od tego, czy identyfikator URI jest zwykle obsługiwany przez router po stronie klienta.</li></ul> |
| `LocationChanged` | Zdarzenie, które jest wyzwalane po zmianie lokalizacji nawigacji. |
| `ToAbsoluteUri` | Konwertuje względny identyfikator URI na bezwzględny identyfikator URI. |
| `ToBaseRelativePath` | Przy użyciu podstawowego identyfikatora URI (na przykład identyfikatora URI wcześniej zwróconego przez `GetBaseUri`) program konwertuje bezwzględny identyfikator URI na identyfikator URI względem podstawowego prefiksu URI. |

Poniższy składnik przechodzi do składnika `Counter` aplikacji po wybraniu przycisku:

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
