---
title: Układ w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać typowych układów, udostępnianie dyrektyw i uruchomienia wspólnego kodu przed renderowania widoków w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/18/2018
uid: mvc/views/layout
ms.openlocfilehash: 1bd225c804b333efea834a46b7d9ba46b1bb69d8
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410576"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="4b665-103">Układ w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b665-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="4b665-104">Przez [Steve Smith](https://ardalis.com/) i [firmy Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="4b665-104">By [Steve Smith](https://ardalis.com/) and [Dave Brock](https://twitter.com/daveabrock)</span></span>

<span data-ttu-id="4b665-105">Widoki i strony często Udostępnianie elementów wizualnych i programowe.</span><span class="sxs-lookup"><span data-stu-id="4b665-105">Pages and views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="4b665-106">W tym artykule przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="4b665-106">This article demonstrates how to:</span></span>

* <span data-ttu-id="4b665-107">Użyj wspólnego układów.</span><span class="sxs-lookup"><span data-stu-id="4b665-107">Use common layouts.</span></span>
* <span data-ttu-id="4b665-108">Udostępnij dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="4b665-108">Share directives.</span></span>
* <span data-ttu-id="4b665-109">Uruchom wspólny kod przed przystąpieniem do renderowania stron lub widoków.</span><span class="sxs-lookup"><span data-stu-id="4b665-109">Run common code before rendering pages or views.</span></span>

<span data-ttu-id="4b665-110">W tym dokumencie omówiono układów dla dwa różne podejścia do platformy ASP.NET Core MVC: Strony razor i kontrolery, za pomocą widoków.</span><span class="sxs-lookup"><span data-stu-id="4b665-110">This document discusses layouts for the two different approaches to ASP.NET Core MVC: Razor Pages and controllers with views.</span></span> <span data-ttu-id="4b665-111">W tym temacie różnice są minimalne:</span><span class="sxs-lookup"><span data-stu-id="4b665-111">For this topic, the differences are minimal:</span></span>

* <span data-ttu-id="4b665-112">Strony razor znajdują się w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="4b665-112">Razor Pages are in the *Pages* folder.</span></span>
* <span data-ttu-id="4b665-113">Kontrolery używa widoków *widoków* folderu dla widoków.</span><span class="sxs-lookup"><span data-stu-id="4b665-113">Controllers with views uses a *Views* folder for views.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="4b665-114">Co to jest układ</span><span class="sxs-lookup"><span data-stu-id="4b665-114">What is a Layout</span></span>

<span data-ttu-id="4b665-115">Większość aplikacji sieci web ma typowe układ, który zapewnia spójne środowisko użytkownika zgodnie z jego nawigowania między stronami.</span><span class="sxs-lookup"><span data-stu-id="4b665-115">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="4b665-116">Układ zwykle zawiera wspólne elementy interfejsu użytkownika, takie jak nagłówek aplikacji, nawigacji lub elementy menu i stopki.</span><span class="sxs-lookup"><span data-stu-id="4b665-116">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Przykładowy układ strony](layout/_static/page-layout.png)

<span data-ttu-id="4b665-118">Wspólnej struktury kodu HTML, takie jak skrypty i arkusze stylów również często są używane przez wielu stronach w obrębie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b665-118">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="4b665-119">Wszystkie te elementy udostępnione może być zdefiniowana w *układ* pliku, który można odwoływać się do dowolnego widoku używany w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b665-119">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="4b665-120">Układy zmniejszenie kodu zduplikowanego w widokach.</span><span class="sxs-lookup"><span data-stu-id="4b665-120">Layouts reduce duplicate code in views.</span></span>

