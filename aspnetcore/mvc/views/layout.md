---
title: Układ w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać typowych układów, udostępnianie dyrektyw i uruchomienia wspólnego kodu przed renderowania widoków w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757366"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="6d593-103">Układ w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d593-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="6d593-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6d593-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6d593-105">Widoki często Udostępnianie elementów wizualnych i programowe.</span><span class="sxs-lookup"><span data-stu-id="6d593-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="6d593-106">W tym artykule dowiesz się, jak używać typowych układów, udostępnianie dyrektywy i uruchomienia wspólnego kodu przed renderowania widoków w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d593-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET Core app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="6d593-107">Co to jest układ</span><span class="sxs-lookup"><span data-stu-id="6d593-107">What is a Layout</span></span>

<span data-ttu-id="6d593-108">Większość aplikacji sieci web ma typowe układ, który zapewnia spójne środowisko użytkownika zgodnie z jego nawigowania między stronami.</span><span class="sxs-lookup"><span data-stu-id="6d593-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="6d593-109">Układ zwykle zawiera wspólne elementy interfejsu użytkownika, takie jak nagłówek aplikacji, nawigacji lub elementy menu i stopki.</span><span class="sxs-lookup"><span data-stu-id="6d593-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Przykładowy układ strony](layout/_static/page-layout.png)

<span data-ttu-id="6d593-111">Wspólnej struktury kodu HTML, takie jak skrypty i arkusze stylów również często są używane przez wielu stronach w obrębie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6d593-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="6d593-112">Wszystkie te elementy udostępnione może być zdefiniowana w *układ* pliku, który można odwoływać się do dowolnego widoku używany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6d593-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="6d593-113">Układy zmniejszyć kodu zduplikowanego w widokach, pomagając im wykonaj [zasady nie Powtórz samodzielnie (PRÓBNEGO)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="6d593-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="6d593-114">Zgodnie z Konwencją, nosi nazwę domyślny układ dla aplikacji ASP.NET Core `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="6d593-114">By convention, the default layout for an ASP.NET Core app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="6d593-115">Szablon projektu Visual Studio ASP.NET Core MVC obejmuje to pliku układu `Views/Shared` folderu:</span><span class="sxs-lookup"><span data-stu-id="6d593-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Widoki folder w Eksploratorze rozwiązań](layout/_static/web-project-views.png)

<span data-ttu-id="6d593-117">Definiuje szablon najwyższego poziomu dla widoków w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6d593-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="6d593-118">Aplikacje nie wymagają układu, a aplikacje można zdefiniować więcej niż jeden układ z różnych widoków, określając różne układy.</span><span class="sxs-lookup"><span data-stu-id="6d593-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="6d593-119">Przykład `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="6d593-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="6d593-120">Określanie układu</span><span class="sxs-lookup"><span data-stu-id="6d593-120">Specifying a Layout</span></span>

