---
title: Układ w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać typowych układów, udostępnianie dyrektyw i uruchomienia wspólnego kodu przed renderowania widoków w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 02/26/2019
uid: mvc/views/layout
ms.openlocfilehash: 7a60ee15e688d6f0e531302457604fa759213758
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899245"
---
# <a name="layout-in-aspnet-core"></a>Układ w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [firmy Dave Brock](https://twitter.com/daveabrock)

Widoki i strony często Udostępnianie elementów wizualnych i programowe. W tym artykule przedstawiono sposób:

* Użyj wspólnego układów.
* Udostępnij dyrektywy.
* Uruchom wspólny kod przed przystąpieniem do renderowania stron lub widoków.

W tym dokumencie omówiono układów dla dwa różne podejścia do platformy ASP.NET Core MVC: Strony razor i kontrolery, za pomocą widoków. W tym temacie różnice są minimalne:

* Strony razor znajdują się w *stron* folderu.
* Kontrolery używa widoków *widoków* folderu dla widoków.

## <a name="what-is-a-layout"></a>Co to jest układ

Większość aplikacji sieci web ma typowe układ, który zapewnia spójne środowisko użytkownika zgodnie z jego nawigowania między stronami. Układ zwykle zawiera wspólne elementy interfejsu użytkownika, takie jak nagłówek aplikacji, nawigacji lub elementy menu i stopki.

![Przykładowy układ strony](layout/_static/page-layout.png)

Wspólnej struktury kodu HTML, takie jak skrypty i arkusze stylów również często są używane przez wielu stronach w obrębie aplikacji. Wszystkie te elementy udostępnione może być zdefiniowana w *układ* pliku, który można odwoływać się do dowolnego widoku używany w aplikacji. Układy zmniejszenie kodu zduplikowanego w widokach.

Zgodnie z Konwencją, nosi nazwę domyślny układ dla aplikacji ASP.NET Core *_Layout.cshtml*. Plik układu dla nowych projektów ASP.NET Core utworzona przy użyciu szablonów:

* Strony razor: *Pages/Shared/_Layout.cshtml*

  ![folder stron w Eksploratorze rozwiązań](layout/_static/rp-web-project-views.png)

* Kontroler z widokami: *Views/Shared/_Layout.cshtml*

 ![Widoki folder w Eksploratorze rozwiązań](layout/_static/mvc-web-project-views.png)

Układ określa szablon najwyższego poziomu dla widoków w aplikacji. Aplikacje nie wymagają układu. Aplikacje można zdefiniować więcej niż jeden układ z różnych widoków, określając różne układy.

Poniższy kod przedstawia plik układu dla szablonu, który został utworzony projekt za pomocą kontrolera i widoki:

