---
title: Układy składniki razor
author: guardrex
description: Dowiedz się, jak tworzyć składniki wielokrotnego użytku układu dla aplikacji Blazor i składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159470"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="91268-103">Układy składniki razor</span><span class="sxs-lookup"><span data-stu-id="91268-103">Razor Components layouts</span></span>

<span data-ttu-id="91268-104">Przez [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="91268-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="91268-105">Aplikacje zwykle zawierają więcej niż jedną stronę.</span><span class="sxs-lookup"><span data-stu-id="91268-105">Apps typically contain more than one page.</span></span> <span data-ttu-id="91268-106">Układ elementów, takich jak menu, komunikaty o prawach autorskich i logo musi być obecny na wszystkich stronach.</span><span class="sxs-lookup"><span data-stu-id="91268-106">Layout elements, such as menus, copyright messages, and logos, must be present on all pages.</span></span> <span data-ttu-id="91268-107">Skopiować kod z tych elementów układu do wszystkich stron aplikacji nie jest wydajne rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="91268-107">Copying the code of these layout elements into all of the pages of an app isn't an efficient solution.</span></span> <span data-ttu-id="91268-108">Takie duplikowanie jest trudne do utrzymania i prawdopodobnie prowadzi do niespójne zawartość wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="91268-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="91268-109">*Układy* rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="91268-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="91268-110">Technicznie rzecz biorąc układ jest po prostu inny składnik.</span><span class="sxs-lookup"><span data-stu-id="91268-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="91268-111">Układ jest zdefiniowany w szablonie Razor lub w C# kodu i może zawierać wiązania danych, wstrzykiwanie zależności i inne funkcje zwykłych składników.</span><span class="sxs-lookup"><span data-stu-id="91268-111">A layout is defined in a Razor template or in C# code and can contain data binding, dependency injection, and other ordinary features of components.</span></span> <span data-ttu-id="91268-112">Włącz dwa dodatkowe aspekty *składnika* do *układ*:</span><span class="sxs-lookup"><span data-stu-id="91268-112">Two additional aspects turn a *component* into a *layout*:</span></span>

* <span data-ttu-id="91268-113">Składnik układu musi dziedziczyć `BlazorLayoutComponent`.</span><span class="sxs-lookup"><span data-stu-id="91268-113">The layout component must inherit from `BlazorLayoutComponent`.</span></span> <span data-ttu-id="91268-114">`BlazorLayoutComponent` definiuje `Body` właściwość, która zawiera zawartość do wyrenderowania wewnątrz układu.</span><span class="sxs-lookup"><span data-stu-id="91268-114">`BlazorLayoutComponent` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="91268-115">Składnik układ używa `Body` właściwości w celu określenia, w którym treść powinna być renderowany przy użyciu składni Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="91268-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="91268-116">Podczas renderowania, `@Body` zastępuje zawartość układu.</span><span class="sxs-lookup"><span data-stu-id="91268-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="91268-117">Poniższy przykład kodu pokazuje szablon Razor składnika układu.</span><span class="sxs-lookup"><span data-stu-id="91268-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="91268-118">Zwróć uwagę na użycie `BlazorLayoutComponent` i `@Body`:</span><span class="sxs-lookup"><span data-stu-id="91268-118">Note the use of `BlazorLayoutComponent` and `@Body`:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="91268-119">Używanie układu w składniku</span><span class="sxs-lookup"><span data-stu-id="91268-119">Use a layout in a component</span></span>

<span data-ttu-id="91268-120">Użyj dyrektywy Razor `@layout` można zastosować układu do składnika.</span><span class="sxs-lookup"><span data-stu-id="91268-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="91268-121">Kompilator konwertuje tej dyrektywy do `LayoutAttribute`, który jest stosowany do klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="91268-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="91268-122">W poniższym przykładzie kodu pokazano pojęcia.</span><span class="sxs-lookup"><span data-stu-id="91268-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="91268-123">Zawartość tego składnika jest wstawiany do *MasterLayout* w położeniu `@Body`:</span><span class="sxs-lookup"><span data-stu-id="91268-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="91268-124">Wybór układu scentralizowane</span><span class="sxs-lookup"><span data-stu-id="91268-124">Centralized layout selection</span></span>

<span data-ttu-id="91268-125">Każdy folder z aplikacji, można opcjonalnie zawiera plik szablonu o nazwie *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="91268-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="91268-126">Kompilator zawiera dyrektywy określone w pliku importu widoku we wszystkich szablony Razor, w tym samym folderze i cyklicznie we wszystkich jego podfolderów.</span><span class="sxs-lookup"><span data-stu-id="91268-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="91268-127">W związku z tym *_ViewImports.cshtml* plik zawierający `@layout MainLayout` zapewnia, że wszystkie składniki użycie folderu *MainLayout* układu.</span><span class="sxs-lookup"><span data-stu-id="91268-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="91268-128">Nie ma potrzeby można wielokrotnie dodać `@layout` do wszystkich  *\*.cshtml* plików.</span><span class="sxs-lookup"><span data-stu-id="91268-128">There's no need to repeatedly add `@layout` to all of the *\*.cshtml* files.</span></span>

<span data-ttu-id="91268-129">Należy zauważyć, że korzysta z domyślnego szablonu *_ViewImports.cshtml* mechanizm wyboru układu.</span><span class="sxs-lookup"><span data-stu-id="91268-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="91268-130">Zawiera nowo utworzoną aplikację *_ViewImports.cshtml* w pliku *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="91268-130">A newly created app contains the *_ViewImports.cshtml* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="91268-131">Układy zagnieżdżonych</span><span class="sxs-lookup"><span data-stu-id="91268-131">Nested layouts</span></span>

<span data-ttu-id="91268-132">Aplikacje może zawierać zagnieżdżone układów.</span><span class="sxs-lookup"><span data-stu-id="91268-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="91268-133">Składnik może odwoływać się układ, który z kolei odwołuje się do innego układu.</span><span class="sxs-lookup"><span data-stu-id="91268-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="91268-134">Na przykład zagnieżdżenia układów może służyć do odzwierciedlenia struktury wielopoziomowe menu.</span><span class="sxs-lookup"><span data-stu-id="91268-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="91268-135">Poniższe przykłady kodu przedstawiają sposób używać zagnieżdżonych układów.</span><span class="sxs-lookup"><span data-stu-id="91268-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="91268-136">*CustomersComponent.cshtml* plik jest składnikiem do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="91268-136">The *CustomersComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="91268-137">Należy pamiętać, że składnik odwołuje się do układu `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="91268-137">Note that the component references the layout `MasterDataLayout`.</span></span>

<span data-ttu-id="91268-138">*CustomersComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="91268-138">*CustomersComponent.cshtml*:</span></span>

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

<span data-ttu-id="91268-139">*MasterDataLayout.cshtml* plik zawiera `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="91268-139">The *MasterDataLayout.cshtml* file provides the `MasterDataLayout`.</span></span> <span data-ttu-id="91268-140">Układ odwołuje się do innego układu `MainLayout`, gdzie ma to być osadzone.</span><span class="sxs-lookup"><span data-stu-id="91268-140">The layout references another layout, `MainLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="91268-141">*MasterDataLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="91268-141">*MasterDataLayout.cshtml*:</span></span>

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

<span data-ttu-id="91268-142">Na koniec `MainLayout` zawiera elementy układu najwyższego poziomu, takie jak nagłówek, stopka i menu głównego.</span><span class="sxs-lookup"><span data-stu-id="91268-142">Finally, `MainLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="91268-143">*MainLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="91268-143">*MainLayout.cshtml*:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
