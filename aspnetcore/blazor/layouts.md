---
title: ASP.NET Core układy Blazor
author: guardrex
description: Dowiedz się, jak tworzyć składniki układu wielokrotnego użytku dla aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/layouts
ms.openlocfilehash: 05a38c10e18407d50422192ab1ddf3ff4b0f3a5b
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800363"
---
# <a name="aspnet-core-blazor-layouts"></a>ASP.NET Core układy Blazor

Autorzy [Rainer Stropek](https://www.timecockpit.com) i [Luke Latham](https://github.com/guardrex)

Niektóre elementy aplikacji, takie jak menu, wiadomości o prawach autorskich i logo firmy, są zwykle częścią ogólnego układu aplikacji i są używane przez każdy składnik w aplikacji. Kopiowanie kodu tych elementów do wszystkich składników aplikacji nie jest skutecznym podejściem&mdash;za każdym razem, gdy jeden z elementów wymaga aktualizacji, należy zaktualizować każdy składnik. Taka duplikacja jest trudna do utrzymania i może prowadzić do niespójnej zawartości z upływem czasu. *Układy* rozwiązują ten problem.

Technicznie, układ jest tylko innym składnikiem. Układ jest zdefiniowany w szablonie Razor lub w C# kodzie i może używać [powiązań danych](xref:blazor/components#data-binding), [iniekcji zależności](xref:blazor/dependency-injection)i innych scenariuszy składników.

Aby przekształcić *składnik* do *układu*, składnik:

* Dziedziczy z `LayoutComponentBase`, który `Body` definiuje właściwość dla renderowanej zawartości wewnątrz układu.
* Używa składnia Razor `@Body` do określenia lokalizacji w znaczniku układu, w którym jest renderowana zawartość.

Poniższy przykład kodu pokazuje szablon Razor składnika układu: *MainLayout. Razor*. Układ dziedziczy `LayoutComponentBase` i `@Body` ustawia między paskiem nawigacyjnym i stopką:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

W aplikacji opartej na jednym z szablonów `MainLayout` aplikacji Blazor składnik (*MainLayout. Razor*) znajduje się w folderze *udostępnionym* aplikacji.

## <a name="default-layout"></a>Układ domyślny

Określ domyślny układ aplikacji w `Router` składniku w pliku *App. Razor* aplikacji. Poniższy `Router` składnik, który jest dostarczany przez domyślne szablony Blazor, ustawia domyślny układ `MainLayout` składnika:

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

Aby podać domyślny układ `NotFound` zawartości, `LayoutView` Określ dla `NotFound` zawartości:

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

Aby uzyskać więcej informacji na `Router` temat składnika, <xref:blazor/routing>Zobacz.

## <a name="specify-a-layout-in-a-component"></a>Określanie układu w składniku

Użyj dyrektywy `@layout` Razor, aby zastosować układ do składnika. Kompilator konwertuje `@layout` `LayoutAttribute`do, który jest stosowany do klasy składnika.

Zawartość następującego `MasterList` składnika jest wstawiana `MasterLayout` do pozycji `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a>Scentralizowany wybór układu

Każdy folder aplikacji może opcjonalnie zawierać plik szablonu o nazwie *_Imports. Razor*. Kompilator zawiera dyrektywy określone w pliku Imports we wszystkich szablonach Razor w tym samym folderze i rekursywnie we wszystkich jego podfolderach. W związku z tym plik *_Imports. Razor* zawiera `@layout MyCoolLayout` pewność, że wszystkie składniki w folderze są używane `MyCoolLayout`. Nie trzeba wielokrotnie dodawać `@layout MyCoolLayout` do wszystkich plików *Razor* w folderze i podfolderach. `@using`dyrektywy są również stosowane do składników w ten sam sposób.

Następujące Importy pliku *_Imports. Razor* :

* `MyCoolLayout`.
* Wszystkie składniki Razor w tym samym folderze i wszystkie podfoldery.
* `BlazorApp1.Data` Przestrzeń nazw.
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

Plik *_Imports. Razor* jest podobny do [pliku _ViewImports. cshtml dla widoków i stron Razor,](xref:mvc/views/layout#importing-shared-directives) ale jest stosowany w odniesieniu do plików składników Razor.

## <a name="nested-layouts"></a>Układy zagnieżdżone

Aplikacje mogą składać się z zagnieżdżonych układów. Składnik może odwoływać się do układu, który z kolei odwołuje się do innego układu. Na przykład zagnieżdżanie układów służy do tworzenia struktury menu wielopoziomowego.

Poniższy przykład pokazuje, jak używać układów zagnieżdżonych. Plik *EpisodesComponent. Razor* jest składnikiem do wyświetlenia. Składnik odwołuje się `MasterListLayout`do:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

Plik *MasterListLayout. Razor* zawiera `MasterListLayout`. Układ odwołuje się do innego `MasterLayout`układu, w którym jest renderowany. `EpisodesComponent`jest renderowany w `@Body` miejscu, gdzie pojawia się:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Na koniec w *MasterLayout. Razor* zawiera elementy układu najwyższego poziomu, takie jak nagłówek, menu główne i stopka. `MasterLayout` `MasterListLayout`z renderowanym miejscem `EpisodesComponent` , `@Body` gdzie pojawia się:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/layout>
