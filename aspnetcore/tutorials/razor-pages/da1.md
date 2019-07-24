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
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualizowanie wygenerowanych stron w aplikacji ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Aplikacja ze szkieletem jest dobrze uruchomiona, ale prezentacja nie jest idealnym rozwiązaniem. **ReleaseDate** powinna być **datą wydania** (dwa wyrazy).

![Aplikacja filmowa otwarta w przeglądarce Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizowanie wygenerowanego kodu

Otwórz plik *models/Movie. cs* i Dodaj wyróżnione wiersze wyświetlane w następującym kodzie:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

Adnotacja `Price` danych umożliwia Entity Framework Core poprawne mapowanie na walutę w bazie danych. `[Column(TypeName = "decimal(18, 2)")]` Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

[Adnotacje](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) DataAnnotations zostały omówione w następnym samouczku. Atrybut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data), więc informacje o czasie przechowywane w polu nie są wyświetlane.

Przejdź do stron/filmów i umieść kursor na linku **edycji** , aby zobaczyć docelowy adres URL.

![Okno przeglądarki z myszą nad linkiem edycji i pokazanym http://localhost:1234/Movies/Edit/5 adresem URL linku](~/tutorials/razor-pages/da1/edit7.png)

Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez [pomocnika tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w pliku Pages */Films/index. cshtml* .

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor. W `AnchorTagHelper` poprzednim kodzie, dynamicznie generuje wartość atrybutu HTML `href` ze strony Razor (trasa `asp-page`jest względna), i identyfikator trasy (`asp-route-id`). Aby uzyskać więcej informacji, zobacz [generowanie adresów URL na stronach](xref:razor-pages/index#url-generation-for-pages) .

Użyj **widoku źródła** z ulubionej przeglądarki, aby sprawdzić wygenerowane znaczniki. Poniżej przedstawiono część wygenerowanego kodu HTML:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Dynamicznie generowane linki przekażą identyfikator filmu z ciągiem zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).

Zaktualizuj Razor Pages edycji, szczegółów i usuwania, aby użyć szablonu trasy "{ID: int}". Zmień dyrektywę Page dla każdej z tych stron z `@page` na. `@page "{id:int}"` Uruchom aplikację, a następnie Wyświetl źródło. Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Żądanie do strony z szablonem trasy "{ID: int}", który **nie** zawiera liczby całkowitej, zwróci błąd HTTP 404 (nie znaleziono). Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404. Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

Aby przetestować zachowanie `@page "{id:int?}"`:

* Ustaw dyrektywę Page na stronie */filmy/details. cshtml* na `@page "{id:int?}"`.
* Ustaw punkt `public async Task<IActionResult> OnGetAsync(int? id)` przerwania (w obszarze *strony/filmy/szczegóły. cshtml. cs*).
* Przejdź do adresu `https://localhost:5001/Movies/Details/`.

W przypadku `@page "{id:int}"` dyrektywy punkt przerwania nigdy nie trafi. Aparat routingu zwraca protokół HTTP 404. Przy użyciu `@page "{id:int?}"` Metodazwraca`NotFound` (HTTP 404). `OnGetAsync`

### <a name="review-concurrency-exception-handling"></a>Przejrzyj obsługę wyjątków współbieżności

Przejrzyj metodę w pliku *Pages/Films/Edit. cshtml. cs:* `OnPostAsync`

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

Poprzedni kod wykrywa wyjątki współbieżności, gdy jeden klient usuwa film, a pozostałe wpisy wprowadzane przez klienta do filmu.

Aby przetestować `catch` blok:

* Ustaw punkt przerwania na`catch (DbUpdateConcurrencyException)`
* Wybierz pozycję **Edytuj** dla filmu, wprowadź zmiany, ale nie wprowadzaj opcji **Zapisz**.
* W innym oknie przeglądarki wybierz łącze **Usuń** dla tego samego filmu, a następnie usuń film.
* W poprzednim oknie przeglądarki Opublikuj zmiany w filmie.

Kod produkcyjny może chcieć wykryć konflikty współbieżności. Aby uzyskać więcej informacji, zobacz sekcję [obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) .

### <a name="posting-and-binding-review"></a>Przegląd ogłaszania i powiązania

Przejrzyj *stronę/filmy/plik Edit. cshtml. cs* :

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

Gdy żądanie HTTP GET zostanie wysłane do strony filmy/Edycja (na przykład `http://localhost:5000/Movies/Edit/2`):

* Metoda pobiera film z bazy danych i `Page` zwraca metodę. `OnGetAsync`
* Metoda renderuje stronę */filmy/edytowanie. cshtml.* `Page` Plik *Pages/Movies/Edit. cshtml* zawiera dyrektywę model (`@model RazorPagesMovie.Pages.Movies.EditModel`), która sprawia, że model filmu jest dostępny na stronie.
* Zostanie wyświetlony formularz edycji z wartościami z filmu.

Po opublikowaniu strony filmy/Edycja:

* Wartości formularza na stronie są powiązane z `Movie` właściwością. Ten `[BindProperty]` atrybut włącza [powiązanie modelu](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Jeśli występują błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na datę), formularz jest ponownie wyświetlany z przesłanymi wartościami.
* Jeśli nie ma żadnych błędów modelu, film zostanie zapisany.

Metody GET protokołu HTTP na stronach index, Create i DELETE Razor są zgodne z podobnym wzorcem. Metoda post `OnPostAsync` protokołu HTTP na stronie Tworzenie Razor podąża za podobnym wzorcem `OnPostAsync` metody na stronie Edytuj Razor.

## <a name="additional-resources"></a>Dodatkowe zasoby

> [!div class="step-by-step"]
> [Ubiegł Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [dalej: Dodaj wyszukiwanie](xref:tutorials/razor-pages/search)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Aplikacja ze szkieletem jest dobrze uruchomiona, ale prezentacja nie jest idealnym rozwiązaniem. **ReleaseDate** powinna być **datą wydania** (dwa wyrazy).

![Aplikacja filmowa otwarta w przeglądarce Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizowanie wygenerowanego kodu

Otwórz plik *models/Movie. cs* i Dodaj wyróżnione wiersze wyświetlane w następującym kodzie:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

Adnotacja `Price` danych umożliwia Entity Framework Core poprawne mapowanie na walutę w bazie danych. `[Column(TypeName = "decimal(18, 2)")]` Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

[Adnotacje](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) DataAnnotations zostały omówione w następnym samouczku. Atrybut [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). Atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data), więc informacje o czasie przechowywane w polu nie są wyświetlane.

Przejdź do stron/filmów i umieść kursor na linku **edycji** , aby zobaczyć docelowy adres URL.

![Okno przeglądarki z myszą nad linkiem edycji i pokazanym http://localhost:1234/Movies/Edit/5 adresem URL linku](~/tutorials/razor-pages/da1/edit7.png)

Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez [pomocnika tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w pliku Pages */Films/index. cshtml* .

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor. W `AnchorTagHelper` poprzednim kodzie, dynamicznie generuje wartość atrybutu HTML `href` ze strony Razor (trasa `asp-page`jest względna), i identyfikator trasy (`asp-route-id`). Aby uzyskać więcej informacji, zobacz [generowanie adresów URL na stronach](xref:razor-pages/index#url-generation-for-pages) .

Użyj **widoku źródła** z ulubionej przeglądarki, aby sprawdzić wygenerowane znaczniki. Poniżej przedstawiono część wygenerowanego kodu HTML:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Dynamicznie generowane linki przekażą identyfikator filmu z ciągiem zapytania (na przykład `?id=1` w `https://localhost:5001/Movies/Details?id=1`).

Zaktualizuj Razor Pages edycji, szczegółów i usuwania, aby użyć szablonu trasy "{ID: int}". Zmień dyrektywę Page dla każdej z tych stron z `@page` na. `@page "{id:int}"` Uruchom aplikację, a następnie Wyświetl źródło. Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Żądanie do strony z szablonem trasy "{ID: int}", który **nie** zawiera liczby całkowitej, zwróci błąd HTTP 404 (nie znaleziono). Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404. Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

Aby przetestować zachowanie `@page "{id:int?}"`:

* Ustaw dyrektywę Page na stronie */filmy/details. cshtml* na `@page "{id:int?}"`.
* Ustaw punkt `public async Task<IActionResult> OnGetAsync(int? id)` przerwania (w obszarze *strony/filmy/szczegóły. cshtml. cs*).
* Przejdź do adresu `https://localhost:5001/Movies/Details/`.

W przypadku `@page "{id:int}"` dyrektywy punkt przerwania nigdy nie trafi. Aparat routingu zwraca protokół HTTP 404. Przy użyciu `@page "{id:int?}"` Metodazwraca`NotFound` (HTTP 404). `OnGetAsync`

### <a name="review-concurrency-exception-handling"></a>Przejrzyj obsługę wyjątków współbieżności

Przejrzyj metodę w pliku *Pages/Films/Edit. cshtml. cs:* `OnPostAsync`

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

Poprzedni kod wykrywa wyjątki współbieżności, gdy jeden klient usuwa film, a pozostałe wpisy wprowadzane przez klienta do filmu.

Aby przetestować `catch` blok:

* Ustaw punkt przerwania na`catch (DbUpdateConcurrencyException)`
* Wybierz pozycję **Edytuj** dla filmu, wprowadź zmiany, ale nie wprowadzaj opcji **Zapisz**.
* W innym oknie przeglądarki wybierz łącze **Usuń** dla tego samego filmu, a następnie usuń film.
* W poprzednim oknie przeglądarki Opublikuj zmiany w filmie.

Kod produkcyjny może chcieć wykryć konflikty współbieżności. Aby uzyskać więcej informacji, zobacz sekcję [obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) .

### <a name="posting-and-binding-review"></a>Przegląd ogłaszania i powiązania

Przejrzyj *stronę/filmy/plik Edit. cshtml. cs* :

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

Gdy żądanie HTTP GET zostanie wysłane do strony filmy/Edycja (na przykład `http://localhost:5000/Movies/Edit/2`):

* Metoda pobiera film z bazy danych i `Page` zwraca metodę. `OnGetAsync` 
* Metoda renderuje stronę */filmy/edytowanie. cshtml.* `Page` Plik *Pages/Movies/Edit. cshtml* zawiera dyrektywę model (`@model RazorPagesMovie.Pages.Movies.EditModel`), która sprawia, że model filmu jest dostępny na stronie.
* Zostanie wyświetlony formularz edycji z wartościami z filmu.

Po opublikowaniu strony filmy/Edycja:

* Wartości formularza na stronie są powiązane z `Movie` właściwością. Ten `[BindProperty]` atrybut włącza [powiązanie modelu](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Jeśli występują błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na datę), formularz zostanie wyświetlony z przesłanymi wartościami.
* Jeśli nie ma żadnych błędów modelu, film zostanie zapisany.

Metody GET protokołu HTTP na stronach index, Create i DELETE Razor są zgodne z podobnym wzorcem. Metoda post `OnPostAsync` protokołu HTTP na stronie Tworzenie Razor podąża za podobnym wzorcem `OnPostAsync` metody na stronie Edytuj Razor.

W następnym samouczku zostanie dodane Wyszukiwanie.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> [Ubiegł Praca z bazą danych](xref:tutorials/razor-pages/sql)
> [dalej: Dodaj wyszukiwanie](xref:tutorials/razor-pages/search)

::: moniker-end
