<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="32c43-101">Dodaj klasę kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="32c43-101">Add a database context class</span></span>

<span data-ttu-id="32c43-102">W projekcie RazorPagesMovie Utwórz nowy folder o nazwie *dane*.</span><span class="sxs-lookup"><span data-stu-id="32c43-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="32c43-103">Dodaj następującą `RazorPagesMovieContext` klasę do folderu *danych* :</span><span class="sxs-lookup"><span data-stu-id="32c43-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="32c43-104">Poprzedni kod tworzy `DbSet` Właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="32c43-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="32c43-105">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych, a jednostka odpowiada wierszowi w tabeli.</span><span class="sxs-lookup"><span data-stu-id="32c43-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="32c43-106">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="32c43-106">Add a database connection string</span></span>

<span data-ttu-id="32c43-107">Dodaj parametry połączenia do pliku *appSettings. JSON* , jak pokazano w następującym wyróżnionym kodzie:</span><span class="sxs-lookup"><span data-stu-id="32c43-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="32c43-108">Dodawanie pakietów NuGet i narzędzi EF</span><span class="sxs-lookup"><span data-stu-id="32c43-108">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="32c43-109">Otwórz Terminal dla projektu RazorPagesMovie.</span><span class="sxs-lookup"><span data-stu-id="32c43-109">Open a terminal for the RazorPagesMovie project.</span></span>  <span data-ttu-id="32c43-110">Kliknij prawym przyciskiem myszy nazwę projektu na pasku projektowania/układu i przejdź do pozycji **narzędzia > Otwórz** w terminalu.</span><span class="sxs-lookup"><span data-stu-id="32c43-110">Right click the project name in the design/layout bar and go to **Tools > Open** in Terminal.</span></span> <span data-ttu-id="32c43-111">Uruchom następujące polecenia interfejs wiersza polecenia platformy .NET Core w tym czasie:</span><span class="sxs-lookup"><span data-stu-id="32c43-111">Run the following .NET Core CLI commands in the Termial:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="32c43-112">Powyższe polecenia umożliwiają dodanie Entity Framework Core narzędzi dla interfejsu wiersza polecenia platformy .NET i kilku pakietów do projektu.</span><span class="sxs-lookup"><span data-stu-id="32c43-112">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="32c43-113">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakiet jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="32c43-113">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="32c43-114">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="32c43-114">Register the database context</span></span>

<span data-ttu-id="32c43-115">Dodaj następujące `using` instrukcje w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32c43-115">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="32c43-116">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="32c43-116">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="32c43-117">Dodaj wymagane pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="32c43-117">Add required NuGet packages</span></span>

<span data-ttu-id="32c43-118">Uruchom następujące polecenie interfejs wiersza polecenia platformy .NET Core, aby dodać program SQLite i CodeGeneration. Design do projektu:</span><span class="sxs-lookup"><span data-stu-id="32c43-118">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="32c43-119">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakiet jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="32c43-119">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="32c43-120">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="32c43-120">Register the database context</span></span>

<span data-ttu-id="32c43-121">Dodaj następujące `using` instrukcje w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="32c43-121">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="32c43-122">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="32c43-122">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="32c43-123">Kompiluj projekt jako sprawdzenie pod kątem błędów.</span><span class="sxs-lookup"><span data-stu-id="32c43-123">Build the project as a check for errors.</span></span>
::: moniker-end
