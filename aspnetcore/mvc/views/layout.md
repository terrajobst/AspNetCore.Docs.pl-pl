---
title: Układ platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać typowych układów, udostępnić dyrektywy i uruchom typowy kod przed renderowania widoków w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: a99b239a0aeeb14492b1eee962dc1149f056f0eb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274121"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="af16a-103">Układ platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af16a-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="af16a-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="af16a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="af16a-105">Widoki często Udostępnianie elementów wizualnych i programowe.</span><span class="sxs-lookup"><span data-stu-id="af16a-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="af16a-106">W tym artykule dowiesz się, jak używać typowych układów, udostępnić dyrektywy i uruchomić typowy kod przed renderowania widoków w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="af16a-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="af16a-107">Co to jest układ</span><span class="sxs-lookup"><span data-stu-id="af16a-107">What is a Layout</span></span>

<span data-ttu-id="af16a-108">Większość aplikacji sieci web mają wspólne układu, który zapewnia spójne środowisko użytkownika zgodnie z ich przechodzenie między stronami.</span><span class="sxs-lookup"><span data-stu-id="af16a-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="af16a-109">Układ zazwyczaj zawiera wspólne elementy interfejsu użytkownika, takie jak nagłówek aplikacji, nawigacji lub elementy menu i stopce strony.</span><span class="sxs-lookup"><span data-stu-id="af16a-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Przykładowy układ strony](layout/_static/page-layout.png)

