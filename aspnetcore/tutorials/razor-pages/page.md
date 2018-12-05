---
title: Strony razor ze szkieletami w programie ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono stron Razor, generowane przez tworzenie szkieletów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: acfc446732803c67714943fe3e5b7a31055ebcd7
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862008"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="2f34f-103">Strony razor ze szkieletami w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f34f-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="2f34f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f34f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2f34f-105">W tym samouczku sprawdza, czy strony Razor, powstałe w wyniku tworzenia szkieletów w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="2f34f-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="2f34f-106">[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) próbki.</span><span class="sxs-lookup"><span data-stu-id="2f34f-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="2f34f-107">Tworzenie, usuwanie, szczegóły i edycji stron</span><span class="sxs-lookup"><span data-stu-id="2f34f-107">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="2f34f-108">Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="2f34f-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="2f34f-109">Strony razor są uzyskiwane z `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="2f34f-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="2f34f-110">Zgodnie z Konwencją `PageModel`-klasy pochodnej jest wywoływana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="2f34f-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="2f34f-111">Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) dodać `RazorPagesMovieContext` ze stroną.</span><span class="sxs-lookup"><span data-stu-id="2f34f-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="2f34f-112">Wszystkie strony szkieletu korzystać z tego wzoru.</span><span class="sxs-lookup"><span data-stu-id="2f34f-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="2f34f-113">Zobacz [kodu asynchronicznego](xref:data/ef-rp/intro#asynchronous-code) więcej informacji na temat asynchronicznej programistyczne z programem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2f34f-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="2f34f-114">Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="2f34f-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="2f34f-115">`OnGetAsync` lub `OnGet` jest wywoływana w stronę Razor do zainicjowania stanu dla strony.</span><span class="sxs-lookup"><span data-stu-id="2f34f-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="2f34f-116">W tym przypadku `OnGetAsync` pobiera listę filmów, a następnie wyświetli je.</span><span class="sxs-lookup"><span data-stu-id="2f34f-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="2f34f-117">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak zwracany metody jest używana.</span><span class="sxs-lookup"><span data-stu-id="2f34f-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="2f34f-118">Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, musi być podana instrukcji return.</span><span class="sxs-lookup"><span data-stu-id="2f34f-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="2f34f-119">Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="2f34f-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="2f34f-120">Sprawdź *Pages/Movies/Index.cshtml* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="2f34f-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="2f34f-121">Razor można przejść z kodu HTML w języku C# lub w znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="2f34f-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="2f34f-122">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor, w przeciwnym razie przejścia w języku C#.</span><span class="sxs-lookup"><span data-stu-id="2f34f-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="2f34f-123">`@page` Dyrektywy Razor sprawia, że plik do Akcja kontrolera MVC, co oznacza, że może obsługiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="2f34f-123">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="2f34f-124">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="2f34f-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="2f34f-125">`@page` jest przykładem przechodzi do znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="2f34f-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="2f34f-126">Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2f34f-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="2f34f-127">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="2f34f-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="2f34f-128">`DisplayNameFor` Sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="2f34f-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="2f34f-129">Wyrażenie lambda jest kontrolowane zamiast ocenione.</span><span class="sxs-lookup"><span data-stu-id="2f34f-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="2f34f-130">Oznacza, że ma nie naruszenie zasad dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="2f34f-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="2f34f-131">Kiedy jest obliczane wyrażenie lambda (na przykład za pomocą `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="2f34f-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="2f34f-132">@model — Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="2f34f-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="2f34f-133">`@model` Dyrektywa określa typ modelu przekazywane do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="2f34f-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="2f34f-134">W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`— dostępny na stronie Razor klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="2f34f-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="2f34f-135">Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayFor` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.</span><span class="sxs-lookup"><span data-stu-id="2f34f-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<a name="vd"></a>
### <a name="viewdata-and-layout"></a><span data-ttu-id="2f34f-136">ViewData i układu</span><span class="sxs-lookup"><span data-stu-id="2f34f-136">ViewData and layout</span></span>

<span data-ttu-id="2f34f-137">Rozważmy poniższy kod z *Pages/Movies/Index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="2f34f-137">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="2f34f-138">Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#.</span><span class="sxs-lookup"><span data-stu-id="2f34f-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="2f34f-139">`{` i `}` znaków należy umieścić blok kodu C#.</span><span class="sxs-lookup"><span data-stu-id="2f34f-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="2f34f-140">`PageModel` Ma klasę bazową `ViewData` słownika właściwości, który może służyć do dodania danych, które mają być przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="2f34f-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="2f34f-141">Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="2f34f-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="2f34f-142">W poprzednim przykładzie, właściwość "Title" zostanie dodany do `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="2f34f-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

