<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="e4413-101">Dodaj klasę kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="e4413-101">Add a database context class</span></span>

<span data-ttu-id="e4413-102">Dodaj następującą `RazorPagesMovieContext` klasę do folderu *danych* :</span><span class="sxs-lookup"><span data-stu-id="e4413-102">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="e4413-103">Poprzedni kod tworzy `DbSet` Właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="e4413-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="e4413-104">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych, a jednostka odpowiada wierszowi w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e4413-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="e4413-105">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="e4413-105">Add a database connection string</span></span>

<span data-ttu-id="e4413-106">Dodaj parametry połączenia do pliku *appSettings. JSON* , jak pokazano w następującym wyróżnionym kodzie:</span><span class="sxs-lookup"><span data-stu-id="e4413-106">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="e4413-107">Dodaj wymagane pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="e4413-107">Add required NuGet packages</span></span>

<span data-ttu-id="e4413-108">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core, aby dodać do projektu oprogramowania SQLite, Entity Framework Core i CodeGeneration. Design:</span><span class="sxs-lookup"><span data-stu-id="e4413-108">Run the following .NET Core CLI commands to add SQLite, Entity Framework Core, and  CodeGeneration.Design to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="e4413-109">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakiet jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="e4413-109">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="e4413-110">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="e4413-110">Register the database context</span></span>

<span data-ttu-id="e4413-111">Dodaj następujące `using` instrukcje w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4413-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="e4413-112">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4413-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="e4413-113">Dodaj wymagane pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="e4413-113">Add required NuGet packages</span></span>

<span data-ttu-id="e4413-114">Uruchom następujące polecenie interfejs wiersza polecenia platformy .NET Core, aby dodać program SQLite i CodeGeneration. Design do projektu:</span><span class="sxs-lookup"><span data-stu-id="e4413-114">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="e4413-115">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakiet jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="e4413-115">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="e4413-116">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="e4413-116">Register the database context</span></span>

<span data-ttu-id="e4413-117">Dodaj następujące `using` instrukcje w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4413-117">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="e4413-118">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e4413-118">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="e4413-119">Kompiluj projekt jako sprawdzenie pod kątem błędów.</span><span class="sxs-lookup"><span data-stu-id="e4413-119">Build the project as a check for errors.</span></span>
::: moniker-end