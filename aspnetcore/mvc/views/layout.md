---
title: Układ w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać typowych układów, udostępnianie dyrektyw i uruchomienia wspólnego kodu przed renderowania widoków w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757366"
---
# <a name="layout-in-aspnet-core"></a>Układ w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Widoki często Udostępnianie elementów wizualnych i programowe. W tym artykule dowiesz się, jak używać typowych układów, udostępnianie dyrektywy i uruchomienia wspólnego kodu przed renderowania widoków w aplikacji platformy ASP.NET Core.

## <a name="what-is-a-layout"></a>Co to jest układ

Większość aplikacji sieci web ma typowe układ, który zapewnia spójne środowisko użytkownika zgodnie z jego nawigowania między stronami. Układ zwykle zawiera wspólne elementy interfejsu użytkownika, takie jak nagłówek aplikacji, nawigacji lub elementy menu i stopki.

![Przykładowy układ strony](layout/_static/page-layout.png)

Wspólnej struktury kodu HTML, takie jak skrypty i arkusze stylów również często są używane przez wielu stronach w obrębie aplikacji. Wszystkie te elementy udostępnione może być zdefiniowana w *układ* pliku, który można odwoływać się do dowolnego widoku używany w aplikacji. Układy zmniejszyć kodu zduplikowanego w widokach, pomagając im wykonaj [zasady nie Powtórz samodzielnie (PRÓBNEGO)](http://deviq.com/don-t-repeat-yourself/).

Zgodnie z Konwencją, nosi nazwę domyślny układ dla aplikacji ASP.NET Core `_Layout.cshtml`. Szablon projektu Visual Studio ASP.NET Core MVC obejmuje to pliku układu `Views/Shared` folderu:

![Widoki folder w Eksploratorze rozwiązań](layout/_static/web-project-views.png)

Definiuje szablon najwyższego poziomu dla widoków w aplikacji. Aplikacje nie wymagają układu, a aplikacje można zdefiniować więcej niż jeden układ z różnych widoków, określając różne układy.

Przykład `_Layout.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Określanie układu

Widoki razor oferują `Layout` właściwości. Pojedyncze widoki określ układ przez ustawienie tej właściwości:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Określony układ, można użyć pełnej ścieżki (przykład: `/Views/Shared/_Layout.cshtml`) lub część nazwy (przykład: `_Layout`). Gdy część nazwy jest podana, aparat widoku Razor wyszuka plik układu przy użyciu swojego procesu odnajdywania standardowego. Folder skojarzonego kontrolera jest przeszukiwany w pierwszej kolejności, a następnie `Shared` folderu. Ten proces odnajdywania jest taka sama jak używaną w celu odnalezienia [widoki częściowe](partial.md).

Domyślnie każdy układu musi wywołać `RenderBody`. Wszędzie tam, gdzie wywołanie `RenderBody` jest umieszczany, zawartość widoku będzie renderowana.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Sekcje

Układ Opcjonalnie można odwoływać się do co najmniej jeden *sekcje*, wywołując `RenderSection`. Sekcje zawierają sposób organizowania, w którym mają zostać umieszczone niektóre elementy strony. Każde wywołanie `RenderSection` można określić, czy tej sekcji jest wymagane lub opcjonalne. Jeśli wymaganej sekcji nie zostanie znaleziona, zostanie zgłoszony wyjątek. Pojedyncze widoki określenie zawartości do renderowania w ramach sekcji przy użyciu `@section` składni Razor. Jeśli widok definiuje sekcję, musi być renderowany lub wystąpi błąd.

Przykład `@section` definicję w widoku:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

W powyższym kodzie skrypty sprawdzania poprawności są dodawane do `scripts` sekcji w widoku, który zawiera formularz. Inne widoki w tej samej aplikacji może nie wymagać dowolne dodatkowe skrypty i dlatego nie należy zdefiniować sekcję skryptów.

Sekcje zdefiniowane w widoku są dostępne tylko w jego natychmiastowego układ strony. One nie może odwoływać się ze częściowych, składniki widoków lub inne części systemu widoku.

### <a name="ignoring-sections"></a>Ignorowanie sekcji

Domyślnie jednostka i wszystkie sekcje w zawartości strony musi wszystkie renderowana przez stronę układu. Aparat widoku Razor wymusza to przez śledzenie czy nadano treści i każdej sekcji.

Aby nakazać aparat widoku, aby zignorować treści lub sekcje, należy wywołać `IgnoreBody` i `IgnoreSection` metody.

Treść oraz każdej sekcji strony Razor musi być renderowany lub ignorowane.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importowanie udostępnionego dyrektywy

Widoki za pomocą dyrektywy Razor można wykonywać wiele elementów, takich jak importowania przestrzeni nazw lub wykonywanie [wstrzykiwanie zależności](dependency-injection.md). Dyrektywy współużytkowane przez wiele widoków, mogą być określone w często `_ViewImports.cshtml` pliku. `_ViewImports` Pliku obsługuje następujące dyrektywy:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Plik nie obsługuje inne funkcje Razor, takie jak definicje sekcji i funkcji.

Przykład `_ViewImports.cshtml` pliku:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

`_ViewImports.cshtml` Plików dla aplikacji ASP.NET Core MVC znajduje się zwykle w `Views` folderu. A `_ViewImports.cshtml` pliku można umieścić w dowolnym folderze, w którym przypadku zostaną zastosowane tylko do wyświetlenia w tym folderze i jego podfolderach. `_ViewImports` pliki są przetwarzane, zaczynając od katalogu głównego, a następnie dla każdego folderu, co prowadzi do lokalizacji widoku, aby określić ustawienia na poziomie głównym może zostać zastąpione na poziomie folderu.

Na przykład, jeśli poziom główny `_ViewImports.cshtml` plik Określa `@model` i `@addTagHelper`, a inny `_ViewImports.cshtml` pliku w folderze skojarzonego kontrolera widoku Określa inną `@model` i dodaje kolejny `@addTagHelper`, widok będą mieć dostęp do obu pomocników tagów i użyje one `@model`.

Jeśli wiele `_ViewImports.cshtml` pliki są uruchamiane w celu wyświetlenia, zachowanie dyrektywy zawarte w połączeniu `ViewImports.cshtml` pliki będą znajdować się w następujący sposób:

* `@addTagHelper`, `@removeTagHelper`: wszystkie działania w kolejności

* `@tagHelperPrefix`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola

* `@model`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola

* `@inherits`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola

* `@using`: uwzględniono wszystkie; duplikaty są ignorowane.

* `@inject`: dla każdej właściwości znajdującego się najbliżej do widoku zastępuje wszystkie inne pola o takiej samej nazwie właściwości

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Uruchamianie kodu przed każdym widoku

Jeśli masz kod do uruchomienia przed każdym widoku, to należy umieścić w `_ViewStart.cshtml` pliku. Zgodnie z Konwencją `_ViewStart.cshtml` plik znajduje się w `Views` folderu. Instrukcje, na liście `_ViewStart.cshtml` są uruchamiane przed każdym pełnego widoku (nie układ i widoki częściowe nie). Podobnie jak [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` są hierarchiczne. Jeśli `_ViewStart.cshtml` jest zdefiniowany plik w folderze skojarzonego kontrolera widoku, zostanie ono uruchomione po jednym określonym w katalogu głównym `Views` folder (jeśli istnieje).

Przykład `_ViewStart.cshtml` pliku:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Pliku powyżej Określa, które będą używane we wszystkich widokach `_Layout.cshtml` układu.

> [!NOTE]
> Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` są zazwyczaj umieszczane `/Views/Shared` folderu. Poziom aplikacji wersje tych plików powinny być umieszczane bezpośrednio w `/Views` folderu.
