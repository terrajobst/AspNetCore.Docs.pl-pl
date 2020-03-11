<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="ced87-101">Dodaj klasę kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="ced87-101">Add a database context class</span></span>

<span data-ttu-id="ced87-102">W projekcie RazorPagesMovie Utwórz nowy folder o nazwie *dane*.</span><span class="sxs-lookup"><span data-stu-id="ced87-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="ced87-103">Dodaj następującą klasę `RazorPagesMovieContext` do folderu *danych* :</span><span class="sxs-lookup"><span data-stu-id="ced87-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="ced87-104">Poprzedni kod tworzy właściwość `DbSet` dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="ced87-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="ced87-105">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych, a jednostka odpowiada wierszowi w tabeli.</span><span class="sxs-lookup"><span data-stu-id="ced87-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="ced87-106">Kod nie zostanie skompilowany do momentu dodania zależności w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="ced87-106">The code won't compile until dependencies are added in a later step.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="ced87-107">Dodaj parametry połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="ced87-107">Add a database connection string</span></span>

<span data-ttu-id="ced87-108">Dodaj parametry połączenia do pliku *appSettings. JSON* , jak pokazano w następującym wyróżnionym kodzie:</span><span class="sxs-lookup"><span data-stu-id="ced87-108">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="ced87-109">Dodawanie pakietów NuGet i narzędzi EF</span><span class="sxs-lookup"><span data-stu-id="ced87-109">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="ced87-110">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="ced87-110">Register the database context</span></span>

<span data-ttu-id="ced87-111">Dodaj następujące instrukcje `using` w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ced87-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="ced87-112">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ced87-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="ced87-113">Dodaj wymagane pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="ced87-113">Add required NuGet packages</span></span>

<span data-ttu-id="ced87-114">Uruchom następujące polecenie interfejs wiersza polecenia platformy .NET Core, aby dodać program SQLite i CodeGeneration. Design do projektu:</span><span class="sxs-lookup"><span data-stu-id="ced87-114">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="ced87-115">Pakiet `Microsoft.VisualStudio.Web.CodeGeneration.Design` jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="ced87-115">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="ced87-116">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="ced87-116">Register the database context</span></span>

<span data-ttu-id="ced87-117">Dodaj następujące instrukcje `using` w górnej części *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ced87-117">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="ced87-118">Zarejestruj kontekst bazy danych z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ced87-118">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="ced87-119">Kompiluj projekt jako sprawdzenie pod kątem błędów.</span><span class="sxs-lookup"><span data-stu-id="ced87-119">Build the project as a check for errors.</span></span>

::: moniker-end
