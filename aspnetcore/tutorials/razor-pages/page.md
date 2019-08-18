---
title: Razor Pages szkieletowe w ASP.NET Core
author: rick-anderson
description: Wyjaśnia Razor Pages wygenerowane przez szkielety.
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 00a8458b9bee4d30c5774a980ff5c23fb8872737
ms.sourcegitcommit: 38cac2552029fc19428722bb204ff9e16eb94225
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2019
ms.locfileid: "69573145"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="55abc-103">Razor Pages szkieletowe w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55abc-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="55abc-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="55abc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="55abc-105">Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w [poprzednim samouczku](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="55abc-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="55abc-106">Strony tworzenie, usuwanie, szczegóły i edycja</span><span class="sxs-lookup"><span data-stu-id="55abc-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="55abc-107">Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="55abc-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="55abc-108">Razor Pages pochodzą od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="55abc-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="55abc-109">Zgodnie z `PageModel`Konwencją Klasa pochodna jest wywoływana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="55abc-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="55abc-110">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `RazorPagesMovieContext` do strony.</span><span class="sxs-lookup"><span data-stu-id="55abc-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="55abc-111">Wszystkie strony szkieletowe są zgodne z tym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="55abc-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="55abc-112">Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="55abc-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="55abc-113">Gdy żądanie jest wykonywane dla strony, `OnGetAsync` Metoda zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="55abc-114">`OnGetAsync`lub `OnGet` jest wywoływana na stronie Razor, aby zainicjować stan dla strony.</span><span class="sxs-lookup"><span data-stu-id="55abc-114">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="55abc-115">W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.</span><span class="sxs-lookup"><span data-stu-id="55abc-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="55abc-116">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca,niejestużywanażadnametodaReturn.`Task`</span><span class="sxs-lookup"><span data-stu-id="55abc-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="55abc-117">Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return.</span><span class="sxs-lookup"><span data-stu-id="55abc-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="55abc-118">Na przykład: *Pages/Films/Create. cshtml. cs.* `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="55abc-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="55abc-119">Przejrzyj stronę */filmy/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="55abc-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="55abc-120">Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="55abc-121">Gdy po C#symbolu następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do. `@`</span><span class="sxs-lookup"><span data-stu-id="55abc-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="55abc-122">Dyrektywa `@page` Razor powoduje, że plik jest akcją MVC, co oznacza, że może obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="55abc-122">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="55abc-123">`@page`musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="55abc-123">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="55abc-124">`@page`jest przykładem przejścia do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-124">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="55abc-125">Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="55abc-125">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="55abc-126">Bada wyrażenie lambda użyte w następującym Pomocniku HTML:</span><span class="sxs-lookup"><span data-stu-id="55abc-126">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="55abc-127">Pomocnik html sprawdza `Title` właściwość, do której istnieje odwołanie w wyrażeniu lambda, aby określić nazwę wyświetlaną. `DisplayNameFor`</span><span class="sxs-lookup"><span data-stu-id="55abc-127">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="55abc-128">Wyrażenie lambda jest sprawdzane, a nie oceniane.</span><span class="sxs-lookup"><span data-stu-id="55abc-128">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="55abc-129">Oznacza to, że nie ma żadnych naruszeń `model`dostępu `model.Movie`, gdy `model.Movie[0]` , `null` , lub są puste.</span><span class="sxs-lookup"><span data-stu-id="55abc-129">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="55abc-130">Gdy wyrażenie lambda jest oceniane (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.</span><span class="sxs-lookup"><span data-stu-id="55abc-130">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="55abc-131">@model Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="55abc-131">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="55abc-132">`@model` Dyrektywa określa typ modelu przekazaną do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-132">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="55abc-133">W poprzednim przykładzie `@model` wiersz `PageModel`powoduje, że Klasa pochodna jest dostępna dla strony Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-133">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="55abc-134">Model jest używany w `@Html.DisplayNameFor` [pomocnikach HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) i `@Html.DisplayFor` na stronie.</span><span class="sxs-lookup"><span data-stu-id="55abc-134">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="55abc-135">Strona układu</span><span class="sxs-lookup"><span data-stu-id="55abc-135">The layout page</span></span>

<span data-ttu-id="55abc-136">Wybierz linki menu (**RazorPagesMovie**, **Home**i **privacy**).</span><span class="sxs-lookup"><span data-stu-id="55abc-136">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="55abc-137">Każda Strona wyświetla ten sam układ menu.</span><span class="sxs-lookup"><span data-stu-id="55abc-137">Each page shows the same menu layout.</span></span> <span data-ttu-id="55abc-138">Układ menu jest implementowany w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-138">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="55abc-139">Otwórz plik *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-139">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="55abc-140">Szablony [układów](xref:mvc/views/layout) umożliwiają układ kontenera HTML:</span><span class="sxs-lookup"><span data-stu-id="55abc-140">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="55abc-141">Określone w jednym miejscu.</span><span class="sxs-lookup"><span data-stu-id="55abc-141">Specified in one place.</span></span>
* <span data-ttu-id="55abc-142">Stosowane na wielu stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="55abc-142">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="55abc-143">`@RenderBody()` Znajdź wiersz.</span><span class="sxs-lookup"><span data-stu-id="55abc-143">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="55abc-144">`RenderBody`jest symbolem zastępczym, w którym wszystkie widoki specyficzne dla strony są wyświetlane, opakowane na stronie układ.</span><span class="sxs-lookup"><span data-stu-id="55abc-144">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="55abc-145">Na przykład wybierz łącze **prywatność** i widok *strony/prywatność. cshtml* jest `RenderBody` renderowany wewnątrz metody.</span><span class="sxs-lookup"><span data-stu-id="55abc-145">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="55abc-146">ViewData i układ</span><span class="sxs-lookup"><span data-stu-id="55abc-146">ViewData and layout</span></span>

<span data-ttu-id="55abc-147">Rozważ następujące oznakowanie w pliku *Pages/Films/index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="55abc-147">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="55abc-148">Poprzedni wyróżniony znacznik jest przykładem przejścia Razor do C#.</span><span class="sxs-lookup"><span data-stu-id="55abc-148">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="55abc-149">Znaki `{` i `}` należy ująć w blok C# kodu.</span><span class="sxs-lookup"><span data-stu-id="55abc-149">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="55abc-150">Klasa `PageModel` bazowa `ViewData` zawiera właściwość słownika, która może służyć do dodawania danych i przekazywania ich do widoku.</span><span class="sxs-lookup"><span data-stu-id="55abc-150">The `PageModel` base class contains a `ViewData` dictionary property that can be used to add data that and pass it to a View.</span></span> <span data-ttu-id="55abc-151">Obiekty są dodawane do `ViewData` słownika przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="55abc-151">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="55abc-152">W poprzednim przykładzie `"Title"` właściwość jest dodawana `ViewData` do słownika.</span><span class="sxs-lookup"><span data-stu-id="55abc-152">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="55abc-153">Właściwość jest używana w pliku *Pages/Shared/_Layout. cshtml.* `"Title"`</span><span class="sxs-lookup"><span data-stu-id="55abc-153">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="55abc-154">Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-154">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="55abc-155">Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-155">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="55abc-156">W przeciwieństwie do komentarzy`<!-- -->`HTML (), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="55abc-156">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="55abc-157">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="55abc-157">Update the layout</span></span>

<span data-ttu-id="55abc-158">Zmień element w pliku *Pages/Shared/_Layout. cshtml* , aby wyświetlał **film** , a nie **RazorPagesMovie.** `<title>`</span><span class="sxs-lookup"><span data-stu-id="55abc-158">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="55abc-159">Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-159">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="55abc-160">Zastąp poprzedni element następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="55abc-160">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="55abc-161">Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="55abc-161">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="55abc-162">W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="55abc-162">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="55abc-163">Atrybut pomocnika `/Movies/Index`tagówi wartość tworzy łącze do strony Razor. `asp-page="/Movies/Index"`</span><span class="sxs-lookup"><span data-stu-id="55abc-163">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="55abc-164">Wartość `asp-area` atrybutu jest pusta, dlatego obszar nie jest używany w łączu.</span><span class="sxs-lookup"><span data-stu-id="55abc-164">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="55abc-165">Aby uzyskać więcej informacji, zobacz [obszary](xref:mvc/controllers/areas) .</span><span class="sxs-lookup"><span data-stu-id="55abc-165">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="55abc-166">Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** .</span><span class="sxs-lookup"><span data-stu-id="55abc-166">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="55abc-167">Jeśli występują problemy, zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="55abc-167">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="55abc-168">Przetestuj inne linki (**Narzędzia główne**, **RpMovie**, **Utwórz**, **Edytuj**i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="55abc-168">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="55abc-169">Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki.</span><span class="sxs-lookup"><span data-stu-id="55abc-169">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="55abc-170">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="55abc-170">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="55abc-171">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55abc-171">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="55abc-172">Aby uzyskać instrukcje dotyczące dodawania przecinków dziesiętnych, zobacz ten problem w usłudze [GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) .</span><span class="sxs-lookup"><span data-stu-id="55abc-172">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="55abc-173">Właściwość jest ustawiana w pliku *Pages/_ViewStart. cshtml:* `Layout`</span><span class="sxs-lookup"><span data-stu-id="55abc-173">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="55abc-174">Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="55abc-174">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="55abc-175">Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="55abc-175">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="55abc-176">Model tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="55abc-176">The Create page model</span></span>

<span data-ttu-id="55abc-177">Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="55abc-177">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="55abc-178">`OnGet` Metoda inicjuje wszystkie Stany potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="55abc-178">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="55abc-179">Strona tworzenia nie ma żadnego stanu do zainicjowania, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="55abc-179">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="55abc-180">W dalszej części tego samouczka przedstawiono `OnGet` przykład inicjowania stanu.</span><span class="sxs-lookup"><span data-stu-id="55abc-180">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="55abc-181">Metoda tworzy obiekt, który renderuje stronę *Create. cshtml.* `Page` `PageResult`</span><span class="sxs-lookup"><span data-stu-id="55abc-181">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="55abc-182">Właściwość używa atrybutu, `[BindProperty]` aby wybrać [powiązanie modelu.](xref:mvc/models/model-binding) `Movie`</span><span class="sxs-lookup"><span data-stu-id="55abc-182">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="55abc-183">Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core tworzy powiązanie opublikowanych wartości z `Movie` modelem.</span><span class="sxs-lookup"><span data-stu-id="55abc-183">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="55abc-184">`OnPostAsync` Metoda jest uruchamiana, gdy strona zapisuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="55abc-184">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="55abc-185">Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza.</span><span class="sxs-lookup"><span data-stu-id="55abc-185">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="55abc-186">Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza.</span><span class="sxs-lookup"><span data-stu-id="55abc-186">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="55abc-187">Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę.</span><span class="sxs-lookup"><span data-stu-id="55abc-187">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="55abc-188">Weryfikowanie po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="55abc-188">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="55abc-189">Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="55abc-189">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="55abc-190">Strona tworzenie Razor</span><span class="sxs-lookup"><span data-stu-id="55abc-190">The Create Razor Page</span></span>

<span data-ttu-id="55abc-191">Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:</span><span class="sxs-lookup"><span data-stu-id="55abc-191">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55abc-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55abc-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="55abc-193">Program Visual Studio wyświetla następujące znaczniki w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="55abc-193">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Widok VS17 na stronie Tworzenie. cshtml](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55abc-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55abc-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="55abc-196">Następujące pomocnicy tagów są pokazane w powyższym znaczniku:</span><span class="sxs-lookup"><span data-stu-id="55abc-196">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55abc-197">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55abc-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="55abc-198">Program Visual Studio wyświetla następujące znaczniki w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="55abc-198">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="55abc-199">Element jest pomocnikiem [tagu formularza.](xref:mvc/views/working-with-forms#the-form-tag-helper) `<form method="post">`</span><span class="sxs-lookup"><span data-stu-id="55abc-199">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="55abc-200">Pomocnik tagu formularza automatycznie zawiera [token](xref:security/anti-request-forgery)antysfałszowany.</span><span class="sxs-lookup"><span data-stu-id="55abc-200">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="55abc-201">Aparat szkieletu tworzy znaczniki Razor dla każdego pola w modelu (z wyjątkiem identyfikatora) podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="55abc-201">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="55abc-202">[Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i `<span asp-validation-for`) wyświetla błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="55abc-202">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="55abc-203">Sprawdzanie poprawności jest omówione bardziej szczegółowo w dalszej części tej serii.</span><span class="sxs-lookup"><span data-stu-id="55abc-203">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="55abc-204">[Pomocnik tagu etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety `Title` i `for` atrybut właściwości.</span><span class="sxs-lookup"><span data-stu-id="55abc-204">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="55abc-205">[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) używa atrybutów [](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) DataAnnotations i tworzy atrybuty HTML, które są zbędne do walidacji jQuery po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="55abc-205">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="55abc-206">Aby uzyskać więcej informacji na temat pomocników tagów `<form method="post">`, takich jak, zobacz [pomocnicy tagów w ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="55abc-206">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="55abc-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="55abc-207">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="55abc-208">[Ubiegł Dodawanie modelu](xref:tutorials/razor-pages/model)
> donastępnego:[ Database](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="55abc-208">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="55abc-209">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="55abc-209">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="55abc-210">Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w [poprzednim samouczku](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="55abc-210">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="55abc-211">[Wyświetlanie lub pobieranie](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) próbki.</span><span class="sxs-lookup"><span data-stu-id="55abc-211">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="55abc-212">Strony tworzenie, usuwanie, szczegóły i edycja</span><span class="sxs-lookup"><span data-stu-id="55abc-212">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="55abc-213">Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="55abc-213">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="55abc-214">Razor Pages pochodzą od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="55abc-214">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="55abc-215">Zgodnie z `PageModel`Konwencją Klasa pochodna jest wywoływana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="55abc-215">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="55abc-216">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `RazorPagesMovieContext` do strony.</span><span class="sxs-lookup"><span data-stu-id="55abc-216">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="55abc-217">Wszystkie strony szkieletowe są zgodne z tym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="55abc-217">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="55abc-218">Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="55abc-218">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="55abc-219">Gdy żądanie jest wykonywane dla strony, `OnGetAsync` Metoda zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-219">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="55abc-220">`OnGetAsync`lub `OnGet` jest wywoływana na stronie Razor, aby zainicjować stan dla strony.</span><span class="sxs-lookup"><span data-stu-id="55abc-220">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="55abc-221">W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.</span><span class="sxs-lookup"><span data-stu-id="55abc-221">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="55abc-222">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca,niejestużywanażadnametodaReturn.`Task`</span><span class="sxs-lookup"><span data-stu-id="55abc-222">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="55abc-223">Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return.</span><span class="sxs-lookup"><span data-stu-id="55abc-223">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="55abc-224">Na przykład: *Pages/Films/Create. cshtml. cs.* `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="55abc-224">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="55abc-225">Przejrzyj stronę */filmy/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="55abc-225">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="55abc-226">Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-226">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="55abc-227">Gdy po C#symbolu następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do. `@`</span><span class="sxs-lookup"><span data-stu-id="55abc-227">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="55abc-228">Dyrektywa `@page` Razor powoduje, że plik jest akcją MVC, co oznacza, że może obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="55abc-228">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="55abc-229">`@page`musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="55abc-229">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="55abc-230">`@page`jest przykładem przejścia do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-230">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="55abc-231">Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="55abc-231">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="55abc-232">Bada wyrażenie lambda użyte w następującym Pomocniku HTML:</span><span class="sxs-lookup"><span data-stu-id="55abc-232">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="55abc-233">Pomocnik html sprawdza `Title` właściwość, do której istnieje odwołanie w wyrażeniu lambda, aby określić nazwę wyświetlaną. `DisplayNameFor`</span><span class="sxs-lookup"><span data-stu-id="55abc-233">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="55abc-234">Wyrażenie lambda jest sprawdzane, a nie oceniane.</span><span class="sxs-lookup"><span data-stu-id="55abc-234">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="55abc-235">Oznacza to, że nie ma żadnych naruszeń `model`dostępu `model.Movie`, gdy `model.Movie[0]` , `null` , lub są puste.</span><span class="sxs-lookup"><span data-stu-id="55abc-235">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="55abc-236">Gdy wyrażenie lambda jest oceniane (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.</span><span class="sxs-lookup"><span data-stu-id="55abc-236">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="55abc-237">@model Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="55abc-237">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="55abc-238">`@model` Dyrektywa określa typ modelu przekazaną do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-238">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="55abc-239">W poprzednim przykładzie `@model` wiersz `PageModel`powoduje, że Klasa pochodna jest dostępna dla strony Razor.</span><span class="sxs-lookup"><span data-stu-id="55abc-239">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="55abc-240">Model jest używany w `@Html.DisplayNameFor` [pomocnikach HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) i `@Html.DisplayFor` na stronie.</span><span class="sxs-lookup"><span data-stu-id="55abc-240">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="55abc-241">Strona układu</span><span class="sxs-lookup"><span data-stu-id="55abc-241">The layout page</span></span>

<span data-ttu-id="55abc-242">Wybierz linki menu (**RazorPagesMovie**, **Home**i **privacy**).</span><span class="sxs-lookup"><span data-stu-id="55abc-242">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="55abc-243">Każda Strona wyświetla ten sam układ menu.</span><span class="sxs-lookup"><span data-stu-id="55abc-243">Each page shows the same menu layout.</span></span> <span data-ttu-id="55abc-244">Układ menu jest implementowany w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-244">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="55abc-245">Otwórz plik *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-245">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="55abc-246">Szablony [układów](xref:mvc/views/layout) umożliwiają określenie układu kontenera HTML witryny w jednym miejscu, a następnie zastosowanie go na wielu stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="55abc-246">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="55abc-247">`@RenderBody()` Znajdź wiersz.</span><span class="sxs-lookup"><span data-stu-id="55abc-247">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="55abc-248">`RenderBody`jest symbolem zastępczym, w którym wszystkie utworzone widoki związane ze stroną są wyświetlane, opakowane na stronie układ.</span><span class="sxs-lookup"><span data-stu-id="55abc-248">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="55abc-249">Na przykład w przypadku wybrania linku **prywatność** widok **strony/prywatność. cshtml** jest `RenderBody` renderowany wewnątrz metody.</span><span class="sxs-lookup"><span data-stu-id="55abc-249">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="55abc-250">ViewData i układ</span><span class="sxs-lookup"><span data-stu-id="55abc-250">ViewData and layout</span></span>

<span data-ttu-id="55abc-251">Rozważmy następujący kod z pliku *Pages/Films/index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="55abc-251">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="55abc-252">Poprzedni wyróżniony kod jest przykładem przejścia Razor do C#.</span><span class="sxs-lookup"><span data-stu-id="55abc-252">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="55abc-253">Znaki `{` i `}` należy ująć w blok C# kodu.</span><span class="sxs-lookup"><span data-stu-id="55abc-253">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="55abc-254">Klasa `PageModel` bazowa `ViewData` ma właściwość dictionary, która może służyć do dodawania danych, które mają zostać przekazane do widoku.</span><span class="sxs-lookup"><span data-stu-id="55abc-254">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="55abc-255">Obiekty można dodawać do `ViewData` słownika przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="55abc-255">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="55abc-256">W poprzednim przykładzie właściwość "title" została dodana do `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="55abc-256">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="55abc-257">Właściwość "title" jest używana w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-257">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="55abc-258">Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-258">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="55abc-259">Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor, które nie pojawia się w pliku układu.</span><span class="sxs-lookup"><span data-stu-id="55abc-259">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="55abc-260">W przeciwieństwie do komentarzy`<!-- -->`HTML (), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="55abc-260">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="55abc-261">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="55abc-261">Update the layout</span></span>

<span data-ttu-id="55abc-262">Zmień element w pliku *Pages/Shared/_Layout. cshtml* , aby wyświetlał **film** , a nie **RazorPagesMovie.** `<title>`</span><span class="sxs-lookup"><span data-stu-id="55abc-262">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="55abc-263">Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="55abc-263">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="55abc-264">Zastąp poprzedni element następującym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="55abc-264">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="55abc-265">Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="55abc-265">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="55abc-266">W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="55abc-266">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="55abc-267">Atrybut pomocnika `/Movies/Index`tagówi wartość tworzy łącze do strony Razor. `asp-page="/Movies/Index"`</span><span class="sxs-lookup"><span data-stu-id="55abc-267">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="55abc-268">Wartość `asp-area` atrybutu jest pusta, dlatego obszar nie jest używany w łączu.</span><span class="sxs-lookup"><span data-stu-id="55abc-268">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="55abc-269">Aby uzyskać więcej informacji, zobacz [obszary](xref:mvc/controllers/areas) .</span><span class="sxs-lookup"><span data-stu-id="55abc-269">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="55abc-270">Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** .</span><span class="sxs-lookup"><span data-stu-id="55abc-270">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="55abc-271">Jeśli występują problemy, zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="55abc-271">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="55abc-272">Przetestuj inne linki (**Narzędzia główne**, **RpMovie**, **Utwórz**, **Edytuj**i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="55abc-272">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="55abc-273">Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki.</span><span class="sxs-lookup"><span data-stu-id="55abc-273">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="55abc-274">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="55abc-274">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="55abc-275">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55abc-275">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="55abc-276">Ten [problem w usłudze GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) zawiera instrukcje dotyczące dodawania przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="55abc-276">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="55abc-277">Właściwość jest ustawiana w pliku *Pages/_ViewStart. cshtml:* `Layout`</span><span class="sxs-lookup"><span data-stu-id="55abc-277">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="55abc-278">Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="55abc-278">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="55abc-279">Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="55abc-279">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="55abc-280">Model tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="55abc-280">The Create page model</span></span>

<span data-ttu-id="55abc-281">Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="55abc-281">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="55abc-282">`OnGet` Metoda inicjuje wszystkie Stany potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="55abc-282">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="55abc-283">Strona tworzenia nie ma żadnego stanu do zainicjowania, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="55abc-283">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="55abc-284">W dalszej części tego samouczka zobaczysz `OnGet` stan zainicjowania metody.</span><span class="sxs-lookup"><span data-stu-id="55abc-284">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="55abc-285">Metoda tworzy obiekt, który renderuje stronę *Create. cshtml.* `Page` `PageResult`</span><span class="sxs-lookup"><span data-stu-id="55abc-285">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="55abc-286">Właściwość używa atrybutu, `[BindProperty]` aby wybrać [powiązanie modelu.](xref:mvc/models/model-binding) `Movie`</span><span class="sxs-lookup"><span data-stu-id="55abc-286">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="55abc-287">Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core tworzy powiązanie opublikowanych wartości z `Movie` modelem.</span><span class="sxs-lookup"><span data-stu-id="55abc-287">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="55abc-288">`OnPostAsync` Metoda jest uruchamiana, gdy strona zapisuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="55abc-288">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="55abc-289">Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza.</span><span class="sxs-lookup"><span data-stu-id="55abc-289">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="55abc-290">Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza.</span><span class="sxs-lookup"><span data-stu-id="55abc-290">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="55abc-291">Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę.</span><span class="sxs-lookup"><span data-stu-id="55abc-291">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="55abc-292">Weryfikowanie po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="55abc-292">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="55abc-293">Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="55abc-293">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="55abc-294">Strona tworzenie Razor</span><span class="sxs-lookup"><span data-stu-id="55abc-294">The Create Razor Page</span></span>

<span data-ttu-id="55abc-295">Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:</span><span class="sxs-lookup"><span data-stu-id="55abc-295">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="55abc-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="55abc-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="55abc-297">Program Visual Studio Wyświetla `<form method="post">` tag w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="55abc-297">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Widok VS17 na stronie Tworzenie. cshtml](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="55abc-299">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="55abc-299">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="55abc-300">Aby uzyskać więcej informacji na temat pomocników tagów `<form method="post">`, takich jak, zobacz [pomocnicy tagów w ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="55abc-300">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="55abc-301">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="55abc-301">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="55abc-302">Visual Studio dla komputerów Mac wyświetla `<form method="post">` tag w wyróżnionej pogrubionej czcionce używanej przez pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="55abc-302">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="55abc-303">Element jest pomocnikiem [tagu formularza.](xref:mvc/views/working-with-forms#the-form-tag-helper) `<form method="post">`</span><span class="sxs-lookup"><span data-stu-id="55abc-303">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="55abc-304">Pomocnik tagu formularza automatycznie zawiera [token](xref:security/anti-request-forgery)antysfałszowany.</span><span class="sxs-lookup"><span data-stu-id="55abc-304">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="55abc-305">Aparat szkieletu tworzy znaczniki Razor dla każdego pola w modelu (z wyjątkiem identyfikatora) podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="55abc-305">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="55abc-306">[Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i `<span asp-validation-for`) wyświetla błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="55abc-306">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="55abc-307">Sprawdzanie poprawności jest omówione bardziej szczegółowo w dalszej części tej serii.</span><span class="sxs-lookup"><span data-stu-id="55abc-307">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="55abc-308">[Pomocnik tagu etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety `Title` i `for` atrybut właściwości.</span><span class="sxs-lookup"><span data-stu-id="55abc-308">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="55abc-309">[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) używa atrybutów [](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) DataAnnotations i tworzy atrybuty HTML, które są zbędne do walidacji jQuery po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="55abc-309">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="55abc-310">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="55abc-310">Additional resources</span></span>

* [<span data-ttu-id="55abc-311">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="55abc-311">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="55abc-312">[Ubiegł Dodawanie modelu](xref:tutorials/razor-pages/model)
> donastępnego:[ Database](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="55abc-312">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