<span data-ttu-id="4b665-121">Zgodnie z Konwencją, nosi nazwę domyślny układ dla aplikacji ASP.NET Core *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4b665-121">By convention, the default layout for an ASP.NET Core app is named *_Layout.cshtml*.</span></span> <span data-ttu-id="4b665-122">Plik układu dla nowych projektów ASP.NET Core utworzona przy użyciu szablonów:</span><span class="sxs-lookup"><span data-stu-id="4b665-122">The layout file for new ASP.NET Core projects created with the templates:</span></span>

* <span data-ttu-id="4b665-123">Strony razor: *Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4b665-123">Razor Pages: *Pages/Shared/_Layout.cshtml*</span></span>

  ![folder stron w Eksploratorze rozwiązań](layout/_static/rp-web-project-views.png)

* <span data-ttu-id="4b665-125">Kontroler z widokami: *Views/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4b665-125">Controller with views: *Views/Shared/_Layout.cshtml*</span></span>

 ![Widoki folder w Eksploratorze rozwiązań](layout/_static/mvc-web-project-views.png)

<span data-ttu-id="4b665-127">Układ określa szablon najwyższego poziomu dla widoków w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4b665-127">The layout defines a top level template for views in the app.</span></span> <span data-ttu-id="4b665-128">Aplikacje nie wymagają układu.</span><span class="sxs-lookup"><span data-stu-id="4b665-128">Apps don't require a layout.</span></span> <span data-ttu-id="4b665-129">Aplikacje można zdefiniować więcej niż jeden układ z różnych widoków, określając różne układy.</span><span class="sxs-lookup"><span data-stu-id="4b665-129">Apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="4b665-130">Poniższy kod przedstawia plik układu dla szablonu, który został utworzony projekt za pomocą kontrolera i widoki:</span><span class="sxs-lookup"><span data-stu-id="4b665-130">The following code shows the layout file for a template created project with a controller and views:</span></span>