<span data-ttu-id="af16a-111">Wspólnej struktury HTML, takich jak skrypty i arkusze stylów również często używane przez wiele stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="af16a-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="af16a-112">Wszystkie te elementy udostępnionych można zdefiniować w *układu* pliku, który można odwoływać się do dowolnego widoku używany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="af16a-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="af16a-113">Układy zmniejszyć zduplikowany kod w widokach, pomagając im wykonaj [zasady nie powtarzaj samodzielnie (BIBUŁĄ)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="af16a-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="af16a-114">Konwencja, nosi nazwę domyślny układ dla aplikacji ASP.NET `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="af16a-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="af16a-115">Szablon projektu Visual Studio platformy ASP.NET Core MVC obejmuje to pliku układu `Views/Shared` folderu:</span><span class="sxs-lookup"><span data-stu-id="af16a-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Widoki folder w Eksploratorze rozwiązań](layout/_static/web-project-views.png)

<span data-ttu-id="af16a-117">Definiuje szablon najwyższego poziomu do widoków w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="af16a-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="af16a-118">Aplikacje nie wymagają układ, a aplikacje można zdefiniować więcej niż jeden układ różne widoki, określając układów.</span><span class="sxs-lookup"><span data-stu-id="af16a-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="af16a-119">Przykład `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="af16a-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="af16a-120">Określanie układu</span><span class="sxs-lookup"><span data-stu-id="af16a-120">Specifying a Layout</span></span>

<span data-ttu-id="af16a-121">Widoki razor oferują `Layout` właściwości.</span><span class="sxs-lookup"><span data-stu-id="af16a-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="af16a-122">Poszczególnych widoków określ układ przez ustawienie dla tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="af16a-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="af16a-123">Określony układ, można użyć pełną ścieżkę (przykład: `/Views/Shared/_Layout.cshtml`) lub część nazwy (przykład: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="af16a-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="af16a-124">Gdy częściowa nazwa została podana, aparat widoku Razor wyszuka plik układu przy użyciu procesu odnajdywania standardowego.</span><span class="sxs-lookup"><span data-stu-id="af16a-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="af16a-125">Folder skojarzonego kontrolera przeszukiwany jest najpierw, a następnie `Shared` folderu.</span><span class="sxs-lookup"><span data-stu-id="af16a-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="af16a-126">Ten proces odnajdywania jest taki sam jak używaną do wykrywania [widoki częściowe](partial.md).</span><span class="sxs-lookup"><span data-stu-id="af16a-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="af16a-127">Domyślnie każdy układ należy wywołać metodę `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="af16a-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="af16a-128">Wszędzie tam, gdzie wywołanie `RenderBody` jest umieszczony, zawartość widoku będzie renderowany.</span><span class="sxs-lookup"><span data-stu-id="af16a-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="af16a-129">Sekcje</span><span class="sxs-lookup"><span data-stu-id="af16a-129">Sections</span></span>

<span data-ttu-id="af16a-130">Układ opcjonalnie może odwoływać się co najmniej jeden *sekcje*, wywołując `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="af16a-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="af16a-131">Sekcje zawierają sposób organizowania, w którym mają zostać umieszczone niektóre elementy na stronie.</span><span class="sxs-lookup"><span data-stu-id="af16a-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="af16a-132">Każde wywołanie `RenderSection` można określić, czy tej sekcji jest wymagany lub opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="af16a-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="af16a-133">Jeśli nie zostanie odnaleziony wymaganej sekcji, zostanie wygenerowany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="af16a-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="af16a-134">Poszczególnych widoków określenie zawartości do umieszczony wewnątrz sekcji przy użyciu `@section` składni Razor.</span><span class="sxs-lookup"><span data-stu-id="af16a-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="af16a-135">Jeśli widok definiuje sekcję, musi być renderowane lub wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="af16a-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="af16a-136">Przykład `@section` definicji w widoku:</span><span class="sxs-lookup"><span data-stu-id="af16a-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="af16a-137">W powyższym kodzie skrypty sprawdzania poprawności są dodawane do `scripts` sekcji w widoku, który zawiera formularz.</span><span class="sxs-lookup"><span data-stu-id="af16a-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="af16a-138">Innych widoków w tej samej aplikacji nie może wymagać dodatkowych skryptów i dlatego nie trzeba definiują sekcję skryptów.</span><span class="sxs-lookup"><span data-stu-id="af16a-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="af16a-139">Sekcje zdefiniowane w widoku są dostępne tylko w jego natychmiastowego układ strony.</span><span class="sxs-lookup"><span data-stu-id="af16a-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="af16a-140">One nie może odwoływać się ze częściowe, składniki w widoku lub innymi składnikami systemu widoku.</span><span class="sxs-lookup"><span data-stu-id="af16a-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="af16a-141">Ignorowanie sekcji</span><span class="sxs-lookup"><span data-stu-id="af16a-141">Ignoring sections</span></span>

<span data-ttu-id="af16a-142">Domyślnie treść i wszystkich części strony zawartość musi wszystkie przetworzyć, strony układu.</span><span class="sxs-lookup"><span data-stu-id="af16a-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="af16a-143">Aparat widoku Razor wymusza to poprzez śledzenie czy nadano treść i każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="af16a-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="af16a-144">Aby nakazać przez aparat widoku, aby zignorować treści lub sekcje, należy wywołać `IgnoreBody` i `IgnoreSection` metody.</span><span class="sxs-lookup"><span data-stu-id="af16a-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="af16a-145">Treść i każdej sekcji stronie aparatu Razor musi być renderowane albo ignorowane.</span><span class="sxs-lookup"><span data-stu-id="af16a-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="af16a-146">Importowanie udostępnionego dyrektywy</span><span class="sxs-lookup"><span data-stu-id="af16a-146">Importing Shared Directives</span></span>

<span data-ttu-id="af16a-147">Widoki umożliwiają wykonywanie wielu czynności, takich jak importowania przestrzeni nazw lub wykonywania dyrektywy Razor [iniekcji zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="af16a-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="af16a-148">Dyrektywy współużytkowane przez wiele widoków mogą być określone w wspólnego `_ViewImports.cshtml` pliku.</span><span class="sxs-lookup"><span data-stu-id="af16a-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="af16a-149">`_ViewImports` Plik obsługuje następujące dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="af16a-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="af16a-150">Plik nie obsługuje inne funkcje Razor, takich jak funkcje i definicje sekcji.</span><span class="sxs-lookup"><span data-stu-id="af16a-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="af16a-151">Przykładowe `_ViewImports.cshtml` pliku:</span><span class="sxs-lookup"><span data-stu-id="af16a-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="af16a-152">`_ViewImports.cshtml` Pliku dla aplikacji ASP.NET Core MVC zazwyczaj znajduje się w `Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="af16a-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="af16a-153">A `_ViewImports.cshtml` pliku można umieścić w dowolnym folderze, w którym zostaną one zastosowane tylko do sprawę widoków w tym folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="af16a-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="af16a-154">`_ViewImports` pliki są przetwarzane uruchamiania na poziomie głównym, a następnie dla każdego folderu, co prowadzi do lokalizacji widoku, aby określić ustawienia na poziomie głównym może być zastąpiona na poziomie folderu.</span><span class="sxs-lookup"><span data-stu-id="af16a-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="af16a-155">Na przykład, jeśli poziom główny `_ViewImports.cshtml` plik Określa `@model` i `@addTagHelper`i innym `_ViewImports.cshtml` plik w folderze skojarzonego kontrolera widoku Określa inną `@model` i dodaje innego `@addTagHelper`, widok będą mieć dostęp do obu pomocników tagów i korzysta z jego `@model`.</span><span class="sxs-lookup"><span data-stu-id="af16a-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="af16a-156">Jeśli wiele `_ViewImports.cshtml` pliki są uruchamiane w widoku, zachowanie dyrektywy uwzględnione w połączeniu `ViewImports.cshtml` pliki będą znajdować się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="af16a-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="af16a-157">`@addTagHelper`, `@removeTagHelper`: wszystkie działania w kolejności</span><span class="sxs-lookup"><span data-stu-id="af16a-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="af16a-158">`@tagHelperPrefix`: najbliższy, do widoku powoduje zastąpienie innych</span><span class="sxs-lookup"><span data-stu-id="af16a-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="af16a-159">`@model`: najbliższy, do widoku powoduje zastąpienie innych</span><span class="sxs-lookup"><span data-stu-id="af16a-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="af16a-160">`@inherits`: najbliższy, do widoku powoduje zastąpienie innych</span><span class="sxs-lookup"><span data-stu-id="af16a-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="af16a-161">`@using`: wszystkie są uwzględniane; duplikaty zostały zignorowane.</span><span class="sxs-lookup"><span data-stu-id="af16a-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="af16a-162">`@inject`: dla każdej właściwości, najbliższego do widoku zastępuje innych o takiej samej nazwie właściwości</span><span class="sxs-lookup"><span data-stu-id="af16a-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="af16a-163">Uruchomienia kodu przed każdym widoku</span><span class="sxs-lookup"><span data-stu-id="af16a-163">Running Code Before Each View</span></span>

<span data-ttu-id="af16a-164">Jeśli masz kod, należy uruchomić przed każdym widoku, to powinna zostać umieszczona w `_ViewStart.cshtml` pliku.</span><span class="sxs-lookup"><span data-stu-id="af16a-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="af16a-165">Według konwencji `_ViewStart.cshtml` plik znajduje się w `Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="af16a-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="af16a-166">Instrukcje na liście `_ViewStart.cshtml` są uruchamiane przed każdym pełnego widoku (nie układy i widoki częściowe nie).</span><span class="sxs-lookup"><span data-stu-id="af16a-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="af16a-167">Podobnie jak [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` hierarchicznie.</span><span class="sxs-lookup"><span data-stu-id="af16a-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="af16a-168">Jeśli `_ViewStart.cshtml` pliku jest zdefiniowany w folderze skojarzonego kontrolera widoku, zostanie ono uruchomione po jednym określonym w folderze głównym `Views` folder (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="af16a-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="af16a-169">Przykładowe `_ViewStart.cshtml` pliku:</span><span class="sxs-lookup"><span data-stu-id="af16a-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="af16a-170">Plik powyżej Określa, które będą używane przez wszystkie widoki `_Layout.cshtml` układu.</span><span class="sxs-lookup"><span data-stu-id="af16a-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="af16a-171">Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` są zwykle umieszczane w `/Views/Shared` folderu.</span><span class="sxs-lookup"><span data-stu-id="af16a-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="af16a-172">Wersje aplikacji na poziomie plików powinny być umieszczane bezpośrednio w `/Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="af16a-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
