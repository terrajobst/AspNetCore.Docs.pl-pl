---
title: Widoki częściowe w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak za pomocą widoków częściowych Podziel znaczników dużych plików i zmniejszenia duplikowania typowych znaczników na stronach sieci web w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: a836ed073dfe769fc3cc0cd0622b17937747928b
ms.sourcegitcommit: 70fb7c9d5f2ddfcf4747382a9f7159feca7a6aa7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45601759"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="19769-103">Widoki częściowe w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19769-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="19769-104">Przez [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="19769-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="19769-105">Widok częściowy jest [Razor](xref:mvc/views/razor) pliku znaczników (*.cshtml*) który powoduje wyświetlenie danych wyjściowych HTML *w ramach* innego pliku znaczników użytkownika renderowania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="19769-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19769-106">Termin *widoku częściowego* jest używany podczas tworzenia obu aplikacji MVC, gdzie są nazywane plikami znaczników *widoków*, lub aplikacji stron Razor, w którym są nazywane plikami znaczników *stron*.</span><span class="sxs-lookup"><span data-stu-id="19769-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="19769-107">W tym temacie ogólnie odnosi się do widoków MVC i stron Razor strony jako *plikami znaczników*.</span><span class="sxs-lookup"><span data-stu-id="19769-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="19769-108">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19769-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="19769-109">Kiedy należy używać widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="19769-109">When to use partial views</span></span>

