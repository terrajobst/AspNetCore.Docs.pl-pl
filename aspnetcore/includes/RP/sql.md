# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="ed21f-101">Praca z SQLite w programie ASP.NET Core Razor strony aplikacji</span><span class="sxs-lookup"><span data-stu-id="ed21f-101">Work with SQLite in an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="ed21f-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed21f-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed21f-103">`MovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed21f-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="ed21f-104">Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="ed21f-104">The database context is registered with the [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

<span data-ttu-id="ed21f-105">Aby uzyskać więcej informacji na temat korzystania z `DbContext` DI, widoczne [przy użyciu typu DbContext z DI](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ed21f-105">For more information on using `DbContext` with DI, see [Using DbContext with DI](/ef/core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection).</span></span>

## <a name="sqlite"></a><span data-ttu-id="ed21f-106">Bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="ed21f-106">SQLite</span></span>

<span data-ttu-id="ed21f-107">[SQLite](https://www.sqlite.org/) stany witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="ed21f-107">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="ed21f-108">Bazy danych SQLite jest niezależna o wysokiej niezawodności, osadzonych, w pełni funkcjonalne, domeny publicznej, aparatu bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="ed21f-108">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="ed21f-109">Bazy danych SQLite jest najczęściej używanych aparatu bazy danych na całym świecie.</span><span class="sxs-lookup"><span data-stu-id="ed21f-109">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="ed21f-110">Istnieje wiele narzędzi innych firm, które można pobrać do zarządzania oraz wyświetlić bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="ed21f-110">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="ed21f-111">Poniższy obraz pochodzi z [przeglądarka bazy danych dla bazy danych SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="ed21f-111">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="ed21f-112">Jeśli masz ulubionego Narzędzia bazy danych SQLite, pozostaw komentarz na takich jak na jego temat.</span><span class="sxs-lookup"><span data-stu-id="ed21f-112">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![Przeglądarka bazy danych dla bazy danych Film przedstawiający SQLite](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="ed21f-114">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="ed21f-114">Seed the database</span></span>

<span data-ttu-id="ed21f-115">Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="ed21f-115">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="ed21f-116">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ed21f-116">Replace the generated code with the following:</span></span>

[!code-csharp[](code/Models/SeedData.cs)]

<span data-ttu-id="ed21f-117">Czy istnieją wszystkie filmy w bazie danych, zwraca wartość inicjator inicjatora.</span><span class="sxs-lookup"><span data-stu-id="ed21f-117">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="ed21f-118">Dodaj inicjator inicjatora</span><span class="sxs-lookup"><span data-stu-id="ed21f-118">Add the seed initializer</span></span>

<span data-ttu-id="ed21f-119">Dodaj inicjator inicjator, tak aby `Main` method in Class metoda *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="ed21f-119">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a><span data-ttu-id="ed21f-120">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ed21f-120">Test the app</span></span>

<span data-ttu-id="ed21f-121">(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ed21f-121">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="ed21f-122">Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ed21f-122">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="ed21f-123">Aplikacja zawiera wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="ed21f-123">The app shows the seeded data.</span></span>