<span data-ttu-id="2f34f-143">Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2f34f-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="2f34f-144">Następujący kod przedstawia pierwszych kilka wierszy tego *_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2f34f-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="2f34f-145">Wiersz `@*Markup removed for brevity.*@` komentarz Razor, które nie znajdują się w pliku układu.</span><span class="sxs-lookup"><span data-stu-id="2f34f-145">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="2f34f-146">W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="2f34f-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="2f34f-147">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="2f34f-147">Update the layout</span></span>

<span data-ttu-id="2f34f-148">Zmiana `<title>` element *Pages/Shared/_Layout.cshtml* pliku wyświetlania **filmu** zamiast **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="2f34f-148">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


<span data-ttu-id="2f34f-149">Znajdź następujący element zakotwiczenia w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2f34f-149">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="2f34f-150">Zastąp poprzedzający element następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="2f34f-150">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="2f34f-151">Poprzedni element zakotwiczenia jest [Pomocnik tagu](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="2f34f-151">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="2f34f-152">W tym przypadku ma [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2f34f-152">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="2f34f-153">`asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` strona Razor.</span><span class="sxs-lookup"><span data-stu-id="2f34f-153">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="2f34f-154">`asp-area` Wartość atrybutu jest puste, dzięki czemu obszar nie jest używane w linku.</span><span class="sxs-lookup"><span data-stu-id="2f34f-154">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="2f34f-155">Zobacz [obszarów](xref:mvc/controllers/areas) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2f34f-155">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="2f34f-156">Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza.</span><span class="sxs-lookup"><span data-stu-id="2f34f-156">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="2f34f-157">Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) plik w usłudze GitHub, jeśli masz jakiekolwiek problemy.</span><span class="sxs-lookup"><span data-stu-id="2f34f-157">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="2f34f-158">Testowanie inne linki (**Home**, **RpMovie**, **Utwórz**, **Edytuj**, i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="2f34f-158">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="2f34f-159">Każda strona Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki.</span><span class="sxs-lookup"><span data-stu-id="2f34f-159">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="2f34f-160">*Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale możesz modyfikować je mogą mieć różne wartości.</span><span class="sxs-lookup"><span data-stu-id="2f34f-160">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="2f34f-161">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="2f34f-161">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="2f34f-162">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację.</span><span class="sxs-lookup"><span data-stu-id="2f34f-162">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="2f34f-163">To [problem w usłudze GitHub 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) instrukcje dotyczące dodawania przecinek dziesiętny.</span><span class="sxs-lookup"><span data-stu-id="2f34f-163">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="2f34f-164">`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="2f34f-164">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="2f34f-165">Poprzedni kod znaczników ustawia plik układu *Pages/Shared/_Layout.cshtml* dla wszystkich plików Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="2f34f-165">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="2f34f-166">Zobacz [układ](xref:razor-pages/index#layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2f34f-166">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="2f34f-167">Tworzenie modelu strony</span><span class="sxs-lookup"><span data-stu-id="2f34f-167">The Create page model</span></span>