<span data-ttu-id="19769-110">Widoki częściowe są efektywnym sposobem:</span><span class="sxs-lookup"><span data-stu-id="19769-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="19769-111">Podziel znaczników dużych plików na mniejsze składniki.</span><span class="sxs-lookup"><span data-stu-id="19769-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="19769-112">W dużych złożonych znaczników pliku składa się z wielu części logiczne, ma swoją zaletę do pracy z każdego z nich izolowane do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="19769-113">Kod w pliku znaczników jest zarządzany, ponieważ znaczniki zawiera tylko ogólną strukturę strony i odwołań pozwalającą na widoki częściowe.</span><span class="sxs-lookup"><span data-stu-id="19769-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="19769-114">Zmniejsz duplikowania typowych treść znaczników w plikach kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="19769-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="19769-115">Te same elementy znaczników są używane w plikach kodu znaczników, widoku częściowego usuwa związanych z duplikowaniem treść znaczników w jeden plik widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="19769-116">Po zmianie kodu znaczników w widoku częściowego powoduje zaktualizowanie wyniku renderowania plików znaczników, które używają widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="19769-117">Widoki częściowe nie powinien być używany do obsługi wspólnych elementów układu.</span><span class="sxs-lookup"><span data-stu-id="19769-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="19769-118">Typowe elementy układu powinny być określone w [_Layout.cshtml](xref:mvc/views/layout) plików.</span><span class="sxs-lookup"><span data-stu-id="19769-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="19769-119">Nie używaj widoku częściowego których wykonywanie złożonych renderowania logiki lub kodu jest wymagane do renderowania kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="19769-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="19769-120">Zamiast widoku częściowego, użyj [widoku składnika](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="19769-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="19769-121">Zadeklaruj widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="19769-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19769-122">Widok częściowy jest *.cshtml* pliku znaczników utrzymane w *widoków* folder (MVC) lub *stron* folder (stron Razor).</span><span class="sxs-lookup"><span data-stu-id="19769-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="19769-123">W przypadku platformy ASP.NET Core MVC, kontroler <xref:Microsoft.AspNetCore.Mvc.ViewResult> zwraca widok lub widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="19769-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="19769-124">Podobne możliwości jest planowana na stron Razor programu ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="19769-124">An analogous capability is planned for Razor Pages in ASP.NET Core 2.2.</span></span> <span data-ttu-id="19769-125">W przypadku stron Razor <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> może zwrócić <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span><span class="sxs-lookup"><span data-stu-id="19769-125">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="19769-126">Odwoływanie się do i renderowania widoków częściowych, które jest opisane w [odwoływać się do widoku częściowego](#reference-a-partial-view) sekcji.</span><span class="sxs-lookup"><span data-stu-id="19769-126">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="19769-127">W przeciwieństwie do widoku składnika MVC lub renderowania stron widoku częściowego nie zostanie uruchomiona *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="19769-127">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="19769-128">Aby uzyskać więcej informacji na temat *_ViewStart.cshtml*, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="19769-128">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="19769-129">Nazwy plików widoku częściowego często rozpoczynają się od znaku podkreślenia (`_`).</span><span class="sxs-lookup"><span data-stu-id="19769-129">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="19769-130">Konwencja nazewnictwa nie jest wymagana, ale pomaga wizualnie odróżnienie widoki częściowe z widoków i stron.</span><span class="sxs-lookup"><span data-stu-id="19769-130">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span> <span data-ttu-id="19769-131">Jeśli nazwa pliku zaczyna się od znaku podkreślenia, strony Razor nie przetwarza pliku znaczników jako stronę stron Razor, nawet wtedy, gdy zawiera pliku znaczników `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="19769-131">When the file name starts with an underscore, Razor Pages doesn't process the markup file as a Razor Pages page, even when the file's markup includes the `@page` directive.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="19769-132">Widok częściowy jest *.cshtml* pliku znaczników utrzymane w *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="19769-132">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="19769-133">Kontroler <xref:Microsoft.AspNetCore.Mvc.ViewResult> zwraca widok lub widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="19769-133">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="19769-134">W przeciwieństwie do renderowania widoku MVC, nie zostanie uruchomiona widoku częściowego *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="19769-134">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="19769-135">Aby uzyskać więcej informacji na temat *_ViewStart.cshtml*, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="19769-135">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="19769-136">Nazwy plików widoku częściowego często rozpoczynają się od znaku podkreślenia (`_`).</span><span class="sxs-lookup"><span data-stu-id="19769-136">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="19769-137">Konwencja nazewnictwa nie jest wymagana, ale pomaga wizualnie odróżnienie widoki częściowe z widoków.</span><span class="sxs-lookup"><span data-stu-id="19769-137">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="19769-138">Odwołanie do widoku częściowego</span><span class="sxs-lookup"><span data-stu-id="19769-138">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19769-139">W pliku znaczników istnieje kilka sposobów, aby odwoływać się do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-139">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="19769-140">Firma Microsoft zaleca aplikacji użyj jednej z następujących metod asynchronicznych renderowania:</span><span class="sxs-lookup"><span data-stu-id="19769-140">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="19769-141">Pomocnik tagu częściowego</span><span class="sxs-lookup"><span data-stu-id="19769-141">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="19769-142">Asynchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="19769-142">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="19769-143">W pliku znaczników istnieją odwoływać się do widoku częściowego na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="19769-143">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="19769-144">Asynchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="19769-144">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="19769-145">Synchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="19769-145">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="19769-146">Firma Microsoft zaleca, aby używać aplikacji [asynchronicznego pomocnika kodu HTML](#asynchronous-html-helper).</span><span class="sxs-lookup"><span data-stu-id="19769-146">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="19769-147">Pomocnik tagu częściowego</span><span class="sxs-lookup"><span data-stu-id="19769-147">Partial Tag Helper</span></span>

<span data-ttu-id="19769-148">[Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) wymaga platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="19769-148">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="19769-149">Częściowe Pomocnik tagu asynchronicznie renderuje zawartość i używa składni HTML:</span><span class="sxs-lookup"><span data-stu-id="19769-149">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="19769-150">Jeśli występuje rozszerzenie pliku Pomocnik tagu odwołuje się widok częściowy, który musi znajdować się w tym samym folderze co wywołanie widoku częściowego pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="19769-150">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="19769-151">Poniższy przykład odwołuje się do widoku częściowego z katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19769-151">The following example references a partial view from the app root.</span></span> <span data-ttu-id="19769-152">Ścieżki zaczynające się od ukośnika tyldy (`~/`) lub ukośnika (`/`) można znaleźć w katalogu głównym aplikacji:</span><span class="sxs-lookup"><span data-stu-id="19769-152">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="19769-153">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="19769-153">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="19769-154">**MVC**</span><span class="sxs-lookup"><span data-stu-id="19769-154">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="19769-155">Poniższy przykład odwołuje się widok częściowy przy użyciu ścieżki względnej:</span><span class="sxs-lookup"><span data-stu-id="19769-155">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="19769-156">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="19769-156">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="19769-157">Asynchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="19769-157">Asynchronous HTML Helper</span></span>

<span data-ttu-id="19769-158">Korzystając z Pomocnika kodu HTML, najlepszym rozwiązaniem jest użycie <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="19769-158">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="19769-159">`PartialAsync` Zwraca <xref:Microsoft.AspNetCore.Html.IHtmlContent> typu zapakowane w <xref:System.Threading.Tasks.Task`1>.</span><span class="sxs-lookup"><span data-stu-id="19769-159">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task`1>.</span></span> <span data-ttu-id="19769-160">Metoda odwołuje się do niej poprzedzania ich oczekiwane wywołanie za pomocą `@` znaków:</span><span class="sxs-lookup"><span data-stu-id="19769-160">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="19769-161">Jeśli występuje rozszerzenie pliku pomocnika kodu HTML odwołuje się widok częściowy, który musi znajdować się w tym samym folderze co wywołanie widoku częściowego pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="19769-161">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="19769-162">Poniższy przykład odwołuje się do widoku częściowego z katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19769-162">The following example references a partial view from the app root.</span></span> <span data-ttu-id="19769-163">Ścieżki zaczynające się od ukośnika tyldy (`~/`) lub ukośnika (`/`) można znaleźć w katalogu głównym aplikacji:</span><span class="sxs-lookup"><span data-stu-id="19769-163">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19769-164">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="19769-164">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="19769-165">**MVC**</span><span class="sxs-lookup"><span data-stu-id="19769-165">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="19769-166">Poniższy przykład odwołuje się widok częściowy przy użyciu ścieżki względnej:</span><span class="sxs-lookup"><span data-stu-id="19769-166">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="19769-167">Alternatywnie można renderować widok częściowy przy użyciu <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="19769-167">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="19769-168">Ta metoda nie zwraca <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="19769-168">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="19769-169">Strumieniowo wyniku renderowania bezpośrednio do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="19769-169">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="19769-170">Ponieważ metoda nie zwraca wyników, musi ona zostać wywołana w bloku kodu Razor:</span><span class="sxs-lookup"><span data-stu-id="19769-170">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="19769-171">Ponieważ `RenderPartialAsync` strumieni renderowana zawartość, zapewnia lepszą wydajność w niektórych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="19769-171">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="19769-172">W sytuacjach krytycznych dla wydajności testu porównawczego stronę korzystając z obu metod i użyć metody, która generuje szybciej uzyskać odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="19769-172">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="19769-173">Synchroniczne pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="19769-173">Synchronous HTML Helper</span></span>

<span data-ttu-id="19769-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> i <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> są synchroniczne odpowiedników `PartialAsync` i `RenderPartialAsync`, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="19769-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="19769-175">Odpowiedniki synchroniczne nie są zalecane, ponieważ istnieją scenariusze, w których one zakleszczenie.</span><span class="sxs-lookup"><span data-stu-id="19769-175">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="19769-176">Także synchroniczne metody są przeznaczone do usunięcia w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="19769-176">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19769-177">Jeśli zachodzi potrzeba wykonania kodu, należy użyć [widoku składnika](xref:mvc/views/view-components) zamiast widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-177">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19769-178">Wywoływanie `Partial` lub `RenderPartial` powoduje ostrzeżenie analizatora programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19769-178">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="19769-179">Na przykład obecność `Partial` pojawi się następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="19769-179">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="19769-180">Użyj IHtmlHelper.Partial może spowodować zakleszczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="19769-180">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="19769-181">Należy rozważyć użycie &lt;częściowe&gt; Pomocnik tagu lub IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="19769-181">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="19769-182">Zastępują wywołania `@Html.Partial` z `@await Html.PartialAsync` lub [Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="19769-182">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="19769-183">Aby uzyskać więcej informacji na temat migracji Pomocnik tagu częściowego, zobacz [migracja z usługi pomocnika kodu HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="19769-183">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="19769-184">Widok częściowy odnajdywania</span><span class="sxs-lookup"><span data-stu-id="19769-184">Partial view discovery</span></span>

<span data-ttu-id="19769-185">Gdy widok częściowy odwołuje się nazwę bez rozszerzenia pliku, w określonej kolejności, przeszukiwane są następujące lokalizacje:</span><span class="sxs-lookup"><span data-stu-id="19769-185">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19769-186">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="19769-186">**Razor Pages**</span></span>

1. <span data-ttu-id="19769-187">Na stronie folderu w trakcie wykonywania</span><span class="sxs-lookup"><span data-stu-id="19769-187">Currently executing page's folder</span></span>
1. <span data-ttu-id="19769-188">Wykres katalogu powyżej folderu strony</span><span class="sxs-lookup"><span data-stu-id="19769-188">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="19769-189">**MVC**</span><span class="sxs-lookup"><span data-stu-id="19769-189">**MVC**</span></span>

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

<span data-ttu-id="19769-190">Następujących Konwencji stosuje się do widoku częściowego odnajdywania:</span><span class="sxs-lookup"><span data-stu-id="19769-190">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="19769-191">Widoki częściowe o takiej samej nazwie pliku są dozwolone, gdy widoki częściowe są w różnych folderach.</span><span class="sxs-lookup"><span data-stu-id="19769-191">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="19769-192">Po odwołujące się do widoku częściowego według nazwy bez rozszerzenia pliku i widoku częściowego znajduje się w folderze zarówno wywołującego i *Shared* folderze widoku częściowego w folderze obiektu wywołującego dostarcza widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-192">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="19769-193">Jeśli widok częściowy nie jest obecny w folderze obiektu wywołującego, widoku częściowego znajduje się w *Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="19769-193">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="19769-194">Częściowe widoki w *Shared* noszą nazwę folderu *udostępnione widoki częściowe* lub *domyślne widoki częściowe*.</span><span class="sxs-lookup"><span data-stu-id="19769-194">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="19769-195">Widoki częściowe mogą być *łańcuchowa*&mdash;widoku częściowego może wywołać inny widok częściowy, jeśli odwołanie cykliczne nie jest tworzona przez wywołania.</span><span class="sxs-lookup"><span data-stu-id="19769-195">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="19769-196">Ścieżki względne są zawsze względne wobec bieżącego pliku, a nie do katalogu głównego lub nadrzędnego pliku.</span><span class="sxs-lookup"><span data-stu-id="19769-196">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="19769-197">A [Razor](xref:mvc/views/razor) `section` zdefiniowane w częściowym widok jest niewidoczne dla nadrzędnej plikach kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="19769-197">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="19769-198">`section` Jest widoczne tylko dla widoku częściowego, w którym jest zdefiniowany.</span><span class="sxs-lookup"><span data-stu-id="19769-198">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="19769-199">Dostęp do danych z widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="19769-199">Access data from partial views</span></span>

<span data-ttu-id="19769-200">Podczas tworzenia wystąpienia widoku częściowego odbiera *kopiowania* z elementu nadrzędnego `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="19769-200">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="19769-201">Aktualizacje wprowadzone do danych w widoku częściowego nie są utrwalane dla widoku nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="19769-201">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="19769-202">`ViewData` zmiany w widoku częściowego zostaną utracone w przypadku, gdy zwraca widok częściowy.</span><span class="sxs-lookup"><span data-stu-id="19769-202">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="19769-203">W poniższym przykładzie pokazano, jak przekazać wystąpienia [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do widoku częściowego:</span><span class="sxs-lookup"><span data-stu-id="19769-203">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="19769-204">Model można przekazać do widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-204">You can pass a model into a partial view.</span></span> <span data-ttu-id="19769-205">Model można niestandardowych obiektów.</span><span class="sxs-lookup"><span data-stu-id="19769-205">The model can be a custom object.</span></span> <span data-ttu-id="19769-206">Można przekazać model przy użyciu `PartialAsync` (powoduje wyświetlenie bloku zawartości do obiektu wywołującego) lub `RenderPartialAsync` (przesyła strumieniowo zawartość w danych wyjściowych):</span><span class="sxs-lookup"><span data-stu-id="19769-206">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="19769-207">**Strony Razor**</span><span class="sxs-lookup"><span data-stu-id="19769-207">**Razor Pages**</span></span>

<span data-ttu-id="19769-208">Następujący kod zawiera Przykładowa aplikacja pochodzi z *Pages/ArticlesRP/ReadRP.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="19769-208">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="19769-209">Strona zawiera dwa widoki częściowe.</span><span class="sxs-lookup"><span data-stu-id="19769-209">The page contains two partial views.</span></span> <span data-ttu-id="19769-210">Drugi widoku częściowego przekazuje się w modelu i `ViewData` widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-210">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="19769-211">`ViewDataDictionary` Przeciążenia konstruktora jest używany do przekazywania nowej `ViewData` słownika przy zachowaniu istniejących `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="19769-211">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

<span data-ttu-id="19769-212">*Pages/Shared/_AuthorPartialRP.cshtml* jest pierwszy widok częściowy odwołuje się *ReadRP.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="19769-212">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="19769-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* jest drugim widoku częściowego odwołuje się *ReadRP.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="19769-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="19769-214">**MVC**</span><span class="sxs-lookup"><span data-stu-id="19769-214">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="19769-215">Następujące znaczniki w pokazuje aplikacji przykładowej *Views/Articles/Read.cshtml* widoku.</span><span class="sxs-lookup"><span data-stu-id="19769-215">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="19769-216">Widok zawiera dwa widoki częściowe.</span><span class="sxs-lookup"><span data-stu-id="19769-216">The view contains two partial views.</span></span> <span data-ttu-id="19769-217">Drugi widoku częściowego przekazuje się w modelu i `ViewData` widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="19769-217">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="19769-218">`ViewDataDictionary` Przeciążenia konstruktora jest używany do przekazywania nowej `ViewData` słownika przy zachowaniu istniejących `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="19769-218">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

<span data-ttu-id="19769-219">*Views/Shared/_AuthorPartial.cshtml* jest pierwszy widok częściowy odwołuje się *ReadRP.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="19769-219">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="19769-220">*Views/Articles/_ArticleSection.cshtml* jest drugim widoku częściowego odwołuje się *Read.cshtml* pliku znaczników:</span><span class="sxs-lookup"><span data-stu-id="19769-220">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="19769-221">W czasie wykonywania, częściowe są renderowane w wyniku renderowania nadrzędnego pliku znaczników, który sam jest renderowany w ramach udostępnionej *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="19769-221">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="19769-222">Pierwszy widoku częściowego powoduje wyświetlenie daty nazwy i publikacji Autor artykułu:</span><span class="sxs-lookup"><span data-stu-id="19769-222">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="19769-223">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="19769-223">Abraham Lincoln</span></span>
>
> <span data-ttu-id="19769-224">Ten widok częściowy z &lt;ścieżka pliku udostępnionego widoku częściowego&gt;.</span><span class="sxs-lookup"><span data-stu-id="19769-224">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="19769-225">11/19/1863 12:00:00 AM</span><span class="sxs-lookup"><span data-stu-id="19769-225">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="19769-226">Drugi widoku częściowego powoduje wyświetlenie sekcji tego artykułu:</span><span class="sxs-lookup"><span data-stu-id="19769-226">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="19769-227">Sekcja jeden indeks: 0</span><span class="sxs-lookup"><span data-stu-id="19769-227">Section One Index: 0</span></span>
>
> <span data-ttu-id="19769-228">Wynik czterech i siedem lat temu...</span><span class="sxs-lookup"><span data-stu-id="19769-228">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="19769-229">Indeks sekcji 2: 1</span><span class="sxs-lookup"><span data-stu-id="19769-229">Section Two Index: 1</span></span>
>
> <span data-ttu-id="19769-230">Teraz możemy są zaangażowane w doskonałe wojnę, testowanie...</span><span class="sxs-lookup"><span data-stu-id="19769-230">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="19769-231">Indeks sekcji 3: 2</span><span class="sxs-lookup"><span data-stu-id="19769-231">Section Three Index: 2</span></span>
>
> <span data-ttu-id="19769-232">Jednak w sensie większe, firma Microsoft może rezerwuje...</span><span class="sxs-lookup"><span data-stu-id="19769-232">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="19769-233">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="19769-233">Additional resources</span></span>

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
