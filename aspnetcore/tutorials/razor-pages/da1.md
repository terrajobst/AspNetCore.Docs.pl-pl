---
title: Aktualizowanie stron wygenerowane w aplikacji ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zaktualizować wygenerowanych stron w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 62385f33dc86609726305728fbc19dd9ff27dc87
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410197"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="32437-103">Aktualizowanie stron wygenerowane w aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32437-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="32437-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="32437-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="32437-105">Aplikacja filmu szkieletu ma dobry początek, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="32437-105">The scaffolded movie app has a good start, but the presentation isn't ideal.</span></span> <span data-ttu-id="32437-106">**Data wersji** powinien być **Data wydania** (dwa słowa).</span><span class="sxs-lookup"><span data-stu-id="32437-106">**ReleaseDate** should be **Release Date** (two words).</span></span>

![Otwórz w przeglądarce Chrome aplikacji filmów](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="32437-108">Aktualizacja wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="32437-108">Update the generated code</span></span>

<span data-ttu-id="32437-109">Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="32437-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

<span data-ttu-id="32437-110">`[Column(TypeName = "decimal(18, 2)")]` Adnotacji danych umożliwia Entity Framework Core poprawnie mapowane `Price` walutę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="32437-110">The `[Column(TypeName = "decimal(18, 2)")]` data annotation enables Entity Framework Core to correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="32437-111">Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).</span><span class="sxs-lookup"><span data-stu-id="32437-111">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="32437-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) zostało omówione w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="32437-112">[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) is covered in the next tutorial.</span></span> <span data-ttu-id="32437-113">[Wyświetlić](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) atrybut określa, co będzie wyświetlone nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="32437-113">The [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="32437-114">[DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data), co nie jest wyświetlane informacje o godzinie, w tym polu.</span><span class="sxs-lookup"><span data-stu-id="32437-114">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="32437-115">Przejdź do strony/filmów i umieść kursor nad **Edytuj** link, aby wyświetlić docelowy adres URL.</span><span class="sxs-lookup"><span data-stu-id="32437-115">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Okno przeglądarki z myszy nad łącze edycji i łącze adres Url http://localhost:1234/Movies/Edit/5 jest wyświetlany](~/tutorials/razor-pages/da1/edit7.png)

