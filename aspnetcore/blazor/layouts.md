---
title: Układy Blazor
author: guardrex
description: Dowiedz się, jak tworzyć składniki wielokrotnego użytku układu dla Blazor aplikacji.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: blazor/layouts
ms.openlocfilehash: 8641400cd97a74572d1bcd8c6eb6891656903e1b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898489"
---
# <a name="blazor-layouts"></a>Układy Blazor

Przez [Rainer Stropek](https://www.timecockpit.com)

Niektóre elementy aplikacji, takich jak menu, komunikaty o prawach autorskich i logo firmy, są zazwyczaj ogólny układu aplikacji i używane przez każdy składnik w aplikacji. Skopiować kod z tych elementów do wszystkich składników aplikacji nie jest wydajne podejście&mdash;za każdym razem, gdy jeden z elementów wymaga aktualizacji, każdy składnik musi zostać zaktualizowany. Takie duplikowanie jest trudne do utrzymania i może spowodować niespójne zawartość wraz z upływem czasu. *Układy* rozwiązać ten problem.

Technicznie rzecz biorąc układ jest po prostu inny składnik. Układ jest zdefiniowany w szablonie Razor lub w C# kod i użyć [powiązanie danych](xref:blazor/components#data-binding), [wstrzykiwanie zależności](xref:blazor/dependency-injection)oraz innych scenariuszy składnika.

Aby włączyć *składnika* do *układ*, składnik:

* Dziedziczy `LayoutComponentBase`, która definiuje `Body` właściwość, która zawiera zawartość do wyrenderowania wewnątrz układu.
* Używa składni Razor `@Body` Aby określić lokalizację, w znacznikach, której zawartość ma być renderowany.

Poniższy przykład kodu pokazuje szablon Razor składnika układ *MainLayout.razor*. Układ dziedziczy `LayoutComponentBase` i ustawia `@Body` między paskiem nawigacji i stopki:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a>Określ układ w składniku

Użyj dyrektywy Razor `@layout` można zastosować układu do składnika. Kompilator konwertuje `@layout` do `LayoutAttribute`, który jest stosowany do klasy składnika.

Zawartość następującym składniku *MasterList.razor*, jest wstawiany do *MainLayout* w położeniu `@Body`.

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a>Wybór układu scentralizowane

Każdy folder aplikacji opcjonalnie mogą zawierać plik szablonu o nazwie *_Imports.razor*. Kompilator zawiera dyrektywy określone w pliku importu we wszystkich szablony Razor, w tym samym folderze i cyklicznie we wszystkich jego podfolderów. W związku z tym *_Imports.razor* plik zawierający `@layout MainLayout` zapewnia, że wszystkie składniki użycie folderu *MainLayout*. Nie ma potrzeby można wielokrotnie dodać `@layout MainLayout` do wszystkich *.razor* pliki znajdujące się w folderze i jego podfolderach. `@using` dyrektywy są również stosowane do składników w taki sam sposób.

Następujące *_Imports.razor* pliku importu:

* `MainLayout`.
* Wszystkie składniki Razor, w tym samym folderze i wszelkich podfolderów.
* `BlazorApp1.Data` Przestrzeni nazw.
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

*_Imports.razor* pliku jest podobny do [widokami Razor i stron w pliku _ViewImports.cshtml](xref:mvc/views/layout#importing-shared-directives) jednak stosowane do plików składników Razor.

Użyj szablonów Blazor *_Imports.razor* pliki do wyboru układu. Zawiera aplikacji utworzonych na podstawie szablonu Blazor *_Imports.razor* plik w folderze głównym projektu i w *stron* folderu.

## <a name="nested-layouts"></a>Układy zagnieżdżonych

Aplikacje może zawierać zagnieżdżone układów. Składnik może odwoływać się układ, który z kolei odwołuje się do innego układu. Na przykład układy zagnieżdżenia może służyć do tworzenia struktury wielopoziomowe menu.

Poniższy przykład pokazuje, jak używać zagnieżdżonych układów. *EpisodesComponent.razor* plik jest składnikiem do wyświetlenia. Odwołania do składników `MasterListLayout`:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

*MasterListLayout.razor* plik zawiera `MasterListLayout`. Układ odwołuje się do innego układu `MasterLayout`, gdy jest on renderowany. `EpisodesComponent` gdzie renderowanego `@Body` pojawia się:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Na koniec `MasterLayout` w *MasterLayout.razor* zawiera elementy układu najwyższego poziomu, takie jak nagłówek, menu głównego i stopki. *MasterListLayout* z *EpisodesComponent* — zostaną zrenderowane gdzie `@Body` pojawia się:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/layout>
