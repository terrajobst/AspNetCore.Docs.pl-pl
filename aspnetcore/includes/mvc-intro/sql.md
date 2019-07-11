# <a name="work-with-sqlite-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="91927-101">Praca z SQLite w aplikacji MVC platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91927-101">Work with SQLite in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="91927-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="91927-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="91927-103">`MvcMovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="91927-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="91927-104">Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="91927-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="91927-105">Bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="91927-105">SQLite</span></span>

<span data-ttu-id="91927-106">[SQLite](https://www.sqlite.org/) stany witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="91927-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="91927-107">Bazy danych SQLite jest niezależna o wysokiej niezawodności, osadzonych, w pełni funkcjonalne, domeny publicznej, aparatu bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="91927-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="91927-108">Bazy danych SQLite jest najczęściej używanych aparatu bazy danych na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="91927-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="91927-109">Istnieje wiele narzędzi innych firm, które można pobrać do zarządzania oraz wyświetlić bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="91927-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="91927-110">Poniższy obraz pochodzi z [przeglądarka bazy danych dla bazy danych SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="91927-110">The image below is from [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span> <span data-ttu-id="91927-111">Jeśli masz ulubionego Narzędzia bazy danych SQLite, pozostaw komentarz na takich jak na jego temat.</span><span class="sxs-lookup"><span data-stu-id="91927-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![Przeglądarka bazy danych dla bazy danych Film przedstawiający SQLite](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="91927-113">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="91927-113">Seed the database</span></span>

<span data-ttu-id="91927-114">Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="91927-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="91927-115">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="91927-115">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="91927-116">Czy istnieją wszystkie filmy w bazie danych, zwraca wartość inicjator inicjatora.</span><span class="sxs-lookup"><span data-stu-id="91927-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="91927-117">Dodaj inicjator inicjatora</span><span class="sxs-lookup"><span data-stu-id="91927-117">Add the seed initializer</span></span>

<span data-ttu-id="91927-118">Dodaj inicjator inicjator, tak aby `Main` method in Class metoda *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="91927-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

::: moniker-end

### <a name="test-the-app"></a><span data-ttu-id="91927-119">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="91927-119">Test the app</span></span>

<span data-ttu-id="91927-120">(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="91927-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="91927-121">Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="91927-121">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="91927-122">Aplikacja zawiera wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="91927-122">The app shows the seeded data.</span></span>

![Przeglądarki Otwórz aplikacja filmu MVC wyświetlane dane dotyczące filmu](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
