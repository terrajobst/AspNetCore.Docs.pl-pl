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
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualizowanie stron wygenerowane w aplikacji ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Aplikacja filmu szkieletu ma dobry początek, ale prezentacji nie jest najlepszym rozwiązaniem. **Data wersji** powinien być **Data wydania** (dwa słowa).

![Otwórz w przeglądarce Chrome aplikacji filmów](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizacja wygenerowanego kodu

Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze pokazano w poniższym kodzie:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

`[Column(TypeName = "decimal(18, 2)")]` Adnotacji danych umożliwia Entity Framework Core poprawnie mapowane `Price` walutę w bazie danych. Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) zostało omówione w następnym samouczku. [Wyświetlić](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) atrybut określa, co będzie wyświetlone nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data), co nie jest wyświetlane informacje o godzinie, w tym polu.

Przejdź do strony/filmów i umieść kursor nad **Edytuj** link, aby wyświetlić docelowy adres URL.

![Okno przeglądarki z myszy nad łącze edycji i łącze adres Url http://localhost:1234/Movies/Edit/5 jest wyświetlany](~/tutorials/razor-pages/da1/edit7.png)

**Edytuj**, **szczegóły**, i **Usuń** łącza są generowane przez [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/filmy / Index.cshtml* pliku.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor. W poprzednim kodzie `AnchorTagHelper` dynamicznie generuje kod HTML `href` wartość atrybutu ze strony Razor (trasy jest względna) `asp-page`oraz identyfikator trasy (`asp-route-id`). Zobacz [Generowanie adresu URL dla stron](xref:razor-pages/index#url-generation-for-pages) Aby uzyskać więcej informacji.

Użyj **Wyświetl źródło** w ulubionej przeglądarce do sprawdzenia wygenerowany kod znaczników. Poniżej przedstawiono część wygenerowanego kodu HTML:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Dynamicznie generowana linki przekazują identyfikator filmu przy użyciu ciągu zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).

Zaktualizuj, Edytuj, szczegóły i usuwanie stron Razor, aby użyć szablonu trasy "{id: int}". Zmiany strony dyrektyw dla każdej z tych stron z `@page` do `@page "{id:int}"`. Uruchom aplikację, a następnie Wyświetl źródło. Wygenerowany kod HTML dodaje identyfikator do część ścieżki adresu URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Żądanie strony za pomocą szablonu trasy "{id: int}", który wykonuje **nie** obejmują liczbę całkowitą zwróci błąd HTTP 404 (nie znaleziono). Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404. Aby wprowadzić identyfikator jest opcjonalne, należy dołączyć `?` do ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

Aby przetestować działanie `@page "{id:int?}"`:

* Ustawianie strony dyrektyw w *Pages/Movies/Details.cshtml* do `@page "{id:int?}"`.
* Ustaw punkt przerwania w `public async Task<IActionResult> OnGetAsync(int? id)` (w *Pages/Movies/Details.cshtml.cs*).
* Przejdź do adresu `https://localhost:5001/Movies/Details/`.

Za pomocą `@page "{id:int}"` dyrektywy, punkt przerwania zostanie nigdy osiągnięty. Aparat routingu zwraca HTTP 404. Za pomocą `@page "{id:int?}"`, `OnGetAsync` metoda zwraca `NotFound` (HTTP 404).

Chociaż nie jest to zalecane, można napisać `OnGetAsync` — metoda (w *Pages/Movies/Delete.cshtml.cs*) jako:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

Przetestuj poprzedni kod:

* Wybierz **Usuń** łącza.
* Usuń identyfikator z adresu URL. Na przykład zmienić `https://localhost:5001/Movies/Delete/8` do `https://localhost:5001/Movies/Delete`.
* Przejść przez kod w debugerze.

### <a name="review-concurrency-exception-handling"></a>Przejrzyj Obsługa wyjątku współbieżności

Przegląd `OnPostAsync` method in Class metoda *Pages/Movies/Edit.cshtml.cs* pliku:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

Powyższy kod wykrywa wyjątki współbieżności jeden klient usunie film i inny klient wysyła zmiany do filmu.

Aby przetestować `catch` bloku:

* Ustawianie punktu przerwania `catch (DbUpdateConcurrencyException)`
* Wybierz **Edytuj** filmu, wprowadzić zmiany, ale nie wprowadzaj **Zapisz**.
* W innym oknie przeglądarki, zaznacz **Usuń** łączy dla tego samego filmu, a następnie usuń ten film.
* W poprzednim oknie przeglądarki Opublikuj zmiany do filmu.

Kod w środowisku produkcyjnym warto wykrywanie konfliktów współbieżności. Zobacz [Obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) Aby uzyskać więcej informacji.

### <a name="posting-and-binding-review"></a>Umieszczanie i powiązanie przeglądu

Sprawdź *Pages/Movies/Edit.cshtml.cs* pliku:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

Nawiązaniem żądanie HTTP GET do strony filmy oraz pozwala edytować (na przykład `http://localhost:5000/Movies/Edit/2`):

* `OnGetAsync` Metoda pobiera film z bazy danych i zwraca `Page` metody. 
* `Page` Metoda renderuje *Pages/Movies/Edit.cshtml* strona Razor. *Pages/Movies/Edit.cshtml* plik zawiera dyrektywy model (`@model RazorPagesMovie.Pages.Movies.EditModel`), co sprawia, że model film dostępny na stronie.
* Z wartościami z film zostanie wyświetlony formularz edycji.

Po opublikowaniu strony filmy oraz pozwala edytować:

* Wartości formularza na tej stronie są powiązane z `Movie` właściwości. `[BindProperty]` Atrybut umożliwia [wiązanie modelu](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Jeśli występują błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na wartość typu date), zostanie wyświetlony formularz wartościami przesłane.
* Jeśli nie ma żadnych błędów modelu, film zostanie zapisana.

Metod HTTP GET, na stronach indeksu, tworzenie i usuwanie Razor wykonaj podobny wzorzec. HTTP POST `OnPostAsync` metody w tworzenie strony Razor ze wzorcem podobne do `OnPostAsync` metody w edytowanie strony Razor.

Wyszukiwanie zostanie dodany do następnego samouczka.

> [!div class="step-by-step"]
> [Poprzednie: Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [dalej: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
