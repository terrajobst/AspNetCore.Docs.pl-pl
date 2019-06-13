---
title: Widoki częściowe w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak za pomocą widoków częściowych Podziel znaczników dużych plików i zmniejszenia duplikowania typowych znaczników na stronach sieci web w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 06/12/2019
uid: mvc/views/partial
ms.openlocfilehash: 901fd52f89969141713e443890781a77308bd901
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034915"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="8481e-103">Widoki częściowe w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8481e-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="8481e-104">Przez [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="8481e-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="8481e-105">Widok częściowy jest [Razor](xref:mvc/views/razor) pliku znaczników ( *.cshtml*) który powoduje wyświetlenie danych wyjściowych HTML *w ramach* innego pliku znaczników użytkownika renderowania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="8481e-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8481e-106">Termin *widoku częściowego* jest używany podczas tworzenia obu aplikacji MVC, gdzie są nazywane plikami znaczników *widoków*, lub aplikacji stron Razor, w którym są nazywane plikami znaczników *stron*.</span><span class="sxs-lookup"><span data-stu-id="8481e-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="8481e-107">W tym temacie ogólnie odnosi się do widoków MVC i stron Razor strony jako *plikami znaczników*.</span><span class="sxs-lookup"><span data-stu-id="8481e-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="8481e-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8481e-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="8481e-109">Kiedy należy używać widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="8481e-109">When to use partial views</span></span>

