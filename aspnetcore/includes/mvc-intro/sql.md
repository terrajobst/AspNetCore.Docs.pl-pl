# <a name="work-with-sqlite-in-an-aspnet-core-mvc-app"></a>Praca z SQLite w aplikacji MVC platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>Bazy danych SQLite

[SQLite](https://www.sqlite.org/) stany witryny sieci Web:

> Bazy danych SQLite jest niezależna o wysokiej niezawodności, osadzonych, w pełni funkcjonalne, domeny publicznej, aparatu bazy danych SQL. Bazy danych SQLite jest najczęściej używanych aparatu bazy danych na całym świecie.

Istnieje wiele narzędzi innych firm, które można pobrać do zarządzania oraz wyświetlić bazy danych SQLite. Poniższy obraz pochodzi z [przeglądarka bazy danych dla bazy danych SQLite](https://sqlitebrowser.org/). Jeśli masz ulubionego Narzędzia bazy danych SQLite, pozostaw komentarz na takich jak na jego temat.

![Przeglądarka bazy danych dla bazy danych Film przedstawiający SQLite](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu. Zastąp wygenerowany kod następujących czynności:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Czy istnieją wszystkie filmy w bazie danych, zwraca wartość inicjator inicjatora.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>Dodaj inicjator inicjatora

Dodaj inicjator inicjator, tak aby `Main` method in Class metoda *Program.cs* pliku:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

::: moniker-end

### <a name="test-the-app"></a>Testowanie aplikacji

(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych. Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.
   
Aplikacja zawiera wprowadzonych danych.

![Przeglądarki Otwórz aplikacja filmu MVC wyświetlane dane dotyczące filmu](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
