<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="84714-101">Dodaj klasę kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="84714-101">Add a database context class</span></span>

<span data-ttu-id="84714-102">W projekcie RazorPagesMovie Utwórz nowy folder o nazwie *dane*.</span><span class="sxs-lookup"><span data-stu-id="84714-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="84714-103">Dodaj następującą klasę `RazorPagesMovieContext` do folderu *danych* :</span><span class="sxs-lookup"><span data-stu-id="84714-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="84714-104">Poprzedni kod tworzy właściwość `DbSet` dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="84714-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="84714-105">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych, a jednostka odpowiada wierszowi w tabeli.</span><span class="sxs-lookup"><span data-stu-id="84714-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="84714-106">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="84714-106">Add a database connection string</span></span>

<span data-ttu-id="84714-107">Dodaj parametry połączenia do pliku *appSettings. JSON* , jak pokazano w następującym wyróżnionym kodzie:</span><span class="sxs-lookup"><span data-stu-id="84714-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="84714-108">Dodawanie pakietów NuGet i narzędzi EF</span><span class="sxs-lookup"><span data-stu-id="84714-108">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="84714-109">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="84714-109">Register the database context</span></span>

<span data-ttu-id="84714-110">Dodaj następujące instrukcje `using` w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84714-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="84714-111">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="84714-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="84714-112">Dodaj wymagane pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="84714-112">Add required NuGet packages</span></span>

<span data-ttu-id="84714-113">Uruchom następujące polecenie interfejs wiersza polecenia platformy .NET Core, aby dodać program SQLite i CodeGeneration. Design do projektu:</span><span class="sxs-lookup"><span data-stu-id="84714-113">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="84714-114">Pakiet `Microsoft.VisualStudio.Web.CodeGeneration.Design` jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="84714-114">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="84714-115">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="84714-115">Register the database context</span></span>

<span data-ttu-id="84714-116">Dodaj następujące instrukcje `using` w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="84714-116">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="84714-117">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="84714-117">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="84714-118">Kompiluj projekt jako sprawdzenie pod kątem błędów.</span><span class="sxs-lookup"><span data-stu-id="84714-118">Build the project as a check for errors.</span></span>

::: moniker-end