<span data-ttu-id="8481e-110">Widoki częściowe są efektywnym sposobem:</span><span class="sxs-lookup"><span data-stu-id="8481e-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="8481e-111">Podziel znaczników dużych plików na mniejsze składniki.</span><span class="sxs-lookup"><span data-stu-id="8481e-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="8481e-112">W dużych złożonych znaczników pliku składa się z wielu części logiczne, ma swoją zaletę do pracy z każdego z nich izolowane do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="8481e-113">Kod w pliku znaczników jest zarządzany, ponieważ znaczniki zawiera tylko ogólną strukturę strony i odwołań pozwalającą na widoki częściowe.</span><span class="sxs-lookup"><span data-stu-id="8481e-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="8481e-114">Zmniejsz duplikowania typowych treść znaczników w plikach kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="8481e-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="8481e-115">Te same elementy znaczników są używane w plikach kodu znaczników, widoku częściowego usuwa związanych z duplikowaniem treść znaczników w jeden plik widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="8481e-116">Po zmianie kodu znaczników w widoku częściowego powoduje zaktualizowanie wyniku renderowania plików znaczników, które używają widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="8481e-117">Widoki częściowe nie powinien być używany do obsługi wspólnych elementów układu.</span><span class="sxs-lookup"><span data-stu-id="8481e-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="8481e-118">Typowe elementy układu powinny być określone w [_Layout.cshtml](xref:mvc/views/layout) plików.</span><span class="sxs-lookup"><span data-stu-id="8481e-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="8481e-119">Nie używaj widoku częściowego których wykonywanie złożonych renderowania logiki lub kodu jest wymagane do renderowania kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="8481e-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="8481e-120">Zamiast widoku częściowego, użyj [widoku składnika](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="8481e-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="8481e-121">Zadeklaruj widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="8481e-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8481e-122">Widok częściowy jest *.cshtml* pliku znaczników utrzymane w *widoków* folder (MVC) lub *stron* folder (stron Razor).</span><span class="sxs-lookup"><span data-stu-id="8481e-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="8481e-123">W przypadku platformy ASP.NET Core MVC, kontroler <xref:Microsoft.AspNetCore.Mvc.ViewResult> zwraca widok lub widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="8481e-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="8481e-124">W przypadku stron Razor <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> może zwrócić reprezentowane jako widok częściowy <xref:Microsoft.AspNetCore.Mvc.PartialViewResult> obiektu.</span><span class="sxs-lookup"><span data-stu-id="8481e-124">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a partial view represented as a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult> object.</span></span> <span data-ttu-id="8481e-125">Odwoływanie się do i renderowania widoków częściowych, które jest opisane w [odwoływać się do widoku częściowego](#reference-a-partial-view) sekcji.</span><span class="sxs-lookup"><span data-stu-id="8481e-125">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="8481e-126">W przeciwieństwie do widoku składnika MVC lub renderowania stron widoku częściowego nie zostanie uruchomiona *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8481e-126">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="8481e-127">Aby uzyskać więcej informacji na temat *_ViewStart.cshtml*, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="8481e-127">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="8481e-128">Nazwy plików widoku częściowego często rozpoczynają się od znaku podkreślenia (`_`).</span><span class="sxs-lookup"><span data-stu-id="8481e-128">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="8481e-129">Konwencja nazewnictwa nie jest wymagana, ale pomaga wizualnie odróżnienie widoki częściowe z widoków i stron.</span><span class="sxs-lookup"><span data-stu-id="8481e-129">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="8481e-130">Widok częściowy jest *.cshtml* pliku znaczników utrzymane w *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="8481e-130">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="8481e-131">Kontroler <xref:Microsoft.AspNetCore.Mvc.ViewResult> zwraca widok lub widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="8481e-131">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="8481e-132">Odwoływanie się do i renderowania widoków częściowych, które jest opisane w [odwoływać się do widoku częściowego](#reference-a-partial-view) sekcji.</span><span class="sxs-lookup"><span data-stu-id="8481e-132">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="8481e-133">W przeciwieństwie do renderowania widoku MVC, nie zostanie uruchomiona widoku częściowego *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8481e-133">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="8481e-134">Aby uzyskać więcej informacji na temat *_ViewStart.cshtml*, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="8481e-134">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="8481e-135">Nazwy plików widoku częściowego często rozpoczynają się od znaku podkreślenia (`_`).</span><span class="sxs-lookup"><span data-stu-id="8481e-135">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="8481e-136">Konwencja nazewnictwa nie jest wymagana, ale pomaga wizualnie odróżnienie widoki częściowe z widoków.</span><span class="sxs-lookup"><span data-stu-id="8481e-136">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="8481e-137">Odwołanie do widoku częściowego</span><span class="sxs-lookup"><span data-stu-id="8481e-137">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.0"

### <a name="use-a-partial-view-in-a-razor-pages-pagemodel"></a><span data-ttu-id="8481e-138">Użyj widoku częściowego w PageModel stron Razor</span><span class="sxs-lookup"><span data-stu-id="8481e-138">Use a partial view in a Razor Pages PageModel</span></span>

<span data-ttu-id="8481e-139">W programie ASP.NET Core 2.0 lub 2.1 renderuje następującą metodę programu obsługi  *\_AuthorPartialRP.cshtml* widoku częściowego na potrzeby odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="8481e-139">In ASP.NET Core 2.0 or 2.1, the following handler method renders the *\_AuthorPartialRP.cshtml* partial view to the response:</span></span>

```csharp
public IActionResult OnGetPartial() =>
    new PartialViewResult
    {
        ViewName = "_AuthorPartialRP",
        ViewData = ViewData,
    };
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8481e-140">W programie ASP.NET Core 2.2 lub nowszej, można również wywołać metody obsługi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Partial*> metody do tworzenia `PartialViewResult` obiektu:</span><span class="sxs-lookup"><span data-stu-id="8481e-140">In ASP.NET Core 2.2 or later, a handler method can alternatively call the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Partial*> method to produce a `PartialViewResult` object:</span></span>

[!code-csharp[](partial/sample/PartialViewsSample/Pages/DiscoveryRP.cshtml.cs?name=snippet_OnGetPartial)]

::: moniker-end

### <a name="use-a-partial-view-in-a-markup-file"></a><span data-ttu-id="8481e-141">Użyj widoku częściowego w pliku znaczników</span><span class="sxs-lookup"><span data-stu-id="8481e-141">Use a partial view in a markup file</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8481e-142">W pliku znaczników istnieje kilka sposobów, aby odwoływać się do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-142">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="8481e-143">Firma Microsoft zaleca aplikacji użyj jednej z następujących metod asynchronicznych renderowania:</span><span class="sxs-lookup"><span data-stu-id="8481e-143">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="8481e-144">Pomocnik tagu częściowego</span><span class="sxs-lookup"><span data-stu-id="8481e-144">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="8481e-145">Asynchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="8481e-145">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="8481e-146">W pliku znaczników istnieją odwoływać się do widoku częściowego na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="8481e-146">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="8481e-147">Asynchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="8481e-147">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="8481e-148">Synchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="8481e-148">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="8481e-149">Firma Microsoft zaleca, aby używać aplikacji [asynchronicznego pomocnika kodu HTML](#asynchronous-html-helper).</span><span class="sxs-lookup"><span data-stu-id="8481e-149">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="8481e-150">Pomocnik tagu częściowego</span><span class="sxs-lookup"><span data-stu-id="8481e-150">Partial Tag Helper</span></span>

<span data-ttu-id="8481e-151">[Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) wymaga platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="8481e-151">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="8481e-152">Częściowe Pomocnik tagu asynchronicznie renderuje zawartość i używa składni HTML:</span><span class="sxs-lookup"><span data-stu-id="8481e-152">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="8481e-153">Jeśli występuje rozszerzenie pliku Pomocnik tagu odwołuje się widok częściowy, który musi znajdować się w tym samym folderze co wywołanie widoku częściowego pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="8481e-153">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="8481e-154">Poniższy przykład odwołuje się do widoku częściowego z katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8481e-154">The following example references a partial view from the app root.</span></span> <span data-ttu-id="8481e-155">Ścieżki zaczynające się od ukośnika tyldy (`~/`) lub ukośnika (`/`) można znaleźć w katalogu głównym aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8481e-155">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="8481e-156">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="8481e-156">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="8481e-157">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8481e-157">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="8481e-158">Poniższy przykład odwołuje się widok częściowy przy użyciu ścieżki względnej:</span><span class="sxs-lookup"><span data-stu-id="8481e-158">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="8481e-159">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="8481e-159">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="8481e-160">Asynchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="8481e-160">Asynchronous HTML Helper</span></span>

<span data-ttu-id="8481e-161">Korzystając z Pomocnika kodu HTML, najlepszym rozwiązaniem jest użycie <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="8481e-161">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="8481e-162">`PartialAsync` Zwraca <xref:Microsoft.AspNetCore.Html.IHtmlContent> typu zapakowane w <xref:System.Threading.Tasks.Task%601>.</span><span class="sxs-lookup"><span data-stu-id="8481e-162">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task%601>.</span></span> <span data-ttu-id="8481e-163">Metoda odwołuje się do niej poprzedzania ich oczekiwane wywołanie za pomocą `@` znaków:</span><span class="sxs-lookup"><span data-stu-id="8481e-163">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="8481e-164">Jeśli występuje rozszerzenie pliku pomocnika kodu HTML odwołuje się widok częściowy, który musi znajdować się w tym samym folderze co wywołanie widoku częściowego pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="8481e-164">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="8481e-165">Poniższy przykład odwołuje się do widoku częściowego z katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8481e-165">The following example references a partial view from the app root.</span></span> <span data-ttu-id="8481e-166">Ścieżki zaczynające się od ukośnika tyldy (`~/`) lub ukośnika (`/`) można znaleźć w katalogu głównym aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8481e-166">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8481e-167">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="8481e-167">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="8481e-168">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8481e-168">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="8481e-169">Poniższy przykład odwołuje się widok częściowy przy użyciu ścieżki względnej:</span><span class="sxs-lookup"><span data-stu-id="8481e-169">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="8481e-170">Alternatywnie można renderować widok częściowy przy użyciu <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="8481e-170">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="8481e-171">Ta metoda nie zwraca <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="8481e-171">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="8481e-172">Strumieniowo wyniku renderowania bezpośrednio do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8481e-172">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="8481e-173">Ponieważ metoda nie zwraca wyników, musi ona zostać wywołana w bloku kodu Razor:</span><span class="sxs-lookup"><span data-stu-id="8481e-173">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="8481e-174">Ponieważ `RenderPartialAsync` strumieni renderowana zawartość, zapewnia lepszą wydajność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="8481e-174">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="8481e-175">W sytuacjach krytycznych dla wydajności testu porównawczego stronę korzystając z obu metod i użyć metody, która generuje szybciej uzyskać odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="8481e-175">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="8481e-176">Synchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="8481e-176">Synchronous HTML Helper</span></span>

<span data-ttu-id="8481e-177"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> i <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> są synchroniczne odpowiedników `PartialAsync` i `RenderPartialAsync`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="8481e-177"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="8481e-178">Odpowiedniki synchroniczne nie są zalecane, ponieważ istnieją scenariusze, w których one zakleszczenie.</span><span class="sxs-lookup"><span data-stu-id="8481e-178">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="8481e-179">Także synchroniczne metody są przeznaczone do usunięcia w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="8481e-179">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8481e-180">Jeśli zachodzi potrzeba wykonania kodu, należy użyć [widoku składnika](xref:mvc/views/view-components) zamiast widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-180">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8481e-181">Wywoływanie `Partial` lub `RenderPartial` powoduje ostrzeżenie analizatora programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8481e-181">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="8481e-182">Na przykład obecność `Partial` pojawi się następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="8481e-182">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="8481e-183">Użyj IHtmlHelper.Partial może spowodować zakleszczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8481e-183">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="8481e-184">Należy rozważyć użycie &lt;częściowe&gt; Pomocnik tagu lub IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="8481e-184">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="8481e-185">Zastępują wywołania `@Html.Partial` z `@await Html.PartialAsync` lub [Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8481e-185">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="8481e-186">Aby uzyskać więcej informacji na temat migracji Pomocnik tagu częściowego, zobacz [migracja z usługi pomocnika kodu HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="8481e-186">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="8481e-187">Widok częściowy odnajdywania</span><span class="sxs-lookup"><span data-stu-id="8481e-187">Partial view discovery</span></span>

<span data-ttu-id="8481e-188">Gdy widok częściowy odwołuje się nazwę bez rozszerzenia pliku, w określonej kolejności, przeszukiwane są następujące lokalizacje:</span><span class="sxs-lookup"><span data-stu-id="8481e-188">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8481e-189">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="8481e-189">**Razor Pages**</span></span>

1. <span data-ttu-id="8481e-190">Na stronie folderu w trakcie wykonywania</span><span class="sxs-lookup"><span data-stu-id="8481e-190">Currently executing page's folder</span></span>
1. <span data-ttu-id="8481e-191">Wykres katalogu powyżej folderu strony</span><span class="sxs-lookup"><span data-stu-id="8481e-191">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="8481e-192">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8481e-192">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="8481e-193">Następujących Konwencji stosuje się do widoku częściowego odnajdywania:</span><span class="sxs-lookup"><span data-stu-id="8481e-193">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="8481e-194">Widoki częściowe o takiej samej nazwie pliku są dozwolone, gdy widoki częściowe są w różnych folderach.</span><span class="sxs-lookup"><span data-stu-id="8481e-194">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="8481e-195">Po odwołujące się do widoku częściowego według nazwy bez rozszerzenia pliku i widoku częściowego znajduje się w folderze zarówno wywołującego i *Shared* folderze widoku częściowego w folderze obiektu wywołującego dostarcza widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-195">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="8481e-196">Jeśli widok częściowy nie jest obecny w folderze obiektu wywołującego, widoku częściowego znajduje się w *Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="8481e-196">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="8481e-197">Częściowe widoki w *Shared* noszą nazwę folderu *udostępnione widoki częściowe* lub *domyślne widoki częściowe*.</span><span class="sxs-lookup"><span data-stu-id="8481e-197">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="8481e-198">Widoki częściowe mogą być *łańcuchowa*&mdash;widoku częściowego może wywołać inny widok częściowy, jeśli odwołanie cykliczne nie jest tworzona przez wywołania.</span><span class="sxs-lookup"><span data-stu-id="8481e-198">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="8481e-199">Ścieżki względne są zawsze względne wobec bieżącego pliku, a nie do katalogu głównego lub nadrzędnego pliku.</span><span class="sxs-lookup"><span data-stu-id="8481e-199">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="8481e-200">A [Razor](xref:mvc/views/razor) `section` zdefiniowane w częściowym widok jest niewidoczne dla nadrzędnej plikach kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="8481e-200">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="8481e-201">`section` Jest widoczne tylko dla widoku częściowego, w którym jest zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="8481e-201">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="8481e-202">Dostęp do danych z widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="8481e-202">Access data from partial views</span></span>

<span data-ttu-id="8481e-203">Podczas tworzenia wystąpienia widoku częściowego odbiera *kopiowania* z elementu nadrzędnego `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="8481e-203">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="8481e-204">Aktualizacje wprowadzone do danych w widoku częściowego nie są utrwalane dla widoku nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="8481e-204">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="8481e-205">`ViewData` zmiany w widoku częściowego zostaną utracone w przypadku, gdy zwraca widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="8481e-205">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="8481e-206">W poniższym przykładzie pokazano, jak przekazać wystąpienia [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="8481e-206">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="8481e-207">Model można przekazać do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-207">You can pass a model into a partial view.</span></span> <span data-ttu-id="8481e-208">Model można niestandardowych obiektów.</span><span class="sxs-lookup"><span data-stu-id="8481e-208">The model can be a custom object.</span></span> <span data-ttu-id="8481e-209">Można przekazać model przy użyciu `PartialAsync` (powoduje wyświetlenie bloku zawartości do obiektu wywołującego) lub `RenderPartialAsync` (przesyła strumieniowo zawartość w danych wyjściowych):</span><span class="sxs-lookup"><span data-stu-id="8481e-209">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8481e-210">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="8481e-210">**Razor Pages**</span></span>

<span data-ttu-id="8481e-211">Następujący kod zawiera Przykładowa aplikacja pochodzi z *Pages/ArticlesRP/ReadRP.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="8481e-211">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="8481e-212">Strona zawiera dwa widoki częściowe.</span><span class="sxs-lookup"><span data-stu-id="8481e-212">The page contains two partial views.</span></span> <span data-ttu-id="8481e-213">Drugi widoku częściowego przekazuje się w modelu i `ViewData` widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-213">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="8481e-214">`ViewDataDictionary` Przeciążenia konstruktora jest używany do przekazywania nowej `ViewData` słownika przy zachowaniu istniejących `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="8481e-214">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-20)]

<span data-ttu-id="8481e-215">*Pages/Shared/_AuthorPartialRP.cshtml* jest pierwszy widok częściowy odwołuje się *ReadRP.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="8481e-215">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="8481e-216">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* jest drugim widoku częściowego odwołuje się *ReadRP.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="8481e-216">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="8481e-217">**MVC**</span><span class="sxs-lookup"><span data-stu-id="8481e-217">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="8481e-218">Następujące znaczniki w pokazuje aplikacji przykładowej *Views/Articles/Read.cshtml* widoku.</span><span class="sxs-lookup"><span data-stu-id="8481e-218">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="8481e-219">Widok zawiera dwa widoki częściowe.</span><span class="sxs-lookup"><span data-stu-id="8481e-219">The view contains two partial views.</span></span> <span data-ttu-id="8481e-220">Drugi widoku częściowego przekazuje się w modelu i `ViewData` widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="8481e-220">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="8481e-221">`ViewDataDictionary` Przeciążenia konstruktora jest używany do przekazywania nowej `ViewData` słownika przy zachowaniu istniejących `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="8481e-221">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-20)]

<span data-ttu-id="8481e-222">*Views/Shared/_AuthorPartial.cshtml* jest pierwszy widok częściowy odwołuje się *ReadRP.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="8481e-222">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="8481e-223">*Views/Articles/_ArticleSection.cshtml* jest drugim widoku częściowego odwołuje się *Read.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="8481e-223">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="8481e-224">W czasie wykonywania, częściowe są renderowane w wyniku renderowania nadrzędnego pliku znaczników, który sam jest renderowany w ramach udostępnionej *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8481e-224">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="8481e-225">Pierwszy widoku częściowego powoduje wyświetlenie daty nazwy i publikacji Autor artykułu:</span><span class="sxs-lookup"><span data-stu-id="8481e-225">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="8481e-226">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="8481e-226">Abraham Lincoln</span></span>
>
> <span data-ttu-id="8481e-227">Ten widok częściowy z &lt;ścieżka pliku udostępnionego widoku częściowego&gt;.</span><span class="sxs-lookup"><span data-stu-id="8481e-227">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="8481e-228">11/19/1863 12:00:00 AM</span><span class="sxs-lookup"><span data-stu-id="8481e-228">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="8481e-229">Drugi widoku częściowego powoduje wyświetlenie sekcji tego artykułu:</span><span class="sxs-lookup"><span data-stu-id="8481e-229">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="8481e-230">Sekcja jednego indeksu: 0</span><span class="sxs-lookup"><span data-stu-id="8481e-230">Section One Index: 0</span></span>
>
> <span data-ttu-id="8481e-231">Wynik czterech i siedem lat temu...</span><span class="sxs-lookup"><span data-stu-id="8481e-231">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="8481e-232">Indeks dwóch części: 1</span><span class="sxs-lookup"><span data-stu-id="8481e-232">Section Two Index: 1</span></span>
>
> <span data-ttu-id="8481e-233">Teraz możemy są zaangażowane w doskonałe wojnę, testowanie...</span><span class="sxs-lookup"><span data-stu-id="8481e-233">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="8481e-234">Indeks sekcji 3: 2</span><span class="sxs-lookup"><span data-stu-id="8481e-234">Section Three Index: 2</span></span>
>
> <span data-ttu-id="8481e-235">Jednak w sensie większe, firma Microsoft może rezerwuje...</span><span class="sxs-lookup"><span data-stu-id="8481e-235">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8481e-236">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8481e-236">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
