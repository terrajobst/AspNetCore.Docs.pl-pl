---
title: ASP.NET Core układy Blazor
author: guardrex
description: Dowiedz się, jak tworzyć składniki układu wielokrotnego użytku dla aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
no-loc:
- Blazor
uid: blazor/layouts
ms.openlocfilehash: 3546259fc6b622a6137a6baa8f446c5f43af1cab
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962810"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a>ASP.NET Core układy Blazor

Autorzy [Rainer Stropek](https://www.timecockpit.com) i [Luke Latham](https://github.com/guardrex)

Niektóre elementy aplikacji, takie jak menu, wiadomości o prawach autorskich i logo firmy, są zwykle częścią ogólnego układu aplikacji i są używane przez każdy składnik w aplikacji. Kopiowanie kodu tych elementów do wszystkich składników aplikacji nie jest efektywnym podejściem&mdash;za każdym razem, gdy jeden z elementów wymaga aktualizacji, należy zaktualizować każdy składnik. Taka duplikacja jest trudna do utrzymania i może prowadzić do niespójnej zawartości z upływem czasu. *Układy* rozwiązują ten problem.

Technicznie, układ jest tylko innym składnikiem. Układ jest zdefiniowany w szablonie Razor lub w C# kodzie i może używać [powiązań danych](xref:blazor/components#data-binding), [iniekcji zależności](xref:blazor/dependency-injection)i innych scenariuszy składników.

Aby przekształcić *składnik* do *układu*, składnik:

* Dziedziczy z `LayoutComponentBase`, który definiuje Właściwość `Body` dla renderowanej zawartości wewnątrz układu.
* Używa `@Body` składnia Razor do określenia lokalizacji w znaczniku układu, w którym jest renderowana zawartość.

Poniższy przykład kodu pokazuje szablon Razor składnika układu: *MainLayout. Razor*. Układ dziedziczy `LayoutComponentBase` i ustawia `@Body` między paskiem nawigacyjnym i stopką:

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

W aplikacji opartej na jednym z szablonów aplikacji Blazor składnik `MainLayout` (*MainLayout. Razor*) znajduje się w folderze *udostępnionym* aplikacji.

## <a name="default-layout"></a>Układ domyślny

Określ domyślny układ aplikacji w składniku `Router` w pliku *App. Razor* aplikacji. Poniższy składnik `Router`, który jest dostarczany przez domyślne szablony Blazor, ustawia domyślny układ na składnik `MainLayout`:

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

Aby podać domyślny układ zawartości `NotFound`, określ `LayoutView` dla zawartości `NotFound`:

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

Aby uzyskać więcej informacji na temat składnika `Router`, zobacz <xref:blazor/routing>.

Określanie układu jako domyślnego układu w routerze jest przydatnym rozwiązaniem, ponieważ może być zastąpione dla poszczególnych składników lub folderów. Preferuj użycie routera do ustawienia domyślnego układu aplikacji, ponieważ jest to najbardziej ogólna technika.

## <a name="specify-a-layout-in-a-component"></a>Określanie układu w składniku

Użyj dyrektywy Razor `@layout`, aby zastosować układ do składnika. Kompilator konwertuje `@layout` na `LayoutAttribute`, który jest stosowany do klasy składnika.

Zawartość następującego składnika `MasterList` jest wstawiana do `MasterLayout` na pozycji `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

Określanie układu bezpośrednio w składniku przesłania domyślny zestaw *układów* w routerze lub `@layout` dyrektywie zaimportowanej z *_Imports. Razor*.

## <a name="centralized-layout-selection"></a>Scentralizowany wybór układu

Każdy folder aplikacji może opcjonalnie zawierać plik szablonu o nazwie *_Imports. Razor*. Kompilator zawiera dyrektywy określone w pliku Imports we wszystkich szablonach Razor w tym samym folderze i rekursywnie we wszystkich jego podfolderach. W związku z tym plik *_Imports. Razor* zawierający `@layout MyCoolLayout` zapewnia, że wszystkie składniki w folderze używają `MyCoolLayout`. Nie ma potrzeby wielokrotnego dodawania `@layout MyCoolLayout` do wszystkich plików *. Razor* w folderze i podfolderach. dyrektywy `@using` są również stosowane do składników w taki sam sposób.

Następujące *_Imports.* Importy pliku Razor:

* `MyCoolLayout`.,
* Wszystkie składniki Razor w tym samym folderze i wszystkie podfoldery.
* Przestrzeń nazw `BlazorApp1.Data`.
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

Plik *_Imports. Razor* jest podobny do [pliku _ViewImports. cshtml dla widoków i stron Razor,](xref:mvc/views/layout#importing-shared-directives) ale jest stosowany w odniesieniu do plików składników Razor.

Określanie układu w *_Imports. Razor* przesłania układ określony jako *domyślny układ*routera.

## <a name="nested-layouts"></a>Układy zagnieżdżone

Aplikacje mogą składać się z zagnieżdżonych układów. Składnik może odwoływać się do układu, który z kolei odwołuje się do innego układu. Na przykład zagnieżdżanie układów służy do tworzenia struktury menu wielopoziomowego.

Poniższy przykład pokazuje, jak używać układów zagnieżdżonych. Plik *EpisodesComponent. Razor* jest składnikiem do wyświetlenia. Składnik odwołuje się do `MasterListLayout`:

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

Plik *MasterListLayout. Razor* zawiera `MasterListLayout`. Układ odwołuje się do innego układu, `MasterLayout`, gdzie jest renderowany. `EpisodesComponent` jest renderowany w miejscu, w którym pojawia się `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

Na koniec `MasterLayout` w *MasterLayout. Razor* zawiera elementy układu najwyższego poziomu, takie jak nagłówek, menu główne i stopka. `MasterListLayout` z `EpisodesComponent` jest renderowany w miejscu, w którym pojawia się `@Body`:

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/layout>
