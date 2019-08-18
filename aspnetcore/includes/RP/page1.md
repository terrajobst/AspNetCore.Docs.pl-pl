# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="ea413-101">Razor Pages szkieletowe w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea413-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ea413-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ea413-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ea413-103">Ten samouczek służy do badania Razor Pages utworzonych przez tworzenie szkieletów w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="ea413-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="ea413-104">[Wyświetlanie lub pobieranie](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) próbki.</span><span class="sxs-lookup"><span data-stu-id="ea413-104">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="ea413-105">Strony tworzenie, usuwanie, szczegóły i edycja.</span><span class="sxs-lookup"><span data-stu-id="ea413-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="ea413-106">Zapoznaj się z modelem stron */filmów/index. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="ea413-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="ea413-107">Razor Pages pochodzą od `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="ea413-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="ea413-108">Zgodnie z `PageModel`Konwencją Klasa pochodna jest wywoływana `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="ea413-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="ea413-109">Konstruktor używa [iniekcji zależności](xref:fundamentals/dependency-injection) , aby dodać `MovieContext` do strony.</span><span class="sxs-lookup"><span data-stu-id="ea413-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="ea413-110">Wszystkie strony szkieletowe są zgodne z tym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="ea413-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="ea413-111">Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) , aby uzyskać więcej informacji na temat programowania asynchronicznego przy użyciu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ea413-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="ea413-112">Gdy żądanie jest wykonywane dla strony, `OnGetAsync` Metoda zwraca listę filmów do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="ea413-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="ea413-113">`OnGetAsync`lub `OnGet` jest wywoływana na stronie Razor, aby zainicjować stan dla strony.</span><span class="sxs-lookup"><span data-stu-id="ea413-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="ea413-114">W takim przypadku `OnGetAsync` pobiera listę filmów i wyświetla je.</span><span class="sxs-lookup"><span data-stu-id="ea413-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="ea413-115">Gdy `OnGet` zwraca `void` lub `OnGetAsync` zwraca,niejestużywanażadnametodaReturn.`Task`</span><span class="sxs-lookup"><span data-stu-id="ea413-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="ea413-116">Gdy typem zwracanym jest `IActionResult` lub `Task<IActionResult>`, należy podać instrukcję return.</span><span class="sxs-lookup"><span data-stu-id="ea413-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="ea413-117">Na przykład: *Pages/Films/Create. cshtml. cs.* `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="ea413-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a><span data-ttu-id="ea413-118">Przejrzyj stronę */filmy/index. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="ea413-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="ea413-119">Razor można przenieść z HTML do C# lub do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="ea413-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="ea413-120">Gdy po C#symbolu następuje [słowo kluczowe zarezerwowane Razor](xref:mvc/views/razor#razor-reserved-keywords), przechodzi do znacznika specyficznego dla Razor, w przeciwnym razie przechodzi do. `@`</span><span class="sxs-lookup"><span data-stu-id="ea413-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="ea413-121">Dyrektywa Razor powoduje, że plik jest akcją &mdash; MVC, co oznacza, że może obsługiwać żądania. `@page`</span><span class="sxs-lookup"><span data-stu-id="ea413-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="ea413-122">`@page`musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="ea413-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="ea413-123">`@page`jest przykładem przejścia do znacznika specyficznego dla Razor.</span><span class="sxs-lookup"><span data-stu-id="ea413-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="ea413-124">Aby uzyskać więcej informacji, zobacz [składnia Razor](xref:mvc/views/razor#razor-syntax) .</span><span class="sxs-lookup"><span data-stu-id="ea413-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="ea413-125">Bada wyrażenie lambda użyte w następującym Pomocniku HTML:</span><span class="sxs-lookup"><span data-stu-id="ea413-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="ea413-126">Pomocnik html sprawdza `Title` właściwość, do której istnieje odwołanie w wyrażeniu lambda, aby określić nazwę wyświetlaną. `DisplayNameFor`</span><span class="sxs-lookup"><span data-stu-id="ea413-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="ea413-127">Wyrażenie lambda jest sprawdzane, a nie oceniane.</span><span class="sxs-lookup"><span data-stu-id="ea413-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="ea413-128">Oznacza to, że nie ma żadnych naruszeń `model`dostępu `model.Movie`, gdy `model.Movie[0]` , `null` , lub są puste.</span><span class="sxs-lookup"><span data-stu-id="ea413-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="ea413-129">Gdy wyrażenie lambda jest oceniane (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu są oceniane.</span><span class="sxs-lookup"><span data-stu-id="ea413-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="ea413-130">@model Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="ea413-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="ea413-131">`@model` Dyrektywa określa typ modelu przekazaną do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="ea413-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="ea413-132">W poprzednim przykładzie `@model` wiersz `PageModel`powoduje, że Klasa pochodna jest dostępna dla strony Razor.</span><span class="sxs-lookup"><span data-stu-id="ea413-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="ea413-133">Model jest używany w `@Html.DisplayNameFor` [pomocnikach HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) i `@Html.DisplayFor` na stronie.</span><span class="sxs-lookup"><span data-stu-id="ea413-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="ea413-134">ViewData i układ</span><span class="sxs-lookup"><span data-stu-id="ea413-134">ViewData and layout</span></span>

