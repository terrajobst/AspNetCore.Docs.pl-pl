---
title: "Układ"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a><span data-ttu-id="930e6-102">Układ</span><span class="sxs-lookup"><span data-stu-id="930e6-102">Layout</span></span>

<span data-ttu-id="930e6-103">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="930e6-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="930e6-104">Widoki często Udostępnianie elementów wizualnych i programowe.</span><span class="sxs-lookup"><span data-stu-id="930e6-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="930e6-105">W tym artykule dowiesz się, jak używać typowych układów, udostępnić dyrektywy i uruchomić typowy kod przed renderowania widoków w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="930e6-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="930e6-106">Co to jest układ</span><span class="sxs-lookup"><span data-stu-id="930e6-106">What is a Layout</span></span>

<span data-ttu-id="930e6-107">Większość aplikacji sieci web mają wspólne układu, który zapewnia spójne środowisko użytkownika zgodnie z ich przechodzenie między stronami.</span><span class="sxs-lookup"><span data-stu-id="930e6-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="930e6-108">Układ zazwyczaj zawiera wspólne elementy interfejsu użytkownika, takie jak nagłówek aplikacji, nawigacji lub elementy menu i stopce strony.</span><span class="sxs-lookup"><span data-stu-id="930e6-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Przykładowy układ strony](layout/_static/page-layout.png)