<span data-ttu-id="32437-117">**Edytuj**, **szczegóły**, i **Usuń** łącza są generowane przez [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/filmy / Index.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="32437-117">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="32437-118">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="32437-118">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="32437-119">W poprzednim kodzie `AnchorTagHelper` dynamicznie generuje kod HTML `href` wartość atrybutu ze strony Razor (trasy jest względna) `asp-page`oraz identyfikator trasy (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="32437-119">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="32437-120">Zobacz [Generowanie adresu URL dla stron](xref:razor-pages/index#url-generation-for-pages) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="32437-120">See [URL generation for Pages](xref:razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="32437-121">Użyj **Wyświetl źródło** w ulubionej przeglądarce do sprawdzenia wygenerowany kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="32437-121">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="32437-122">Poniżej przedstawiono część wygenerowanego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="32437-122">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="32437-123">Dynamicznie generowana linki przekazują identyfikator filmu przy użyciu ciągu zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).</span><span class="sxs-lookup"><span data-stu-id="32437-123">The dynamically-generated links pass the movie ID with a query string (for example, the `?id=1` in  `https://localhost:5001/Movies/Details?id=1`).</span></span>

<span data-ttu-id="32437-124">Zaktualizuj, Edytuj, szczegóły i usuwanie stron Razor, aby użyć szablonu trasy "{id: int}".</span><span class="sxs-lookup"><span data-stu-id="32437-124">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="32437-125">Zmiany strony dyrektyw dla każdej z tych stron z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="32437-125">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="32437-126">Uruchom aplikację, a następnie Wyświetl źródło.</span><span class="sxs-lookup"><span data-stu-id="32437-126">Run the app and then view source.</span></span> <span data-ttu-id="32437-127">Wygenerowany kod HTML dodaje identyfikator do część ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="32437-127">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="32437-128">Żądanie strony za pomocą szablonu trasy "{id: int}", który wykonuje **nie** obejmują liczbę całkowitą zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="32437-128">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="32437-129">Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404.</span><span class="sxs-lookup"><span data-stu-id="32437-129">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="32437-130">Aby wprowadzić identyfikator jest opcjonalne, należy dołączyć `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="32437-130">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="32437-131">Aby przetestować działanie `@page "{id:int?}"`:</span><span class="sxs-lookup"><span data-stu-id="32437-131">To test the behavior of `@page "{id:int?}"`:</span></span>

* <span data-ttu-id="32437-132">Ustawianie strony dyrektyw w *Pages/Movies/Details.cshtml* do `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="32437-132">Set the page directive in *Pages/Movies/Details.cshtml* to `@page "{id:int?}"`.</span></span>
* <span data-ttu-id="32437-133">Ustaw punkt przerwania w `public async Task<IActionResult> OnGetAsync(int? id)` (w *Pages/Movies/Details.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="32437-133">Set a break point in `public async Task<IActionResult> OnGetAsync(int? id)` (in *Pages/Movies/Details.cshtml.cs*).</span></span>
* <span data-ttu-id="32437-134">Przejdź do adresu `https://localhost:5001/Movies/Details/`.</span><span class="sxs-lookup"><span data-stu-id="32437-134">Navigate to `https://localhost:5001/Movies/Details/`.</span></span>

<span data-ttu-id="32437-135">Za pomocą `@page "{id:int}"` dyrektywy, punkt przerwania zostanie nigdy osiągnięty.</span><span class="sxs-lookup"><span data-stu-id="32437-135">With the `@page "{id:int}"` directive, the break point is never hit.</span></span> <span data-ttu-id="32437-136">Aparat routingu zwraca HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="32437-136">The routing engine returns HTTP 404.</span></span> <span data-ttu-id="32437-137">Za pomocą `@page "{id:int?}"`, `OnGetAsync` metoda zwraca `NotFound` (HTTP 404).</span><span class="sxs-lookup"><span data-stu-id="32437-137">Using `@page "{id:int?}"`, the `OnGetAsync` method returns `NotFound` (HTTP 404).</span></span>

<span data-ttu-id="32437-138">Chociaż nie jest to zalecane, można napisać `OnGetAsync` — metoda (w *Pages/Movies/Delete.cshtml.cs*) jako:</span><span class="sxs-lookup"><span data-stu-id="32437-138">Although not recommended, you could write the `OnGetAsync` method (in *Pages/Movies/Delete.cshtml.cs*) as:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

<span data-ttu-id="32437-139">Przetestuj poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="32437-139">Test the preceding code:</span></span>

* <span data-ttu-id="32437-140">Wybierz **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="32437-140">Select a **Delete** link.</span></span>
* <span data-ttu-id="32437-141">Usuń identyfikator z adresu URL.</span><span class="sxs-lookup"><span data-stu-id="32437-141">Remove the ID from the URL.</span></span> <span data-ttu-id="32437-142">Na przykład zmienić `https://localhost:5001/Movies/Delete/8` do `https://localhost:5001/Movies/Delete`.</span><span class="sxs-lookup"><span data-stu-id="32437-142">For example, change `https://localhost:5001/Movies/Delete/8` to `https://localhost:5001/Movies/Delete`.</span></span>
* <span data-ttu-id="32437-143">Przejść przez kod w debugerze.</span><span class="sxs-lookup"><span data-stu-id="32437-143">Step through the code in the debugger.</span></span>

### <a name="review-concurrency-exception-handling"></a><span data-ttu-id="32437-144">Przejrzyj Obsługa wyjątku współbieżności</span><span class="sxs-lookup"><span data-stu-id="32437-144">Review concurrency exception handling</span></span>

<span data-ttu-id="32437-145">Przegląd `OnPostAsync` method in Class metoda *Pages/Movies/Edit.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="32437-145">Review the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="32437-146">Powyższy kod wykrywa wyjątki współbieżności jeden klient usunie film i inny klient wysyła zmiany do filmu.</span><span class="sxs-lookup"><span data-stu-id="32437-146">The previous code detects concurrency exceptions when the one client deletes the movie and the other client posts changes to the movie.</span></span>

<span data-ttu-id="32437-147">Aby przetestować `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="32437-147">To test the `catch` block:</span></span>

* <span data-ttu-id="32437-148">Ustawianie punktu przerwania `catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="32437-148">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="32437-149">Wybierz **Edytuj** filmu, wprowadzić zmiany, ale nie wprowadzaj **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="32437-149">Select **Edit** for a movie, make changes, but don't enter **Save**.</span></span>
* <span data-ttu-id="32437-150">W innym oknie przeglądarki, zaznacz **Usuń** łączy dla tego samego filmu, a następnie usuń ten film.</span><span class="sxs-lookup"><span data-stu-id="32437-150">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="32437-151">W poprzednim oknie przeglądarki Opublikuj zmiany do filmu.</span><span class="sxs-lookup"><span data-stu-id="32437-151">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="32437-152">Kod w środowisku produkcyjnym warto wykrywanie konfliktów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="32437-152">Production code may want to detect concurrency conflicts.</span></span> <span data-ttu-id="32437-153">Zobacz [Obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="32437-153">See [Handle concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="32437-154">Umieszczanie i powiązanie przeglądu</span><span class="sxs-lookup"><span data-stu-id="32437-154">Posting and binding review</span></span>

<span data-ttu-id="32437-155">Sprawdź *Pages/Movies/Edit.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="32437-155">Examine the *Pages/Movies/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

<span data-ttu-id="32437-156">Nawiązaniem żądanie HTTP GET do strony filmy oraz pozwala edytować (na przykład `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="32437-156">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="32437-157">`OnGetAsync` Metoda pobiera film z bazy danych i zwraca `Page` metody.</span><span class="sxs-lookup"><span data-stu-id="32437-157">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="32437-158">`Page` Metoda renderuje *Pages/Movies/Edit.cshtml* strona Razor.</span><span class="sxs-lookup"><span data-stu-id="32437-158">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="32437-159">*Pages/Movies/Edit.cshtml* plik zawiera dyrektywy model (`@model RazorPagesMovie.Pages.Movies.EditModel`), co sprawia, że model film dostępny na stronie.</span><span class="sxs-lookup"><span data-stu-id="32437-159">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="32437-160">Z wartościami z film zostanie wyświetlony formularz edycji.</span><span class="sxs-lookup"><span data-stu-id="32437-160">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="32437-161">Po opublikowaniu strony filmy oraz pozwala edytować:</span><span class="sxs-lookup"><span data-stu-id="32437-161">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="32437-162">Wartości formularza na tej stronie są powiązane z `Movie` właściwości.</span><span class="sxs-lookup"><span data-stu-id="32437-162">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="32437-163">`[BindProperty]` Atrybut umożliwia [wiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="32437-163">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="32437-164">Jeśli występują błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na wartość typu date), zostanie wyświetlony formularz wartościami przesłane.</span><span class="sxs-lookup"><span data-stu-id="32437-164">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is displayed with the submitted values.</span></span>
* <span data-ttu-id="32437-165">Jeśli nie ma żadnych błędów modelu, film zostanie zapisana.</span><span class="sxs-lookup"><span data-stu-id="32437-165">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="32437-166">Metod HTTP GET, na stronach indeksu, tworzenie i usuwanie Razor wykonaj podobny wzorzec.</span><span class="sxs-lookup"><span data-stu-id="32437-166">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="32437-167">HTTP POST `OnPostAsync` metody w tworzenie strony Razor ze wzorcem podobne do `OnPostAsync` metody w edytowanie strony Razor.</span><span class="sxs-lookup"><span data-stu-id="32437-167">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="32437-168">Wyszukiwanie zostanie dodany do następnego samouczka.</span><span class="sxs-lookup"><span data-stu-id="32437-168">Search is added in the next tutorial.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="32437-169">[Poprzednie: Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [dalej: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="32437-169">[Previous: Working with a database](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
