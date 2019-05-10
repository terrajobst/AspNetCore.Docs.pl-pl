# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="f0f1b-101">Dodawanie nowego pola do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f0f1b-101">Add a new field to an ASP.NET Core MVC app</span></span>

<!-- This include not used by windows version -->

<span data-ttu-id="f0f1b-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f0f1b-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f0f1b-103">W tym samouczku spowoduje dodanie nowego pola do `Movies` tabeli.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="f0f1b-104">Utworzymy porzucenia bazy danych i utworzyć nową w przypadku zmiany schematu (Dodawanie nowego pola).</span><span class="sxs-lookup"><span data-stu-id="f0f1b-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="f0f1b-105">Ten przepływ pracy działa również na wczesnym etapie projektowania, gdy firma Microsoft nie ma żadnych danych produkcyjnych, aby zachować.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-105">This workflow works well early in development when we don't have any production data to preserve.</span></span>

<span data-ttu-id="f0f1b-106">Gdy Twoja aplikacja jest wdrożona i mieć dane, które musisz zachować, nie można porzucić użytkownika bazy danych, gdy zajdzie potrzeba zmiany schematu.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-106">Once your app is deployed and you have data that you need to preserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="f0f1b-107">Entity Framework [migracje Code First](/ef/core/get-started/aspnetcore/new-db) można aktualizować schematu i migracji bazy danych bez utraty danych.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-107">Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="f0f1b-108">Migracje jest popularnych funkcji podczas przy użyciu programu SQL Server, ale SQLite nie obsługuje wielu operacji migracji schematu więc tylko bardzo prosty sposób migracji są możliwe.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-108">Migrations is a popular feature when using SQL Server, but SQLite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="f0f1b-109">Zobacz [ograniczenia SQLite](/ef/core/providers/sqlite/limitations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-109">See [SQLite Limitations](/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="f0f1b-110">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="f0f1b-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="f0f1b-111">Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="f0f1b-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

<span data-ttu-id="f0f1b-112">Ponieważ zostały dodane nowe pole do `Movie` klasy, należy również zaktualizować powiązania listy dozwolonych adresów, dzięki tej nowej właściwości zostaną dołączone.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-112">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="f0f1b-113">W *MoviesController.cs*, zaktualizuj `[Bind]` atrybutu dla obu `Create` i `Edit` metody akcji, aby uwzględnić `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="f0f1b-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="f0f1b-114">Możesz również należy zaktualizować szablonów widoków w celu wyświetlania, tworzenia i edytowania nowy `Rating` właściwości w widoku przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-114">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="f0f1b-115">Edytuj */Views/Movies/Index.cshtml* pliku i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="f0f1b-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="f0f1b-116">Aktualizacja */Views/Movies/Create.cshtml* z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="f0f1b-117">Aplikacja nie będzie działać, dopóki nie możemy zaktualizować bazy danych, aby uwzględnić nowe pole.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-117">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="f0f1b-118">Jeśli uruchamiasz go teraz, uzyskasz następujące `SqliteException`:</span><span class="sxs-lookup"><span data-stu-id="f0f1b-118">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="f0f1b-119">Widzisz ten błąd, ponieważ zaktualizowane klasy modelu Movie różni się od schematu tabeli filmu istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-119">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="f0f1b-120">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="f0f1b-120">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="f0f1b-121">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="f0f1b-121">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="f0f1b-122">Upuść bazę danych i mieć automatycznie ponownie utworzyć bazę danych na podstawie nowego schematu klasy modelu w programie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-122">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="f0f1b-123">W przypadku tej metody, tracisz istniejących danych w bazie danych, więc nie możesz tego zrobić za pomocą produkcyjnej bazy danych!</span><span class="sxs-lookup"><span data-stu-id="f0f1b-123">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="f0f1b-124">Automatycznie zapełnić bazę danych przy użyciu danych testowych za pomocą inicjatora jest często produktywny sposób do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-124">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="f0f1b-125">Ręcznie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-125">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="f0f1b-126">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-126">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="f0f1b-127">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-127">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="f0f1b-128">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-128">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="f0f1b-129">W tym samouczku będziemy Porzuć i ponownie utworzyć bazę danych, po zmianie schematu.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-129">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="f0f1b-130">Uruchom następujące polecenie w terminalu, aby porzucić bazy danych:</span><span class="sxs-lookup"><span data-stu-id="f0f1b-130">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="f0f1b-131">Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-131">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="f0f1b-132">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-132">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="f0f1b-133">Dodaj `Rating` pole `Edit`, `Details`, i `Delete` widoku.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-133">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="f0f1b-134">Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-134">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="f0f1b-135">Szablony.</span><span class="sxs-lookup"><span data-stu-id="f0f1b-135">templates.</span></span>
