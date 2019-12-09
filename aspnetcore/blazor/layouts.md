---
title: ASP.NET Core układy Blazor
author: guardrex
description: Dowiedz się, jak tworzyć składniki układu wielokrotnego użytku dla aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/layouts
ms.openlocfilehash: 90acfb0d4e9daadb12be79de6bd0c99fc545697a
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944060"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a><span data-ttu-id="8d747-103">ASP.NET Core układy Blazor</span><span class="sxs-lookup"><span data-stu-id="8d747-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="8d747-104">Autorzy [Rainer Stropek](https://www.timecockpit.com) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8d747-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8d747-105">Niektóre elementy aplikacji, takie jak menu, wiadomości o prawach autorskich i logo firmy, są zwykle częścią ogólnego układu aplikacji i są używane przez każdy składnik w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8d747-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="8d747-106">Kopiowanie kodu tych elementów do wszystkich składników aplikacji nie jest efektywnym podejściem&mdash;za każdym razem, gdy jeden z elementów wymaga aktualizacji, należy zaktualizować każdy składnik.</span><span class="sxs-lookup"><span data-stu-id="8d747-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="8d747-107">Taka duplikacja jest trudna do utrzymania i może prowadzić do niespójnej zawartości z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="8d747-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="8d747-108">*Układy* rozwiązują ten problem.</span><span class="sxs-lookup"><span data-stu-id="8d747-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="8d747-109">Technicznie, układ jest tylko innym składnikiem.</span><span class="sxs-lookup"><span data-stu-id="8d747-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="8d747-110">Układ jest zdefiniowany w szablonie Razor lub w C# kodzie i może używać [powiązań danych](xref:blazor/components#data-binding), [iniekcji zależności](xref:blazor/dependency-injection)i innych scenariuszy składników.</span><span class="sxs-lookup"><span data-stu-id="8d747-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="8d747-111">Aby przekształcić *składnik* do *układu*, składnik:</span><span class="sxs-lookup"><span data-stu-id="8d747-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="8d747-112">Dziedziczy z `LayoutComponentBase`, który definiuje Właściwość `Body` dla renderowanej zawartości wewnątrz układu.</span><span class="sxs-lookup"><span data-stu-id="8d747-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="8d747-113">Używa `@Body` składnia Razor do określenia lokalizacji w znaczniku układu, w którym jest renderowana zawartość.</span><span class="sxs-lookup"><span data-stu-id="8d747-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="8d747-114">Poniższy przykład kodu pokazuje szablon Razor składnika układu: *MainLayout. Razor*.</span><span class="sxs-lookup"><span data-stu-id="8d747-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="8d747-115">Układ dziedziczy `LayoutComponentBase` i ustawia `@Body` między paskiem nawigacyjnym i stopką:</span><span class="sxs-lookup"><span data-stu-id="8d747-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="8d747-116">W aplikacji opartej na jednym z szablonów aplikacji Blazor składnik `MainLayout` (*MainLayout. Razor*) znajduje się w folderze *udostępnionym* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8d747-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="8d747-117">Układ domyślny</span><span class="sxs-lookup"><span data-stu-id="8d747-117">Default layout</span></span>

<span data-ttu-id="8d747-118">Określ domyślny układ aplikacji w składniku `Router` w pliku *App. Razor* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8d747-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="8d747-119">Poniższy składnik `Router`, który jest dostarczany przez domyślne szablony Blazor, ustawia domyślny układ na składnik `MainLayout`:</span><span class="sxs-lookup"><span data-stu-id="8d747-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="8d747-120">Aby podać domyślny układ zawartości `NotFound`, określ `LayoutView` dla zawartości `NotFound`:</span><span class="sxs-lookup"><span data-stu-id="8d747-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="8d747-121">Aby uzyskać więcej informacji na temat składnika `Router`, zobacz <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="8d747-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

<span data-ttu-id="8d747-122">Określanie układu jako domyślnego układu w routerze jest przydatnym rozwiązaniem, ponieważ może być zastąpione dla poszczególnych składników lub folderów.</span><span class="sxs-lookup"><span data-stu-id="8d747-122">Specifying the layout as a default layout in the router is a useful practice because it can be overridden on a per-component or per-folder basis.</span></span> <span data-ttu-id="8d747-123">Preferuj użycie routera do ustawienia domyślnego układu aplikacji, ponieważ jest to najbardziej ogólna technika.</span><span class="sxs-lookup"><span data-stu-id="8d747-123">Prefer using the router to set the app's default layout because it's the most general technique.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="8d747-124">Określanie układu w składniku</span><span class="sxs-lookup"><span data-stu-id="8d747-124">Specify a layout in a component</span></span>

<span data-ttu-id="8d747-125">Użyj dyrektywy Razor `@layout`, aby zastosować układ do składnika.</span><span class="sxs-lookup"><span data-stu-id="8d747-125">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="8d747-126">Kompilator konwertuje `@layout` na `LayoutAttribute`, który jest stosowany do klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="8d747-126">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="8d747-127">Zawartość następującego składnika `MasterList` jest wstawiana do `MasterLayout` na pozycji `@Body`:</span><span class="sxs-lookup"><span data-stu-id="8d747-127">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

<span data-ttu-id="8d747-128">Określanie układu bezpośrednio w składniku przesłania domyślny zestaw *układów* w routerze lub `@layout` dyrektywie zaimportowanej z *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="8d747-128">Specifying the layout directly in a component overrides a *default layout* set in the router or an `@layout` directive imported from *_Imports.razor*.</span></span>

## <a name="centralized-layout-selection"></a><span data-ttu-id="8d747-129">Scentralizowany wybór układu</span><span class="sxs-lookup"><span data-stu-id="8d747-129">Centralized layout selection</span></span>

<span data-ttu-id="8d747-130">Każdy folder aplikacji może opcjonalnie zawierać plik szablonu o nazwie *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="8d747-130">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="8d747-131">Kompilator zawiera dyrektywy określone w pliku Imports we wszystkich szablonach Razor w tym samym folderze i rekursywnie we wszystkich jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="8d747-131">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="8d747-132">W związku z tym plik *_Imports. Razor* zawierający `@layout MyCoolLayout` zapewnia, że wszystkie składniki w folderze używają `MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="8d747-132">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="8d747-133">Nie ma potrzeby wielokrotnego dodawania `@layout MyCoolLayout` do wszystkich plików *. Razor* w folderze i podfolderach.</span><span class="sxs-lookup"><span data-stu-id="8d747-133">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="8d747-134">dyrektywy `@using` są również stosowane do składników w taki sam sposób.</span><span class="sxs-lookup"><span data-stu-id="8d747-134">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="8d747-135">Następujące *_Imports.* Importy pliku Razor:</span><span class="sxs-lookup"><span data-stu-id="8d747-135">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="8d747-136">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="8d747-136">`MyCoolLayout`.</span></span>
* <span data-ttu-id="8d747-137">Wszystkie składniki Razor w tym samym folderze i wszystkie podfoldery.</span><span class="sxs-lookup"><span data-stu-id="8d747-137">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="8d747-138">Przestrzeń nazw `BlazorApp1.Data`.</span><span class="sxs-lookup"><span data-stu-id="8d747-138">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="8d747-139">Plik *_Imports. Razor* jest podobny do [pliku _ViewImports. cshtml dla widoków i stron Razor,](xref:mvc/views/layout#importing-shared-directives) ale jest stosowany w odniesieniu do plików składników Razor.</span><span class="sxs-lookup"><span data-stu-id="8d747-139">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="8d747-140">Określanie układu w *_Imports. Razor* przesłania układ określony jako *domyślny układ*routera.</span><span class="sxs-lookup"><span data-stu-id="8d747-140">Specifying a layout in *_Imports.razor* overrides a layout specified as the router's *default layout*.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="8d747-141">Układy zagnieżdżone</span><span class="sxs-lookup"><span data-stu-id="8d747-141">Nested layouts</span></span>

<span data-ttu-id="8d747-142">Aplikacje mogą składać się z zagnieżdżonych układów.</span><span class="sxs-lookup"><span data-stu-id="8d747-142">Apps can consist of nested layouts.</span></span> <span data-ttu-id="8d747-143">Składnik może odwoływać się do układu, który z kolei odwołuje się do innego układu.</span><span class="sxs-lookup"><span data-stu-id="8d747-143">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="8d747-144">Na przykład zagnieżdżanie układów służy do tworzenia struktury menu wielopoziomowego.</span><span class="sxs-lookup"><span data-stu-id="8d747-144">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="8d747-145">Poniższy przykład pokazuje, jak używać układów zagnieżdżonych.</span><span class="sxs-lookup"><span data-stu-id="8d747-145">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="8d747-146">Plik *EpisodesComponent. Razor* jest składnikiem do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="8d747-146">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="8d747-147">Składnik odwołuje się do `MasterListLayout`:</span><span class="sxs-lookup"><span data-stu-id="8d747-147">The component references the `MasterListLayout`:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="8d747-148">Plik *MasterListLayout. Razor* zawiera `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="8d747-148">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="8d747-149">Układ odwołuje się do innego układu, `MasterLayout`, gdzie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="8d747-149">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="8d747-150">`EpisodesComponent` jest renderowany w miejscu, w którym pojawia się `@Body`:</span><span class="sxs-lookup"><span data-stu-id="8d747-150">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="8d747-151">Na koniec `MasterLayout` w *MasterLayout. Razor* zawiera elementy układu najwyższego poziomu, takie jak nagłówek, menu główne i stopka.</span><span class="sxs-lookup"><span data-stu-id="8d747-151">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="8d747-152">`MasterListLayout` z `EpisodesComponent` jest renderowany w miejscu, w którym pojawia się `@Body`:</span><span class="sxs-lookup"><span data-stu-id="8d747-152">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="8d747-153">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8d747-153">Additional resources</span></span>

* <xref:mvc/views/layout>
