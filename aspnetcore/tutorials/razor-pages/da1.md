---
title: Aktualizowanie wygenerowanego stron
author: rick-anderson
description: "Uaktualnianie stron wygenerowanych lepsze wyświetlanie."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: abf6839536150f29eaa2d07dafbe0d0c1a08e280
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="updating-the-generated-pages"></a><span data-ttu-id="1800e-103">Aktualizowanie wygenerowanego stron</span><span class="sxs-lookup"><span data-stu-id="1800e-103">Updating the generated pages</span></span>

<span data-ttu-id="1800e-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1800e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1800e-105">Mamy dobry początek aplikacji film, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="1800e-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="1800e-106">Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).</span><span class="sxs-lookup"><span data-stu-id="1800e-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="1800e-108">Aktualizacja wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="1800e-108">Update the generated code</span></span>

<span data-ttu-id="1800e-109">Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:</span><span class="sxs-lookup"><span data-stu-id="1800e-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="1800e-110">Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red > ** Szybkie akcje i Refaktoryzacje **.</span><span class="sxs-lookup"><span data-stu-id="1800e-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](da1/qa.png)

<span data-ttu-id="1800e-112">Wybierz`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="1800e-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  <span data-ttu-id="1800e-114">Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="1800e-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="1800e-115">Omówione zostaną następujące czynności [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="1800e-115">We'll cover [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) in the next tutorial.</span></span> <span data-ttu-id="1800e-116">[Wyświetlić](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) Określa atrybut, co ma być wyświetlany dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate").</span><span class="sxs-lookup"><span data-stu-id="1800e-116">The [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="1800e-117">[DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data), co nie jest wyświetlane w polu informacje o godzinie.</span><span class="sxs-lookup"><span data-stu-id="1800e-117">The [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date), so the time information stored in the field isn't displayed.</span></span>

<span data-ttu-id="1800e-118">Przejdź do strony/filmy i umieść kursor nad **Edytuj** łącze, aby wyświetlić docelowy adres URL.</span><span class="sxs-lookup"><span data-stu-id="1800e-118">Browse to Pages/Movies and  hover over an **Edit** link to see the target URL.</span></span>

![Okna przeglądarki z myszą łącza edycji i link jest wyświetlany adres Url http://localhost:1234/filmów/Edit/5](da1/edit7.png)

<span data-ttu-id="1800e-120">**Edytuj**, **szczegóły**, i **usunąć** łącza są generowane przez [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/filmów / Index.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="1800e-120">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Movies/Index.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

<span data-ttu-id="1800e-121">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="1800e-121">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1800e-122">W powyższym kodzie `AnchorTagHelper` dynamicznie generuje kod HTML `href` wartość atrybutu na stronie aparatu Razor (trasy jest względna), `asp-page`i identyfikator marszruty (`asp-route-id`).</span><span class="sxs-lookup"><span data-stu-id="1800e-122">In the preceding code, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the Razor Page (the route is relative), the `asp-page`,  and the route id (`asp-route-id`).</span></span> <span data-ttu-id="1800e-123">Zobacz [generowania adresu URL dla stron](xref:mvc/razor-pages/index#url-generation-for-pages) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1800e-123">See [URL generation for Pages](xref:mvc/razor-pages/index#url-generation-for-pages) for more information.</span></span>

<span data-ttu-id="1800e-124">Użyj **Wyświetl źródło** z ulubionej przeglądarce zbadanie wygenerowanego kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="1800e-124">Use **View Source** from your favorite browser to examine the generated markup.</span></span> <span data-ttu-id="1800e-125">Poniżej przedstawiono fragment wygenerowanego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="1800e-125">A portion of the generated HTML is shown below:</span></span>

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

<span data-ttu-id="1800e-126">Generowane dynamicznie linki przekazują identyfikator film z ciągu zapytania (na przykład `http://localhost:5000/Movies/Details?id=2` ).</span><span class="sxs-lookup"><span data-stu-id="1800e-126">The dynamically-generated links pass the movie ID with a query string (for example, `http://localhost:5000/Movies/Details?id=2` ).</span></span> 

