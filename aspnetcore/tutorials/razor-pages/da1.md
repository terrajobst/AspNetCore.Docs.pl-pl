---
title: Aktualizowanie wygenerowanych stron w aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować wygenerowane strony w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f1f69b7facf584d46248405c808e75bdd8448d2b
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440323"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="3bddd-103">Aktualizowanie wygenerowanych stron w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3bddd-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="3bddd-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3bddd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3bddd-105">Aplikacja ze szkieletem jest dobrze uruchomiona, ale prezentacja nie jest idealnym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="3bddd-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="3bddd-106">**ReleaseDate** powinna być **datą wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="3bddd-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplikacja filmowa otwarta w przeglądarce Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="3bddd-108">Aktualizowanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="3bddd-108">Update the generated code</span></span>

<span data-ttu-id="3bddd-109">Otwórz plik *models/Movie. cs* i Dodaj wyróżnione wiersze wyświetlane w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="3bddd-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="3bddd-110">Adnotacja `Price` danych umożliwia Entity Framework Core poprawne mapowanie na walutę w bazie danych. `[Column(TypeName = "decimal(18, 2)")]`</span><span class="sxs-lookup"><span data-stu-id="3bddd-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="3bddd-111">Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="3bddd-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="3bddd-112">[Adnotacje](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) DataAnnotations zostały omówione w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="3bddd-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="3bddd-113">Atrybut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="3bddd-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="3bddd-114">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data), więc informacje o czasie przechowywane w polu nie są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="3bddd-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="3bddd-115">Przejdź do stron/filmów i umieść kursor na linku **edycji** , aby zobaczyć docelowy adres URL.</span><span class="sxs-lookup"><span data-stu-id="3bddd-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Okno przeglądarki z myszą nad linkiem edycji i pokazanym http://localhost:1234/Movies/Edit/5 adresem URL linku](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="3bddd-117">Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez [pomocnika tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w pliku Pages */Films/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="3bddd-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="3bddd-118">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="3bddd-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="3bddd-119">W `AnchorTagHelper` poprzednim kodzie, dynamicznie generuje wartość atrybutu HTML `href` ze strony Razor (trasa `asp-page`jest względna), i identyfikator trasy (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="3bddd-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="3bddd-120">Aby uzyskać więcej informacji, zobacz [generowanie adresów URL na stronach](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="3bddd-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="3bddd-121">Użyj **widoku źródła** z ulubionej przeglądarki, aby sprawdzić wygenerowane znaczniki.</span><span class="sxs-lookup"><span data-stu-id="3bddd-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="3bddd-122">Poniżej przedstawiono część wygenerowanego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="3bddd-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="3bddd-123">Dynamicznie generowane linki przekażą identyfikator filmu z ciągiem zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="3bddd-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="3bddd-124">Zaktualizuj Razor Pages edycji, szczegółów i usuwania, aby użyć szablonu trasy "{ID: int}".</span><span class="sxs-lookup"><span data-stu-id="3bddd-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="3bddd-125">Zmień dyrektywę Page dla każdej z tych stron z `@page` na. `@page "{id:int}"`</span><span class="sxs-lookup"><span data-stu-id="3bddd-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="3bddd-126">Uruchom aplikację, a następnie Wyświetl źródło.</span><span class="sxs-lookup"><span data-stu-id="3bddd-126">Run the app and then view source.</span></span> <span data-ttu-id="3bddd-127">Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="3bddd-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="3bddd-128">Żądanie do strony z szablonem trasy "{ID: int}", który **nie** zawiera liczby całkowitej, zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="3bddd-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3bddd-129">Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404.</span><span class="sxs-lookup"><span data-stu-id="3bddd-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="3bddd-130">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="3bddd-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3bddd-131">Aby przetestować zachowanie `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="3bddd-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="3bddd-132">Ustaw dyrektywę Page na stronie */filmy/details. cshtml* na `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="3bddd-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="3bddd-133">Ustaw punkt `public async Task<IActionResult> OnGetAsync(int? id)` przerwania (w obszarze *strony/filmy/szczegóły. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="3bddd-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="3bddd-134">Przejdź do adresu `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="3bddd-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="3bddd-135">W przypadku `@page "{id:int}"` dyrektywy punkt przerwania nigdy nie trafi.</span><span class="sxs-lookup"><span data-stu-id="3bddd-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="3bddd-136">Aparat routingu zwraca protokół HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="3bddd-136">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="3bddd-137">Przy użyciu `@page "{id:int?}"` Metodazwraca`NotFound` (HTTP 404). `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="3bddd-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="3bddd-138">Przejrzyj obsługę wyjątków współbieżności</span><span class="sxs-lookup"><span data-stu-id="3bddd-138">Review concurrency exception handling</span></span>

<span data-ttu-id="3bddd-139">Przejrzyj metodę w pliku *Pages/Films/Edit. cshtml. cs:* `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="3bddd-139">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="3bddd-140">Poprzedni kod wykrywa wyjątki współbieżności, gdy jeden klient usuwa film, a pozostałe wpisy wprowadzane przez klienta do filmu.</span><span class="sxs-lookup"><span data-stu-id="3bddd-140">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="3bddd-141">Aby przetestować `catch` blok:</span><span class="sxs-lookup"><span data-stu-id="3bddd-141">To test the `catch` block:</span></span>

* <span data-ttu-id="3bddd-142">Ustaw punkt przerwania na`catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="3bddd-142">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="3bddd-143">Wybierz pozycję **Edytuj** dla filmu, wprowadź zmiany, ale nie wprowadzaj opcji **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="3bddd-143">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="3bddd-144">W innym oknie przeglądarki wybierz łącze **Usuń** dla tego samego filmu, a następnie usuń film.</span><span class="sxs-lookup"><span data-stu-id="3bddd-144">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="3bddd-145">W poprzednim oknie przeglądarki Opublikuj zmiany w filmie.</span><span class="sxs-lookup"><span data-stu-id="3bddd-145">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="3bddd-146">Kod produkcyjny może chcieć wykryć konflikty współbieżności.</span><span class="sxs-lookup"><span data-stu-id="3bddd-146">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="3bddd-147">Aby uzyskać więcej informacji, zobacz sekcję [obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="3bddd-147">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="3bddd-148">Przegląd ogłaszania i powiązania</span><span class="sxs-lookup"><span data-stu-id="3bddd-148">Posting and binding review</span></span>

<span data-ttu-id="3bddd-149">Przejrzyj *stronę/filmy/plik Edit. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="3bddd-149">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

<span data-ttu-id="3bddd-150">Gdy żądanie HTTP GET zostanie wysłane do strony filmy/Edycja (na przykład `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="3bddd-150">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="3bddd-151">Metoda pobiera film z bazy danych i `Page` zwraca metodę. `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="3bddd-151">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span>
* <span data-ttu-id="3bddd-152">Metoda renderuje stronę */filmy/edytowanie. cshtml.* `Page`</span><span class="sxs-lookup"><span data-stu-id="3bddd-152">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="3bddd-153">Plik *Pages/Movies/Edit. cshtml* zawiera dyrektywę model (`@model RazorPagesMovie.Pages.Movies.EditModel`), która sprawia, że model filmu jest dostępny na stronie.</span><span class="sxs-lookup"><span data-stu-id="3bddd-153">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="3bddd-154">Zostanie wyświetlony formularz edycji z wartościami z filmu.</span><span class="sxs-lookup"><span data-stu-id="3bddd-154">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="3bddd-155">Po opublikowaniu strony filmy/Edycja:</span><span class="sxs-lookup"><span data-stu-id="3bddd-155">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="3bddd-156">Wartości formularza na stronie są powiązane z `Movie` właściwością.</span><span class="sxs-lookup"><span data-stu-id="3bddd-156">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="3bddd-157">Ten `[BindProperty]` atrybut włącza [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="3bddd-157">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="3bddd-158">Jeśli występują błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na datę), formularz jest ponownie wyświetlany z przesłanymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="3bddd-158">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is redisplayed with the submitted values.</span></span>
* <span data-ttu-id="3bddd-159">Jeśli nie ma żadnych błędów modelu, film zostanie zapisany.</span><span class="sxs-lookup"><span data-stu-id="3bddd-159">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="3bddd-160">Metody GET protokołu HTTP na stronach index, Create i DELETE Razor są zgodne z podobnym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="3bddd-160">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="3bddd-161">Metoda post `OnPostAsync` protokołu HTTP na stronie Tworzenie Razor podąża za podobnym wzorcem `OnPostAsync` metody na stronie Edytuj Razor.</span><span class="sxs-lookup"><span data-stu-id="3bddd-161">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bddd-162">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3bddd-162">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3bddd-163">[Ubiegł Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [dalej: Dodaj wyszukiwanie](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="3bddd-163">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3bddd-164">Aplikacja ze szkieletem jest dobrze uruchomiona, ale prezentacja nie jest idealnym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="3bddd-164">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="3bddd-165">**ReleaseDate** powinna być **datą wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="3bddd-165">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Aplikacja filmowa otwarta w przeglądarce Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="3bddd-167">Aktualizowanie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="3bddd-167">Update the generated code</span></span>

<span data-ttu-id="3bddd-168">Otwórz plik *models/Movie. cs* i Dodaj wyróżnione wiersze wyświetlane w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="3bddd-168">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

<span data-ttu-id="3bddd-169">Adnotacja `Price` danych umożliwia Entity Framework Core poprawne mapowanie na walutę w bazie danych. `[Column(TypeName = "decimal(18, 2)")]`</span><span class="sxs-lookup"><span data-stu-id="3bddd-169">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="3bddd-170">Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="3bddd-170">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="3bddd-171">[Adnotacje](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) DataAnnotations zostały omówione w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="3bddd-171">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="3bddd-172">Atrybut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="3bddd-172">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="3bddd-173">Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data), więc informacje o czasie przechowywane w polu nie są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="3bddd-173">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="3bddd-174">Przejdź do stron/filmów i umieść kursor na linku **edycji** , aby zobaczyć docelowy adres URL.</span><span class="sxs-lookup"><span data-stu-id="3bddd-174">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Okno przeglądarki z myszą nad linkiem edycji i pokazanym http://localhost:1234/Movies/Edit/5 adresem URL linku](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="3bddd-176">Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez [pomocnika tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w pliku Pages */Films/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="3bddd-176">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="3bddd-177">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="3bddd-177">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="3bddd-178">W `AnchorTagHelper` poprzednim kodzie, dynamicznie generuje wartość atrybutu HTML `href` ze strony Razor (trasa `asp-page`jest względna), i identyfikator trasy (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="3bddd-178">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="3bddd-179">Aby uzyskać więcej informacji, zobacz [generowanie adresów URL na stronach](xref:razor-pages/index#url-generation-for-pages) .</span><span class="sxs-lookup"><span data-stu-id="3bddd-179">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="3bddd-180">Użyj **widoku źródła** z ulubionej przeglądarki, aby sprawdzić wygenerowane znaczniki.</span><span class="sxs-lookup"><span data-stu-id="3bddd-180">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="3bddd-181">Poniżej przedstawiono część wygenerowanego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="3bddd-181">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="3bddd-182">Dynamicznie generowane linki przekażą identyfikator filmu z ciągiem zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="3bddd-182">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="3bddd-183">Zaktualizuj Razor Pages edycji, szczegółów i usuwania, aby użyć szablonu trasy "{ID: int}".</span><span class="sxs-lookup"><span data-stu-id="3bddd-183">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="3bddd-184">Zmień dyrektywę Page dla każdej z tych stron z `@page` na. `@page "{id:int}"`</span><span class="sxs-lookup"><span data-stu-id="3bddd-184">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="3bddd-185">Uruchom aplikację, a następnie Wyświetl źródło.</span><span class="sxs-lookup"><span data-stu-id="3bddd-185">Run the app and then view source.</span></span> <span data-ttu-id="3bddd-186">Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="3bddd-186">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="3bddd-187">Żądanie do strony z szablonem trasy "{ID: int}", który **nie** zawiera liczby całkowitej, zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="3bddd-187">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3bddd-188">Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404.</span><span class="sxs-lookup"><span data-stu-id="3bddd-188">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="3bddd-189">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="3bddd-189">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3bddd-190">Aby przetestować zachowanie `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="3bddd-190">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="3bddd-191">Ustaw dyrektywę Page na stronie */filmy/details. cshtml* na `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="3bddd-191">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="3bddd-192">Ustaw punkt `public async Task<IActionResult> OnGetAsync(int? id)` przerwania (w obszarze *strony/filmy/szczegóły. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="3bddd-192">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="3bddd-193">Przejdź do adresu `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="3bddd-193">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="3bddd-194">W przypadku `@page "{id:int}"` dyrektywy punkt przerwania nigdy nie trafi.</span><span class="sxs-lookup"><span data-stu-id="3bddd-194">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="3bddd-195">Aparat routingu zwraca protokół HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="3bddd-195">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="3bddd-196">Przy użyciu `@page "{id:int?}"` Metodazwraca`NotFound` (HTTP 404). `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="3bddd-196">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="3bddd-197">Przejrzyj obsługę wyjątków współbieżności</span><span class="sxs-lookup"><span data-stu-id="3bddd-197">Review concurrency exception handling</span></span>

<span data-ttu-id="3bddd-198">Przejrzyj metodę w pliku *Pages/Films/Edit. cshtml. cs:* `OnPostAsync`</span><span class="sxs-lookup"><span data-stu-id="3bddd-198">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="3bddd-199">Poprzedni kod wykrywa wyjątki współbieżności, gdy jeden klient usuwa film, a pozostałe wpisy wprowadzane przez klienta do filmu.</span><span class="sxs-lookup"><span data-stu-id="3bddd-199">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="3bddd-200">Aby przetestować `catch` blok:</span><span class="sxs-lookup"><span data-stu-id="3bddd-200">To test the `catch` block:</span></span>

* <span data-ttu-id="3bddd-201">Ustaw punkt przerwania na`catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="3bddd-201">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="3bddd-202">Wybierz pozycję **Edytuj** dla filmu, wprowadź zmiany, ale nie wprowadzaj opcji **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="3bddd-202">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="3bddd-203">W innym oknie przeglądarki wybierz łącze **Usuń** dla tego samego filmu, a następnie usuń film.</span><span class="sxs-lookup"><span data-stu-id="3bddd-203">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="3bddd-204">W poprzednim oknie przeglądarki Opublikuj zmiany w filmie.</span><span class="sxs-lookup"><span data-stu-id="3bddd-204">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="3bddd-205">Kod produkcyjny może chcieć wykryć konflikty współbieżności.</span><span class="sxs-lookup"><span data-stu-id="3bddd-205">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="3bddd-206">Aby uzyskać więcej informacji, zobacz sekcję [obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="3bddd-206">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="3bddd-207">Przegląd ogłaszania i powiązania</span><span class="sxs-lookup"><span data-stu-id="3bddd-207">Posting and binding review</span></span>

<span data-ttu-id="3bddd-208">Przejrzyj *stronę/filmy/plik Edit. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="3bddd-208">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="3bddd-209">Gdy żądanie HTTP GET zostanie wysłane do strony filmy/Edycja (na przykład `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="3bddd-209">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="3bddd-210">Metoda pobiera film z bazy danych i `Page` zwraca metodę. `OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="3bddd-210">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="3bddd-211">Metoda renderuje stronę */filmy/edytowanie. cshtml.* `Page`</span><span class="sxs-lookup"><span data-stu-id="3bddd-211">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="3bddd-212">Plik *Pages/Movies/Edit. cshtml* zawiera dyrektywę model (`@model RazorPagesMovie.Pages.Movies.EditModel`), która sprawia, że model filmu jest dostępny na stronie.</span><span class="sxs-lookup"><span data-stu-id="3bddd-212">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="3bddd-213">Zostanie wyświetlony formularz edycji z wartościami z filmu.</span><span class="sxs-lookup"><span data-stu-id="3bddd-213">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="3bddd-214">Po opublikowaniu strony filmy/Edycja:</span><span class="sxs-lookup"><span data-stu-id="3bddd-214">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="3bddd-215">Wartości formularza na stronie są powiązane z `Movie` właściwością.</span><span class="sxs-lookup"><span data-stu-id="3bddd-215">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="3bddd-216">Ten `[BindProperty]` atrybut włącza [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="3bddd-216">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="3bddd-217">Jeśli występują błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na datę), formularz zostanie wyświetlony z przesłanymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="3bddd-217">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="3bddd-218">Jeśli nie ma żadnych błędów modelu, film zostanie zapisany.</span><span class="sxs-lookup"><span data-stu-id="3bddd-218">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="3bddd-219">Metody GET protokołu HTTP na stronach index, Create i DELETE Razor są zgodne z podobnym wzorcem.</span><span class="sxs-lookup"><span data-stu-id="3bddd-219">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="3bddd-220">Metoda post `OnPostAsync` protokołu HTTP na stronie Tworzenie Razor podąża za podobnym wzorcem `OnPostAsync` metody na stronie Edytuj Razor.</span><span class="sxs-lookup"><span data-stu-id="3bddd-220">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="3bddd-221">W następnym samouczku zostanie dodane Wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="3bddd-221">Search is added in the next tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bddd-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3bddd-222">Additional resources</span></span>

* [<span data-ttu-id="3bddd-223">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="3bddd-223">YouTube version of this tutorial</span></span>](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> <span data-ttu-id="3bddd-224">[Ubiegł Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [dalej: Dodaj wyszukiwanie](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="3bddd-224">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>

::: moniker-end
