# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Dodawanie nowego pola do aplikacji ASP.NET Core MVC

<!-- This include not used by windows version -->

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku spowoduje dodanie nowego pola do `Movies` tabeli. Utworzymy porzucenia bazy danych i utworzyć nową w przypadku zmiany schematu (Dodawanie nowego pola). Ten przepływ pracy działa również na wczesnym etapie projektowania, gdy firma Microsoft nie ma żadnych danych produkcyjnych, aby zachować.

Gdy Twoja aplikacja jest wdrożona i mieć dane, które musisz zachować, nie można porzucić użytkownika bazy danych, gdy zajdzie potrzeba zmiany schematu. Entity Framework [migracje Code First](/ef/core/get-started/aspnetcore/new-db) można aktualizować schematu i migracji bazy danych bez utraty danych. Migracje jest popularnych funkcji podczas przy użyciu programu SQL Server, ale SQLite nie obsługuje wielu operacji migracji schematu więc tylko bardzo prosty sposób migracji są możliwe. Zobacz [ograniczenia SQLite](/ef/core/providers/sqlite/limitations) Aby uzyskać więcej informacji.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu Movie

Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania listy dozwolonych adresów, dzięki tej nowej właściwości zostaną dołączone. W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Możesz również należy zaktualizować szablonów widoków w celu wyświetlania, tworzenia i edytowania nowy `Rating` właściwości w widoku przeglądarki.

Edytuj */Views/Movies/Index.cshtml* pliku i Dodaj `Rating` pola:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.

Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazy danych, aby uwzględnić nowe pole. Jeśli uruchamiasz go teraz, uzyskasz następujące `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Widzisz ten błąd, ponieważ zaktualizowane klasy modelu Movie różni się od schematu tabeli filmu istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Istnieje kilka sposobów rozwiązania problemu:

1. Upuść bazę danych i mieć automatycznie ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu w programie Entity Framework. W przypadku tej metody, tracisz istniejących danych w bazie danych, więc nie możesz tego zrobić za pomocą produkcyjnej bazy danych! Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób do tworzenia aplikacji.

2. Ręcznie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu. Zaletą tego podejścia jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.

3. Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.

W tym samouczku będziemy Porzuć i ponownie utworzyć bazę danych, po zmianie schematu. Uruchom następujące polecenie w terminalu, aby porzucić bazy danych:

`dotnet ef database drop`

Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Dodaj `Rating` pole `Edit`, `Details`, i `Delete` widoku.

Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola. Szablony.
