::: moniker range=">= aspnetcore-2.1"

Kliknij prawym przyciskiem myszy czerwona linia falista > **szybkie akcje i Refaktoryzacje** na `[Column]` atribute i wybierz pozycję `using System.ComponentModel.DataAnnotations.Schema;`

`[Column(TypeName = "decimal(18, 2)")]` Wymagana jest adnotacja danych, dzięki czemu można poprawnie mapowane na platformy Entity Framework Core `Price` walutę w bazie danych. Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

Ukończone modelu:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateFixed.cs?name=snippet_1)]

::: moniker-end

Omówimy [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) w następnym samouczku. [Wyświetlić](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) atrybut określa, co będzie wyświetlone nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data), co nie jest wyświetlane informacje o godzinie, w tym polu.

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

Dynamicznie generowana linki przekazują identyfikator filmu przy użyciu ciągu zapytania (na przykład `http://localhost:5000/Movies/Details?id=2`).

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

::: moniker range="= aspnetcore-2.0"

### <a name="update-concurrency-exception-handling"></a>Aktualizacja obsługi wyjątku współbieżności

Aktualizacja `OnPostAsync` method in Class metoda *Pages/Movies/Edit.cshtml.cs* pliku. Następujący wyróżniony kod pokazuje zmiany:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

Powyższy kod tylko wykrywa wyjątki współbieżności, gdy pierwszy klient współbieżnych usuwa film i drugiego klienta współbieżnych zapisuje zmiany do filmu.

Aby przetestować `catch` bloku:

* Ustawianie punktu przerwania `catch (DbUpdateConcurrencyException)`
* Edytowanie filmu.
* W innym oknie przeglądarki, zaznacz **Usuń** łączy dla tego samego filmu, a następnie usuń ten film.
* W poprzednim oknie przeglądarki Opublikuj zmiany do filmu.

Kodzie produkcyjnym zazwyczaj będzie wykrywanie konfliktów współbieżności, gdy dwie lub więcej klientów współbieżnie aktualizowana rekordu. Zobacz [Obsługa konfliktów współbieżności](xref:data/ef-rp/concurrency) Aby uzyskać więcej informacji.

::: moniker-end

### <a name="posting-and-binding-review"></a>Umieszczanie i powiązanie przeglądu

Sprawdź *Pages/Movies/Edit.cshtml.cs* pliku:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

::: moniker-end

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

* Jeśli występują błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na wartość typu date), ponownie opublikowania formularza z wartościami przesłane.
* Jeśli nie ma żadnych błędów modelu, film zostanie zapisana.

Metod HTTP GET, na stronach indeksu, tworzenie i usuwanie Razor wykonaj podobny wzorzec. HTTP POST `OnPostAsync` metody w tworzenie strony Razor ze wzorcem podobne do `OnPostAsync` metody w edytowanie strony Razor.

Wyszukiwanie zostanie dodany do następnego samouczka.
