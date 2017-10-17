---
title: Aktualizowanie wygenerowanego stron
author: rick-anderson
description: "Uaktualnianie stron wygenerowanych lepsze wyświetlanie."
keywords: Platformy ASP.NET Core stron Razor
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 290d752ea5f177348ff3e749cc125e946ae6e763
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2017
---
# <a name="updating-the-generated-pages"></a>Aktualizowanie wygenerowanego stron

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Mamy dobry początek aplikacji film, ale nie jest idealnym rozwiązaniem prezentacji. Nie chcemy zobaczyć czas (00:00:00: 00 w obrazie poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa wyrazy).

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizacja wygenerowanego kodu

Otwórz *Models/Movie.cs* i Dodaj wyróżnione wiersze poniższym kodem:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Kliknij prawym przyciskiem myszy na linii o dowolnym kształcie red > ** Szybkie akcje i Refaktoryzacje **.

  ![Pokazuje menu kontekstowe ** > Szybkie akcje i Refaktoryzacje **.](da1/qa.png)

Wybierz`using System.ComponentModel.DataAnnotations;`

  ![przy użyciu System.ComponentModel.DataAnnotations u góry listy](da1/da.png)

  Dodaje programu Visual studio `using System.ComponentModel.DataAnnotations;`.

Omówione zostaną następujące czynności [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) w następnym samouczku. [Wyświetlić](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) Określa atrybut, co ma być wyświetlany dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (Data), co nie jest wyświetlane informacje o godzinie przechowywana w polu.

Przejdź do strony/filmy i umieść kursor nad **Edytuj** łącze, aby wyświetlić docelowy adres URL.

![Okna przeglądarki z myszą łącza edycji i link jest wyświetlany adres Url http://localhost:1234/filmów/Edit/5](da1/edit7.png)

**Edytuj**, **szczegóły**, i **usunąć** łącza są generowane przez [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/filmów / Index.cshtml* pliku.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Pomocników tagów](xref:mvc/views/tag-helpers/intro) kodu po stronie serwera do tworzenia i renderowania elementów HTML w plikach Razor. W powyższym kodzie `AnchorTagHelper` dynamicznie generuje kod HTML `href` wartość atrybutu na stronie aparatu Razor (trasy jest względna), `asp-page`i identyfikator marszruty (`asp-route-id`). Zobacz [generowania adresu URL dla stron](xref:mvc/razor-pages/index#url-generation-for-pages) Aby uzyskać więcej informacji.

Użyj **Wyświetl źródło** z ulubionej przeglądarce zbadanie wygenerowanego kodu znaczników. Poniżej przedstawiono fragment wygenerowanego kodu HTML:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Generowane dynamicznie linki przekazują identyfikator film z ciągu zapytania (na przykład `http://localhost:5000/Movies/Details?id=2` ). 

Zaktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć szablonu trasy "{identyfikator: int}". Zmiany w dyrektywie page dla każdego z tych stron do `@page "{id:int}"`. Uruchom aplikację, a następnie Wyświetl źródło. Wygenerowany kod HTML dodaje identyfikator do części ścieżki adresu URL:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Żądanie do strony przy użyciu szablonu trasy "{identyfikator: int}", który wykonuje **nie** obejmują liczbę całkowitą zwróci błąd 404 protokołu HTTP (nie znaleziono). Na przykład `http://localhost:5000/Movies/Details` zwróci błąd 404. Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Obsługa wyjątków współbieżności aktualizacji

Aktualizacja `OnPostAsync` metody w *Pages/Movies/Edit.cshtml.cs* pliku. Następujący wyróżniony kod przedstawia zmiany:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

Poprzedni kod wykrywa wyjątków współbieżności tylko, gdy pierwszy klient równoczesnych usuwa filmu i drugi klient równoczesnych zapisuje zmiany do filmu.

Aby przetestować `catch` bloku:

* Ustaw punkt przerwania w`catch (DbUpdateConcurrencyException)`
* Edytowanie filmu.
* W innym oknie przeglądarki, wybierz **usunąć** łączy dla tego samego film, a następnie usuń filmu.
* W oknie przeglądarki poprzedniej Opublikuj zmiany do filmu.

Kodzie produkcyjnym zwykle może wykryć konfliktom współbieżności, gdy dwie lub więcej klientów jednocześnie zaktualizować rekord. Zobacz [konfliktów współbieżności](xref:data/ef-mvc/concurrency) Aby uzyskać więcej informacji.

### <a name="posting-and-binding-review"></a>Zamieszczając i powiązanie przeglądu

Sprawdź *Pages/Movies/Edit.cshtml.cs* pliku:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

Nawiązaniem żądanie HTTP GET do strony filmów oraz edytować (na przykład `http://localhost:5000/Movies/Edit/2`):

* `OnGetAsync` Metoda pobiera film z bazy danych i zwraca `Page` metody. 
* `Page` Metoda renderuje *Pages/Movies/Edit.cshtml* Razor strony. *Pages/Movies/Edit.cshtml* plik zawiera dyrektywa modelu (`@model RazorPagesMovie.Pages.Movies.EditModel`), co czyni modelu filmu dostępne na stronie.
* Wartościami z filmu zostanie wyświetlony formularz edycji.

Gdy filmów oraz edytować strona jest przesyłana:

* Na stronie wartości formularza jest powiązana z `Movie` właściwości. `[BindProperty]` Atrybutu umożliwia [modelu powiązania](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Jeśli wystąpią błędy w stanie modelu (na przykład `ReleaseDate` nie można przekonwertować na typ date), ponownie opublikowania formularza z wartościami zostało przesłane.
* Jeśli nie ma żadnych błędów w modelu, zapisać.

Metody HTTP GET, na stronach indeksu, tworzenie i usuwanie Razor wykonaj podobnego wzorca. HTTP POST `OnPostAsync` metody na stronie Utwórz Razor następuje podobnego wzorca do `OnPostAsync` metody na stronie edycji Razor.

Wyszukiwania został dodany w następnym samouczku.

>[!div class="step-by-step"]
[Poprzedni: Praca z bazy danych LocalDB programu SQL Server](xref:tutorials/razor-pages/sql)
[Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
