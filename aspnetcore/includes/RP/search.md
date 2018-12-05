# <a name="add-search-to-an-aspnet-core-razor-pages-app"></a>Dodawanie wyszukiwania do aplikacji stron Razor programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym dokumencie możliwości wyszukiwania jest dodawany do strony indeksu, który umożliwia wyszukiwanie filmów przez *gatunku* lub *nazwa*.

Aktualizuj stronę indeksu `OnGetAsync` metoda następującym kodem:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

W pierwszym wierszu `OnGetAsync` metoda tworzy [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) zapytanie, aby wybrać filmów:

```csharp
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest *tylko* zdefiniowane w tym momencie, ma ona **nie** zostały uruchomione dla bazy danych.

Jeśli `searchString` parametru zawiera ciąg, zapytanie filmy zostanie zmodyfikowany na potrzeby filtrowania ciąg wyszukiwania:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Kod jest [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) jako argumenty do standardowych metod operatorów kwerendy takich zapytań jak [gdzie](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) metody lub `Contains` (używane w poprzednim kodzie). Zapytania LINQ nie są wykonywane, gdy są one definiowane lub ich modyfikacji przez wywołanie metody (takie jak `Where`, `Contains` lub `OrderBy`). Przeciwnie wykonanie zapytania jest odroczone. Oznacza to, że obliczania wyrażenia została opóźniona do momentu jego rzeczywista wartość jest powtarzana lub `ToListAsync` metoda jest wywoływana. Zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution) Aby uzyskać więcej informacji.

**Uwaga:** [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metoda jest uruchomiona w bazie danych, nie w kodzie języka C#. Rozróżnianie wielkości liter w zapytaniu, zależy od bazy danych i sortowanie. W programie SQL Server `Contains` mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter. W SQLite przy użyciu sortowania domyślnego, jest uwzględniana wielkość liter.

Przejdź do strony filmy i Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL (na przykład `http://localhost:5000/Movies?searchString=Ghost`). Wyświetlane są filtrowane filmów.

![Widok indeksu](../../tutorials/razor-pages/search/_static/ghost.png)

Jeśli następujący szablon trasy zostanie dodany do strony indeksu, ciąg wyszukiwania mogą być przekazywane jako segment adresu URL (na przykład `http://localhost:5000/Movies/ghost`).

```cshtml
@page "{searchString?}"
```

Poprzedni ograniczenia trasy umożliwia wyszukiwanie tytuł, np. dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.  `?` w `"{searchString?}"` oznacza, że jest to parametr opcjonalny trasy.

![Widok indeksu z ghost word dodawany do adresu Url i zwrócony filmu listę filmów Ghostbusters i Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

Jednak nie można oczekiwać od użytkowników, aby zmodyfikować adres URL, aby wyszukać film. W tym kroku interfejs użytkownika jest dodawany do filtrowania filmów. Jeśli dodano ograniczenia trasy `"{searchString?}"`, usuń go.

Otwórz *Pages/Movies/Index.cshtml* pliku i Dodaj `<form>` znaczników wyróżnione w poniższym kodzie:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

Kod HTML `<form>` tagów używa [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Po przesłaniu formularza ciąg filtru są wysyłane do *stron/filmy/Index* strony. Zapisz zmiany i przetestować filtr.

![Widok indeksu z ghost słowa wpisane w polu tekstowym filtru tytułu](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a>Wyszukaj według gatunku

Dodaj następujące właściwości wyróżniony, aby *Pages/Movies/Index.cshtml.cs*:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

`SelectList Genres` Zawiera listę gatunki. Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.

`MovieGenre` Właściwość zawiera gatunku określonych przez użytkownika (na przykład "zachodni").

Aktualizacja `OnGetAsync` metoda następującym kodem:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Poniższy kod jest zapytanie LINQ, która pobiera wszystkie gatunki z bazy danych.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` Gatunków jest tworzona przy wyświetlaniu gatunki distinct.

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według gatunku

Aktualizacja *Index.cshtml* w następujący sposób:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i obu.