[!code-cshtml[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a>Określanie układu

Widoki razor oferują `Layout` właściwości. Pojedyncze widoki określ układ przez ustawienie tej właściwości:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Określony układ, można użyć pełnej ścieżki (na przykład */Pages/Shared/_Layout.cshtml* lub */Views/Shared/_Layout.cshtml*) lub część nazwy (przykład: `_Layout`). Częściowa nazwa zostanie podana, aparat widoku Razor wyszukuje plik układu przy użyciu swojego procesu odnajdywania standardowego. Folder, w której istnieje metoda obsługi (lub kontroler) jest przeszukiwany w pierwszej kolejności, a następnie *Shared* folderu. Ten proces odnajdywania jest taka sama jak proces używany do odnajdywania [widoki częściowe](xref:mvc/views/partial#partial-view-discovery).

Domyślnie każdy układu musi wywołać `RenderBody`. Wszędzie tam, gdzie wywołanie `RenderBody` jest umieszczany, zawartość widoku będzie renderowana.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Sekcje

Układ Opcjonalnie można odwoływać się do co najmniej jeden *sekcje*, wywołując `RenderSection`. Sekcje zawierają sposób organizowania, w którym mają zostać umieszczone niektóre elementy strony. Każde wywołanie `RenderSection` można określić, czy tej sekcji jest wymagane lub opcjonalne:

```html
@section Scripts {
    @RenderSection("Scripts", required: false)
}
```

Jeśli wymaganej sekcji nie zostanie znaleziona, zostanie zgłoszony wyjątek. Pojedyncze widoki określenie zawartości do renderowania w ramach sekcji przy użyciu `@section` składni Razor. Jeśli strona lub widok definiuje sekcję, musi być renderowany lub wystąpi błąd.

Przykład `@section` definicję w widoku strony Razor:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
}
```

W poprzednim kodzie *scripts/main.js* jest dodawany do `scripts` sekcji na stronie lub widoku. Innych stron lub widoków w tej samej aplikacji może nie wymagać tego skryptu, a nie zdefiniować sekcję skryptów.

Korzysta z niej następujące znaczniki [Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) do renderowania *_ValidationScriptsPartial.cshtml*:

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

Poprzedni kod znaczników został wygenerowany przez [tworzenia szkieletów tożsamości](xref:security/authentication/scaffold-identity).

Sekcje zdefiniowane w widoku lub strony są dostępne tylko w jego natychmiastowego układ strony. One nie może odwoływać się ze częściowych, składniki widoków lub inne części systemu widoku.

### <a name="ignoring-sections"></a>Ignorowanie sekcji

Domyślnie jednostka i wszystkie sekcje w zawartości strony musi wszystkie renderowana przez stronę układu. Aparat widoku Razor wymusza to przez śledzenie czy nadano treści i każdej sekcji.

Aby nakazać aparat widoku, aby zignorować treści lub sekcje, należy wywołać `IgnoreBody` i `IgnoreSection` metody.

Treść oraz każdej sekcji strony Razor musi być renderowany lub ignorowane.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importowanie udostępnionego dyrektywy

Widoków i stron można użyć dyrektywy Razor do importowania przestrzeni nazw i użyj [wstrzykiwanie zależności](dependency-injection.md). Dyrektywy współużytkowane przez wiele widoków, mogą być określone w często *_ViewImports.cshtml* pliku. `_ViewImports` Pliku obsługuje następujące dyrektywy:

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

Plik nie obsługuje inne funkcje Razor, takie jak definicje sekcji i funkcji.

Przykład `_ViewImports.cshtml` pliku:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

*_ViewImports.cshtml* plików dla aplikacji ASP.NET Core MVC znajduje się zwykle w *stron* (lub *widoków*) folder. A *_ViewImports.cshtml* pliku można umieścić w dowolnym folderze, w którym to przypadku go one stosowane tylko do stron lub widoków w tym folderze i jego podfolderach. `_ViewImports` pliki są przetwarzane od na poziomie głównym, a następnie dla każdego folderu prowadzących do lokalizacji strony lub wyświetlić sam. `_ViewImports` na poziomie folderu, mogą zostać zastąpione ustawieniami określonymi na poziomie głównym.

Załóżmy na przykład:

* Poziom główny *_ViewImports.cshtml* plik zawiera `@model MyModel1` i `@addTagHelper *, MyTagHelper1`.
* Podfolder *_ViewImports.cshtml* plik zawiera `@model MyModel2` i `@addTagHelper *, MyTagHelper2`.

Widoki w podfolderze i strony będą mieć dostęp do obu pomocników tagów i `MyModel2` modelu.

Jeśli wiele *_ViewImports.cshtml* pliki znajdują się w hierarchii plików są połączone zachowanie dyrektywy:

* `@addTagHelper`, `@removeTagHelper`: wszystkie działania w kolejności
* `@tagHelperPrefix`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola
* `@model`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola
* `@inherits`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola
* `@using`: uwzględniono wszystkie; duplikaty są ignorowane.
* `@inject`: dla każdej właściwości znajdującego się najbliżej do widoku zastępuje wszystkie inne pola o takiej samej nazwie właściwości

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Uruchamianie kodu przed każdym widoku

Kod, który musi zostać uruchomiony przed każdym widoku lub strony powinny być umieszczone w *_ViewStart.cshtml* pliku. Zgodnie z Konwencją *_ViewStart.cshtml* plik znajduje się w *stron* (lub *widoków*) folder. Instrukcje, na liście *_ViewStart.cshtml* są uruchamiane przed każdym pełnego widoku (nie układ i widoki częściowe nie). Podobnie jak [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* są hierarchiczne. Jeśli *_ViewStart.cshtml* jest zdefiniowany plik w folderze widoku lub strony będzie uruchamiana po jednym określonym w katalogu głównym *stron* (lub *widoków*) folder (jeśli istnieje).

Przykład *_ViewStart.cshtml* pliku:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Pliku powyżej Określa, które będą używane we wszystkich widokach *_Layout.cshtml* układu.

*_ViewStart.cshtml* i *_ViewImports.cshtml* są **nie** zazwyczaj umieszczane */stron/Shared* (lub   */widoków/Shared*) folder. Poziom aplikacji wersje tych plików powinny być umieszczane bezpośrednio w */strony* (lub */widoków*) folder.