<span data-ttu-id="2f34f-168">Sprawdź *Pages/Movies/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="2f34f-168">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="2f34f-169">`OnGet` Metoda inicjuje każdy stan potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="2f34f-169">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="2f34f-170">Strona tworzenia nie ma każdy stan, aby zainicjować, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="2f34f-170">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="2f34f-171">W dalszej części tego samouczka zostanie wyświetlony `OnGet` metoda inicjowania stanu.</span><span class="sxs-lookup"><span data-stu-id="2f34f-171">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="2f34f-172">`Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="2f34f-172">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="2f34f-173">`Movie` Używa właściwości `[BindProperty]` atrybutu, aby wyrazić zgodę na [wiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="2f34f-173">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="2f34f-174">Gdy formularz Utwórz publikuje wartości formularza, środowisko uruchomieniowe programu ASP.NET Core wiąże przesłanych wartości w celu `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="2f34f-174">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="2f34f-175">`OnPostAsync` Metoda jest uruchomiona, gdy Strona publikuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="2f34f-175">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="2f34f-176">Jeśli występują błędy modelu, formularza zostanie wyświetlony ponownie, oraz wszelkie dane formularza opublikowane.</span><span class="sxs-lookup"><span data-stu-id="2f34f-176">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="2f34f-177">Większość błędów modelu mogą być przechwytywane po stronie klienta, zanim opublikowania formularza.</span><span class="sxs-lookup"><span data-stu-id="2f34f-177">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="2f34f-178">Przykładem błąd modelu publikuje wartość pola daty, którego nie można przekonwertować na wartość typu date.</span><span class="sxs-lookup"><span data-stu-id="2f34f-178">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="2f34f-179">Weryfikacja po stronie klienta i sprawdzanie poprawności modelu zostały omówione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="2f34f-179">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="2f34f-180">Jeśli nie ma żadnych błędów modelu, dane są zapisywane, a przeglądarka jest przekierowywana na strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="2f34f-180">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="2f34f-181">Tworzenie strony Razor</span><span class="sxs-lookup"><span data-stu-id="2f34f-181">The Create Razor Page</span></span>

<span data-ttu-id="2f34f-182">Sprawdź *Pages/Movies/Create.cshtml* wartość pola plik strony Razor:</span><span class="sxs-lookup"><span data-stu-id="2f34f-182">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f34f-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f34f-183">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2f34f-184">Visual Studio Wyświetla `<form method="post">` tag szczególne pogrubioną czcionką używany dla pomocników tagów:</span><span class="sxs-lookup"><span data-stu-id="2f34f-184">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

<span data-ttu-id="2f34f-185">![VS17 widok Create.cshtml strony](page/_static/th.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="2f34f-185">![VS17 view of Create.cshtml page](page/_static/th.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2f34f-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2f34f-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2f34f-187">Aby uzyskać więcej informacji na temat pomocnicy tagów, takie jak `<form method="post">`, zobacz [pomocników tagów w programie ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="2f34f-187">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2f34f-188">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2f34f-188">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2f34f-189">Program Visual Studio for Mac Wyświetla `<form method="post">` tag szczególne pogrubioną czcionką używany dla pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="2f34f-189">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="2f34f-190">`<form method="post">` Element jest [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2f34f-190">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="2f34f-191">Pomocnik tagu formularza automatycznie uwzględnia [antiforgery token](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="2f34f-191">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="2f34f-192">Aparat tworzenia szkieletów tworzy znaczników Razor dla każdego pola w modelu (z wyjątkiem identyfikator) podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="2f34f-192">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="2f34f-193">[Pomocników tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i ` <span asp-validation-for`) wyświetla błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="2f34f-193">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="2f34f-194">Sprawdzanie poprawności jest omówiona bardziej szczegółowo w dalszej części tej serii.</span><span class="sxs-lookup"><span data-stu-id="2f34f-194">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="2f34f-195">[Pomocnik tagu etykiet](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i `for` atrybutu dla `Title` właściwości.</span><span class="sxs-lookup"><span data-stu-id="2f34f-195">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="2f34f-196">[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="2f34f-196">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2f34f-197">[Poprzedni: Dodawanie modelu](xref:tutorials/razor-pages/model)
> [dalej: bazy danych](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="2f34f-197">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Data Base](xref:tutorials/razor-pages/sql)</span></span>
