---
title: Szkieletu Razor strony platformy ASP.NET Core
author: rick-anderson
description: "W tym artykule wyjaśniono stron Razor generowane przez funkcję szkieletów."
keywords: Platformy ASP.NET Core stron Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: e42e7e469e411d2d4bc1bd1b3a3995a77c355ebd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="ba58b-104">Szkieletu Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba58b-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ba58b-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ba58b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ba58b-106">W tym samouczku sprawdza stron Razor, tworzonych przez szkieletów w poprzednim temacie samouczek [Dodawanie modelu](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span><span class="sxs-lookup"><span data-stu-id="ba58b-106">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="ba58b-107">[Wyświetlanie lub pobieranie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) próbki.</span><span class="sxs-lookup"><span data-stu-id="ba58b-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="ba58b-108">Utwórz, Usuń, szczegóły i Edycja stron.</span><span class="sxs-lookup"><span data-stu-id="ba58b-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="ba58b-109">Sprawdź *Pages/Movies/Index.cshtml.cs* pliku CodeBehind:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="ba58b-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="ba58b-110">Pochodne stron razor `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="ba58b-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="ba58b-111">Według konwencji `PageModel`-nosi nazwę klasy pochodnej `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="ba58b-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="ba58b-112">Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) można dodać `MovieContext` do strony.</span><span class="sxs-lookup"><span data-stu-id="ba58b-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="ba58b-113">Wszystkie strony szkieletu wykonaj tego wzorca.</span><span class="sxs-lookup"><span data-stu-id="ba58b-113">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="ba58b-114">Zobacz [kod asynchroniczny](xref:data/ef-rp/intro#asynchronous-code) uzyskać więcej informacji o asynchronicznych programing Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ba58b-114">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="ba58b-115">Po wysłaniu żądania dla tej strony, `OnGetAsync` metoda zwraca listę filmów na stronie aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba58b-115">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="ba58b-116">`OnGetAsync`lub `OnGet` jest wywoływana na stronie aparatu Razor zainicjować stan strony.</span><span class="sxs-lookup"><span data-stu-id="ba58b-116">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="ba58b-117">W takim przypadku `OnGetAsync` pobiera listę filmów do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="ba58b-117">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="ba58b-118">Sprawdź *Pages/Movies/Index.cshtml* Razor strony:</span><span class="sxs-lookup"><span data-stu-id="ba58b-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="ba58b-119">Razor można przejście z kodu HTML w C# lub znacznika specyficzne dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba58b-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="ba58b-120">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](xref:mvc/views/razor#razor-reserved-keywords), jego przejścia do znaczników specyficzne dla elementu Razor, w przeciwnym razie przejścia w języku C#.</span><span class="sxs-lookup"><span data-stu-id="ba58b-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="ba58b-121">`@page` Dyrektywy Razor sprawia, że plik na akcję MVC &mdash; co oznacza, że może obsłużyć żądania.</span><span class="sxs-lookup"><span data-stu-id="ba58b-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="ba58b-122">`@page`musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="ba58b-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="ba58b-123">`@page`jest przykładem przechodzi do znaczników specyficzne dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba58b-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="ba58b-124">Zobacz [składni Razor](xref:mvc/views/razor#razor-syntax) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ba58b-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="ba58b-125">Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="ba58b-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="ba58b-126">`DisplayNameFor` Przeprowadzający pomocnika kodu HTML `Title` właściwości, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="ba58b-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="ba58b-127">Wyrażenie lambda jest sprawdzana zamiast obliczone.</span><span class="sxs-lookup"><span data-stu-id="ba58b-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="ba58b-128">Oznacza to, że istnieje żadne naruszenie dostępu podczas `model`, `model.Movie`, lub `model.Movie[0]` są `null` lub jest pusty.</span><span class="sxs-lookup"><span data-stu-id="ba58b-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="ba58b-129">Podczas oceny wyrażenia lambda (na przykład z `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="ba58b-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="ba58b-130">@model — Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="ba58b-130">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="ba58b-131">`@model` Dyrektywa określa typ modelu przekazywane do strony Razor.</span><span class="sxs-lookup"><span data-stu-id="ba58b-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="ba58b-132">W powyższym przykładzie `@model` wiersz sprawia, że `PageModel`-klasy dostępny na stronie aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="ba58b-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="ba58b-133">Model jest używany w `@Html.DisplayNameFor` i `@Html.DisplayName` [pomocników HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) na stronie.</span><span class="sxs-lookup"><span data-stu-id="ba58b-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="ba58b-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData i układu</span><span class="sxs-lookup"><span data-stu-id="ba58b-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="ba58b-135">Rozważmy następujący kod:</span><span class="sxs-lookup"><span data-stu-id="ba58b-135">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="ba58b-136">Poprzedni wyróżniony kod jest przykładem Razor przechodzi do języka C#.</span><span class="sxs-lookup"><span data-stu-id="ba58b-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="ba58b-137">`{` i `}` znaków ujmij bloku kodu C#.</span><span class="sxs-lookup"><span data-stu-id="ba58b-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="ba58b-138">`PageModel` Ma klasy podstawowej `ViewData` właściwości słownik, który może służyć do dodania danych, które mają być przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="ba58b-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="ba58b-139">Dodawanie obiektów do `ViewData` słownika przy użyciu wzorca klucza i wartości.</span><span class="sxs-lookup"><span data-stu-id="ba58b-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="ba58b-140">W poprzednim przykładzie właściwość "Title" została dodana do `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="ba58b-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="ba58b-141">Właściwość "Title" jest używana w *Pages/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="ba58b-141">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="ba58b-142">Następujący kod przedstawia pierwsze kilka wierszy *Pages/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="ba58b-142">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="ba58b-143">Wiersz `@*Markup removed for brevity.*@` komentarza Razor.</span><span class="sxs-lookup"><span data-stu-id="ba58b-143">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="ba58b-144">W przeciwieństwie do komentarzy HTML (`<!-- -->`), Razor komentarze nie są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="ba58b-144">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="ba58b-145">Uruchom aplikację i przetestować linki w projekcie (**Home**, **o**, **skontaktuj się z**, **Utwórz**, **Edytuj**, i **usunąć**).</span><span class="sxs-lookup"><span data-stu-id="ba58b-145">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="ba58b-146">Każdej strony Ustawia tytuł, który można wyświetlić na karcie przeglądarki. Gdy zakładki na stronie tytuł jest używana do zakładki.</span><span class="sxs-lookup"><span data-stu-id="ba58b-146">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="ba58b-147">*Pages/Index.cshtml* i *Pages/Movies/Index.cshtml* obecnie o takiej samej nazwie, ale można modyfikować mają różne wartości.</span><span class="sxs-lookup"><span data-stu-id="ba58b-147">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="ba58b-148">`Layout` Właściwość jest ustawiona *Pages/_ViewStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="ba58b-148">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="ba58b-149">Poprzedni kod znaczników ustawia plik układu *Pages/_Layout.cshtml* wszystkich plików Razor w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="ba58b-149">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="ba58b-150">Zobacz [układu](xref:mvc/razor-pages/index#layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ba58b-150">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="ba58b-151">Aktualizacja układu</span><span class="sxs-lookup"><span data-stu-id="ba58b-151">Update the layout</span></span>

<span data-ttu-id="ba58b-152">Zmień `<title>` element *Pages/_Layout.cshtml* plik krótszego ciągu.</span><span class="sxs-lookup"><span data-stu-id="ba58b-152">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="ba58b-153">Znajdź następujący element zakotwiczenia w *Pages/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="ba58b-153">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="ba58b-154">Zastąp następujący kod znaczników poprzedni element.</span><span class="sxs-lookup"><span data-stu-id="ba58b-154">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="ba58b-155">Poprzedni element zakotwiczenia jest [pomocnika tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ba58b-155">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="ba58b-156">W takim przypadku ma [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ba58b-156">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="ba58b-157">`asp-page="/Movies/Index"` Atrybut pomocnika tagów i wartość tworzy łącze do `/Movies/Index` Razor strony.</span><span class="sxs-lookup"><span data-stu-id="ba58b-157">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="ba58b-158">Zapisz zmiany i przetestować aplikację, klikając **RpMovie** łącza.</span><span class="sxs-lookup"><span data-stu-id="ba58b-158">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="ba58b-159">Zobacz [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) pliku w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="ba58b-159">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="ba58b-160">Utwórz stronę związane z kodem</span><span class="sxs-lookup"><span data-stu-id="ba58b-160">The Create code-behind page</span></span>

<span data-ttu-id="ba58b-161">Sprawdź *Pages/Movies/Create.cshtml.cs* pliku CodeBehind:</span><span class="sxs-lookup"><span data-stu-id="ba58b-161">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="ba58b-162">`OnGet` Metoda inicjuje każdy stan potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="ba58b-162">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="ba58b-163">Utwórz stronę nie ma żadnych stan inicjowania.</span><span class="sxs-lookup"><span data-stu-id="ba58b-163">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="ba58b-164">`Page` Metoda tworzy `PageResult` obiektu, który renderuje *Create.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="ba58b-164">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="ba58b-165">`Movie` Używa właściwości `[BindProperty]` atrybut, aby wyrazić zgodę na [modelu powiązania](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="ba58b-165">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="ba58b-166">Gdy formularz Utwórz przesyła wartości formularza, środowisko uruchomieniowe platformy ASP.NET Core wiąże przesłanych wartości do `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="ba58b-166">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="ba58b-167">`OnPostAsync` Metody jest uruchamiany, gdy strona zapisuje dane formularza:</span><span class="sxs-lookup"><span data-stu-id="ba58b-167">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="ba58b-168">Jeśli wystąpią jakieś błędy modelu, formularza zostanie wyświetlony ponownie, wraz z danymi formularza opublikowane.</span><span class="sxs-lookup"><span data-stu-id="ba58b-168">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="ba58b-169">Większość błędów modelu może być wykrytych po stronie klienta, przed opublikowania formularza.</span><span class="sxs-lookup"><span data-stu-id="ba58b-169">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="ba58b-170">Przykład błąd modelu jest przesyłanie wartość pola daty, której nie można przekonwertować na typ date.</span><span class="sxs-lookup"><span data-stu-id="ba58b-170">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="ba58b-171">Firma Microsoft będzie komunikować więcej informacji na temat weryfikacji po stronie klienta i sprawdzania poprawności modelu później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="ba58b-171">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="ba58b-172">Jeśli nie ma żadnych błędów w modelu, dane są zapisywane i przeglądarki, zostanie przekierowany do strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="ba58b-172">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="ba58b-173">Utwórz stronę Razor</span><span class="sxs-lookup"><span data-stu-id="ba58b-173">The Create Razor Page</span></span>

<span data-ttu-id="ba58b-174">Sprawdź *Pages/Movies/Create.cshtml* pliku Razor strony:</span><span class="sxs-lookup"><span data-stu-id="ba58b-174">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="ba58b-175">Wyświetla programu Visual Studio `<form method="post">` tag w charakterystyczne czcionkę dla pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="ba58b-175">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="ba58b-176">`<form method="post">` Jest element [pomocnika Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ba58b-176">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="ba58b-177">Pomocnik Tag formularza automatycznie uwzględnia [antiforgery token](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="ba58b-177">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![VS17 widok Create.cshtml strony](page/_static/th.png)

<span data-ttu-id="ba58b-179">Aparat szkieletów tworzy znaczników Razor dla każdego pola w modelu (z wyjątkiem Identyfikatora) podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="ba58b-179">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="ba58b-180">[Pomocników tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i ` <span asp-validation-for`) wyświetla błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="ba58b-180">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="ba58b-181">Sprawdzanie poprawności zostało opisane bardziej szczegółowo w dalszej części tej serii.</span><span class="sxs-lookup"><span data-stu-id="ba58b-181">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="ba58b-182">[Pomocnika tagów etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i `for` atrybutu dla `Title` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ba58b-182">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="ba58b-183">[Pomocnika Tag danych wejściowych](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) używa [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów i tworzy wymagane dla technologii jQuery weryfikacji po stronie klienta atrybutów HTML.</span><span class="sxs-lookup"><span data-stu-id="ba58b-183">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="ba58b-184">Następny samouczek wyjaśnia bazy danych LocalDB programu SQL Server i wstępne wypełnianie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ba58b-184">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ba58b-185">[Poprzedni: Dodawanie modelu](xref:tutorials/razor-pages/model)
[dalej: bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="ba58b-185">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
