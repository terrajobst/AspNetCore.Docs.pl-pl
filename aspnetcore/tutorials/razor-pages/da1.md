---
title: Aktualizowanie wygenerowanych stron w aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować wygenerowane strony w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0f6535462fe2d308825bf7289c10d2b0690cebd4
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334112"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="1ba3e-103">Aktualizowanie wygenerowanych stron w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ba3e-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="1ba3e-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ba3e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1ba3e-105">Aplikacja ze szkieletem jest dobrze uruchomiona, ale prezentacja nie jest idealnym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="1ba3e-106">**ReleaseDate** powinna być **datą wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplikacja filmowa otwarta w przeglądarce Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="1ba3e-108">Aktualizowanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="1ba3e-108">Update the generated code</span></span>

<span data-ttu-id="1ba3e-109">Otwórz plik *models/Movie. cs* i Dodaj wyróżnione wiersze wyświetlane w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="1ba3e-110">`[Column(TypeName = "decimal(18, 2)")]` adnotacji danych umożliwia Entity Framework Core prawidłowe mapowanie `Price` na walutę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="1ba3e-111">Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="1ba3e-112">[Adnotacje DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) zostały omówione w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="1ba3e-113">Atrybut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="1ba3e-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="1ba3e-114">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data), więc informacje o czasie przechowywane w polu nie są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="1ba3e-115">Przejdź do stron/filmów i umieść kursor na linku **edycji** , aby zobaczyć docelowy adres URL.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Okno przeglądarki z myszą nad linkiem edycji i adresem URL linku http://localhost:1234/Movies/Edit/5 jest pokazywany](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="1ba3e-117">Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez [pomocnika tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w pliku *Pages/Films/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="1ba3e-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="1ba3e-118">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1ba3e-119">W poprzednim kodzie `AnchorTagHelper` dynamicznie generuje wartość atrybutu `href` HTML ze strony Razor (trasa jest względna), `asp-page`i identyfikator trasy (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="1ba3e-120">Aby uzyskać więcej informacji, zobacz [generowanie adresów URL na stronach](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="1ba3e-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="1ba3e-121">Użyj **widoku źródła** z ulubionej przeglądarki, aby sprawdzić wygenerowane znaczniki.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="1ba3e-122">Poniżej przedstawiono część wygenerowanego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="1ba3e-123">Dynamicznie generowane linki przekażą identyfikator filmu z ciągiem zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

### <a name="add-route-template"></a><span data-ttu-id="1ba3e-124">Dodawanie szablonu trasy</span><span class="sxs-lookup"><span data-stu-id="1ba3e-124">Add route template</span></span>

