# <a name="adding-a-new-field"></a>Dodanie nowego pola

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku doda nowe pole do `Movies` tabeli. Firma Microsoft będzie porzucić bazy danych i utworzyć nową, możemy zmiany schematu (Dodaj nowe pole). Ten przepływ pracy działa dobrze w rozwoju, gdy nie mamy żadnych danych produkcyjnych perserve.

Po wdrożeniu aplikacji i dane potrzebne do perserve, nie można usunąć z bazy danych, jeśli należy zmienić schemat. Entity Framework [migracje Code First](/ef/core/get-started/aspnetcore/new-db) można aktualizować schematu i migrację bazy danych bez utraty danych. Migracje jest popularnych funkcji podczas używania programu SQL Server, ale SQLlite nie obsługuje wielu operacji schematu migracji, dlatego tylko bardzo prosty są możliwe do migracji. Zobacz [ograniczenia SQLite](/ef/core/providers/sqlite/limitations) Aby uzyskać więcej informacji.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Dodawanie właściwości klasyfikacji do modelu film

Otwórz *Models/Movie.cs* plik i dodać `Rating` właściwości:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Ponieważ zostały dodane nowe pole do `Movie` klasy, również należy zaktualizować powiązania listy dozwolonych, ta nowa właściwość zostanie uwzględniony. W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metod akcji, aby uwzględnić `Rating` właściwości:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Możesz również konieczne aktualizacja szablonów widok, aby wyświetlić, tworzenia i edytowania nowe `Rating` właściwości w widoku przeglądarki.

Edytuj */Views/Movies/Index.cshtml* plik i dodać `Rating` pola:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.

Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazę danych, aby uwzględnić nowe pole. Jeżeli możesz uruchomić go teraz, uzyskasz następujące `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Ten błąd jest wyświetlane, ponieważ zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu istniejącej bazy danych. (Brak nie `Rating` kolumny w tabeli bazy danych.)

Istnieje kilka sposobów rozwiązania problemu:

1. Porzucenia bazy danych i ma automatycznie ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu Entity Framework. Z tej metody, utracisz istniejące dane w bazie danych, więc nie można tego zrobić z produkcyjną bazę danych! Automatycznie inicjatora bazy danych z danych testowych za pomocą inicjatora jest często produktywności możliwości opracowywania aplikacji.

2. Ręcznie zmodyfikować schemat z istniejącej bazy danych, tak aby był zgodny z klasy modelu. Zaletą tej metody jest, aby zachować dane. Można to zrobić to ręcznie lub przez tworzenie bazy danych należy zmienić skryptu.

3. Użyj migracje Code First, aby zaktualizować schemat bazy danych.

W tym samouczku będziemy Porzuć i ponownie utworzyć bazę danych podczas zmiany schematu. W terminalu, aby usunąć bazę danych, uruchom następujące polecenie:

`dotnet ef database drop`

Aktualizacja `SeedData` klasy, dzięki czemu zapewnia wartość dla nowej kolumny. Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Dodaj `Rating` do `Edit`, `Details`, i `Delete` widoku.

Uruchom aplikację i sprawdzić, można utworzyć/edycji/wyświetlania filmów `Rating` pola. Szablony.
