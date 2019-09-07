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
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="110d2-103">ASP.NET Core układy Blazor</span><span class="sxs-lookup"><span data-stu-id="110d2-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="110d2-104">Autorzy [Rainer Stropek](https://www.timecockpit.com) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="110d2-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="110d2-105">Niektóre elementy aplikacji, takie jak menu, wiadomości o prawach autorskich i logo firmy, są zwykle częścią ogólnego układu aplikacji i są używane przez każdy składnik w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="110d2-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="110d2-106">Kopiowanie kodu tych elementów do wszystkich składników aplikacji nie jest skutecznym podejściem&mdash;za każdym razem, gdy jeden z elementów wymaga aktualizacji, należy zaktualizować każdy składnik.</span><span class="sxs-lookup"><span data-stu-id="110d2-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="110d2-107">Taka duplikacja jest trudna do utrzymania i może prowadzić do niespójnej zawartości z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="110d2-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="110d2-108">*Układy* rozwiązują ten problem.</span><span class="sxs-lookup"><span data-stu-id="110d2-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="110d2-109">Technicznie, układ jest tylko innym składnikiem.</span><span class="sxs-lookup"><span data-stu-id="110d2-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="110d2-110">Układ jest zdefiniowany w szablonie Razor lub w C# kodzie i może używać [powiązań danych](xref:blazor/components#data-binding), [iniekcji zależności](xref:blazor/dependency-injection)i innych scenariuszy składników.</span><span class="sxs-lookup"><span data-stu-id="110d2-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="110d2-111">Aby przekształcić *składnik* do *układu*, składnik:</span><span class="sxs-lookup"><span data-stu-id="110d2-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="110d2-112">Dziedziczy z `LayoutComponentBase`, który `Body` definiuje właściwość dla renderowanej zawartości wewnątrz układu.</span><span class="sxs-lookup"><span data-stu-id="110d2-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="110d2-113">Używa składnia Razor `@Body` do określenia lokalizacji w znaczniku układu, w którym jest renderowana zawartość.</span><span class="sxs-lookup"><span data-stu-id="110d2-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="110d2-114">Poniższy przykład kodu pokazuje szablon Razor składnika układu: *MainLayout. Razor*.</span><span class="sxs-lookup"><span data-stu-id="110d2-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="110d2-115">Układ dziedziczy `LayoutComponentBase` i `@Body` ustawia między paskiem nawigacyjnym i stopką:</span><span class="sxs-lookup"><span data-stu-id="110d2-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="110d2-116">W aplikacji opartej na jednym z szablonów `MainLayout` aplikacji Blazor składnik (*MainLayout. Razor*) znajduje się w folderze *udostępnionym* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="110d2-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="110d2-117">Układ domyślny</span><span class="sxs-lookup"><span data-stu-id="110d2-117">Default layout</span></span>

<span data-ttu-id="110d2-118">Określ domyślny układ aplikacji w `Router` składniku w pliku *App. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="110d2-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="110d2-119">Poniższy `Router` składnik, który jest dostarczany przez domyślne szablony Blazor, ustawia domyślny układ `MainLayout` składnika:</span><span class="sxs-lookup"><span data-stu-id="110d2-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="110d2-120">Aby podać domyślny układ `NotFound` zawartości, `LayoutView` Określ dla `NotFound` zawartości:</span><span class="sxs-lookup"><span data-stu-id="110d2-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="110d2-121">Aby uzyskać więcej informacji na `Router` temat składnika, <xref:blazor/routing>Zobacz.</span><span class="sxs-lookup"><span data-stu-id="110d2-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="110d2-122">Określanie układu w składniku</span><span class="sxs-lookup"><span data-stu-id="110d2-122">Specify a layout in a component</span></span>

<span data-ttu-id="110d2-123">Użyj dyrektywy `@layout` Razor, aby zastosować układ do składnika.</span><span class="sxs-lookup"><span data-stu-id="110d2-123">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="110d2-124">Kompilator konwertuje `@layout` `LayoutAttribute`do, który jest stosowany do klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="110d2-124">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="110d2-125">Zawartość następującego `MasterList` składnika jest wstawiana `MasterLayout` do pozycji `@Body`:</span><span class="sxs-lookup"><span data-stu-id="110d2-125">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="110d2-126">Scentralizowany wybór układu</span><span class="sxs-lookup"><span data-stu-id="110d2-126">Centralized layout selection</span></span>