<span data-ttu-id="ea413-135">Rozważmy następujący kod:</span><span class="sxs-lookup"><span data-stu-id="ea413-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="ea413-136">Poprzedni wyróżniony kod jest przykładem przejścia Razor do C#.</span><span class="sxs-lookup"><span data-stu-id="ea413-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="ea413-137">Znaki `{` i `}` należy ująć w blok C# kodu.</span><span class="sxs-lookup"><span data-stu-id="ea413-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="ea413-138">Klasa `PageModel` bazowa `ViewData` ma właściwość dictionary, która może służyć do dodawania danych, które mają zostać przekazane do widoku.</span><span class="sxs-lookup"><span data-stu-id="ea413-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="ea413-139">Obiekty można dodawać do `ViewData` słownika przy użyciu wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="ea413-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="ea413-140">W poprzednim przykładzie właściwość "title" została dodana do `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="ea413-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ea413-141">Właściwość "title" jest używana w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ea413-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="ea413-142">Poniższe znaczniki pokazują kilka pierwszych wierszy *stron/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ea413-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ea413-143">Właściwość "title" jest używana w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ea413-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="ea413-144">Poniższy znacznik pokazuje pierwsze kilka wierszy pliku *_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ea413-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="ea413-145">Wiersz `@*Markup removed for brevity.*@` jest komentarzem Razor.</span><span class="sxs-lookup"><span data-stu-id="ea413-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="ea413-146">W przeciwieństwie do komentarzy`<!-- -->`HTML (), komentarze Razor nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="ea413-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="ea413-147">Uruchom aplikację i przetestuj linki w projekcie (**Strona główna**, **informacje**, **kontakt**, **Utwórz**, **Edytuj**i **Usuń**).</span><span class="sxs-lookup"><span data-stu-id="ea413-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="ea413-148">Każda Strona ustawia tytuł, który można zobaczyć na karcie przeglądarki. Po utworzeniu zakładki na stronie tytuł jest używany dla zakładki.</span><span class="sxs-lookup"><span data-stu-id="ea413-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="ea413-149">*Pages/index. cshtml* i *Pages/Films/index. cshtml* mają teraz ten sam tytuł, ale można je zmodyfikować tak, aby zawierały różne wartości.</span><span class="sxs-lookup"><span data-stu-id="ea413-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="ea413-150">Nie można wprowadzić dziesiętna przecinkami w `Price` pola.</span><span class="sxs-lookup"><span data-stu-id="ea413-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="ea413-151">Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i formatów dat innych niż angielski, należy wykonać kroki w celu globalizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ea413-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="ea413-152">Ten [problem w usłudze GitHub 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) zawiera instrukcje dotyczące dodawania przecinków dziesiętnych.</span><span class="sxs-lookup"><span data-stu-id="ea413-152">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="ea413-153">Właściwość jest ustawiana w pliku *Pages/_ViewStart. cshtml:* `Layout`</span><span class="sxs-lookup"><span data-stu-id="ea413-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="ea413-154">Powyższe oznakowanie ustawia plik układu na *Pages/Shared/_Layout. cshtml* dla wszystkich plików Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="ea413-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="ea413-155">Aby uzyskać więcej informacji, zobacz [Układ](xref:razor-pages/index#layout) .</span><span class="sxs-lookup"><span data-stu-id="ea413-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="ea413-156">Aktualizowanie układu</span><span class="sxs-lookup"><span data-stu-id="ea413-156">Update the layout</span></span>