<span data-ttu-id="1800e-127">Zaktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć szablonu trasy "{identyfikator: int}".</span><span class="sxs-lookup"><span data-stu-id="1800e-127">Update the Edit, Details, and Delete Razor Pages to use the "{id:int}" route template.</span></span> <span data-ttu-id="1800e-128">Zmiany w dyrektywie page dla każdego z tych stron z `@page` do `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="1800e-128">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span> <span data-ttu-id="1800e-129">Uruchom aplikację, a następnie Wyświetl źródło.</span><span class="sxs-lookup"><span data-stu-id="1800e-129">Run the app and then view source.</span></span> <span data-ttu-id="1800e-130">Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:</span><span class="sxs-lookup"><span data-stu-id="1800e-130">The generated HTML adds the ID to the path portion of the URL:</span></span>

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

<span data-ttu-id="1800e-131">Żądanie do strony przy użyciu szablonu trasy "{identyfikator: int}", który wykonuje **nie** obejmują liczbę całkowitą zwróci błąd 404 protokołu HTTP (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="1800e-131">A request to the page with the "{id:int}" route template that does **not** include the integer will return an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1800e-132">Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404.</span><span class="sxs-lookup"><span data-stu-id="1800e-132">For example, `http://localhost:5000/Movies/Details` will return a 404 error.</span></span> <span data-ttu-id="1800e-133">Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="1800e-133">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a><span data-ttu-id="1800e-134">Obsługa wyjątków współbieżności aktualizacji</span><span class="sxs-lookup"><span data-stu-id="1800e-134">Update concurrency exception handling</span></span>

<span data-ttu-id="1800e-135">Aktualizacja `OnPostAsync` metody w *Pages/Movies/Edit.cshtml.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="1800e-135">Update the `OnPostAsync` method in the *Pages/Movies/Edit.cshtml.cs* file.</span></span> <span data-ttu-id="1800e-136">Następujący wyróżniony kod przedstawia zmiany:</span><span class="sxs-lookup"><span data-stu-id="1800e-136">The following highlighted code shows the changes:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

<span data-ttu-id="1800e-137">Poprzedni kod wykrywa wyjątków współbieżności tylko, gdy pierwszy klient równoczesnych usuwa filmu i drugi klient równoczesnych zapisuje zmiany do filmu.</span><span class="sxs-lookup"><span data-stu-id="1800e-137">The previous code only detects concurrency exceptions when the first concurrent client deletes the movie, and the second concurrent client posts changes to the movie.</span></span>

<span data-ttu-id="1800e-138">Aby przetestować `catch` bloku:</span><span class="sxs-lookup"><span data-stu-id="1800e-138">To test the `catch` block:</span></span>

* <span data-ttu-id="1800e-139">Ustaw punkt przerwania w`catch (DbUpdateConcurrencyException)`</span><span class="sxs-lookup"><span data-stu-id="1800e-139">Set a breakpoint on `catch (DbUpdateConcurrencyException)`</span></span>
* <span data-ttu-id="1800e-140">Edytowanie filmu.</span><span class="sxs-lookup"><span data-stu-id="1800e-140">Edit a movie.</span></span>
* <span data-ttu-id="1800e-141">W innym oknie przeglądarki, wybierz **usunąć** łączy dla tego samego film, a następnie usuń filmu.</span><span class="sxs-lookup"><span data-stu-id="1800e-141">In another browser window, select the **Delete** link for the same movie, and then delete the movie.</span></span>
* <span data-ttu-id="1800e-142">W oknie przeglądarki poprzedniej Opublikuj zmiany do filmu.</span><span class="sxs-lookup"><span data-stu-id="1800e-142">In the previous browser window, post changes to the movie.</span></span>

<span data-ttu-id="1800e-143">Kodzie produkcyjnym zwykle może wykryć konfliktom współbieżności, gdy dwie lub więcej klientów jednocześnie zaktualizować rekord.</span><span class="sxs-lookup"><span data-stu-id="1800e-143">Production code would generally detect concurrency conflicts when two or more clients concurrently updated a record.</span></span> <span data-ttu-id="1800e-144">Zobacz [konfliktów współbieżności](xref:data/ef-rp/concurrency) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1800e-144">See [Handling concurrency conflicts](xref:data/ef-rp/concurrency) for more information.</span></span>

### <a name="posting-and-binding-review"></a><span data-ttu-id="1800e-145">Zamieszczając i powiązanie przeglądu</span><span class="sxs-lookup"><span data-stu-id="1800e-145">Posting and binding review</span></span>

