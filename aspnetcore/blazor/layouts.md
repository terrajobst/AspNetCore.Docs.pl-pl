---
title: Układy Blazor
author: guardrex
description: Dowiedz się, jak tworzyć składniki wielokrotnego użytku układu dla Blazor aplikacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/layouts
ms.openlocfilehash: 42d5d03c50c525c3d819f6dac39109eff6534800
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614803"
---
# <a name="blazor-layouts"></a>Układy Blazor

Przez [Rainer Stropek](https://www.timecockpit.com)

Aplikacje zwykle zawierają więcej niż jeden składnik. Układ elementów, takich jak menu, komunikaty o prawach autorskich i logo musi być obecny na wszystkie elementy. Skopiować kod z tych elementów układu do wszystkich składników aplikacji nie jest wydajne podejście. Takie duplikowanie jest trudne do utrzymania i prawdopodobnie prowadzi do niespójne zawartość wraz z upływem czasu. *Układy* rozwiązać ten problem.

Technicznie rzecz biorąc układ jest po prostu inny składnik. Układ jest zdefiniowany w szablonie Razor lub w C# kodu i może zawierać [powiązanie danych](xref:blazor/components#data-binding), [wstrzykiwanie zależności](xref:blazor/dependency-injection)i inne funkcje zwykłych składników.

Włącz dwa dodatkowe aspekty *składnika* do *układu*

* Składnik układu musi dziedziczyć `LayoutComponentBase`. `LayoutComponentBase` definiuje `Body` właściwość, która zawiera zawartość do wyrenderowania wewnątrz układu.
* Składnik układ używa `Body` właściwości w celu określenia, w którym treść powinna być renderowany przy użyciu składni Razor `@Body`. Podczas renderowania, `@Body` zastępuje zawartość układu.

Poniższy przykład kodu pokazuje szablon Razor składnika układu. Zwróć uwagę na użycie `LayoutComponentBase` i `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a>Używanie układu w składniku

Użyj dyrektywy Razor `@layout` można zastosować układu do składnika. Kompilator konwertuje tej dyrektywy do `LayoutAttribute`, który jest stosowany do klasy składnika.

W poniższym przykładzie kodu pokazano pojęcia. Zawartość tego składnika jest wstawiany do *MasterLayout* w położeniu `@Body`:

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a>Wybór układu scentralizowane

Każdy folder z aplikacji, można opcjonalnie zawiera plik szablonu o nazwie *_ViewImports.cshtml*. Kompilator zawiera dyrektywy określone w pliku importu widoku we wszystkich szablony Razor, w tym samym folderze i cyklicznie we wszystkich jego podfolderów. W związku z tym *_ViewImports.cshtml* plik zawierający `@layout MainLayout` zapewnia, że wszystkie składniki użycie folderu *MainLayout* układu. Nie ma potrzeby można wielokrotnie dodać `@layout` do wszystkich *.razor* plików.

Należy zauważyć, że korzysta z domyślnego szablonu *_ViewImports.cshtml* mechanizm wyboru układu. Zawiera nowo utworzoną aplikację *_ViewImports.cshtml* w pliku *składniki/strony* folderu.

## <a name="nested-layouts"></a>Układy zagnieżdżonych

Aplikacje może zawierać zagnieżdżone układów. Składnik może odwoływać się układ, który z kolei odwołuje się do innego układu. Na przykład zagnieżdżenia układów może służyć do odzwierciedlenia struktury wielopoziomowe menu.

Poniższe przykłady kodu przedstawiają sposób używać zagnieżdżonych układów. *EpisodesComponent.cshtml* plik jest składnikiem do wyświetlenia. Należy pamiętać, że składnik odwołuje się do układu `MasterListLayout`.

*EpisodesComponent.cshtml*:

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

*MasterListLayout.cshtml* plik zawiera `MasterListLayout`. Układ odwołuje się do innego układu `MasterLayout`, gdzie ma to być osadzone.

*MasterListLayout.cshtml*:

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

Na koniec `MasterLayout` zawiera elementy układu najwyższego poziomu, takie jak nagłówek, stopka i menu głównego.

*MasterLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
