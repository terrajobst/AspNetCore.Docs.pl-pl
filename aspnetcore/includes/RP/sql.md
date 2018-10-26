# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a>Praca z SQLite w programie ASP.NET Core Razor strony aplikacji

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

`MovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

Aby uzyskać więcej informacji na temat korzystania z `DbContext` DI, widoczne [przy użyciu typu DbContext z DI](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).

## <a name="sqlite"></a>Bazy danych SQLite

[SQLite](https://www.sqlite.org/) stany witryny sieci Web:

> Bazy danych SQLite jest niezależna o wysokiej niezawodności, osadzonych, w pełni funkcjonalne, domeny publicznej, aparatu bazy danych SQL. Bazy danych SQLite jest najczęściej używanych aparatu bazy danych na całym świecie.

Istnieje wiele narzędzi innych firm, które można pobrać do zarządzania oraz wyświetlić bazy danych SQLite. Poniższy obraz pochodzi z [przeglądarka bazy danych dla bazy danych SQLite](http://sqlitebrowser.org/). Jeśli masz ulubionego Narzędzia bazy danych SQLite, pozostaw komentarz na takich jak na jego temat.

![Przeglądarka bazy danych dla bazy danych Film przedstawiający SQLite](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu. Zastąp wygenerowany kod następujących czynności:

[!code-csharp[](code/Models/SeedData.cs)]

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

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a>Testowanie aplikacji

(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych. Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.

Aplikacja zawiera wprowadzonych danych.
