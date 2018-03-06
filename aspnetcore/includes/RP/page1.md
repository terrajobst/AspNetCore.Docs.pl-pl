# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="ba215-101">Szkieletu Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba215-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ba215-102">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ba215-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ba215-103">W tym samouczku sprawdza stron Razor, tworzonych przez szkieletów poprzedniej samouczka.</span><span class="sxs-lookup"><span data-stu-id="ba215-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="ba215-104">[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.</span><span class="sxs-lookup"><span data-stu-id="ba215-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="ba215-105">Utwórz, Usuń, szczegóły i Edycja stron.</span><span class="sxs-lookup"><span data-stu-id="ba215-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="ba215-106">Sprawdź *Pages/Movies/Index.cshtml.cs* modelu strony: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="ba215-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="ba215-107">Pochodne stron razor `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="ba215-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="ba215-108">Według konwencji `PageModel`-nosi nazwę klasy pochodnej `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="ba215-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="ba215-109">Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) można dodać `MovieContext` do strony.</span><span class="sxs-lookup"><span data-stu-id="ba215-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="ba215-110">Wszystkie strony szkieletu wykonaj tego wzorca.</span><span class="sxs-lookup"><span data-stu-id="ba215-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="ba215-111">Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) uzyskać więcej informacji o asynchronicznych programing Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ba215-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="ba215-112">Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów na stronie aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba215-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="ba215-113">`OnGetAsync` lub `OnGet` jest wywoływana na stronie aparatu Razor zainicjować stan strony.</span><span class="sxs-lookup"><span data-stu-id="ba215-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="ba215-114">W takim przypadku `OnGetAsync` pobiera listę filmy i wyświetla je.</span><span class="sxs-lookup"><span data-stu-id="ba215-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="ba215-115">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca`Task`, brak metody zwracany jest używany.</span><span class="sxs-lookup"><span data-stu-id="ba215-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="ba215-116">Gdy typ zwrotny jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcji return.</span><span class="sxs-lookup"><span data-stu-id="ba215-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="ba215-117">Na przykład *Pages/Movies/Create.cshtml.cs* `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="ba215-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
<span data-ttu-id="ba215-118">Sprawdź *Pages/Movies/Index.cshtml* Razor strony:</span><span class="sxs-lookup"><span data-stu-id="ba215-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="ba215-119">Razor można przejście z kodu HTML w C# lub znacznika specyficzne dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba215-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="ba215-120">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), jego przejścia do znaczników specyficzne dla elementu Razor, w przeciwnym razie przejścia w języku C#.</span><span class="sxs-lookup"><span data-stu-id="ba215-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="ba215-121">`@page` Dyrektywy Razor sprawia, że plik na akcję MVC &mdash; co oznacza, że może obsłużyć żądania.</span><span class="sxs-lookup"><span data-stu-id="ba215-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="ba215-122">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="ba215-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="ba215-123">`@page` jest przykładem przechodzi do znaczników specyficzne dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba215-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="ba215-124">Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ba215-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="ba215-125">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="ba215-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="ba215-126">`DisplayNameFor` Przeprowadzający pomocnika kodu HTML `Title` właściwości, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="ba215-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="ba215-127">Wyrażenie lambda jest sprawdzana zamiast obliczone.</span><span class="sxs-lookup"><span data-stu-id="ba215-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="ba215-128">Oznacza to, że istnieje żadne naruszenie dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="ba215-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="ba215-129">Podczas oceny wyrażenia lambda (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="ba215-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="ba215-130">@model — Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="ba215-130">The @model directive</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="ba215-131">`@model` Dyrektywa określa typ modelu przekazywane do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="ba215-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="ba215-132">W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`-klasy dostępny na stronie aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba215-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="ba215-133">Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayName` [pomocników HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.</span><span class="sxs-lookup"><span data-stu-id="ba215-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="ba215-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData i układu</span><span class="sxs-lookup"><span data-stu-id="ba215-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="ba215-135">Rozważmy następujący kod:</span><span class="sxs-lookup"><span data-stu-id="ba215-135">Consider the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="ba215-136">Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#.</span><span class="sxs-lookup"><span data-stu-id="ba215-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="ba215-137">`{` i `}` znaków ujmij bloku kodu C#.</span><span class="sxs-lookup"><span data-stu-id="ba215-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="ba215-138">`PageModel` Ma klasy podstawowej `ViewData` właściwości słownik, który może służyć do dodania danych, które mają być przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="ba215-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="ba215-139">Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucza i wartości.</span><span class="sxs-lookup"><span data-stu-id="ba215-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="ba215-140">W poprzednim przykładzie właściwość "Title" została dodana do `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="ba215-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="ba215-141">Właściwość "Title" jest używana w *Pages/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="ba215-141">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="ba215-142">Następujący kod przedstawia pierwsze kilka wierszy *Pages/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="ba215-142">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="ba215-143">Wiersz `@*Markup removed for brevity.*@` komentarza Razor.</span><span class="sxs-lookup"><span data-stu-id="ba215-143">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="ba215-144">W przeciwieństwie do komentarzy HTML (`<!-- -->`), Razor komentarze nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="ba215-144">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="ba215-145">Uruchom aplikację i przetestować linki w projekcie (**Home**, **o**, **skontaktuj się z**, **Utwórz**, **Edytuj**, i **usunąć**).</span><span class="sxs-lookup"><span data-stu-id="ba215-145">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="ba215-146">Każdej strony Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki.</span><span class="sxs-lookup"><span data-stu-id="ba215-146">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="ba215-147">*Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale można modyfikować mają różne wartości.</span><span class="sxs-lookup"><span data-stu-id="ba215-147">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="ba215-148">`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="ba215-148">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="ba215-149">Poprzedni kod znaczników ustawia plik układu *Pages/_Layout.cshtml* wszystkich plików Razor w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="ba215-149">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="ba215-150">Zobacz [układu](xref:mvc/razor-pages/index#layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ba215-150">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="ba215-151">Aktualizacja układu</span><span class="sxs-lookup"><span data-stu-id="ba215-151">Update the layout</span></span>

<span data-ttu-id="ba215-152">Zmień `<title>` element *Pages/_Layout.cshtml* plik krótszego ciągu.</span><span class="sxs-lookup"><span data-stu-id="ba215-152">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="ba215-153">Znajdź następujący element zakotwiczenia w *Pages/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="ba215-153">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="ba215-154">Zastąp następujący kod znaczników poprzedni element.</span><span class="sxs-lookup"><span data-stu-id="ba215-154">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="ba215-155">Poprzedni element zakotwiczenia jest [pomocnika tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ba215-155">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="ba215-156">W takim przypadku ma [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ba215-156">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="ba215-157">`asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` Razor strony.</span><span class="sxs-lookup"><span data-stu-id="ba215-157">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="ba215-158">Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza.</span><span class="sxs-lookup"><span data-stu-id="ba215-158">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="ba215-159">Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) pliku w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="ba215-159">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="ba215-160">Utwórz model strony</span><span class="sxs-lookup"><span data-stu-id="ba215-160">The Create page model</span></span>

<span data-ttu-id="ba215-161">Sprawdź *Pages/Movies/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="ba215-161">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="ba215-162">`OnGet` Metoda inicjuje każdy stan potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="ba215-162">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="ba215-163">Utwórz stronę nie ma żadnych stan inicjowania.</span><span class="sxs-lookup"><span data-stu-id="ba215-163">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="ba215-164">`Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="ba215-164">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="ba215-165">`Movie` Używa właściwości `[BindProperty]` atrybut, aby wyrazić zgodę na [modelu powiązania](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="ba215-165">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="ba215-166">Gdy formularz Utwórz przesyła wartości formularza, środowisko uruchomieniowe platformy ASP.NET Core wiąże przesłanych wartości do `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="ba215-166">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="ba215-167">`OnPostAsync` Metody jest uruchamiany, gdy strona zapisuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="ba215-167">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="ba215-168">Jeśli wystąpią jakieś błędy modelu, formularza zostanie wyświetlony ponownie, wraz z danymi formularza opublikowane.</span><span class="sxs-lookup"><span data-stu-id="ba215-168">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="ba215-169">Większość błędów modelu może być wykrytych po stronie klienta, przed opublikowania formularza.</span><span class="sxs-lookup"><span data-stu-id="ba215-169">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="ba215-170">Przykład błąd modelu jest przesyłanie wartość pola daty, której nie można przekonwertować na typ date.</span><span class="sxs-lookup"><span data-stu-id="ba215-170">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="ba215-171">Firma Microsoft będzie komunikować więcej informacji na temat weryfikacji po stronie klienta i sprawdzania poprawności modelu później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="ba215-171">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="ba215-172">Jeśli nie ma żadnych błędów w modelu, dane są zapisywane i przeglądarki, zostanie przekierowany do strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="ba215-172">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="ba215-173">Utwórz stronę Razor</span><span class="sxs-lookup"><span data-stu-id="ba215-173">The Create Razor Page</span></span>

<span data-ttu-id="ba215-174">Sprawdź *Pages/Movies/Create.cshtml* pliku Razor strony:</span><span class="sxs-lookup"><span data-stu-id="ba215-174">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
