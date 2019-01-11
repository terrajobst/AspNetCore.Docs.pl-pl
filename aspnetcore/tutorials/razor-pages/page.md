---
title: Strony razor ze szkieletami w programie ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono stron Razor, generowane przez tworzenie szkieletów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: 2b68523ac84596bda2914d9c348ddea8c7ea9465
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207385"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="e0c3c-103">Strony razor ze szkieletami w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0c3c-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="e0c3c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e0c3c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e0c3c-105">W tym samouczku sprawdza, czy strony Razor, powstałe w wyniku tworzenia szkieletów w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="e0c3c-106">[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) próbki.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="e0c3c-107">Tworzenie, usuwanie, szczegóły i edycji stron</span><span class="sxs-lookup"><span data-stu-id="e0c3c-107">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="e0c3c-108">Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="e0c3c-109">Strony razor są uzyskiwane z `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="e0c3c-110">Zgodnie z Konwencją `PageModel`-klasy pochodnej jest wywoływana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="e0c3c-111">Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) dodać `RazorPagesMovieContext` ze stroną.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="e0c3c-112">Wszystkie strony szkieletu korzystać z tego wzoru.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="e0c3c-113">Zobacz [kodu asynchronicznego](xref:data/ef-rp/intro#asynchronous-code) więcej informacji na temat asynchronicznej programistyczne z programem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="e0c3c-114">Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="e0c3c-115">`OnGetAsync` lub `OnGet` jest wywoływana w stronę Razor do zainicjowania stanu dla strony.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="e0c3c-116">W tym przypadku `OnGetAsync` pobiera listę filmów, a następnie wyświetli je.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="e0c3c-117">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak zwracany metody jest używana.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="e0c3c-118">Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, musi być podana instrukcji return.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="e0c3c-119">Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="e0c3c-120">Sprawdź *Pages/Movies/Index.cshtml* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="e0c3c-121">Razor można przejść z kodu HTML w języku C# lub w znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="e0c3c-122">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor, w przeciwnym razie przejścia w języku C#.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="e0c3c-123">`@page` Dyrektywy Razor sprawia, że plik do Akcja kontrolera MVC, co oznacza, że może obsługiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-123">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="e0c3c-124">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="e0c3c-125">`@page` jest przykładem przechodzi do znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="e0c3c-126">Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="e0c3c-127">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="e0c3c-128">`DisplayNameFor` Sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="e0c3c-129">Wyrażenie lambda jest kontrolowane zamiast ocenione.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="e0c3c-130">Oznacza, że ma nie naruszenie zasad dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="e0c3c-131">Kiedy jest obliczane wyrażenie lambda (na przykład za pomocą `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="e0c3c-132">@model — Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="e0c3c-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="e0c3c-133">`@model` Dyrektywa określa typ modelu przekazywane do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="e0c3c-134">W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`— dostępny na stronie Razor klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="e0c3c-135">Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayFor` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="e0c3c-136">Strona układu</span><span class="sxs-lookup"><span data-stu-id="e0c3c-136">The layout page</span></span>

<span data-ttu-id="e0c3c-137">Wybierz linki menu (**RazorPagesMovie**, **Home**, i **zachowania**).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="e0c3c-138">Każda strona zawiera ten sam układ menu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="e0c3c-139">Układ menu jest zaimplementowana w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="e0c3c-140">Otwórz *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="e0c3c-141">[Układ](xref:mvc/views/layout) Szablony umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-141">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="e0c3c-142">Znajdź `@RenderBody()` wiersza.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-142">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="e0c3c-143">`RenderBody` jest symbolem zastępczym, gdzie wszystkie specyficzne dla strony widoki, których tworzenie pokazu, *opakowane* w stronę układu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-143">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="e0c3c-144">Na przykład w przypadku wybrania **zachowania** linku **Pages/Privacy.cshtml** renderowania widoku w `RenderBody` metody.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-144">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>
### <a name="viewdata-and-layout"></a><span data-ttu-id="e0c3c-145">ViewData i układu</span><span class="sxs-lookup"><span data-stu-id="e0c3c-145">ViewData and layout</span></span>

<span data-ttu-id="e0c3c-146">Rozważmy poniższy kod z *Pages/Movies/Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-146">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="e0c3c-147">Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-147">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="e0c3c-148">`{` i `}` znaków należy umieścić blok kodu C#.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-148">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="e0c3c-149">`PageModel` Ma klasę bazową `ViewData` słownika właściwości, który może służyć do dodania danych, które mają być przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-149">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="e0c3c-150">Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-150">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="e0c3c-151">W poprzednim przykładzie, właściwość "Title" zostanie dodany do `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-151">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

<span data-ttu-id="e0c3c-152">Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-152">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="e0c3c-153">Następujący kod przedstawia pierwszych kilka wierszy tego *_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-153">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="e0c3c-154">Wiersz `@*Markup removed for brevity.*@` komentarz Razor, które nie znajdują się w pliku układu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-154">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="e0c3c-155">W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-155">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="e0c3c-156">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="e0c3c-156">Update the layout</span></span>

<span data-ttu-id="e0c3c-157">Zmiana `<title>` element *Pages/Shared/_Layout.cshtml* pliku wyświetlania **filmu** zamiast **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


<span data-ttu-id="e0c3c-158">Znajdź następujący element zakotwiczenia w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="e0c3c-159">Zastąp poprzedzający element następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="e0c3c-160">Poprzedni element zakotwiczenia jest [Pomocnik tagu](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="e0c3c-161">W tym przypadku ma [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="e0c3c-162">`asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` strona Razor.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="e0c3c-163">`asp-area` Wartość atrybutu jest puste, dzięki czemu obszar nie jest używane w linku.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-163">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="e0c3c-164">Zobacz [obszarów](xref:mvc/controllers/areas) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-164">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="e0c3c-165">Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-165">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="e0c3c-166">Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) plik w usłudze GitHub, jeśli masz jakiekolwiek problemy.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-166">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="e0c3c-167">Testowanie inne linki (**Home**, **RpMovie**, **Utwórz**, **Edytuj**, i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-167">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="e0c3c-168">Każda strona Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-168">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="e0c3c-169">*Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale możesz modyfikować je mogą mieć różne wartości.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-169">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="e0c3c-170">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-170">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e0c3c-171">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-171">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="e0c3c-172">To [problem w usłudze GitHub 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) instrukcje dotyczące dodawania przecinek dziesiętny.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-172">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="e0c3c-173">`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-173">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="e0c3c-174">Poprzedni kod znaczników ustawia plik układu *Pages/Shared/_Layout.cshtml* dla wszystkich plików Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-174">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="e0c3c-175">Zobacz [układ](xref:razor-pages/index#layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-175">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="e0c3c-176">Tworzenie modelu strony</span><span class="sxs-lookup"><span data-stu-id="e0c3c-176">The Create page model</span></span>

<span data-ttu-id="e0c3c-177">Sprawdź *Pages/Movies/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-177">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="e0c3c-178">`OnGet` Metoda inicjuje każdy stan potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-178">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="e0c3c-179">Strona tworzenia nie ma każdy stan, aby zainicjować, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-179">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="e0c3c-180">W dalszej części tego samouczka zostanie wyświetlony `OnGet` metoda inicjowania stanu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-180">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="e0c3c-181">`Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-181">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="e0c3c-182">`Movie` Używa właściwości `[BindProperty]` atrybutu, aby wyrazić zgodę na [wiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-182">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="e0c3c-183">Gdy formularz Utwórz publikuje wartości formularza, środowisko uruchomieniowe programu ASP.NET Core wiąże przesłanych wartości w celu `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-183">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="e0c3c-184">`OnPostAsync` Metoda jest uruchomiona, gdy Strona publikuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-184">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="e0c3c-185">Jeśli występują błędy modelu, formularza zostanie wyświetlony ponownie, oraz wszelkie dane formularza opublikowane.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-185">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="e0c3c-186">Większość błędów modelu mogą być przechwytywane po stronie klienta, zanim opublikowania formularza.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-186">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="e0c3c-187">Przykładem błąd modelu publikuje wartość pola daty, którego nie można przekonwertować na wartość typu date.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-187">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="e0c3c-188">Weryfikacja po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-188">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="e0c3c-189">Jeśli nie ma żadnych błędów modelu, dane są zapisywane, a przeglądarka jest przekierowywana na strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-189">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="e0c3c-190">Tworzenie strony Razor</span><span class="sxs-lookup"><span data-stu-id="e0c3c-190">The Create Razor Page</span></span>

<span data-ttu-id="e0c3c-191">Sprawdź *Pages/Movies/Create.cshtml* wartość pola plik strony Razor:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-191">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e0c3c-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e0c3c-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e0c3c-193">Visual Studio Wyświetla `<form method="post">` tag szczególne pogrubioną czcionką używany dla pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-193">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

<span data-ttu-id="e0c3c-194">![VS17 widok Create.cshtml strony](page/_static/th.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="e0c3c-194">![VS17 view of Create.cshtml page](page/_static/th.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e0c3c-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e0c3c-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e0c3c-196">Aby uzyskać więcej informacji na temat pomocnicy tagów, takie jak `<form method="post">`, zobacz [pomocników tagów w programie ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-196">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e0c3c-197">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e0c3c-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e0c3c-198">Program Visual Studio for Mac Wyświetla `<form method="post">` tag szczególne pogrubioną czcionką używany dla pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-198">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="e0c3c-199">`<form method="post">` Element jest [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-199">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="e0c3c-200">Pomocnik tagu formularza automatycznie uwzględnia [antiforgery token](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="e0c3c-200">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="e0c3c-201">Aparat tworzenia szkieletów tworzy znaczników Razor dla każdego pola w modelu (z wyjątkiem identyfikator) podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="e0c3c-201">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="e0c3c-202">[Pomocników tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i ` <span asp-validation-for`) wyświetla błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-202">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="e0c3c-203">Sprawdzanie poprawności jest omówiona bardziej szczegółowo w dalszej części tej serii.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-203">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="e0c3c-204">[Pomocnik tagu etykiet](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i `for` atrybutu dla `Title` właściwości.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-204">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="e0c3c-205">[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="e0c3c-205">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e0c3c-206">[Poprzednie: Dodawanie modelu](xref:tutorials/razor-pages/model)
> [dalej: Bazy danych](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="e0c3c-206">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Data Base](xref:tutorials/razor-pages/sql)</span></span>
