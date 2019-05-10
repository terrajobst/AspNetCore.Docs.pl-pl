<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Teraz gdy prześlesz wyszukiwania, adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.

![Okno przeglądarki, przedstawiające Ciągwyszukiwania = ghost w adresie Url i filmów, zwrócone, Ghostbusters i Ghostbusters 2, zawierają ghost programu word](~/tutorials/first-mvc-app/search/_static/search_get.png)

Następujący kod przedstawia zmiany `form` tag:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według gatunku

Dodaj następujący kod `MovieGenreViewModel` klasy *modeli* folderu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Model widoku gatunku filmu będzie zawierać:

* Lista filmów.
* A `SelectList` zawierającego listę gatunki. Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.
* `MovieGenre`, zawierającą wybrane gatunku.
* `SearchString`, który zawiera tekst, użytkownicy wprowadzają w polu tekstowym wyszukiwania.

Zastąp `Index` method in Class metoda `MoviesController.cs` następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Poniższy kod jest `LINQ` zapytania, który pobiera wszystkie gatunki z bazy danych.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

`SelectList` Gatunków jest tworzona przy wyświetlaniu distinct gatunki (nie chcemy naszej listy wyboru mają zduplikowane gatunki).

Gdy użytkownik wyszukuje element, do wartości wyszukiwania są przechowywane w polu wyszukiwania. Aby zachować wartości wyszukiwania, należy wypełnić `SearchString` właściwość o wartości wyszukiwania. Wyszukaj wartość `searchString` parametr `Index` akcji kontrolera.

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a>Dodawanie wyszukiwania według gatunku do widoku indeksu

Aktualizacja `Index.cshtml` w następujący sposób:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
W poprzednim kodzie `DisplayNameFor` sprawdza pomocnika kodu HTML `Title` właściwość, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną. Ponieważ wyrażenie lambda jest kontrolowane zamiast oceniane, nie otrzymasz naruszenie zasad dostępu podczas `model`, `model.Movies`, lub `model.Movies[0]` są `null` lub jest pusty. Kiedy jest obliczane wyrażenie lambda (na przykład `@Html.DisplayFor(modelItem => item.Title)`), są oceniane wartości właściwości modelu.

Testowanie aplikacji przez wyszukiwanie według gatunku, tytuł filmu i obu.
