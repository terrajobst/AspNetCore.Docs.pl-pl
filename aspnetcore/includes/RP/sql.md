# <a name="working-with-sqlite-in-and-razor-pages"></a>Praca z bazy danych SQLite w i stron Razor

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

`MovieContext` Obiektu obsługuje zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych. Kontekst bazy danych został zarejestrowany za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` metody w *Startup.cs* pliku:

[!code-csharp[Main](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

[SQLite](https://www.sqlite.org/) stanów witryny sieci Web:

> SQLite jest niezależna, wysokiej niezawodności, osadzone, oferujący wszystkie funkcje, domeny publicznej, aparatu bazy danych SQL. SQLite jest najczęściej używanych aparatu bazy danych na całym świecie.

Istnieje wiele narzędzi innych firm, które można pobrać do zarządzania i wyświetlić bazy danych SQLite. Na poniższym obrazie pochodzi z [przeglądarki bazy danych dla bazy danych SQLite](http://sqlitebrowser.org/). Jeśli masz ulubionego narzędzia SQLite, zostaw komentarz w takich jak informacji na ten temat.

![Przeglądarka bazy danych dla bazy danych Film przedstawiający SQLite](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Inicjatora bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modele* folderu. Zastąp wygenerowany kod poniżej:

[!code-csharp[Main](code\Models\SeedData.cs)]

Jeśli w bazie danych są wszystkie filmy, zwraca inicjatora inicjatora.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Dodaj inicjatora inicjatora

Inicjator inicjatora, aby dodać `Main` metody w *Program.cs* pliku:

[!code-csharp[Main](../../tutorials/razor-pages\razor-pages-start\sample\RazorPagesMovie\Program.cs)]

### <a name="test-the-app"></a>Testowanie aplikacji

Usuń wszystkie rekordy w bazie danych (co seed — metoda będzie uruchamiane). Zatrzymać i uruchomić aplikację w celu umieszczenia bazy danych.

Aplikacja zawiera wprowadzonych danych.