[!code-html[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a><span data-ttu-id="4b665-131">Określanie układu</span><span class="sxs-lookup"><span data-stu-id="4b665-131">Specifying a Layout</span></span>

<span data-ttu-id="4b665-132">Widoki razor oferują `Layout` właściwości.</span><span class="sxs-lookup"><span data-stu-id="4b665-132">Razor views have a `Layout` property.</span></span> <span data-ttu-id="4b665-133">Pojedyncze widoki określ układ przez ustawienie tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="4b665-133">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="4b665-134">Określony układ, można użyć pełnej ścieżki (na przykład */Pages/Shared/_Layout.cshtml* lub */Views/Shared/_Layout.cshtml*) lub część nazwy (przykład: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="4b665-134">The layout specified can use a full path (for example, */Pages/Shared/_Layout.cshtml* or */Views/Shared/_Layout.cshtml*) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="4b665-135">Gdy część nazwy jest podana, aparat widoku Razor wyszuka plik układu przy użyciu swojego procesu odnajdywania standardowego.</span><span class="sxs-lookup"><span data-stu-id="4b665-135">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="4b665-136">Folder, w której istnieje metoda obsługi (lub kontroler) jest przeszukiwany w pierwszej kolejności, a następnie *Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="4b665-136">The folder where the handler method (or controller) exists is searched first, followed by the *Shared* folder.</span></span> <span data-ttu-id="4b665-137">Ten proces odnajdywania jest taka sama jak używaną w celu odnalezienia [widoki częściowe](partial.md).</span><span class="sxs-lookup"><span data-stu-id="4b665-137">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="4b665-138">Domyślnie każdy układu musi wywołać `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="4b665-138">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="4b665-139">Wszędzie tam, gdzie wywołanie `RenderBody` jest umieszczany, zawartość widoku będzie renderowana.</span><span class="sxs-lookup"><span data-stu-id="4b665-139">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="4b665-140">Sekcje</span><span class="sxs-lookup"><span data-stu-id="4b665-140">Sections</span></span>

<span data-ttu-id="4b665-141">Układ Opcjonalnie można odwoływać się do co najmniej jeden *sekcje*, wywołując `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="4b665-141">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="4b665-142">Sekcje zawierają sposób organizowania, w którym mają zostać umieszczone niektóre elementy strony.</span><span class="sxs-lookup"><span data-stu-id="4b665-142">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="4b665-143">Każde wywołanie `RenderSection` można określić, czy tej sekcji jest wymagane lub opcjonalne:</span><span class="sxs-lookup"><span data-stu-id="4b665-143">Each call to `RenderSection` can specify whether that section is required or optional:</span></span>

```html
@section Scripts {
    @RenderSection("Scripts", required: false)
}
```

<span data-ttu-id="4b665-144">Jeśli wymaganej sekcji nie zostanie znaleziona, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4b665-144">If a required section isn't found, an exception is thrown.</span></span> <span data-ttu-id="4b665-145">Pojedyncze widoki określenie zawartości do renderowania w ramach sekcji przy użyciu `@section` składni Razor.</span><span class="sxs-lookup"><span data-stu-id="4b665-145">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="4b665-146">Jeśli strona lub widok definiuje sekcję, musi być renderowany lub wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="4b665-146">If a page or view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="4b665-147">Przykład `@section` definicję w widoku strony Razor:</span><span class="sxs-lookup"><span data-stu-id="4b665-147">An example `@section` definition in Razor Pages view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
}
```

<span data-ttu-id="4b665-148">W poprzednim kodzie *scripts/main.js* jest dodawany do `scripts` sekcji na stronie lub widoku.</span><span class="sxs-lookup"><span data-stu-id="4b665-148">In the preceding code, *scripts/main.js* is added to the `scripts` section on a page or view.</span></span> <span data-ttu-id="4b665-149">Innych stron lub widoków w tej samej aplikacji może nie wymagać tego skryptu, a nie zdefiniować sekcję skryptów.</span><span class="sxs-lookup"><span data-stu-id="4b665-149">Other pages or views in the same app might not require this script and wouldn't define a scripts section.</span></span>

<span data-ttu-id="4b665-150">Korzysta z niej następujące znaczniki [Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) do renderowania *_ValidationScriptsPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4b665-150">The following markup uses the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) to render  *_ValidationScriptsPartial.cshtml*:</span></span>

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

<span data-ttu-id="4b665-151">Poprzedni kod znaczników został wygenerowany przez [tworzenia szkieletów tożsamości](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="4b665-151">The preceding markup was generated by [scaffolding Identity](xref:security/authentication/scaffold-identity).</span></span>

<span data-ttu-id="4b665-152">Sekcje zdefiniowane w widoku lub strony są dostępne tylko w jego natychmiastowego układ strony.</span><span class="sxs-lookup"><span data-stu-id="4b665-152">Sections defined in a page or view are available only in its immediate layout page.</span></span> <span data-ttu-id="4b665-153">One nie może odwoływać się ze częściowych, składniki widoków lub inne części systemu widoku.</span><span class="sxs-lookup"><span data-stu-id="4b665-153">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="4b665-154">Ignorowanie sekcji</span><span class="sxs-lookup"><span data-stu-id="4b665-154">Ignoring sections</span></span>

<span data-ttu-id="4b665-155">Domyślnie jednostka i wszystkie sekcje w zawartości strony musi wszystkie renderowana przez stronę układu.</span><span class="sxs-lookup"><span data-stu-id="4b665-155">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="4b665-156">Aparat widoku Razor wymusza to przez śledzenie czy nadano treści i każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="4b665-156">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="4b665-157">Aby nakazać aparat widoku, aby zignorować treści lub sekcje, należy wywołać `IgnoreBody` i `IgnoreSection` metody.</span><span class="sxs-lookup"><span data-stu-id="4b665-157">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="4b665-158">Treść oraz każdej sekcji strony Razor musi być renderowany lub ignorowane.</span><span class="sxs-lookup"><span data-stu-id="4b665-158">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="4b665-159">Importowanie udostępnionego dyrektywy</span><span class="sxs-lookup"><span data-stu-id="4b665-159">Importing Shared Directives</span></span>

<span data-ttu-id="4b665-160">Widoków i stron można użyć dyrektywy Razor do importowania przestrzeni nazw i użyj [wstrzykiwanie zależności](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="4b665-160">Views and pages can use Razor directives to importing namespaces and use [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="4b665-161">Dyrektywy współużytkowane przez wiele widoków, mogą być określone w często *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="4b665-161">Directives shared by many views may be specified in a common *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="4b665-162">`_ViewImports` Pliku obsługuje następujące dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="4b665-162">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

<span data-ttu-id="4b665-163">Plik nie obsługuje inne funkcje Razor, takie jak definicje sekcji i funkcji.</span><span class="sxs-lookup"><span data-stu-id="4b665-163">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="4b665-164">Przykład `_ViewImports.cshtml` pliku:</span><span class="sxs-lookup"><span data-stu-id="4b665-164">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="4b665-165">*_ViewImports.cshtml* plików dla aplikacji ASP.NET Core MVC znajduje się zwykle w *stron* (lub *widoków*) folder.</span><span class="sxs-lookup"><span data-stu-id="4b665-165">The *_ViewImports.cshtml* file for an ASP.NET Core MVC app is typically placed in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="4b665-166">A *_ViewImports.cshtml* pliku można umieścić w dowolnym folderze, w którym to przypadku go one stosowane tylko do stron lub widoków w tym folderze i jego podfolderach.</span><span class="sxs-lookup"><span data-stu-id="4b665-166">A *_ViewImports.cshtml* file can be placed within any folder, in which case it will only be applied to pages or views within that folder and its subfolders.</span></span> <span data-ttu-id="4b665-167">`_ViewImports` pliki są przetwarzane od na poziomie głównym, a następnie dla każdego folderu prowadzących do lokalizacji strony lub wyświetlić sam.</span><span class="sxs-lookup"><span data-stu-id="4b665-167">`_ViewImports` files are processed starting at the root level and then for each folder leading up to the location of the page or view itself.</span></span> <span data-ttu-id="4b665-168">`_ViewImports` na poziomie folderu, mogą zostać zastąpione ustawieniami określonymi na poziomie głównym.</span><span class="sxs-lookup"><span data-stu-id="4b665-168">`_ViewImports` settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="4b665-169">Załóżmy na przykład:</span><span class="sxs-lookup"><span data-stu-id="4b665-169">For example, suppose:</span></span>

* <span data-ttu-id="4b665-170">Poziom główny *_ViewImports.cshtml* plik zawiera `@model MyModel1` i `@addTagHelper *, MyTagHelper1`.</span><span class="sxs-lookup"><span data-stu-id="4b665-170">The  root level *_ViewImports.cshtml* file includes `@model MyModel1` and `@addTagHelper *, MyTagHelper1`.</span></span>
* <span data-ttu-id="4b665-171">Podfolder *_ViewImports.cshtml* plik zawiera `@model MyModel2` i `@addTagHelper *, MyTagHelper2`.</span><span class="sxs-lookup"><span data-stu-id="4b665-171">A subfolder  *_ViewImports.cshtml* file includes `@model MyModel2` and `@addTagHelper *, MyTagHelper2`.</span></span>

<span data-ttu-id="4b665-172">Widoki w podfolderze i strony będą mieć dostęp do obu pomocników tagów i `MyModel2` modelu.</span><span class="sxs-lookup"><span data-stu-id="4b665-172">Pages and views in the subfolder will have access to both Tag Helpers and the `MyModel2` model.</span></span>

<span data-ttu-id="4b665-173">Jeśli wiele *_ViewImports.cshtml* pliki znajdują się w hierarchii plików są połączone zachowanie dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="4b665-173">If multiple *_ViewImports.cshtml* files are found in the file hierarchy, the combined behavior of the directives are:</span></span>

* <span data-ttu-id="4b665-174">`@addTagHelper`, `@removeTagHelper`: wszystkie działania w kolejności</span><span class="sxs-lookup"><span data-stu-id="4b665-174">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>
* <span data-ttu-id="4b665-175">`@tagHelperPrefix`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola</span><span class="sxs-lookup"><span data-stu-id="4b665-175">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="4b665-176">`@model`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola</span><span class="sxs-lookup"><span data-stu-id="4b665-176">`@model`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="4b665-177">`@inherits`: znajdującego się najbliżej do widoku zastępuje wszystkie inne pola</span><span class="sxs-lookup"><span data-stu-id="4b665-177">`@inherits`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="4b665-178">`@using`: uwzględniono wszystkie; duplikaty są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="4b665-178">`@using`: all are included; duplicates are ignored</span></span>
* <span data-ttu-id="4b665-179">`@inject`: dla każdej właściwości znajdującego się najbliżej do widoku zastępuje wszystkie inne pola o takiej samej nazwie właściwości</span><span class="sxs-lookup"><span data-stu-id="4b665-179">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="4b665-180">Uruchamianie kodu przed każdym widoku</span><span class="sxs-lookup"><span data-stu-id="4b665-180">Running Code Before Each View</span></span>

<span data-ttu-id="4b665-181">Kod, który musi zostać uruchomiony przed każdym widoku lub strony powinny być umieszczone w *_ViewStart.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="4b665-181">Code that needs to run before each view or page should be placed in the *_ViewStart.cshtml* file.</span></span> <span data-ttu-id="4b665-182">Zgodnie z Konwencją *_ViewStart.cshtml* plik znajduje się w *stron* (lub *widoków*) folder.</span><span class="sxs-lookup"><span data-stu-id="4b665-182">By convention, the *_ViewStart.cshtml* file is located in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="4b665-183">Instrukcje, na liście *_ViewStart.cshtml* są uruchamiane przed każdym pełnego widoku (nie układ i widoki częściowe nie).</span><span class="sxs-lookup"><span data-stu-id="4b665-183">The statements listed in *_ViewStart.cshtml* are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="4b665-184">Podobnie jak [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="4b665-184">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* is hierarchical.</span></span> <span data-ttu-id="4b665-185">Jeśli *_ViewStart.cshtml* jest zdefiniowany plik w folderze widoku lub strony będzie uruchamiana po jednym określonym w katalogu głównym *stron* (lub *widoków*) folder (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="4b665-185">If a *_ViewStart.cshtml* file is defined in the view or pages folder, it will be run after the one defined in the root of the *Pages* (or *Views*) folder (if any).</span></span>

<span data-ttu-id="4b665-186">Przykład *_ViewStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="4b665-186">A sample *_ViewStart.cshtml* file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="4b665-187">Pliku powyżej Określa, które będą używane we wszystkich widokach *_Layout.cshtml* układu.</span><span class="sxs-lookup"><span data-stu-id="4b665-187">The file above specifies that all views will use the *_Layout.cshtml* layout.</span></span>

<span data-ttu-id="4b665-188">*_ViewStart.cshtml* i *_ViewImports.cshtml* są **nie** zazwyczaj umieszczane */stron/Shared* (lub   */widoków/Shared*) folder.</span><span class="sxs-lookup"><span data-stu-id="4b665-188">*_ViewStart.cshtml* and *_ViewImports.cshtml* are **not** typically placed in the */Pages/Shared* (or */Views/Shared*) folder.</span></span> <span data-ttu-id="4b665-189">Poziom aplikacji wersje tych plików powinny być umieszczane bezpośrednio w */strony* (lub */widoków*) folder.</span><span class="sxs-lookup"><span data-stu-id="4b665-189">The app-level versions of these files should be placed directly in the */Pages* (or */Views*) folder.</span></span>
