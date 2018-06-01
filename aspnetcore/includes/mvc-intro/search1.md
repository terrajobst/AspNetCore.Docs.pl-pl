# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Dodawanie wyszukiwania do platformy ASP.NET Core aplikacji MVC

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji możesz dodać możliwości wyszukiwania do `Index` metody akcji, która umożliwia wyszukiwanie filmów przez *genre* lub *nazwa*.

Aktualizacja `Index` metodę z następującym kodem:
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

W pierwszym wierszu `Index` tworzy metody akcji [LINQ](/dotnet/standard/using-linq) zapytanie, aby wybrać filmy:

```csharp
var movies = from m in _context.Movie
             select m;
```

Zapytanie jest *tylko* zdefiniowane w tym momencie, ma **nie** zostały uruchomione w bazie danych.

Jeśli `searchString` parametru zawiera ciąg, filmy zapytania są modyfikowane w celu filtrowania wartość ciągu wyszukiwania:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

`s => s.Title.Contains()` Kod powyżej [wyrażenia Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Wyrażenia lambda są używane w oparte na metodzie [LINQ](/dotnet/standard/using-linq) wysyła zapytanie jako argumenty do metod operator standardowej kwerendy, takich jak [gdzie](/dotnet/api/system.linq.enumerable.where) metody lub `Contains` (używane w powyższym kodzie). Zapytania LINQ nie są wykonywane podczas definiowania lub są one zmienione przez wywołanie metody, takie jak `Where`, `Contains` lub `OrderBy`. Zamiast wykonywania zapytania została odroczona.  Oznacza to, że obliczania wyrażenia jest opóźnione aż do jej wartość rzeczywista jest rzeczywiście iterowane za pośrednictwem lub `ToListAsync` metoda jest wywoływana. Aby uzyskać więcej informacji na temat wykonywania zapytań odroczonych, zobacz [wykonywania zapytania](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Uwaga: [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) metody jest uruchamiane na bazie danych, nie znajduje się w kodzie c# pokazanym powyżej. Uwzględniana wielkość liter w zapytaniu jest zależna od bazy danych i sortowanie. W programie SQL Server [zawiera](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) mapuje [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), która jest uwzględniana wielkość liter. W SQLlite z sortowania domyślnego, jest uwzględniana wielkość liter.

Przejdź do `/Movies/Index`. Dołącz ciąg zapytania, takie jak `?searchString=Ghost` do adresu URL. Filtrowane filmów są wyświetlane.

![Widok indeksu](~/tutorials/first-mvc-app/search/_static/ghost.png)

Jeśli zmienisz podpis `Index` metoda ma parametr o nazwie `id`, `id` parametr będzie odpowiadać opcjonalny `{id}` symbolu zastępczego dla domyślnej kieruje zestawu w *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
