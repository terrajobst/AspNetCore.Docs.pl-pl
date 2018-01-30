# <a name="adding-search-to-a-razor-pages-app"></a>Dodawanie do aplikacji stron Razor wyszukiwania

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym dokumencie możliwości wyszukiwania jest dodawany do strony indeksu, która umożliwia wyszukiwanie filmów przez *genre* lub *nazwa*.

Zaktualizuj strony indeksu `OnGetAsync` metodę z następującym kodem:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

W pierwszym wierszu `OnGetAsync` metoda tworzy [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) zapytanie, aby wybrać filmy:

```csharp
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest *tylko* zdefiniowane w tym momencie, ma **nie** zostały uruchomione w bazie danych.

Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania ciąg wyszukiwania:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Kod [wyrażenia Lambda](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w oparte na metodzie [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) wysyła zapytanie jako argumenty do metod operator standardowej kwerendy, takich jak [gdzie](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metody lub `Contains` (używane w powyższym kodzie). Podczas definiowania lub są one zmienione przez wywołanie metody nie zostaną wykonane zapytań LINQ (takich jak `Where`, `Contains` lub `OrderBy`). Zamiast wykonywania zapytania została odroczona. Oznacza to, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest iterowane za pośrednictwem lub `ToListAsync` metoda jest wywoływana. Zobacz [wykonywania zapytania](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) Aby uzyskać więcej informacji.

**Uwaga:** [zawiera](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metody jest uruchamiane na bazie danych, nie znajduje się w kodzie języka C#. Uwzględniana wielkość liter w zapytaniu jest zależna od bazy danych i sortowanie. W programie SQL Server `Contains` mapuje [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter. W SQLite z sortowania domyślnego, jest uwzględniana wielkość liter.

Przejdź do strony filmy i Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL (na przykład `http://localhost:5000/Movies?searchString=Ghost`). Filtrowane filmów są wyświetlane.

![Widok indeksu](../../tutorials/razor-pages/search/_static/ghost.png)

Jeśli następujący szablon trasy zostanie dodany do strony indeksu, ciąg wyszukiwania mogą być przekazywane jako segment adresu URL (na przykład `http://localhost:5000/Movies/ghost`).

```cshtml
@page "{searchString?}"
```

Poprzednie ograniczenie trasy umożliwia wyszukiwanie tytuł jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.  `?` w `"{searchString?}"` oznacza, że jest to parametr opcjonalny trasy.

![Widok indeksu z widma word dodane do adresu Url i filmu zwrócona lista filmów Ghostbusters i Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, aby wyszukać filmu. W tym kroku interfejsu użytkownika jest dodawany do filtrowania filmów. Jeśli dodano ograniczenia trasy `"{searchString?}"`, usuń go.

Otwórz *Pages/Movies/Index.cshtml* pliku, a następnie dodaj `<form>` wyróżnione następujący kod znaczników:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

Kod HTML `<form>` tagów używa [pomocnika Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Po przesłaniu formularza ciąg filtru jest wysyłana do *stron/filmów/indeksu* strony. Zapisz zmiany i przetestować filtr.

![Widok indeksu z widma word wpisane w polu tekstowym filtru tytułu](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a>Wyszukiwanie według rodzaju

Dodaj następujące wyróżnione właściwości *Pages/Movies/Index.cshtml.cs*:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

`SelectList Genres` Zawiera listę gatunkami muzyki. Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.

`MovieGenre` Właściwość zawiera określonego rodzaju wybiera użytkownika (na przykład "zachodni").

Aktualizacja `OnGetAsync` metodę z następującym kodem:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Następujący kod jest zapytania LINQ, która pobiera wszystkie genres z bazy danych.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` Gatunkami muzyki jest tworzony przez różne genres projekcji.

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według rodzaju

Aktualizacja *Index.cshtml* w następujący sposób:

[!code-cshtml[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Aby przetestować aplikację, wyszukiwanie według rodzaju, tytuł filmu i obu.

>[!div class="step-by-step"]
[Poprzedni: Aktualizowanie stron](xref:tutorials/razor-pages/da1)
[dalej: dodanie nowego pola](xref:tutorials/razor-pages/new-field)
