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
# <a name="blazor-layouts"></a><span data-ttu-id="ffc3f-103">Układy Blazor</span><span class="sxs-lookup"><span data-stu-id="ffc3f-103">Blazor layouts</span></span>

<span data-ttu-id="ffc3f-104">Przez [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="ffc3f-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="ffc3f-105">Aplikacje zwykle zawierają więcej niż jeden składnik.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="ffc3f-106">Układ elementów, takich jak menu, komunikaty o prawach autorskich i logo musi być obecny na wszystkie elementy.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="ffc3f-107">Skopiować kod z tych elementów układu do wszystkich składników aplikacji nie jest wydajne podejście.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="ffc3f-108">Takie duplikowanie jest trudne do utrzymania i prawdopodobnie prowadzi do niespójne zawartość wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="ffc3f-109">*Układy* rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="ffc3f-110">Technicznie rzecz biorąc układ jest po prostu inny składnik.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="ffc3f-111">Układ jest zdefiniowany w szablonie Razor lub w C# kodu i może zawierać [powiązanie danych](xref:blazor/components#data-binding), [wstrzykiwanie zależności](xref:blazor/dependency-injection)i inne funkcje zwykłych składników.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="ffc3f-112">Włącz dwa dodatkowe aspekty *składnika* do *układu*</span><span class="sxs-lookup"><span data-stu-id="ffc3f-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="ffc3f-113">Składnik układu musi dziedziczyć `LayoutComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-113">The layout component must inherit from `LayoutComponentBase`.</span></span> <span data-ttu-id="ffc3f-114">`LayoutComponentBase` definiuje `Body` właściwość, która zawiera zawartość do wyrenderowania wewnątrz układu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-114">`LayoutComponentBase` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="ffc3f-115">Składnik układ używa `Body` właściwości w celu określenia, w którym treść powinna być renderowany przy użyciu składni Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="ffc3f-116">Podczas renderowania, `@Body` zastępuje zawartość układu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="ffc3f-117">Poniższy przykład kodu pokazuje szablon Razor składnika układu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="ffc3f-118">Zwróć uwagę na użycie `LayoutComponentBase` i `@Body`:</span><span class="sxs-lookup"><span data-stu-id="ffc3f-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="ffc3f-119">Używanie układu w składniku</span><span class="sxs-lookup"><span data-stu-id="ffc3f-119">Use a layout in a component</span></span>

<span data-ttu-id="ffc3f-120">Użyj dyrektywy Razor `@layout` można zastosować układu do składnika.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="ffc3f-121">Kompilator konwertuje tej dyrektywy do `LayoutAttribute`, który jest stosowany do klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="ffc3f-122">W poniższym przykładzie kodu pokazano pojęcia.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="ffc3f-123">Zawartość tego składnika jest wstawiany do *MasterLayout* w położeniu `@Body`:</span><span class="sxs-lookup"><span data-stu-id="ffc3f-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="ffc3f-124">Wybór układu scentralizowane</span><span class="sxs-lookup"><span data-stu-id="ffc3f-124">Centralized layout selection</span></span>

<span data-ttu-id="ffc3f-125">Każdy folder z aplikacji, można opcjonalnie zawiera plik szablonu o nazwie *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="ffc3f-126">Kompilator zawiera dyrektywy określone w pliku importu widoku we wszystkich szablony Razor, w tym samym folderze i cyklicznie we wszystkich jego podfolderów.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="ffc3f-127">W związku z tym *_ViewImports.cshtml* plik zawierający `@layout MainLayout` zapewnia, że wszystkie składniki użycie folderu *MainLayout* układu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="ffc3f-128">Nie ma potrzeby można wielokrotnie dodać `@layout` do wszystkich *.razor* plików.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="ffc3f-129">Należy zauważyć, że korzysta z domyślnego szablonu *_ViewImports.cshtml* mechanizm wyboru układu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="ffc3f-130">Zawiera nowo utworzoną aplikację *_ViewImports.cshtml* w pliku *składniki/strony* folderu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="ffc3f-131">Układy zagnieżdżonych</span><span class="sxs-lookup"><span data-stu-id="ffc3f-131">Nested layouts</span></span>

<span data-ttu-id="ffc3f-132">Aplikacje może zawierać zagnieżdżone układów.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="ffc3f-133">Składnik może odwoływać się układ, który z kolei odwołuje się do innego układu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="ffc3f-134">Na przykład zagnieżdżenia układów może służyć do odzwierciedlenia struktury wielopoziomowe menu.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="ffc3f-135">Poniższe przykłady kodu przedstawiają sposób używać zagnieżdżonych układów.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="ffc3f-136">*EpisodesComponent.cshtml* plik jest składnikiem do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="ffc3f-137">Należy pamiętać, że składnik odwołuje się do układu `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="ffc3f-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ffc3f-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="ffc3f-139">*MasterListLayout.cshtml* plik zawiera `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="ffc3f-140">Układ odwołuje się do innego układu `MasterLayout`, gdzie ma to być osadzone.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="ffc3f-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ffc3f-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="ffc3f-142">Na koniec `MasterLayout` zawiera elementy układu najwyższego poziomu, takie jak nagłówek, stopka i menu głównego.</span><span class="sxs-lookup"><span data-stu-id="ffc3f-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="ffc3f-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ffc3f-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