<span data-ttu-id="1800e-146">Sprawdź *Pages/Movies/Edit.cshtml.cs* pliku:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="1800e-146">Examine the *Pages/Movies/Edit.cshtml.cs* file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]</span></span>

<span data-ttu-id="1800e-147">Nawiązaniem żądanie HTTP GET do strony filmów oraz edytować (na przykład `http://localhost:5000/Movies/Edit/2`):</span><span class="sxs-lookup"><span data-stu-id="1800e-147">When an HTTP GET request is made to the Movies/Edit page (for example, `http://localhost:5000/Movies/Edit/2`):</span></span>

* <span data-ttu-id="1800e-148">`OnGetAsync` Metoda pobiera film z bazy danych i zwraca `Page` metody.</span><span class="sxs-lookup"><span data-stu-id="1800e-148">The `OnGetAsync` method fetches the movie from the database and returns the `Page` method.</span></span> 
* <span data-ttu-id="1800e-149">`Page` Metoda renderuje *Pages/Movies/Edit.cshtml* Razor strony.</span><span class="sxs-lookup"><span data-stu-id="1800e-149">The `Page` method renders the *Pages/Movies/Edit.cshtml* Razor Page.</span></span> <span data-ttu-id="1800e-150">*Pages/Movies/Edit.cshtml* plik zawiera dyrektywa modelu (`@model RazorPagesMovie.Pages.Movies.EditModel`), który udostępnia model filmu na stronie.</span><span class="sxs-lookup"><span data-stu-id="1800e-150">The *Pages/Movies/Edit.cshtml* file contains the model directive (`@model RazorPagesMovie.Pages.Movies.EditModel`), which makes the movie model available on the page.</span></span>
* <span data-ttu-id="1800e-151">Wartościami z filmu zostanie wyświetlony formularz edycji.</span><span class="sxs-lookup"><span data-stu-id="1800e-151">The Edit form is displayed with the values from the movie.</span></span>

<span data-ttu-id="1800e-152">Gdy filmów oraz edytować strona jest przesyłana:</span><span class="sxs-lookup"><span data-stu-id="1800e-152">When the Movies/Edit page is posted:</span></span>

* <span data-ttu-id="1800e-153">Na stronie wartości formularza jest powiązana z `Movie` właściwości.</span><span class="sxs-lookup"><span data-stu-id="1800e-153">The form values on the page are bound to the `Movie` property.</span></span> <span data-ttu-id="1800e-154">`[BindProperty]` Atrybutu umożliwia [modelu powiązania](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="1800e-154">The `[BindProperty]` attribute enables [Model binding](xref:mvc/models/model-binding).</span></span>

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* <span data-ttu-id="1800e-155">Jeśli wystąpią błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na typ date), ponownie opublikowania formularza z wartościami zostało przesłane.</span><span class="sxs-lookup"><span data-stu-id="1800e-155">If there are errors in the model state (for example, `ReleaseDate` cannot be converted to a date), the form is posted again with the submitted values.</span></span>
* <span data-ttu-id="1800e-156">Jeśli nie ma żadnych błędów w modelu, zapisać.</span><span class="sxs-lookup"><span data-stu-id="1800e-156">If there are no model errors, the movie is saved.</span></span>

<span data-ttu-id="1800e-157">Metody HTTP GET, na stronach indeksu, tworzenie i usuwanie Razor wykonaj podobnego wzorca.</span><span class="sxs-lookup"><span data-stu-id="1800e-157">The HTTP GET methods in the Index, Create, and Delete Razor pages follow a similar pattern.</span></span> <span data-ttu-id="1800e-158">HTTP POST `OnPostAsync` metody na stronie Utwórz Razor następuje podobnego wzorca do `OnPostAsync` metody na stronie edycji Razor.</span><span class="sxs-lookup"><span data-stu-id="1800e-158">The HTTP POST `OnPostAsync` method in the Create Razor Page follows a similar pattern to the `OnPostAsync` method in the Edit Razor Page.</span></span>

<span data-ttu-id="1800e-159">Wyszukiwania został dodany w następnym samouczku.</span><span class="sxs-lookup"><span data-stu-id="1800e-159">Search is added in the next tutorial.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1800e-160">[Poprzedni: Praca z bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)
[Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="1800e-160">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