<span data-ttu-id="1ba3e-125">Zaktualizuj Razor Pages edycji, szczegółów i usuwania, aby użyć szablonu trasy "{ID: int}".</span><span class="sxs-lookup"><span data-stu-id="1ba3e-125">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="1ba3e-126">Zmień dyrektywę Page dla każdej z tych stron z `@page` na `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-126">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="1ba3e-127">Uruchom aplikację, a następnie Wyświetl źródło.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-127">Run the app and then view source.</span></span> <span data-ttu-id="1ba3e-128">Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-128">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="1ba3e-129">Żądanie do strony z szablonem trasy "{ID: int}", który **nie** zawiera liczby całkowitej, zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-129">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1ba3e-130">Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-130">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="1ba3e-131">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-131">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="1ba3e-132">Aby przetestować zachowanie `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-132">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="1ba3e-133">Ustaw dyrektywę Page na stronie */Films/details. cshtml* na `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-133">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="1ba3e-134">Ustaw punkt przerwania w `public async Task<IActionResult> OnGetAsync(int? id)` (w obszarze *strony/filmy/szczegóły. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-134">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="1ba3e-135">Przejdź do adresu `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-135">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="1ba3e-136">Za pomocą dyrektywy `@page "{id:int}"` punkt przerwania nigdy nie trafi.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-136">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="1ba3e-137">Aparat routingu zwraca protokół HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-137">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="1ba3e-138">Przy użyciu `@page "{id:int?}"`Metoda `OnGetAsync` zwraca `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-138">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="1ba3e-139">Przejrzyj obsługę wyjątków współbieżności</span><span class="sxs-lookup"><span data-stu-id="1ba3e-139">Review concurrency exception handling</span></span>

<span data-ttu-id="1ba3e-140">Przejrzyj metodę `OnPostAsync` w pliku *Pages/Films/Edit. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="1ba3e-140">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="1ba3e-141">Poprzedni kod wykrywa wyjątki współbieżności, gdy jeden klient usuwa film, a pozostałe wpisy wprowadzane przez klienta do filmu.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-141">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="1ba3e-142">Aby przetestować blok `catch`:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-142">To test the `catch` block:</span></span>

* <span data-ttu-id="1ba3e-143">Ustaw punkt przerwania na `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="1ba3e-143">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="1ba3e-144">Wybierz pozycję **Edytuj** dla filmu, wprowadź zmiany, ale nie wprowadzaj opcji **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-144">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="1ba3e-145">W innym oknie przeglądarki wybierz łącze **Usuń** dla tego samego filmu, a następnie usuń film.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-145">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="1ba3e-146">W poprzednim oknie przeglądarki Opublikuj zmiany w filmie.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-146">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="1ba3e-147">Kod produkcyjny może chcieć wykryć konflikty współbieżności.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-147">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="1ba3e-148">Aby uzyskać więcej informacji, zobacz sekcję [obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="1ba3e-148">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="1ba3e-149">Przegląd ogłaszania i powiązania</span><span class="sxs-lookup"><span data-stu-id="1ba3e-149">Posting and binding review</span></span>

<span data-ttu-id="1ba3e-150">Przejrzyj *stronę/filmy/plik Edit. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="1ba3e-150">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

<span data-ttu-id="1ba3e-151">Gdy żądanie HTTP GET zostanie wysłane do strony filmy/Edycja (na przykład `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="1ba3e-151">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="1ba3e-152">Metoda `OnGetAsync` pobiera film z bazy danych i zwraca metodę `Page`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-152">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span>
* <span data-ttu-id="1ba3e-153">Metoda `Page` renderuje stronę */filmy/edytowanie. cshtml* Razor.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-153">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="1ba3e-154">Plik *Pages/Movies/Edit. cshtml* zawiera dyrektywę model (`@model RazorPagesMovie.Pages.Movies.EditModel`), która sprawia, że model filmu jest dostępny na stronie.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-154">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="1ba3e-155">Zostanie wyświetlony formularz edycji z wartościami z filmu.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-155">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="1ba3e-156">Po opublikowaniu strony filmy/Edycja:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-156">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="1ba3e-157">Wartości formularza na stronie są powiązane z właściwością `Movie`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-157">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="1ba3e-158">Atrybut `[BindProperty]` włącza [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-158">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="1ba3e-159">Jeśli wystąpią błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na datę), formularz jest ponownie wyświetlany z przesłanymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-159">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is redisplayed with the submitted values.</span></span>
* <span data-ttu-id="1ba3e-160">Jeśli nie ma żadnych błędów modelu, film zostanie zapisany.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-160">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="1ba3e-161">Metody GET protokołu HTTP na stronach index, Create i DELETE Razor są zgodne z podobnym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-161">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="1ba3e-162">Metoda `OnPostAsync` POST protokołu HTTP na stronie Tworzenie Razor następuje po podobnym wzorcu do metody `OnPostAsync` na stronie Edytuj Razor.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-162">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ba3e-163">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1ba3e-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ba3e-164">[Poprzedni: Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [Dalej: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="1ba3e-164">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1ba3e-165">Aplikacja ze szkieletem jest dobrze uruchomiona, ale prezentacja nie jest idealnym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-165">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="1ba3e-166">**ReleaseDate** powinna być **datą wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-166">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplikacja filmowa otwarta w przeglądarce Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="1ba3e-168">Aktualizowanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="1ba3e-168">Update the generated code</span></span>

<span data-ttu-id="1ba3e-169">Otwórz plik *models/Movie. cs* i Dodaj wyróżnione wiersze wyświetlane w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-169">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="1ba3e-170">`[Column(TypeName = "decimal(18, 2)")]` adnotacji danych umożliwia Entity Framework Core prawidłowe mapowanie `Price` na walutę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-170">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="1ba3e-171">Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-171">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="1ba3e-172">[Adnotacje DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) zostały omówione w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-172">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="1ba3e-173">Atrybut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="1ba3e-173">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="1ba3e-174">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data), więc informacje o czasie przechowywane w polu nie są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-174">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="1ba3e-175">Przejdź do stron/filmów i umieść kursor na linku **edycji** , aby zobaczyć docelowy adres URL.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-175">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Okno przeglądarki z myszą nad linkiem edycji i adresem URL linku http://localhost:1234/Movies/Edit/5 jest pokazywany](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="1ba3e-177">Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez [pomocnika tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w pliku *Pages/Films/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="1ba3e-177">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="1ba3e-178">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-178">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1ba3e-179">W poprzednim kodzie `AnchorTagHelper` dynamicznie generuje wartość atrybutu `href` HTML ze strony Razor (trasa jest względna), `asp-page`i identyfikator trasy (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-179">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="1ba3e-180">Aby uzyskać więcej informacji, zobacz [generowanie adresów URL na stronach](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="1ba3e-180">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="1ba3e-181">Użyj **widoku źródła** z ulubionej przeglądarki, aby sprawdzić wygenerowane znaczniki.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-181">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="1ba3e-182">Poniżej przedstawiono część wygenerowanego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-182">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="1ba3e-183">Dynamicznie generowane linki przekażą identyfikator filmu z ciągiem zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-183">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="1ba3e-184">Zaktualizuj Razor Pages edycji, szczegółów i usuwania, aby użyć szablonu trasy "{ID: int}".</span><span class="sxs-lookup"><span data-stu-id="1ba3e-184">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="1ba3e-185">Zmień dyrektywę Page dla każdej z tych stron z `@page` na `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-185">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="1ba3e-186">Uruchom aplikację, a następnie Wyświetl źródło.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-186">Run the app and then view source.</span></span> <span data-ttu-id="1ba3e-187">Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-187">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="1ba3e-188">Żądanie do strony z szablonem trasy "{ID: int}", który **nie** zawiera liczby całkowitej, zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-188">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1ba3e-189">Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-189">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="1ba3e-190">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-190">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="1ba3e-191">Aby przetestować zachowanie `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-191">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="1ba3e-192">Ustaw dyrektywę Page na stronie */Films/details. cshtml* na `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-192">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="1ba3e-193">Ustaw punkt przerwania w `public async Task<IActionResult> OnGetAsync(int? id)` (w obszarze *strony/filmy/szczegóły. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-193">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="1ba3e-194">Przejdź do adresu `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-194">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="1ba3e-195">Za pomocą dyrektywy `@page "{id:int}"` punkt przerwania nigdy nie trafi.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-195">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="1ba3e-196">Aparat routingu zwraca protokół HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-196">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="1ba3e-197">Przy użyciu `@page "{id:int?}"`Metoda `OnGetAsync` zwraca `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-197">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="1ba3e-198">Przejrzyj obsługę wyjątków współbieżności</span><span class="sxs-lookup"><span data-stu-id="1ba3e-198">Review concurrency exception handling</span></span>

<span data-ttu-id="1ba3e-199">Przejrzyj metodę `OnPostAsync` w pliku *Pages/Films/Edit. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="1ba3e-199">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="1ba3e-200">Poprzedni kod wykrywa wyjątki współbieżności, gdy jeden klient usuwa film, a pozostałe wpisy wprowadzane przez klienta do filmu.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-200">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="1ba3e-201">Aby przetestować blok `catch`:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-201">To test the `catch` block:</span></span>

* <span data-ttu-id="1ba3e-202">Ustaw punkt przerwania na `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="1ba3e-202">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="1ba3e-203">Wybierz pozycję **Edytuj** dla filmu, wprowadź zmiany, ale nie wprowadzaj opcji **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-203">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="1ba3e-204">W innym oknie przeglądarki wybierz łącze **Usuń** dla tego samego filmu, a następnie usuń film.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-204">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="1ba3e-205">W poprzednim oknie przeglądarki Opublikuj zmiany w filmie.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-205">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="1ba3e-206">Kod produkcyjny może chcieć wykryć konflikty współbieżności.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-206">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="1ba3e-207">Aby uzyskać więcej informacji, zobacz sekcję [obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="1ba3e-207">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="1ba3e-208">Przegląd ogłaszania i powiązania</span><span class="sxs-lookup"><span data-stu-id="1ba3e-208">Posting and binding review</span></span>

<span data-ttu-id="1ba3e-209">Przejrzyj *stronę/filmy/plik Edit. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="1ba3e-209">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="1ba3e-210">Gdy żądanie HTTP GET zostanie wysłane do strony filmy/Edycja (na przykład `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="1ba3e-210">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="1ba3e-211">Metoda `OnGetAsync` pobiera film z bazy danych i zwraca metodę `Page`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-211">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="1ba3e-212">Metoda `Page` renderuje stronę */filmy/edytowanie. cshtml* Razor.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-212">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="1ba3e-213">Plik *Pages/Movies/Edit. cshtml* zawiera dyrektywę model (`@model RazorPagesMovie.Pages.Movies.EditModel`), która sprawia, że model filmu jest dostępny na stronie.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-213">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="1ba3e-214">Zostanie wyświetlony formularz edycji z wartościami z filmu.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-214">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="1ba3e-215">Po opublikowaniu strony filmy/Edycja:</span><span class="sxs-lookup"><span data-stu-id="1ba3e-215">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="1ba3e-216">Wartości formularza na stronie są powiązane z właściwością `Movie`.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-216">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="1ba3e-217">Atrybut `[BindProperty]` włącza [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="1ba3e-217">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="1ba3e-218">Jeśli wystąpią błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na datę), formularz zostanie wyświetlony z przesłanymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-218">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="1ba3e-219">Jeśli nie ma żadnych błędów modelu, film zostanie zapisany.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-219">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="1ba3e-220">Metody GET protokołu HTTP na stronach index, Create i DELETE Razor są zgodne z podobnym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-220">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="1ba3e-221">Metoda `OnPostAsync` POST protokołu HTTP na stronie Tworzenie Razor następuje po podobnym wzorcu do metody `OnPostAsync` na stronie Edytuj Razor.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-221">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="1ba3e-222">W następnym samouczku zostanie dodane Wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="1ba3e-222">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ba3e-223">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1ba3e-223">Additional resources</span></span>

* [<span data-ttu-id="1ba3e-224">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="1ba3e-224">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="1ba3e-225">[Poprzedni: Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [Dalej: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="1ba3e-225">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end