<span data-ttu-id="6d593-121">Widoki razor oferują `Layout` właściwości.</span><span class="sxs-lookup"><span data-stu-id="6d593-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="6d593-122">Pojedyncze widoki określ układ przez ustawienie tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="6d593-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="6d593-123">Określony układ, można użyć pełnej ścieżki (przykład: `/Views/Shared/_Layout.cshtml`) lub część nazwy (przykład: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="6d593-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="6d593-124">Gdy część nazwy jest podana, aparat widoku Razor wyszuka plik układu przy użyciu swojego procesu odnajdywania standardowego.</span><span class="sxs-lookup"><span data-stu-id="6d593-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="6d593-125">Folder skojarzonego kontrolera jest przeszukiwany w pierwszej kolejności, a następnie `Shared` folderu.</span><span class="sxs-lookup"><span data-stu-id="6d593-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="6d593-126">Ten proces odnajdywania jest taka sama jak używaną w celu odnalezienia [widoki częściowe](partial.md).</span><span class="sxs-lookup"><span data-stu-id="6d593-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="6d593-127">Domyślnie każdy układu musi wywołać `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="6d593-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="6d593-128">Wszędzie tam, gdzie wywołanie `RenderBody` jest umieszczany, zawartość widoku będzie renderowana.</span><span class="sxs-lookup"><span data-stu-id="6d593-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="6d593-129">Sekcje</span><span class="sxs-lookup"><span data-stu-id="6d593-129">Sections</span></span>

<span data-ttu-id="6d593-130">Układ Opcjonalnie można odwoływać się do co najmniej jeden *sekcje*, wywołując `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="6d593-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="6d593-131">Sekcje zawierają sposób organizowania, w którym mają zostać umieszczone niektóre elementy strony.</span><span class="sxs-lookup"><span data-stu-id="6d593-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="6d593-132">Każde wywołanie `RenderSection` można określić, czy tej sekcji jest wymagane lub opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="6d593-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="6d593-133">Jeśli wymaganej sekcji nie zostanie znaleziona, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="6d593-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="6d593-134">Pojedyncze widoki określenie zawartości do renderowania w ramach sekcji przy użyciu `@section` składni Razor.</span><span class="sxs-lookup"><span data-stu-id="6d593-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="6d593-135">Jeśli widok definiuje sekcję, musi być renderowany lub wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="6d593-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="6d593-136">Przykład `@section` definicję w widoku:</span><span class="sxs-lookup"><span data-stu-id="6d593-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="6d593-137">W powyższym kodzie skrypty sprawdzania poprawności są dodawane do `scripts` sekcji w widoku, który zawiera formularz.</span><span class="sxs-lookup"><span data-stu-id="6d593-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="6d593-138">Inne widoki w tej samej aplikacji może nie wymagać dowolne dodatkowe skrypty i dlatego nie należy zdefiniować sekcję skryptów.</span><span class="sxs-lookup"><span data-stu-id="6d593-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="6d593-139">Sekcje zdefiniowane w widoku są dostępne tylko w jego natychmiastowego układ strony.</span><span class="sxs-lookup"><span data-stu-id="6d593-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="6d593-140">One nie może odwoływać się ze częściowych, składniki widoków lub inne części systemu widoku.</span><span class="sxs-lookup"><span data-stu-id="6d593-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="6d593-141">Ignorowanie sekcji</span><span class="sxs-lookup"><span data-stu-id="6d593-141">Ignoring sections</span></span>

<span data-ttu-id="6d593-142">Domyślnie jednostka i wszystkie sekcje w zawartości strony musi wszystkie renderowana przez stronę układu.</span><span class="sxs-lookup"><span data-stu-id="6d593-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="6d593-143">Aparat widoku Razor wymusza to przez śledzenie czy nadano treści i każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="6d593-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="6d593-144">Aby nakazać aparat widoku, aby zignorować treści lub sekcje, należy wywołać `IgnoreBody` i `IgnoreSection` metody.</span><span class="sxs-lookup"><span data-stu-id="6d593-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="6d593-145">Treść oraz każdej sekcji strony Razor musi być renderowany lub ignorowane.</span><span class="sxs-lookup"><span data-stu-id="6d593-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="6d593-146">Importowanie udostępnionego dyrektywy</span><span class="sxs-lookup"><span data-stu-id="6d593-146">Importing Shared Directives</span></span>

<span data-ttu-id="6d593-147">Widoki za pomocą dyrektywy Razor można wykonywać wiele elementów, takich jak importowania przestrzeni nazw lub wykonywanie [wstrzykiwanie zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="6d593-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="6d593-148">Dyrektywy współużytkowane przez wiele widoków, mogą być określone w często `_ViewImports.cshtml` pliku.</span><span class="sxs-lookup"><span data-stu-id="6d593-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="6d593-149">`_ViewImports` Pliku obsługuje następujące dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="6d593-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="6d593-150">Plik nie obsługuje inne funkcje Razor, takie jak definicje sekcji i funkcji.</span><span class="sxs-lookup"><span data-stu-id="6d593-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="6d593-151">Przykład `_ViewImports.cshtml` pliku:</span><span class="sxs-lookup"><span data-stu-id="6d593-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="6d593-152">`_ViewImports.cshtml` Plików dla aplikacji ASP.NET Core MVC znajduje się zwykle w `Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="6d593-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="6d593-153">A `_ViewImports.cshtml` pliku można umieścić w dowolnym folderze, w którym przypadku zostaną zastosowane tylko do wyświetlenia w tym folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="6d593-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="6d593-154">`_ViewImports` pliki są przetwarzane, zaczynając od katalogu głównego, a następnie dla każdego folderu, co prowadzi do lokalizacji widoku, aby określić ustawienia na poziomie głównym może zostać zastąpione na poziomie folderu.</span><span class="sxs-lookup"><span data-stu-id="6d593-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="6d593-155">Na przykład, jeśli poziom główny `_ViewImports.cshtml` plik Określa `@model` i `@addTagHelper`, a inny `_ViewImports.cshtml` pliku w folderze skojarzonego kontrolera widoku Określa inną `@model` i dodaje kolejny `@addTagHelper`, widok będą mieć dostęp do obu pomocników tagów i użyje one `@model`.</span><span class="sxs-lookup"><span data-stu-id="6d593-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="6d593-156">Jeśli wiele `_ViewImports.cshtml` pliki są uruchamiane w celu wyświetlenia, zachowanie dyrektywy zawarte w połączeniu `ViewImports.cshtml` pliki będą znajdować się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6d593-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="6d593-157">`@addTagHelper`, `@removeTagHelper`: wszystkie działania w kolejności</span><span class="sxs-lookup"><span data-stu-id="6d593-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="6d593-158">`@tagHelperPrefix`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola</span><span class="sxs-lookup"><span data-stu-id="6d593-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="6d593-159">`@model`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola</span><span class="sxs-lookup"><span data-stu-id="6d593-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="6d593-160">`@inherits`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola</span><span class="sxs-lookup"><span data-stu-id="6d593-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="6d593-161">`@using`: uwzględniono wszystkie; duplikaty są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="6d593-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="6d593-162">`@inject`: dla każdej właściwości znajdującego się najbliżej do widoku zastępuje wszystkie inne pola o takiej samej nazwie właściwości</span><span class="sxs-lookup"><span data-stu-id="6d593-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="6d593-163">Uruchamianie kodu przed każdym widoku</span><span class="sxs-lookup"><span data-stu-id="6d593-163">Running Code Before Each View</span></span>

<span data-ttu-id="6d593-164">Jeśli masz kod do uruchomienia przed każdym widoku, to należy umieścić w `_ViewStart.cshtml` pliku.</span><span class="sxs-lookup"><span data-stu-id="6d593-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="6d593-165">Zgodnie z Konwencją `_ViewStart.cshtml` plik znajduje się w `Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="6d593-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="6d593-166">Instrukcje, na liście `_ViewStart.cshtml` są uruchamiane przed każdym pełnego widoku (nie układ i widoki częściowe nie).</span><span class="sxs-lookup"><span data-stu-id="6d593-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="6d593-167">Podobnie jak [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="6d593-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="6d593-168">Jeśli `_ViewStart.cshtml` jest zdefiniowany plik w folderze skojarzonego kontrolera widoku, zostanie ono uruchomione po jednym określonym w katalogu głównym `Views` folder (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="6d593-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="6d593-169">Przykład `_ViewStart.cshtml` pliku:</span><span class="sxs-lookup"><span data-stu-id="6d593-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="6d593-170">Pliku powyżej Określa, które będą używane we wszystkich widokach `_Layout.cshtml` układu.</span><span class="sxs-lookup"><span data-stu-id="6d593-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="6d593-171">Ani `_ViewStart.cshtml` ani `_ViewImports.cshtml` są zazwyczaj umieszczane `/Views/Shared` folderu.</span><span class="sxs-lookup"><span data-stu-id="6d593-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="6d593-172">Poziom aplikacji wersje tych plików powinny być umieszczane bezpośrednio w `/Views` folderu.</span><span class="sxs-lookup"><span data-stu-id="6d593-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
