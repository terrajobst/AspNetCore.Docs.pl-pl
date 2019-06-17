<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="9243e-101">Dodawanie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="9243e-101">Add a database context class</span></span>

<span data-ttu-id="9243e-102">Dodaj następujący kod `RazorPagesMovieContext` klasy *danych* folderu:</span><span class="sxs-lookup"><span data-stu-id="9243e-102">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9243e-103">Powyższy kod tworzy `DbSet` właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="9243e-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="9243e-104">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="9243e-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="9243e-105">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="9243e-105">Add a database connection string</span></span>

<span data-ttu-id="9243e-106">Dodaj parametry połączenia, aby *appsettings.json* pliku, jak pokazano na następujący wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="9243e-106">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="9243e-107">Dodawanie wymaganych pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="9243e-107">Add required NuGet packages</span></span>

<span data-ttu-id="9243e-108">Uruchom następujące polecenie interfejsu wiersza polecenia platformy .NET Core, aby dodać bazy danych SQLite i CodeGeneration.Design do projektu:</span><span class="sxs-lookup"><span data-stu-id="9243e-108">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="9243e-109">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakietu jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="9243e-109">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="9243e-110">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="9243e-110">Register the database context</span></span>

<span data-ttu-id="9243e-111">Dodaj następujący kod `using` instrukcji na górze *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9243e-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="9243e-112">Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9243e-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="9243e-113">Skompiluj projekt, w celu sprawdzenia błędów.</span><span class="sxs-lookup"><span data-stu-id="9243e-113">Build the project as a check for errors.</span></span>
