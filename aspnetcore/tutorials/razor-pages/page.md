---
title: Razor Pages szkieletowe w ASP.NET Core
author: rick-anderson
description: Wyjaśnia Razor Pages wygenerowane przez szkielety.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 594fd6186cc73aa054fc9a1478850fa01e481ef2
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034200"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="82f0b-103">Razor Pages szkieletowe w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82f0b-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="82f0b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="82f0b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="82f0b-105">Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w [poprzednim samouczku](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="82f0b-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="82f0b-106">Strony tworzenie, usuwanie, szczegóły i edycja</span><span class="sxs-lookup"><span data-stu-id="82f0b-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="82f0b-107">Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="82f0b-108">Razor Pages pochodzą od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="82f0b-109">Według Konwencji Klasa pochodna `PageModel`jest nazywana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="82f0b-110">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `RazorPagesMovieContext` do strony.</span><span class="sxs-lookup"><span data-stu-id="82f0b-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="82f0b-111">Wszystkie strony szkieletowe są zgodne z tym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="82f0b-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="82f0b-112">Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="82f0b-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="82f0b-113">Gdy żądanie jest wykonywane dla strony, Metoda `OnGetAsync` zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="82f0b-114">`OnGetAsync` lub `OnGet` jest wywoływana w celu zainicjowania stanu strony.</span><span class="sxs-lookup"><span data-stu-id="82f0b-114">`OnGetAsync` or `OnGet` is called to initialize the state of the page.</span></span> <span data-ttu-id="82f0b-115">W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.</span><span class="sxs-lookup"><span data-stu-id="82f0b-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="82f0b-116">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, żadna instrukcja return nie jest używana.</span><span class="sxs-lookup"><span data-stu-id="82f0b-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return statement is used.</span></span> <span data-ttu-id="82f0b-117">Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return.</span><span class="sxs-lookup"><span data-stu-id="82f0b-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="82f0b-118">Na przykład: *Pages/Films/Create. cshtml. cs* `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="82f0b-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="82f0b-119">Przejrzyj stronę */filmy/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="82f0b-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="82f0b-120">Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="82f0b-121">Gdy po symbolu `@` następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do C#.</span><span class="sxs-lookup"><span data-stu-id="82f0b-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

### <a name="the-page-directive"></a><span data-ttu-id="82f0b-122">Dyrektywa @page</span><span class="sxs-lookup"><span data-stu-id="82f0b-122">The @page directive</span></span>