<span data-ttu-id="930e6-110">Wspólnej struktury HTML, takich jak skrypty i arkusze stylów również często używane przez wiele stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="930e6-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="930e6-111">Wszystkie te elementy udostępnionych można zdefiniować w *układu* pliku, który można odwoływać się do dowolnego widoku używany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="930e6-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="930e6-112">Układy zmniejszyć zduplikowany kod w widokach, pomagając im wykonaj [zasady nie powtarzaj samodzielnie (BIBUŁĄ)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="930e6-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="930e6-113">Konwencja, nosi nazwę domyślny układ dla aplikacji ASP.NET `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="930e6-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="930e6-114">Szablon projektu Visual Studio platformy ASP.NET Core MVC obejmuje to pliku układu `Views/Shared` folderu:</span><span class="sxs-lookup"><span data-stu-id="930e6-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Widoki folder w Eksploratorze rozwiązań](layout/_static/web-project-views.png)

<span data-ttu-id="930e6-116">Definiuje szablon najwyższego poziomu do widoków w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="930e6-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="930e6-117">Aplikacje nie wymagają układ, a aplikacje można zdefiniować więcej niż jeden układ różne widoki, określając układów.</span><span class="sxs-lookup"><span data-stu-id="930e6-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="930e6-118">Przykład `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="930e6-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="930e6-119">Określanie układu</span><span class="sxs-lookup"><span data-stu-id="930e6-119">Specifying a Layout</span></span>

<span data-ttu-id="930e6-120">Widoki razor oferują `Layout` właściwości.</span><span class="sxs-lookup"><span data-stu-id="930e6-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="930e6-121">Poszczególnych widoków określ układ przez ustawienie dla tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="930e6-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="930e6-122">Określony układ, można użyć pełną ścieżkę (przykład: `/Views/Shared/_Layout.cshtml`) lub część nazwy (przykład: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="930e6-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="930e6-123">Gdy częściowa nazwa została podana, aparat widoku Razor wyszuka plik układu przy użyciu procesu odnajdywania standardowego.</span><span class="sxs-lookup"><span data-stu-id="930e6-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="930e6-124">Folder skojarzonego kontrolera przeszukiwany jest najpierw, a następnie `Shared` folderu.</span><span class="sxs-lookup"><span data-stu-id="930e6-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="930e6-125">Ten proces odnajdywania jest taki sam jak używaną do wykrywania [widoki częściowe](partial.md).</span><span class="sxs-lookup"><span data-stu-id="930e6-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="930e6-126">Domyślnie każdy układ należy wywołać metodę `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="930e6-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="930e6-127">Wszędzie tam, gdzie wywołanie `RenderBody` jest umieszczony, zawartość widoku będzie renderowany.</span><span class="sxs-lookup"><span data-stu-id="930e6-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="930e6-128">Sekcje</span><span class="sxs-lookup"><span data-stu-id="930e6-128">Sections</span></span>

<span data-ttu-id="930e6-129">Układ opcjonalnie może odwoływać się co najmniej jeden *sekcje*, wywołując `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="930e6-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="930e6-130">Sekcje zawierają sposób organizowania, w którym mają zostać umieszczone niektóre elementy na stronie.</span><span class="sxs-lookup"><span data-stu-id="930e6-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="930e6-131">Każde wywołanie `RenderSection` można określić, czy tej sekcji jest wymagany lub opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="930e6-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="930e6-132">Jeśli nie zostanie odnaleziony wymaganej sekcji, zostanie wygenerowany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="930e6-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="930e6-133">Poszczególnych widoków określenie zawartości do umieszczony wewnątrz sekcji przy użyciu `@section` składni Razor.</span><span class="sxs-lookup"><span data-stu-id="930e6-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="930e6-134">Jeśli widok definiuje sekcję, musi być renderowane lub wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="930e6-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="930e6-135">Przykład `@section` definicji w widoku:</span><span class="sxs-lookup"><span data-stu-id="930e6-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="930e6-136">W powyższym kodzie skrypty sprawdzania poprawności są dodawane do `scripts` sekcji w widoku, który zawiera formularz.</span><span class="sxs-lookup"><span data-stu-id="930e6-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="930e6-137">Innych widoków w tej samej aplikacji nie może wymagać dodatkowych skryptów i dlatego nie trzeba definiują sekcję skryptów.</span><span class="sxs-lookup"><span data-stu-id="930e6-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="930e6-138">Sekcje zdefiniowane w widoku są dostępne tylko w jego natychmiastowego układ strony.</span><span class="sxs-lookup"><span data-stu-id="930e6-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="930e6-139">One nie może odwoływać się ze częściowe, składniki w widoku lub innymi składnikami systemu widoku.</span><span class="sxs-lookup"><span data-stu-id="930e6-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="930e6-140">Ignorowanie sekcji</span><span class="sxs-lookup"><span data-stu-id="930e6-140">Ignoring sections</span></span>

<span data-ttu-id="930e6-141">Domyślnie treść i wszystkich części strony zawartość musi wszystkie przetworzyć, strony układu.</span><span class="sxs-lookup"><span data-stu-id="930e6-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="930e6-142">Aparat widoku Razor wymusza to poprzez śledzenie czy nadano treść i każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="930e6-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="930e6-143">Aby nakazać przez aparat widoku, aby zignorować treści lub sekcje, należy wywołać `IgnoreBody` i `IgnoreSection` metody.</span><span class="sxs-lookup"><span data-stu-id="930e6-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="930e6-144">Treść i każdej sekcji stronie aparatu Razor musi być renderowane albo ignorowane.</span><span class="sxs-lookup"><span data-stu-id="930e6-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="930e6-145">Importowanie udostępnionego dyrektywy</span><span class="sxs-lookup"><span data-stu-id="930e6-145">Importing Shared Directives</span></span>

<span data-ttu-id="930e6-146">Widoki umożliwiają wykonywanie wielu czynności, takich jak importowania przestrzeni nazw lub wykonywania dyrektywy Razor [iniekcji zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="930e6-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="930e6-147">Dyrektywy współużytkowane przez wiele widoków mogą być określone w wspólnego `_ViewImports.cshtml` pliku.</span><span class="sxs-lookup"><span data-stu-id="930e6-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="930e6-148">`_ViewImports` Plik obsługuje następujące dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="930e6-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="930e6-149">Plik nie obsługuje inne funkcje Razor, takich jak funkcje i definicje sekcji.</span><span class="sxs-lookup"><span data-stu-id="930e6-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="930e6-150">Przykładowe `_ViewImports.cshtml` pliku:</span><span class="sxs-lookup"><span data-stu-id="930e6-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="930e6-151">`_ViewImports.cshtml` Pliku dla aplikacji ASP.NET Core MVC zazwyczaj znajduje się w `Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="930e6-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="930e6-152">A `_ViewImports.cshtml` pliku można umieścić w dowolnym folderze, w którym zostaną one zastosowane tylko do sprawę widoków w tym folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="930e6-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="930e6-153">`_ViewImports`pliki są przetwarzane uruchamiania na poziomie głównym, a następnie dla każdego folderu, co prowadzi do lokalizacji widoku, aby określić ustawienia na poziomie głównym może być zastąpiona na poziomie folderu.</span><span class="sxs-lookup"><span data-stu-id="930e6-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="930e6-154">Na przykład, jeśli poziom główny `_ViewImports.cshtml` plik Określa `@model` i `@addTagHelper`i innym `_ViewImports.cshtml` plik w folderze skojarzonego kontrolera widoku Określa inną `@model` i dodaje innego `@addTagHelper`, widok będą mieć dostęp do obu pomocników tagów i korzysta z jego `@model`.</span><span class="sxs-lookup"><span data-stu-id="930e6-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="930e6-155">Jeśli wiele `_ViewImports.cshtml` pliki są uruchamiane w widoku, zachowanie dyrektywy uwzględnione w połączeniu `ViewImports.cshtml` pliki będą znajdować się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="930e6-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="930e6-156">`@addTagHelper`, `@removeTagHelper`: wszystkie działania w kolejności</span><span class="sxs-lookup"><span data-stu-id="930e6-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="930e6-157">`@tagHelperPrefix`: najbliższy, do widoku powoduje zastąpienie innych</span><span class="sxs-lookup"><span data-stu-id="930e6-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="930e6-158">`@model`: najbliższy, do widoku powoduje zastąpienie innych</span><span class="sxs-lookup"><span data-stu-id="930e6-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="930e6-159">`@inherits`: najbliższy, do widoku powoduje zastąpienie innych</span><span class="sxs-lookup"><span data-stu-id="930e6-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="930e6-160">`@using`: wszystkie są uwzględniane; duplikaty zostały zignorowane.</span><span class="sxs-lookup"><span data-stu-id="930e6-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="930e6-161">`@inject`: dla każdej właściwości, najbliższego do widoku zastępuje innych o takiej samej nazwie właściwości</span><span class="sxs-lookup"><span data-stu-id="930e6-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="930e6-162">Uruchomienia kodu przed każdym widoku</span><span class="sxs-lookup"><span data-stu-id="930e6-162">Running Code Before Each View</span></span>

<span data-ttu-id="930e6-163">Jeśli masz kod, należy uruchomić przed każdym widoku, to powinna zostać umieszczona w `_ViewStart.cshtml` pliku.</span><span class="sxs-lookup"><span data-stu-id="930e6-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="930e6-164">Według konwencji `_ViewStart.cshtml` plik znajduje się w `Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="930e6-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="930e6-165">Instrukcje na liście `_ViewStart.cshtml` są uruchamiane przed każdym pełnego widoku (nie układy i widoki częściowe nie).</span><span class="sxs-lookup"><span data-stu-id="930e6-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="930e6-166">Podobnie jak [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` hierarchicznie.</span><span class="sxs-lookup"><span data-stu-id="930e6-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="930e6-167">Jeśli `_ViewStart.cshtml` pliku jest zdefiniowany w folderze skojarzonego kontrolera widoku, zostanie ono uruchomione po jednym określonym w folderze głównym `Views` folder (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="930e6-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="930e6-168">Przykładowe `_ViewStart.cshtml` pliku:</span><span class="sxs-lookup"><span data-stu-id="930e6-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="930e6-169">Plik powyżej Określa, które będą używane przez wszystkie widoki `_Layout.cshtml` układu.</span><span class="sxs-lookup"><span data-stu-id="930e6-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="930e6-170">Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` są zwykle umieszczane w `/Views/Shared` folderu.</span><span class="sxs-lookup"><span data-stu-id="930e6-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="930e6-171">Wersje aplikacji na poziomie plików powinny być umieszczane bezpośrednio w `/Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="930e6-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
