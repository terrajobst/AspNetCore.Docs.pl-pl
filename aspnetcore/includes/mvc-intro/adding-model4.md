Zaznaczony kod powyżej przedstawiono filmu kontekst bazy danych dodawane do [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera (w *Startup.cs* pliku). `services.AddDbContext<MvcMovieContext>(options =>` Określa bazę danych i parametry połączenia. `=>` jest [operatora lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator).

Otwórz *Controllers/MoviesController.cs* pliku i sprawdź, czy konstruktora:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

Używa konstruktora [iniekcji zależności](xref:fundamentals/dependency-injection) iniekcję kontekst bazy danych (`MvcMovieContext `) z kontrolerem. Kontekst bazy danych jest używany w każdym [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) metod w kontrolerze.

<a name="strongly-typed-models-keyword-label"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Silnie typizowane modeli i @model — słowo kluczowe

Wcześniej w tym samouczku widać, jak kontroler może przekazać dane i obiekty do widoku przy użyciu `ViewData` słownika. `ViewData` Słownik jest obiekt dynamiczny, które oferują wygodny sposób późnym wiązaniem do przekazywania informacji do widoku.

MVC oferuje także możliwość przekazywania silnie typizowanych obiektów modelu do widoku. To silnie typizowane podejście umożliwia lepsze kompilacji Sprawdzanie kodu. Mechanizm szkieletów użyć tej metody, (tj. przekazywanie jednoznacznie modelu) z `MoviesController` klasy i widoki tworzona metod i widoków.

Sprawdź wygenerowany `Details` metody w *Controllers/MoviesController.cs* pliku:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

`id` Parametr jest zwykle przekazywany jako dane trasy. Na przykład `http://localhost:5000/movies/details/1` ustawia:

* Kontroler do `movies` kontrolera (pierwszy segment adresu URL).
* Akcja `details` (drugi segment adresu URL).
* Identyfikator na wartość 1 (ostatni segment adresu URL).

Można również przekazać `id` za pomocą kwerendy ciągu w następujący sposób:

`http://localhost:1234/movies/details?id=1`

`id` Parametr jest zdefiniowany jako [typ dopuszczający wartość null](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) w przypadku, gdy nie jest podana wartość Identyfikatora.

A [wyrażenia lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) jest przekazywany do `SingleOrDefaultAsync` Wybierz film jednostek, które odpowiada wartości ciągu danych lub zapytanie trasy.

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

Jeśli zostanie znaleziony film, wystąpienie `Movie` modelu jest przekazywana do `Details` widoku:

```csharp
return View(movie);
   ```

Sprawdź zawartość *Views/Movies/Details.cshtml* pliku:

[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

W tym `@model` instrukcji w górnej części pliku widoku, można określić typu obiektu, który oczekuje widoku. Podczas tworzenia kontrolera film, Visual Studio automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Details.cshtml* pliku:

```HTML
@model MvcMovie.Models.Movie
   ```

To `@model` dyrektywy umożliwia dostęp do filmów, który kontroler przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Details.cshtml* widoku kodu przekazuje każde pole film, aby `DisplayNameFor` i `DisplayFor` pomocników HTML z silnie typizowaną `Model` obiektu. `Create` i `Edit` metody i widoki również przekazać `Movie` obiekt modelu.

Sprawdź *Index.cshtml* widoku i `Index` metody w kontrolerze filmów. Zwróć uwagę, jak kod tworzy `List` obiektu, gdy wywołuje `View` metody. Kod przekazuje to `Movies` z listy `Index` metody akcji w widoku:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

Podczas tworzenia kontrolera filmów, tworzenia szkieletu automatycznie uwzględnione następujące `@model` instrukcji w górnej części *Index.cshtml* pliku:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` Dyrektywy umożliwia dostęp do listy filmów, które kontrolera przekazywane do widoku przy użyciu `Model` obiekt, który jest silnie typizowane. Na przykład w *Index.cshtml* wyświetlić kod pętlę filmów z `foreach` instrukcji na silnie typizowaną `Model` obiektu:

[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Ponieważ `Model` obiektu jest silnie typizowane (jako `IEnumerable<Movie>` obiektu), jest typu każdego elementu w pętli `Movie`. Wśród innych korzyści, oznacza to, czy możesz uzyskać kompilacji Sprawdzanie kodu:
