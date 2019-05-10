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
# <a name="blazor-layouts"></a><span data-ttu-id="31c76-103">Układy Blazor</span><span class="sxs-lookup"><span data-stu-id="31c76-103">Blazor layouts</span></span>

<span data-ttu-id="31c76-104">Przez [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="31c76-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="31c76-105">Niektóre elementy aplikacji, takich jak menu, komunikaty o prawach autorskich i logo firmy, są zazwyczaj ogólny układu aplikacji i używane przez każdy składnik w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="31c76-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="31c76-106">Skopiować kod z tych elementów do wszystkich składników aplikacji nie jest wydajne podejście&mdash;za każdym razem, gdy jeden z elementów wymaga aktualizacji, każdy składnik musi zostać zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="31c76-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="31c76-107">Takie duplikowanie jest trudne do utrzymania i może spowodować niespójne zawartość wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="31c76-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="31c76-108">*Układy* rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="31c76-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="31c76-109">Technicznie rzecz biorąc układ jest po prostu inny składnik.</span><span class="sxs-lookup"><span data-stu-id="31c76-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="31c76-110">Układ jest zdefiniowany w szablonie Razor lub w C# kod i użyć [powiązanie danych](xref:blazor/components#data-binding), [wstrzykiwanie zależności](xref:blazor/dependency-injection)oraz innych scenariuszy składnika.</span><span class="sxs-lookup"><span data-stu-id="31c76-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="31c76-111">Aby włączyć *składnika* do *układ*, składnik:</span><span class="sxs-lookup"><span data-stu-id="31c76-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="31c76-112">Dziedziczy `LayoutComponentBase`, która definiuje `Body` właściwość, która zawiera zawartość do wyrenderowania wewnątrz układu.</span><span class="sxs-lookup"><span data-stu-id="31c76-112">Inherits from `LayoutComponentBase`, which defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="31c76-113">Używa składni Razor `@Body` Aby określić lokalizację, w znacznikach, której zawartość ma być renderowany.</span><span class="sxs-lookup"><span data-stu-id="31c76-113">Uses the Razor syntax `@Body` to specify the location in the markup where the content should be rendered.</span></span>

<span data-ttu-id="31c76-114">Poniższy przykład kodu pokazuje szablon Razor składnika układ *MainLayout.razor*.</span><span class="sxs-lookup"><span data-stu-id="31c76-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="31c76-115">Układ dziedziczy `LayoutComponentBase` i ustawia `@Body` między paskiem nawigacji i stopki:</span><span class="sxs-lookup"><span data-stu-id="31c76-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="31c76-116">Określ układ w składniku</span><span class="sxs-lookup"><span data-stu-id="31c76-116">Specify a layout in a component</span></span>

<span data-ttu-id="31c76-117">Użyj dyrektywy Razor `@layout` można zastosować układu do składnika.</span><span class="sxs-lookup"><span data-stu-id="31c76-117">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="31c76-118">Kompilator konwertuje `@layout` do `LayoutAttribute`, który jest stosowany do klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="31c76-118">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="31c76-119">Zawartość następującym składniku *MasterList.razor*, jest wstawiany do *MainLayout* w położeniu `@Body`.</span><span class="sxs-lookup"><span data-stu-id="31c76-119">The content of the following component, *MasterList.razor*, is inserted into the *MainLayout* at the position of `@Body`.</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="31c76-120">Wybór układu scentralizowane</span><span class="sxs-lookup"><span data-stu-id="31c76-120">Centralized layout selection</span></span>

<span data-ttu-id="31c76-121">Każdy folder aplikacji opcjonalnie mogą zawierać plik szablonu o nazwie *_Imports.razor*.</span><span class="sxs-lookup"><span data-stu-id="31c76-121">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="31c76-122">Kompilator zawiera dyrektywy określone w pliku importu we wszystkich szablony Razor, w tym samym folderze i cyklicznie we wszystkich jego podfolderów.</span><span class="sxs-lookup"><span data-stu-id="31c76-122">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="31c76-123">W związku z tym *_Imports.razor* plik zawierający `@layout MainLayout` zapewnia, że wszystkie składniki użycie folderu *MainLayout*.</span><span class="sxs-lookup"><span data-stu-id="31c76-123">Therefore, a *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use *MainLayout*.</span></span> <span data-ttu-id="31c76-124">Nie ma potrzeby można wielokrotnie dodać `@layout MainLayout` do wszystkich *.razor* pliki znajdujące się w folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="31c76-124">There's no need to repeatedly add `@layout MainLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="31c76-125">`@using` dyrektywy są również stosowane do składników w taki sam sposób.</span><span class="sxs-lookup"><span data-stu-id="31c76-125">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="31c76-126">Następujące *_Imports.razor* pliku importu:</span><span class="sxs-lookup"><span data-stu-id="31c76-126">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="31c76-127">`MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="31c76-127">`MainLayout`.</span></span>
* <span data-ttu-id="31c76-128">Wszystkie składniki Razor, w tym samym folderze i wszelkich podfolderów.</span><span class="sxs-lookup"><span data-stu-id="31c76-128">All Razor components in a the same folder and any subfolders.</span></span>
* <span data-ttu-id="31c76-129">`BlazorApp1.Data` Przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="31c76-129">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="31c76-130">*_Imports.razor* pliku jest podobny do [widokami Razor i stron w pliku _ViewImports.cshtml](xref:mvc/views/layout#importing-shared-directives) jednak stosowane do plików składników Razor.</span><span class="sxs-lookup"><span data-stu-id="31c76-130">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="31c76-131">Użyj szablonów Blazor *_Imports.razor* pliki do wyboru układu.</span><span class="sxs-lookup"><span data-stu-id="31c76-131">The Blazor templates use *_Imports.razor* files for layout selection.</span></span> <span data-ttu-id="31c76-132">Zawiera aplikacji utworzonych na podstawie szablonu Blazor *_Imports.razor* plik w folderze głównym projektu i w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="31c76-132">An app created from a Blazor template contains the *_Imports.razor* file in the root of the project and in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="31c76-133">Układy zagnieżdżonych</span><span class="sxs-lookup"><span data-stu-id="31c76-133">Nested layouts</span></span>

<span data-ttu-id="31c76-134">Aplikacje może zawierać zagnieżdżone układów.</span><span class="sxs-lookup"><span data-stu-id="31c76-134">Apps can consist of nested layouts.</span></span> <span data-ttu-id="31c76-135">Składnik może odwoływać się układ, który z kolei odwołuje się do innego układu.</span><span class="sxs-lookup"><span data-stu-id="31c76-135">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="31c76-136">Na przykład układy zagnieżdżenia może służyć do tworzenia struktury wielopoziomowe menu.</span><span class="sxs-lookup"><span data-stu-id="31c76-136">For example, nesting layouts can be used to create a multi-level menu structure.</span></span>

<span data-ttu-id="31c76-137">Poniższy przykład pokazuje, jak używać zagnieżdżonych układów.</span><span class="sxs-lookup"><span data-stu-id="31c76-137">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="31c76-138">*EpisodesComponent.razor* plik jest składnikiem do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="31c76-138">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="31c76-139">Odwołania do składników `MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="31c76-139">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="31c76-140">*MasterListLayout.razor* plik zawiera `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="31c76-140">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="31c76-141">Układ odwołuje się do innego układu `MasterLayout`, gdy jest on renderowany.</span><span class="sxs-lookup"><span data-stu-id="31c76-141">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="31c76-142">`EpisodesComponent` gdzie renderowanego `@Body` pojawia się:</span><span class="sxs-lookup"><span data-stu-id="31c76-142">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="31c76-143">Na koniec `MasterLayout` w *MasterLayout.razor* zawiera elementy układu najwyższego poziomu, takie jak nagłówek, menu głównego i stopki.</span><span class="sxs-lookup"><span data-stu-id="31c76-143">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="31c76-144">*MasterListLayout* z *EpisodesComponent* — zostaną zrenderowane gdzie `@Body` pojawia się:</span><span class="sxs-lookup"><span data-stu-id="31c76-144">*MasterListLayout* with *EpisodesComponent* are rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="31c76-145">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="31c76-145">Additional resources</span></span>

* <xref:mvc/views/layout>