<span data-ttu-id="110d2-127">Każdy folder aplikacji może opcjonalnie zawierać plik szablonu o nazwie *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="110d2-127">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="110d2-128">Kompilator zawiera dyrektywy określone w pliku Imports we wszystkich szablonach Razor w tym samym folderze i rekursywnie we wszystkich jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="110d2-128">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="110d2-129">W związku z tym plik *_Imports. Razor* zawiera `@layout MyCoolLayout` pewność, że wszystkie składniki w folderze są używane `MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="110d2-129">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="110d2-130">Nie trzeba wielokrotnie dodawać `@layout MyCoolLayout` do wszystkich plików *Razor* w folderze i podfolderach.</span><span class="sxs-lookup"><span data-stu-id="110d2-130">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="110d2-131">`@using`dyrektywy są również stosowane do składników w ten sam sposób.</span><span class="sxs-lookup"><span data-stu-id="110d2-131">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="110d2-132">Następujące Importy pliku *_Imports. Razor* :</span><span class="sxs-lookup"><span data-stu-id="110d2-132">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="110d2-133">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="110d2-133">`MyCoolLayout`.</span></span>
* <span data-ttu-id="110d2-134">Wszystkie składniki Razor w tym samym folderze i wszystkie podfoldery.</span><span class="sxs-lookup"><span data-stu-id="110d2-134">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="110d2-135">`BlazorApp1.Data` Przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="110d2-135">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="110d2-136">Plik *_Imports. Razor* jest podobny do [pliku _ViewImports. cshtml dla widoków i stron Razor,](xref:mvc/views/layout#importing-shared-directives) ale jest stosowany w odniesieniu do plików składników Razor.</span><span class="sxs-lookup"><span data-stu-id="110d2-136">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="110d2-137">Układy zagnieżdżone</span><span class="sxs-lookup"><span data-stu-id="110d2-137">Nested layouts</span></span>

<span data-ttu-id="110d2-138">Aplikacje mogą składać się z zagnieżdżonych układów.</span><span class="sxs-lookup"><span data-stu-id="110d2-138">Apps can consist of nested layouts.</span></span> <span data-ttu-id="110d2-139">Składnik może odwoływać się do układu, który z kolei odwołuje się do innego układu.</span><span class="sxs-lookup"><span data-stu-id="110d2-139">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="110d2-140">Na przykład zagnieżdżanie układów służy do tworzenia struktury menu wielopoziomowego.</span><span class="sxs-lookup"><span data-stu-id="110d2-140">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="110d2-141">Poniższy przykład pokazuje, jak używać układów zagnieżdżonych.</span><span class="sxs-lookup"><span data-stu-id="110d2-141">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="110d2-142">Plik *EpisodesComponent. Razor* jest składnikiem do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="110d2-142">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="110d2-143">Składnik odwołuje się `MasterListLayout`do:</span><span class="sxs-lookup"><span data-stu-id="110d2-143">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="110d2-144">Plik *MasterListLayout. Razor* zawiera `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="110d2-144">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="110d2-145">Układ odwołuje się do innego `MasterLayout`układu, w którym jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="110d2-145">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="110d2-146">`EpisodesComponent`jest renderowany w `@Body` miejscu, gdzie pojawia się:</span><span class="sxs-lookup"><span data-stu-id="110d2-146">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="110d2-147">Na koniec w *MasterLayout. Razor* zawiera elementy układu najwyższego poziomu, takie jak nagłówek, menu główne i stopka. `MasterLayout`</span><span class="sxs-lookup"><span data-stu-id="110d2-147">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="110d2-148">`MasterListLayout`z renderowanym miejscem `EpisodesComponent` , `@Body` gdzie pojawia się:</span><span class="sxs-lookup"><span data-stu-id="110d2-148">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="110d2-149">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="110d2-149">Additional resources</span></span>

* <xref:mvc/views/layout>
