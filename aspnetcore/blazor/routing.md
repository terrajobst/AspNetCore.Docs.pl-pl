---
title: ASP.NET Core Routing Blazor
author: guardrex
description: Dowiedz się, jak kierować żądania w aplikacjach i informacje o składniku NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/routing
ms.openlocfilehash: ccc8231d1925d4a55eeef618800c10652f9ae36d
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211658"
---
# <a name="aspnet-core-blazor-routing"></a>ASP.NET Core Routing Blazor

Przez [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Dowiedz się, jak kierować żądania oraz jak używać `NavLink` składnika do tworzenia linków nawigacji w aplikacjach Blazor.

## <a name="aspnet-core-endpoint-routing-integration"></a>ASP.NET Core integracja z routingiem punktu końcowego

Serwer Blazor jest zintegrowany z [routingiem punktu końcowego ASP.NET Core](xref:fundamentals/routing). Aplikacja ASP.NET Core jest skonfigurowana do akceptowania połączeń przychodzących dla składników interaktywnych za `MapBlazorHub` pomocą `Startup.Configure`programu w programie:

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

Najbardziej typową konfiguracją jest kierowanie wszystkich żądań do strony Razor, która działa jako host dla części serwerowej aplikacji Blazor Server. Zgodnie z Konwencją strona *hosta* ma zwykle nazwę *_Host. cshtml*. Trasa określona w pliku hosta jest nazywana *trasą rezerwową* , ponieważ działa z niskim priorytetem w dopasowaniu tras. Trasa rezerwowa jest brana pod uwagę, gdy inne trasy nie są zgodne. Dzięki temu aplikacja może korzystać z innych kontrolerów i stron bez zakłócania działania aplikacji serwera Blazor.

## <a name="route-templates"></a>Szablony tras

`Router` Składnik umożliwia kierowanie do każdego składnika z określoną trasą. Składnik pojawi się w pliku *App. Razor:* `Router`

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

Gdy plik *Razor* z `@page` dyrektywą jest kompilowany, wygenerowana Klasa jest dostarczana z <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> określeniem szablonu trasy.

W środowisku uruchomieniowym `RouteView` składnik:

* `RouteData` Odbiera`Router` od siebie wraz z dowolnymi żądanymi parametrami.
* Renderuje określony składnik za pomocą układu (lub opcjonalnego układu domyślnego) przy użyciu określonych parametrów.

Opcjonalnie można określić `DefaultLayout` parametr z klasą układu, która ma być używana dla składników, które nie określają układu. Domyślne szablony Blazor określają `MainLayout` składnik. *MainLayout. Razor* znajduje się w folderze *udostępnionym* projektu szablonu. Aby uzyskać więcej informacji na temat układów <xref:blazor/layouts>, zobacz.

Do składnika można zastosować wiele szablonów tras. Poniższy składnik odpowiada na żądania dla `/BlazorRoute` i: `/DifferentBlazorRoute`

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> Aby można było poprawnie rozwiązać adresy URL, aplikacja musi zawierać `<base>` tag w pliku *wwwroot/index.html* (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server) z ścieżką bazową `href` aplikacji określoną w atrybucie (`<base href="/">`). Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/blazor/index#app-base-path>.

## <a name="provide-custom-content-when-content-isnt-found"></a>Podaj zawartość niestandardową, jeśli nie można odnaleźć zawartości

`Router` Składnik umożliwia aplikacji określenie zawartości niestandardowej, jeśli nie można odnaleźć zawartości dla żądanej trasy.

W pliku *App. Razor* Ustaw zawartość niestandardową w `NotFound` parametrze `Router` szablonu składnika:

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

Zawartość `<NotFound>` tagów może zawierać dowolne elementy, takie jak inne składniki interaktywne. Aby zastosować domyślny układ do `NotFound` zawartości, zobacz. <xref:blazor/layouts>

## <a name="route-to-components-from-multiple-assemblies"></a>Kierowanie do składników z wielu zestawów

Użyj parametru `AdditionalAssemblies` , aby określić dodatkowe zestawy `Router` dla składnika do uwzględnienia podczas wyszukiwania składników routingu. Określone zestawy są traktowane jako uzupełnienie `AppAssembly`określonego zestawu. W poniższym przykładzie `Component1` jest składnikiem rutowanym zdefiniowanym w bibliotece klas, do której się odwołuje. W poniższym `AdditionalAssemblies` przykładzie przedstawiono obsługę routingu dla: `Component1`

< router AppAssembly = "typeof (program). Zestaw "AdditionalAssemblies =" New [] {typeof (Component1). Zestaw} >...</Router>

## <a name="route-parameters"></a>Parametry trasy

Router używa parametrów trasy do wypełniania odpowiednich parametrów składnika o tej samej nazwie (bez uwzględniania wielkości liter):

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

Parametry opcjonalne nie są obsługiwane w przypadku aplikacji Blazor w wersji zapoznawczej ASP.NET Core 3,0. W `@page` poprzednim przykładzie są stosowane dwie dyrektywy. Pierwszy zezwala na nawigowanie do składnika bez parametru. Druga `@page` dyrektywa `Text` przyjmuje parametr Route i przypisuje wartość do właściwości. `{text}`

## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenie trasy wymusza dopasowanie typu w segmencie trasy do składnika.

W poniższym przykładzie trasa do `Users` składnika dopasowuje się tylko wtedy, gdy:

* Segment `Id` trasy jest obecny w adresie URL żądania.
* Segment jest liczbą całkowitą (`int`). `Id`

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
> Ograniczenia trasy, które weryfikują adres URL i są konwertowane na typ CLR (takie `int` jak `DateTime`lub), zawsze używają niezmiennej kultury. W tych ograniczeniach przyjęto założenie, że adres URL nie jest Lokalizowalny.

### <a name="routing-with-urls-that-contain-dots"></a>Routing z adresami URL zawierającymi kropki

W aplikacjach serwera Blazor domyślną trasą w *_Host. cshtml* jest `/` (`@page "/"`). Adres URL żądania, który zawiera kropkę`.`() nie pasuje do trasy domyślnej, ponieważ adres URL wygląda na żądanie pliku. Aplikacja Blazor zwraca *404 — nie odnaleziono* odpowiedzi dla pliku statycznego, który nie istnieje. Aby użyć tras zawierających kropkę, skonfiguruj *_Host. cshtml* przy użyciu następującego szablonu trasy:

```cshtml
@page "/{**path}"
```

`"/{**path}"` Szablon zawiera:

* Podwójna gwiazdka *catch-all* (`**`) do przechwytywania ścieżki między wieloma granicami folderów bez kodowania ukośników (`/`).
* Nazwa `path` parametru trasy.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.

## <a name="navlink-component"></a>Składnik NavLink

Podczas tworzenia linków nawigacji Użyj`<a>` składnikazamiastelementówhiperlinkówHTML().`NavLink` Składnik zachowuje się `<a>` jak element, `active` z wyjątkiem przełączenia klasy CSS w zależności od tego, czy `href` jest on zgodny z bieżącym adresem URL. `NavLink` `active` Klasa pomaga użytkownikowi zrozumieć, która strona jest aktywną stroną między wyświetlanymi łączami nawigacji.

Poniższy `NavMenu` składnik tworzy pasek nawigacyjny [Bootstrap](https://getbootstrap.com/docs/) , który pokazuje, jak używać `NavLink` składników:

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

Dostępne są dwie `NavLinkMatch` opcje, które można przypisać `Match` do atrybutu `<NavLink>` elementu:

* `NavLinkMatch.All`Jest aktywny, `NavLink` gdy jest zgodny z całym bieżącym adresem URL. &ndash;
* `NavLinkMatch.Prefix`(*ustawienie domyślne*) Jest aktywny, `NavLink` gdy pasuje do dowolnego prefiksu bieżącego adresu URL. &ndash;

W `NavLink` poprzednim przykładzie Strona główna `href=""` odpowiada głównemu `active` adresowi URL i odbiera tylko klasy CSS przy `https://localhost:5001/`domyślnym adresie URL ścieżki podstawowej aplikacji (na przykład). Druga `NavLink` otrzymuje klasę, `active` gdy użytkownik `MyComponent` odwiedzi dowolny adres URL z prefiksem (na przykład `https://localhost:5001/MyComponent` i `https://localhost:5001/MyComponent/AnotherSegment`).

Dodatkowe `NavLink` atrybuty składników są przenoszone do renderowanego tagu zakotwiczenia. W poniższym przykładzie `NavLink` składnik `target` zawiera atrybut:

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

Renderuje następujący znacznik HTML:

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a>Pomoc dotycząca stanu identyfikatora URI i nawigacji

Służy `Microsoft.AspNetCore.Components.NavigationManager` do pracy z identyfikatorami URI i C# nawigacją w kodzie. `NavigationManager`zawiera zdarzenie i metody przedstawione w poniższej tabeli.

| Element członkowski | Opis |
| ------ | ----------- |
| `Uri` | Pobiera bieżący bezwzględny identyfikator URI. |
| `BaseUri` | Pobiera podstawowy identyfikator URI (z końcowym ukośnikiem), który można dołączać do względnych ścieżek URI w celu utworzenia bezwzględnego identyfikatora URI. `BaseUri` Zazwyczaj odpowiada `href` *atrybutowi* elementu dokumentu w wwwroot/index.html (Blazor webassembly) lub *Pages/_Host. cshtml* (Blazor Server). `<base>` |
| `NavigateTo` | Przechodzi do określonego identyfikatora URI. Jeśli `forceLoad` jest `true`:<ul><li>Routing po stronie klienta jest pomijany.</li><li>W przeglądarce wymuszone jest załadowanie nowej strony z serwera, niezależnie od tego, czy identyfikator URI jest zwykle obsługiwany przez router po stronie klienta.</li></ul> |
| `LocationChanged` | Zdarzenie, które jest wyzwalane po zmianie lokalizacji nawigacji. |
| `ToAbsoluteUri` | Konwertuje względny identyfikator URI na bezwzględny identyfikator URI. |
| `ToBaseRelativePath` | Przy użyciu podstawowego identyfikatora URI (na przykład identyfikatora URI zwróconego wcześniej `GetBaseUri`przez), program konwertuje bezwzględny identyfikator URI na identyfikator URI względem podstawowego prefiksu URI. |

Poniższy składnik przechodzi do `Counter` składnika aplikacji po wybraniu przycisku:

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
