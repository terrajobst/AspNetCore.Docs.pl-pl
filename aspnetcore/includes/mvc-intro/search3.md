<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Teraz podczas przesyłania wyszukiwania adres URL zawiera ciąg zapytania wyszukiwania. Wyszukiwanie będzie także przejść do `HttpGet Index` metody akcji, nawet jeśli masz `HttpPost Index` metody.

![Okno przeglądarki, przedstawiające parametru Wyszukiwany_ciąg = widma w adres Url i zwracany, filmy Ghostbusters i Ghostbusters 2 zawierają widma programu word](../../tutorials/first-mvc-app/search/_static/search_get.png)

Następujący kod przedstawia zmiany `form` tagu:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Dodawanie wyszukiwania według rodzaju

Dodaj następujące `MovieGenreViewModel` klasy do *modele* folderu:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Model widoku genre film będzie zawierać:

   * Lista filmów.
   * A `SelectList` zawierający listę gatunkami muzyki. Dzięki temu użytkownikowi na wybranie określonego rodzaju z listy.
   * `movieGenre`, zawierającą wybranego rodzaju.

Zastąp `Index` metoda `MoviesController.cs` następującym kodem:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Następujący kod jest `LINQ` kwerendę, która pobiera wszystkie genres z bazy danych.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

`SelectList` Gatunkami muzyki jest tworzony przez projekcji różne gatunki (nie chcemy naszej listy wyboru mają zduplikowane genres).

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a>Dodawanie wyszukiwania według rodzaju do widoku indeksu

Aktualizacja `Index.cshtml` w następujący sposób:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Sprawdź wyrażenie lambda, używane w następujących pomocnika kodu HTML:

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
W powyższym kodzie `DisplayNameFor` przeprowadzający pomocnika kodu HTML `Title` właściwości, do którego odwołuje się wyrażenie lambda, aby ustalić nazwę wyświetlaną. Ponieważ wyrażenie lambda jest sprawdzana zamiast obliczone, nie otrzyma naruszenia zasad dostępu podczas `model`, `model.movies`, lub `model.movies[0]` są `null` lub jest pusty. Podczas oceny wyrażenia lambda (na przykład `@Html.DisplayFor(modelItem => item.Title)`), wartości właściwości modelu.

Aby przetestować aplikację, wyszukiwanie według rodzaju, tytuł filmu i obu.
