---
title: Układ platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać typowych układów, udostępnić dyrektywy i uruchom typowy kod przed renderowania widoków w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: a99b239a0aeeb14492b1eee962dc1149f056f0eb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274121"
---
# <a name="layout-in-aspnet-core"></a>Układ platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Widoki często Udostępnianie elementów wizualnych i programowe. W tym artykule dowiesz się, jak używać typowych układów, udostępnić dyrektywy i uruchomić typowy kod przed renderowania widoków w aplikacji ASP.NET.

## <a name="what-is-a-layout"></a>Co to jest układ

Większość aplikacji sieci web mają wspólne układu, który zapewnia spójne środowisko użytkownika zgodnie z ich przechodzenie między stronami. Układ zazwyczaj zawiera wspólne elementy interfejsu użytkownika, takie jak nagłówek aplikacji, nawigacji lub elementy menu i stopce strony.

![Przykładowy układ strony](layout/_static/page-layout.png)

Wspólnej struktury HTML, takich jak skrypty i arkusze stylów również często używane przez wiele stron w aplikacji. Wszystkie te elementy udostępnionych można zdefiniować w *układu* pliku, który można odwoływać się do dowolnego widoku używany w aplikacji. Układy zmniejszyć zduplikowany kod w widokach, pomagając im wykonaj [zasady nie powtarzaj samodzielnie (BIBUŁĄ)](http://deviq.com/don-t-repeat-yourself/).

Konwencja, nosi nazwę domyślny układ dla aplikacji ASP.NET `_Layout.cshtml`. Szablon projektu Visual Studio platformy ASP.NET Core MVC obejmuje to pliku układu `Views/Shared` folderu:

![Widoki folder w Eksploratorze rozwiązań](layout/_static/web-project-views.png)

Definiuje szablon najwyższego poziomu do widoków w aplikacji. Aplikacje nie wymagają układ, a aplikacje można zdefiniować więcej niż jeden układ różne widoki, określając układów.

Przykład `_Layout.cshtml`:

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>Określanie układu

Widoki razor oferują `Layout` właściwości. Poszczególnych widoków określ układ przez ustawienie dla tej właściwości:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Określony układ, można użyć pełną ścieżkę (przykład: `/Views/Shared/_Layout.cshtml`) lub część nazwy (przykład: `_Layout`). Gdy częściowa nazwa została podana, aparat widoku Razor wyszuka plik układu przy użyciu procesu odnajdywania standardowego. Folder skojarzonego kontrolera przeszukiwany jest najpierw, a następnie `Shared` folderu. Ten proces odnajdywania jest taki sam jak używaną do wykrywania [widoki częściowe](partial.md).

Domyślnie każdy układ należy wywołać metodę `RenderBody`. Wszędzie tam, gdzie wywołanie `RenderBody` jest umieszczony, zawartość widoku będzie renderowany.

<a name="layout-sections-label"></a>

### <a name="sections"></a>Sekcje

Układ opcjonalnie może odwoływać się co najmniej jeden *sekcje*, wywołując `RenderSection`. Sekcje zawierają sposób organizowania, w którym mają zostać umieszczone niektóre elementy na stronie. Każde wywołanie `RenderSection` można określić, czy tej sekcji jest wymagany lub opcjonalny. Jeśli nie zostanie odnaleziony wymaganej sekcji, zostanie wygenerowany wyjątek. Poszczególnych widoków określenie zawartości do umieszczony wewnątrz sekcji przy użyciu `@section` składni Razor. Jeśli widok definiuje sekcję, musi być renderowane lub wystąpi błąd.

Przykład `@section` definicji w widoku:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

W powyższym kodzie skrypty sprawdzania poprawności są dodawane do `scripts` sekcji w widoku, który zawiera formularz. Innych widoków w tej samej aplikacji nie może wymagać dodatkowych skryptów i dlatego nie trzeba definiują sekcję skryptów.

Sekcje zdefiniowane w widoku są dostępne tylko w jego natychmiastowego układ strony. One nie może odwoływać się ze częściowe, składniki w widoku lub innymi składnikami systemu widoku.

### <a name="ignoring-sections"></a>Ignorowanie sekcji

Domyślnie treść i wszystkich części strony zawartość musi wszystkie przetworzyć, strony układu. Aparat widoku Razor wymusza to poprzez śledzenie czy nadano treść i każdej sekcji.

Aby nakazać przez aparat widoku, aby zignorować treści lub sekcje, należy wywołać `IgnoreBody` i `IgnoreSection` metody.

Treść i każdej sekcji stronie aparatu Razor musi być renderowane albo ignorowane.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importowanie udostępnionego dyrektywy

Widoki umożliwiają wykonywanie wielu czynności, takich jak importowania przestrzeni nazw lub wykonywania dyrektywy Razor [iniekcji zależności](dependency-injection.md). Dyrektywy współużytkowane przez wiele widoków mogą być określone w wspólnego `_ViewImports.cshtml` pliku. `_ViewImports` Plik obsługuje następujące dyrektywy:

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

Plik nie obsługuje inne funkcje Razor, takich jak funkcje i definicje sekcji.

Przykładowe `_ViewImports.cshtml` pliku:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

`_ViewImports.cshtml` Pliku dla aplikacji ASP.NET Core MVC zazwyczaj znajduje się w `Views` folderu. A `_ViewImports.cshtml` pliku można umieścić w dowolnym folderze, w którym zostaną one zastosowane tylko do sprawę widoków w tym folderze i jego podfolderach. `_ViewImports` pliki są przetwarzane uruchamiania na poziomie głównym, a następnie dla każdego folderu, co prowadzi do lokalizacji widoku, aby określić ustawienia na poziomie głównym może być zastąpiona na poziomie folderu.

Na przykład, jeśli poziom główny `_ViewImports.cshtml` plik Określa `@model` i `@addTagHelper`i innym `_ViewImports.cshtml` plik w folderze skojarzonego kontrolera widoku Określa inną `@model` i dodaje innego `@addTagHelper`, widok będą mieć dostęp do obu pomocników tagów i korzysta z jego `@model`.

Jeśli wiele `_ViewImports.cshtml` pliki są uruchamiane w widoku, zachowanie dyrektywy uwzględnione w połączeniu `ViewImports.cshtml` pliki będą znajdować się w następujący sposób:

* `@addTagHelper`, `@removeTagHelper`: wszystkie działania w kolejności

* `@tagHelperPrefix`: najbliższy, do widoku powoduje zastąpienie innych

* `@model`: najbliższy, do widoku powoduje zastąpienie innych

* `@inherits`: najbliższy, do widoku powoduje zastąpienie innych

* `@using`: wszystkie są uwzględniane; duplikaty zostały zignorowane.

* `@inject`: dla każdej właściwości, najbliższego do widoku zastępuje innych o takiej samej nazwie właściwości

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Uruchomienia kodu przed każdym widoku

Jeśli masz kod, należy uruchomić przed każdym widoku, to powinna zostać umieszczona w `_ViewStart.cshtml` pliku. Według konwencji `_ViewStart.cshtml` plik znajduje się w `Views` folderu. Instrukcje na liście `_ViewStart.cshtml` są uruchamiane przed każdym pełnego widoku (nie układy i widoki częściowe nie). Podobnie jak [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` hierarchicznie. Jeśli `_ViewStart.cshtml` pliku jest zdefiniowany w folderze skojarzonego kontrolera widoku, zostanie ono uruchomione po jednym określonym w folderze głównym `Views` folder (jeśli istnieje).

Przykładowe `_ViewStart.cshtml` pliku:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Plik powyżej Określa, które będą używane przez wszystkie widoki `_Layout.cshtml` układu.

> [!NOTE]
> Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` są zwykle umieszczane w `/Views/Shared` folderu. Wersje aplikacji na poziomie plików powinny być umieszczane bezpośrednio w `/Views` folderu.