<span data-ttu-id="ea413-157">Zmień element w pliku *Pages/Shared/_Layout. cshtml* , aby użyć krótszego ciągu. `<title>`</span><span class="sxs-lookup"><span data-stu-id="ea413-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="ea413-158">Znajdź następujący element zakotwiczony w pliku *Pages/Shared/_Layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ea413-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```

<span data-ttu-id="ea413-159">Zastąp poprzedni element następującym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="ea413-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="ea413-160">Poprzedni element zakotwiczenia jest [pomocnikiem tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ea413-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="ea413-161">W tym przypadku jest to [pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ea413-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="ea413-162">Atrybut pomocnika `/Movies/Index`tagówi wartość tworzy łącze do strony Razor. `asp-page="/Movies/Index"`</span><span class="sxs-lookup"><span data-stu-id="ea413-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="ea413-163">Zapisz zmiany i przetestuj aplikację, klikając łącze **RpMovie** .</span><span class="sxs-lookup"><span data-stu-id="ea413-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="ea413-164">Zobacz plik [_Layout. cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="ea413-164">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="ea413-165">Model tworzenia strony</span><span class="sxs-lookup"><span data-stu-id="ea413-165">The Create page model</span></span>

<span data-ttu-id="ea413-166">Obejrzyj model stron */filmów/Utwórz. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="ea413-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end

<span data-ttu-id="ea413-167">`OnGet` Metoda inicjuje wszystkie Stany potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="ea413-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="ea413-168">Strona tworzenia nie ma żadnego stanu do zainicjowania, więc `Page` jest zwracana.</span><span class="sxs-lookup"><span data-stu-id="ea413-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="ea413-169">W dalszej części tego samouczka zobaczysz `OnGet` stan zainicjowania metody.</span><span class="sxs-lookup"><span data-stu-id="ea413-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="ea413-170">Metoda tworzy obiekt, który renderuje stronę *Create. cshtml.* `Page` `PageResult`</span><span class="sxs-lookup"><span data-stu-id="ea413-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="ea413-171">Właściwość używa atrybutu, `[BindProperty]` aby wybrać [powiązanie modelu.](xref:mvc/models/model-binding) `Movie`</span><span class="sxs-lookup"><span data-stu-id="ea413-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="ea413-172">Gdy formularz tworzenia zapisuje wartości formularza, środowisko uruchomieniowe ASP.NET Core tworzy powiązanie opublikowanych wartości z `Movie` modelem.</span><span class="sxs-lookup"><span data-stu-id="ea413-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="ea413-173">`OnPostAsync` Metoda jest uruchamiana, gdy strona zapisuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="ea413-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="ea413-174">Jeśli występują jakieś błędy modelu, formularz jest ponownie wyświetlany wraz z wszelkimi opublikowanymi danymi formularza.</span><span class="sxs-lookup"><span data-stu-id="ea413-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="ea413-175">Większość błędów modelu można przechwycić po stronie klienta przed opublikowaniem formularza.</span><span class="sxs-lookup"><span data-stu-id="ea413-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="ea413-176">Przykładem błędu modelu jest opublikowanie wartości pola daty, którego nie można przekonwertować na datę.</span><span class="sxs-lookup"><span data-stu-id="ea413-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="ea413-177">Będziemy dowiedzieć się więcej o sprawdzaniu poprawności i weryfikacji modelu po stronie klienta w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ea413-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="ea413-178">Jeśli nie ma żadnych błędów modelu, dane zostaną zapisane i przeglądarka zostanie przekierowana na stronę indeksu.</span><span class="sxs-lookup"><span data-stu-id="ea413-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="ea413-179">Strona tworzenie Razor</span><span class="sxs-lookup"><span data-stu-id="ea413-179">The Create Razor Page</span></span>

<span data-ttu-id="ea413-180">Przejrzyj strony */filmy/Utwórz* plik stronicowania Razor. cshtml:</span><span class="sxs-lookup"><span data-stu-id="ea413-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).

![VS17 view of Create.cshtml page](page/_static/th.png)
-->