<span data-ttu-id="82f0b-123">Dyrektywa `@page` Razor powoduje, że plik jest akcją MVC, co oznacza, że może obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="82f0b-123">The `@page` Razor directive makes the file an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="82f0b-124">`@page` musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="82f0b-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="82f0b-125">`@page` jest przykładem przejścia do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="82f0b-126">Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="82f0b-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="82f0b-127">Bada wyrażenie lambda użyte w następującym Pomocniku HTML:</span><span class="sxs-lookup"><span data-stu-id="82f0b-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="82f0b-128">Pomocnik HTML `DisplayNameFor` sprawdza Właściwość `Title`, do której odwołuje się wyrażenie lambda w celu określenia nazwy wyświetlanej.</span><span class="sxs-lookup"><span data-stu-id="82f0b-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="82f0b-129">Wyrażenie lambda jest sprawdzane, a nie oceniane.</span><span class="sxs-lookup"><span data-stu-id="82f0b-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="82f0b-130">Oznacza to, że nie ma żadnych naruszeń dostępu, gdy `model`, `model.Movie`lub `model.Movie[0]` jest `null` lub puste.</span><span class="sxs-lookup"><span data-stu-id="82f0b-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` is `null` or empty.</span></span> <span data-ttu-id="82f0b-131">Gdy wyrażenie lambda jest oceniane (na przykład w przypadku `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.</span><span class="sxs-lookup"><span data-stu-id="82f0b-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="82f0b-132">Dyrektywa @model</span><span class="sxs-lookup"><span data-stu-id="82f0b-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="82f0b-133">Dyrektywa `@model` określa typ modelu przekazaną do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="82f0b-134">W powyższym przykładzie wiersz `@model` powoduje, że Klasa pochodna `PageModel`a jest dostępna dla strony Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="82f0b-135">Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayFor` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.</span><span class="sxs-lookup"><span data-stu-id="82f0b-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="82f0b-136">Strona układu</span><span class="sxs-lookup"><span data-stu-id="82f0b-136">The layout page</span></span>

<span data-ttu-id="82f0b-137">Wybierz linki menu (**RazorPagesMovie**, **Home**i **privacy**).</span><span class="sxs-lookup"><span data-stu-id="82f0b-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="82f0b-138">Każda Strona wyświetla ten sam układ menu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="82f0b-139">Układ menu jest implementowany w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="82f0b-140">Otwórz plik *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="82f0b-141">Szablony [układów](xref:mvc/views/layout) umożliwiają układ kontenera HTML:</span><span class="sxs-lookup"><span data-stu-id="82f0b-141">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="82f0b-142">Określone w jednym miejscu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-142">Specified in one place.</span></span>
* <span data-ttu-id="82f0b-143">Stosowane na wielu stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="82f0b-143">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="82f0b-144">Znajdź wiersz `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="82f0b-145">`RenderBody` jest symbolem zastępczym, w którym wszystkie widoki specyficzne dla strony są wyświetlane, *opakowane* na stronie układ.</span><span class="sxs-lookup"><span data-stu-id="82f0b-145">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="82f0b-146">Na przykład wybierz łącze **prywatność** i widok *strony/prywatność. cshtml* jest renderowany wewnątrz metody `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-146">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="82f0b-147">ViewData i układ</span><span class="sxs-lookup"><span data-stu-id="82f0b-147">ViewData and layout</span></span>

<span data-ttu-id="82f0b-148">Rozważ następujące oznakowanie w pliku *Pages/Films/index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-148">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="82f0b-149">Poprzedni wyróżniony znacznik jest przykładem przejścia Razor do C#.</span><span class="sxs-lookup"><span data-stu-id="82f0b-149">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="82f0b-150">`{` i `}` znaków należy umieścić w bloku C# kodu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-150">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="82f0b-151">Klasa bazowa `PageModel` zawiera właściwość słownika `ViewData`, która może służyć do przekazywania danych do widoku.</span><span class="sxs-lookup"><span data-stu-id="82f0b-151">The `PageModel` base class contains a `ViewData` dictionary property that can be used to pass data to a View.</span></span> <span data-ttu-id="82f0b-152">Obiekty są dodawane do słownika `ViewData` przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="82f0b-152">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="82f0b-153">W poprzednim przykładzie właściwość `"Title"` jest dodawana do słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-153">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="82f0b-154">Właściwość `"Title"` jest używana w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-154">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="82f0b-155">Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-155">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="82f0b-156">Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-156">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="82f0b-157">W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="82f0b-157">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="82f0b-158">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="82f0b-158">Update the layout</span></span>

<span data-ttu-id="82f0b-159">Zmień element `<title>` w pliku *Pages/Shared/_Layout. cshtml* w taki sposób, aby wyświetlał **film** zamiast **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="82f0b-159">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="82f0b-160">Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-160">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="82f0b-161">Zastąp poprzedni element następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="82f0b-161">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="82f0b-162">Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="82f0b-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="82f0b-163">W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="82f0b-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="82f0b-164">Atrybut i wartość pomocnika tagu `asp-page="/Movies/Index"` tworzy łącze do strony `/Movies/Index` Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="82f0b-165">Wartość atrybutu `asp-area` jest pusta, dlatego obszar nie jest używany w łączu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-165">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="82f0b-166">Aby uzyskać więcej informacji, zobacz [obszary](xref:mvc/controllers/areas) .</span><span class="sxs-lookup"><span data-stu-id="82f0b-166">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="82f0b-167">Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** .</span><span class="sxs-lookup"><span data-stu-id="82f0b-167">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="82f0b-168">Jeśli występują problemy, zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="82f0b-168">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="82f0b-169">Przetestuj inne linki (**Narzędzia główne**, **RpMovie**, **Utwórz**, **Edytuj**i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="82f0b-169">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="82f0b-170">Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki.</span><span class="sxs-lookup"><span data-stu-id="82f0b-170">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="82f0b-171">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="82f0b-171">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="82f0b-172">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82f0b-172">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="82f0b-173">Aby uzyskać instrukcje dotyczące dodawania przecinków dziesiętnych, zobacz ten problem w usłudze [GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .</span><span class="sxs-lookup"><span data-stu-id="82f0b-173">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="82f0b-174">Właściwość `Layout` jest ustawiona w pliku *Pages/_ViewStart. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-174">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="82f0b-175">Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-175">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="82f0b-176">Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="82f0b-176">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="82f0b-177">Model tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="82f0b-177">The Create page model</span></span>

<span data-ttu-id="82f0b-178">Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-178">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="82f0b-179">Metoda `OnGet` inicjuje wszystkie Stany potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="82f0b-179">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="82f0b-180">Strona tworzenia nie ma żadnego stanu do zainicjowania, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="82f0b-180">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="82f0b-181">W dalszej części tego samouczka zostanie wyświetlony przykład `OnGet` inicjowania stanu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-181">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="82f0b-182">Metoda `Page` tworzy obiekt `PageResult`, który renderuje stronę *Create. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-182">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="82f0b-183">Właściwość `Movie` używa atrybutu `[BindProperty]` do przystąpienia do [powiązania modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="82f0b-183">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="82f0b-184">Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core powiąże ogłoszone wartości z modelem `Movie`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-184">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="82f0b-185">Metoda `OnPostAsync` jest uruchamiana, gdy strona zawiera dane formularza:</span><span class="sxs-lookup"><span data-stu-id="82f0b-185">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="82f0b-186">Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza.</span><span class="sxs-lookup"><span data-stu-id="82f0b-186">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="82f0b-187">Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza.</span><span class="sxs-lookup"><span data-stu-id="82f0b-187">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="82f0b-188">Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę.</span><span class="sxs-lookup"><span data-stu-id="82f0b-188">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="82f0b-189">Weryfikowanie po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="82f0b-189">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="82f0b-190">Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-190">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="82f0b-191">Strona tworzenie Razor</span><span class="sxs-lookup"><span data-stu-id="82f0b-191">The Create Razor Page</span></span>

<span data-ttu-id="82f0b-192">Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:</span><span class="sxs-lookup"><span data-stu-id="82f0b-192">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="82f0b-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="82f0b-193">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="82f0b-194">Program Visual Studio wyświetla następujące znaczniki w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="82f0b-194">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Widok VS17 na stronie Tworzenie. cshtml](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="82f0b-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="82f0b-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="82f0b-197">Następujące pomocnicy tagów są pokazane w powyższym znaczniku:</span><span class="sxs-lookup"><span data-stu-id="82f0b-197">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="82f0b-198">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="82f0b-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="82f0b-199">Program Visual Studio wyświetla następujące znaczniki w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="82f0b-199">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="82f0b-200">Element `<form method="post">` jest [pomocnikiem tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="82f0b-200">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="82f0b-201">Pomocnik tagu formularza automatycznie zawiera [token antysfałszowany](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="82f0b-201">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="82f0b-202">Aparat szkieletu tworzy znaczniki Razor dla każdego pola w modelu (z wyjątkiem identyfikatora) podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="82f0b-202">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="82f0b-203">[Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i `<span asp-validation-for`) wyświetla błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="82f0b-203">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="82f0b-204">Sprawdzanie poprawności jest omówione bardziej szczegółowo w dalszej części tej serii.</span><span class="sxs-lookup"><span data-stu-id="82f0b-204">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="82f0b-205">[Pomocnik tagu etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i atrybut `for` właściwości `Title`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-205">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="82f0b-206">[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) używa atrybutów [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) i tworzy atrybuty HTML, które są zbędne do walidacji jQuery po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="82f0b-206">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="82f0b-207">Aby uzyskać więcej informacji na temat pomocników tagów, takich jak `<form method="post">`, zobacz [pomocników tagów w ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="82f0b-207">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82f0b-208">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="82f0b-208">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="82f0b-209">[Poprzedni: Dodawanie modelu](xref:tutorials/razor-pages/model)
> [Next: Database](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="82f0b-209">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="82f0b-210">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="82f0b-210">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="82f0b-211">Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w [poprzednim samouczku](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="82f0b-211">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="82f0b-212">[Wyświetlanie lub pobieranie](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) próbki.</span><span class="sxs-lookup"><span data-stu-id="82f0b-212">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="82f0b-213">Strony tworzenie, usuwanie, szczegóły i edycja</span><span class="sxs-lookup"><span data-stu-id="82f0b-213">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="82f0b-214">Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-214">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="82f0b-215">Razor Pages pochodzą od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-215">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="82f0b-216">Według Konwencji Klasa pochodna `PageModel`jest nazywana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-216">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="82f0b-217">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `RazorPagesMovieContext` do strony.</span><span class="sxs-lookup"><span data-stu-id="82f0b-217">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="82f0b-218">Wszystkie strony szkieletowe są zgodne z tym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="82f0b-218">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="82f0b-219">Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="82f0b-219">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="82f0b-220">Gdy żądanie jest wykonywane dla strony, Metoda `OnGetAsync` zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-220">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="82f0b-221">do zainicjowania stanu strony `OnGetAsync` lub `OnGet` jest wywoływana na stronie Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-221">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="82f0b-222">W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.</span><span class="sxs-lookup"><span data-stu-id="82f0b-222">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="82f0b-223">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, żadna metoda return nie jest używana.</span><span class="sxs-lookup"><span data-stu-id="82f0b-223">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="82f0b-224">Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return.</span><span class="sxs-lookup"><span data-stu-id="82f0b-224">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="82f0b-225">Na przykład: *Pages/Films/Create. cshtml. cs* `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="82f0b-225">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="82f0b-226">Przejrzyj stronę */filmy/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="82f0b-226">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="82f0b-227">Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-227">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="82f0b-228">Gdy po symbolu `@` następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do C#.</span><span class="sxs-lookup"><span data-stu-id="82f0b-228">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="82f0b-229">Dyrektywa `@page` Razor sprawia, że plik jest akcją MVC, co oznacza, że może obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="82f0b-229">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="82f0b-230">`@page` musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="82f0b-230">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="82f0b-231">`@page` jest przykładem przejścia do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-231">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="82f0b-232">Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="82f0b-232">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="82f0b-233">Bada wyrażenie lambda użyte w następującym Pomocniku HTML:</span><span class="sxs-lookup"><span data-stu-id="82f0b-233">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="82f0b-234">Pomocnik HTML `DisplayNameFor` sprawdza Właściwość `Title`, do której odwołuje się wyrażenie lambda w celu określenia nazwy wyświetlanej.</span><span class="sxs-lookup"><span data-stu-id="82f0b-234">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="82f0b-235">Wyrażenie lambda jest sprawdzane, a nie oceniane.</span><span class="sxs-lookup"><span data-stu-id="82f0b-235">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="82f0b-236">Oznacza to, że nie ma żadnych naruszeń dostępu, gdy `model`, `model.Movie`lub `model.Movie[0]` są `null` lub puste.</span><span class="sxs-lookup"><span data-stu-id="82f0b-236">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="82f0b-237">Gdy wyrażenie lambda jest oceniane (na przykład w przypadku `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.</span><span class="sxs-lookup"><span data-stu-id="82f0b-237">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="82f0b-238">Dyrektywa @model</span><span class="sxs-lookup"><span data-stu-id="82f0b-238">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="82f0b-239">Dyrektywa `@model` określa typ modelu przekazaną do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-239">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="82f0b-240">W powyższym przykładzie wiersz `@model` powoduje, że Klasa pochodna `PageModel`a jest dostępna dla strony Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-240">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="82f0b-241">Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayFor` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.</span><span class="sxs-lookup"><span data-stu-id="82f0b-241">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="82f0b-242">Strona układu</span><span class="sxs-lookup"><span data-stu-id="82f0b-242">The layout page</span></span>

<span data-ttu-id="82f0b-243">Wybierz linki menu (**RazorPagesMovie**, **Home**i **privacy**).</span><span class="sxs-lookup"><span data-stu-id="82f0b-243">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="82f0b-244">Każda Strona wyświetla ten sam układ menu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-244">Each page shows the same menu layout.</span></span> <span data-ttu-id="82f0b-245">Układ menu jest implementowany w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-245">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="82f0b-246">Otwórz plik *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-246">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="82f0b-247">Szablony [układów](xref:mvc/views/layout) umożliwiają określenie układu kontenera HTML witryny w jednym miejscu, a następnie zastosowanie go na wielu stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="82f0b-247">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="82f0b-248">Znajdź wiersz `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-248">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="82f0b-249">`RenderBody` jest symbolem zastępczym *, w* którym wszystkie utworzone widoki specyficzne dla strony są widoczne na stronie układ.</span><span class="sxs-lookup"><span data-stu-id="82f0b-249">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="82f0b-250">Na przykład w przypadku wybrania linku **prywatność** widok **strony/prywatność. cshtml** jest renderowany wewnątrz metody `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-250">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="82f0b-251">ViewData i układ</span><span class="sxs-lookup"><span data-stu-id="82f0b-251">ViewData and layout</span></span>

<span data-ttu-id="82f0b-252">Rozważmy następujący kod z pliku *Pages/Films/index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-252">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="82f0b-253">Poprzedni wyróżniony kod jest przykładem przejścia Razor do C#.</span><span class="sxs-lookup"><span data-stu-id="82f0b-253">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="82f0b-254">`{` i `}` znaków należy umieścić w bloku C# kodu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-254">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="82f0b-255">Klasa bazowa `PageModel` ma właściwość słownika `ViewData`, która może służyć do dodawania danych, które mają zostać przekazane do widoku.</span><span class="sxs-lookup"><span data-stu-id="82f0b-255">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="82f0b-256">Obiekty są dodawane do słownika `ViewData` przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="82f0b-256">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="82f0b-257">W poprzednim przykładzie właściwość "title" jest dodawana do słownika `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-257">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="82f0b-258">Właściwość "title" jest używana w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-258">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="82f0b-259">Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-259">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="82f0b-260">Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor, które nie pojawia się w pliku układu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-260">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="82f0b-261">W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="82f0b-261">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="82f0b-262">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="82f0b-262">Update the layout</span></span>

<span data-ttu-id="82f0b-263">Zmień element `<title>` w pliku *Pages/Shared/_Layout. cshtml* w taki sposób, aby wyświetlał **film** zamiast **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="82f0b-263">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="82f0b-264">Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-264">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="82f0b-265">Zastąp poprzedni element następującym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="82f0b-265">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="82f0b-266">Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="82f0b-266">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="82f0b-267">W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="82f0b-267">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="82f0b-268">Atrybut i wartość pomocnika tagu `asp-page="/Movies/Index"` tworzy łącze do strony `/Movies/Index` Razor.</span><span class="sxs-lookup"><span data-stu-id="82f0b-268">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="82f0b-269">Wartość atrybutu `asp-area` jest pusta, dlatego obszar nie jest używany w łączu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-269">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="82f0b-270">Aby uzyskać więcej informacji, zobacz [obszary](xref:mvc/controllers/areas) .</span><span class="sxs-lookup"><span data-stu-id="82f0b-270">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="82f0b-271">Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** .</span><span class="sxs-lookup"><span data-stu-id="82f0b-271">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="82f0b-272">Jeśli występują problemy, zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="82f0b-272">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="82f0b-273">Przetestuj inne linki (**Narzędzia główne**, **RpMovie**, **Utwórz**, **Edytuj**i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="82f0b-273">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="82f0b-274">Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki.</span><span class="sxs-lookup"><span data-stu-id="82f0b-274">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="82f0b-275">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="82f0b-275">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="82f0b-276">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82f0b-276">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="82f0b-277">Ten [problem w usłudze GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) zawiera instrukcje dotyczące dodawania przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="82f0b-277">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="82f0b-278">Właściwość `Layout` jest ustawiona w pliku *Pages/_ViewStart. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-278">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="82f0b-279">Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-279">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="82f0b-280">Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="82f0b-280">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="82f0b-281">Model tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="82f0b-281">The Create page model</span></span>

<span data-ttu-id="82f0b-282">Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="82f0b-282">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="82f0b-283">Metoda `OnGet` inicjuje wszystkie Stany potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="82f0b-283">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="82f0b-284">Strona tworzenia nie ma żadnego stanu do zainicjowania, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="82f0b-284">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="82f0b-285">W dalszej części tego samouczka zobaczysz stan zainicjowania metody `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-285">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="82f0b-286">Metoda `Page` tworzy obiekt `PageResult`, który renderuje stronę *Create. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="82f0b-286">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="82f0b-287">Właściwość `Movie` używa atrybutu `[BindProperty]` do przystąpienia do [powiązania modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="82f0b-287">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="82f0b-288">Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core powiąże ogłoszone wartości z modelem `Movie`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-288">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="82f0b-289">Metoda `OnPostAsync` jest uruchamiana, gdy strona zawiera dane formularza:</span><span class="sxs-lookup"><span data-stu-id="82f0b-289">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="82f0b-290">Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza.</span><span class="sxs-lookup"><span data-stu-id="82f0b-290">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="82f0b-291">Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza.</span><span class="sxs-lookup"><span data-stu-id="82f0b-291">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="82f0b-292">Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę.</span><span class="sxs-lookup"><span data-stu-id="82f0b-292">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="82f0b-293">Weryfikowanie po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="82f0b-293">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="82f0b-294">Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="82f0b-294">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="82f0b-295">Strona tworzenie Razor</span><span class="sxs-lookup"><span data-stu-id="82f0b-295">The Create Razor Page</span></span>

<span data-ttu-id="82f0b-296">Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:</span><span class="sxs-lookup"><span data-stu-id="82f0b-296">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="82f0b-297">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="82f0b-297">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="82f0b-298">Program Visual Studio Wyświetla tag `<form method="post">` w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="82f0b-298">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Widok VS17 na stronie Tworzenie. cshtml](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="82f0b-300">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="82f0b-300">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="82f0b-301">Aby uzyskać więcej informacji na temat pomocników tagów, takich jak `<form method="post">`, zobacz [pomocników tagów w ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="82f0b-301">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="82f0b-302">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="82f0b-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="82f0b-303">Visual Studio dla komputerów Mac wyświetla tag `<form method="post">` w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="82f0b-303">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="82f0b-304">Element `<form method="post">` jest [pomocnikiem tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="82f0b-304">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="82f0b-305">Pomocnik tagu formularza automatycznie zawiera [token antysfałszowany](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="82f0b-305">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="82f0b-306">Aparat szkieletu tworzy znaczniki Razor dla każdego pola w modelu (z wyjątkiem identyfikatora) podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="82f0b-306">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="82f0b-307">[Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i `<span asp-validation-for`) wyświetla błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="82f0b-307">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="82f0b-308">Sprawdzanie poprawności jest omówione bardziej szczegółowo w dalszej części tej serii.</span><span class="sxs-lookup"><span data-stu-id="82f0b-308">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="82f0b-309">[Pomocnik tagu etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i atrybut `for` właściwości `Title`.</span><span class="sxs-lookup"><span data-stu-id="82f0b-309">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="82f0b-310">[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) używa atrybutów [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) i tworzy atrybuty HTML, które są zbędne do walidacji jQuery po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="82f0b-310">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82f0b-311">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="82f0b-311">Additional resources</span></span>

* [<span data-ttu-id="82f0b-312">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="82f0b-312">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="82f0b-313">[Poprzedni: Dodawanie modelu](xref:tutorials/razor-pages/model)
> [Next: Database](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="82f0b-313">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
