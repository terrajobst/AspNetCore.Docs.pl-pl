# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="2e0b9-101">Strony razor ze szkieletami w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e0b9-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="2e0b9-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2e0b9-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2e0b9-103">W tym samouczku sprawdza, czy strony Razor, powstałe w wyniku tworzenia szkieletów w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="2e0b9-104">[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="2e0b9-105">Tworzenie, usuwanie, szczegóły i edycji stron.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="2e0b9-106">Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="2e0b9-107">Strony razor są uzyskiwane z `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="2e0b9-108">Zgodnie z Konwencją `PageModel`-klasy pochodnej jest wywoływana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="2e0b9-109">Używa konstruktora [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) dodać `MovieContext` ze stroną.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="2e0b9-110">Wszystkie strony szkieletu korzystać z tego wzoru.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="2e0b9-111">Zobacz [kodu asynchronicznego](xref:data/ef-rp/intro#asynchronous-code) więcej informacji na temat asynchronicznej programistyczne z programem Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="2e0b9-112">Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="2e0b9-113">`OnGetAsync` lub `OnGet` jest wywoływana w stronę Razor do zainicjowania stanu dla strony.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="2e0b9-114">W tym przypadku `OnGetAsync` pobiera listę filmów, a następnie wyświetli je.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="2e0b9-115">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak zwracany metody jest używana.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="2e0b9-116">Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, musi być podana instrukcji return.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="2e0b9-117">Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<span data-ttu-id="2e0b9-118">Sprawdź *Pages/Movies/Index.cshtml* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="2e0b9-119">Razor można przejść z kodu HTML w języku C# lub w znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="2e0b9-120">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor, w przeciwnym razie przejścia w języku C#.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="2e0b9-121">`@page` Dyrektywy Razor sprawia, że plik na akcję MVC &mdash; co oznacza, że może obsługiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="2e0b9-122">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="2e0b9-123">`@page` jest przykładem przechodzi do znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="2e0b9-124">Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="2e0b9-125">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="2e0b9-126">`DisplayNameFor` Sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="2e0b9-127">Wyrażenie lambda jest kontrolowane zamiast ocenione.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="2e0b9-128">Oznacza, że ma nie naruszenie zasad dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="2e0b9-129">Kiedy jest obliczane wyrażenie lambda (na przykład za pomocą `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="2e0b9-130">@model — Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="2e0b9-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="2e0b9-131">`@model` Dyrektywa określa typ modelu przekazywane do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="2e0b9-132">W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`— dostępny na stronie Razor klasy pochodnej.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="2e0b9-133">Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayName` [pomocników HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="2e0b9-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData i układu</span><span class="sxs-lookup"><span data-stu-id="2e0b9-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="2e0b9-135">Rozważmy poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="2e0b9-136">Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="2e0b9-137">`{` i `}` znaków należy umieścić blok kodu C#.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="2e0b9-138">`PageModel` Ma klasę bazową `ViewData` słownika właściwości, który może służyć do dodania danych, które mają być przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="2e0b9-139">Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="2e0b9-140">W poprzednim przykładzie, właściwość "Title" zostanie dodany do `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2e0b9-141">Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="2e0b9-142">Następujący kod przedstawia pierwszych kilka wierszy tego *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2e0b9-143">Właściwość "Title" jest używana w *Pages/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="2e0b9-144">Następujący kod przedstawia pierwszych kilka wierszy tego *_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="2e0b9-145">Wiersz `@*Markup removed for brevity.*@` komentarza Razor.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="2e0b9-146">W przeciwieństwie do komentarzy HTML (`<!-- -->`), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="2e0b9-147">Uruchom aplikację i przetestować linki w projekcie (**Home**, **o**, **skontaktuj się z pomocą**, **Utwórz**, **Edytuj**, i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="2e0b9-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="2e0b9-148">Każda strona Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="2e0b9-149">*Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale możesz modyfikować je mogą mieć różne wartości.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="2e0b9-150">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="2e0b9-151">Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="2e0b9-152">To [problem w usłudze GitHub 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) instrukcje dotyczące dodawania przecinek dziesiętny.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="2e0b9-153">`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="2e0b9-154">Poprzedni kod znaczników ustawia plik układu *Pages/Shared/_Layout.cshtml* dla wszystkich plików Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="2e0b9-155">Zobacz [układ](xref:razor-pages/index#layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="2e0b9-156">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="2e0b9-156">Update the layout</span></span>

<span data-ttu-id="2e0b9-157">Zmiana `<title>` element *Pages/Shared/_Layout.cshtml* pliku, aby użyć krótszego ciągu.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="2e0b9-158">Znajdź następujący element zakotwiczenia w *Pages/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-158">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="2e0b9-159">Zastąp poprzedzający element następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="2e0b9-160">Poprzedni element zakotwiczenia jest [Pomocnik tagu](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="2e0b9-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="2e0b9-161">W tym przypadku ma [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2e0b9-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="2e0b9-162">`asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` strona Razor.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="2e0b9-163">Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="2e0b9-164">Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) pliku w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="2e0b9-165">Tworzenie modelu strony</span><span class="sxs-lookup"><span data-stu-id="2e0b9-165">The Create page model</span></span>

<span data-ttu-id="2e0b9-166">Sprawdź *Pages/Movies/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]
::: moniker-end


<span data-ttu-id="2e0b9-167">`OnGet` Metoda inicjuje każdy stan potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="2e0b9-168">Strona tworzenia nie ma każdy stan, aby zainicjować, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="2e0b9-169">W dalszej części tego samouczka zostanie wyświetlony `OnGet` metoda inicjowania stanu.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="2e0b9-170">`Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="2e0b9-171">`Movie` Używa właściwości `[BindProperty]` atrybutu, aby wyrazić zgodę na [wiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="2e0b9-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="2e0b9-172">Gdy formularz Utwórz publikuje wartości formularza, środowisko uruchomieniowe programu ASP.NET Core wiąże przesłanych wartości w celu `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="2e0b9-173">`OnPostAsync` Metoda jest uruchomiona, gdy Strona publikuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="2e0b9-174">Jeśli występują błędy modelu, formularza zostanie wyświetlony ponownie, oraz wszelkie dane formularza opublikowane.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="2e0b9-175">Większość błędów modelu mogą być przechwytywane po stronie klienta, zanim opublikowania formularza.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="2e0b9-176">Przykładem błąd modelu publikuje wartość pola daty, którego nie można przekonwertować na wartość typu date.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="2e0b9-177">Omówimy więcej informacji na temat weryfikacji po stronie klienta i weryfikacja modelu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="2e0b9-178">Jeśli nie ma żadnych błędów modelu, dane są zapisywane, a przeglądarka jest przekierowywana na strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="2e0b9-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="2e0b9-179">Tworzenie strony Razor</span><span class="sxs-lookup"><span data-stu-id="2e0b9-179">The Create Razor Page</span></span>

<span data-ttu-id="2e0b9-180">Sprawdź *Pages/Movies/Create.cshtml* wartość pola plik strony Razor:</span><span class="sxs-lookup"><span data-stu-id="2e0b9-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
