---
ms.openlocfilehash: 86cf1874677dc8b79e3223fb0819eb1881c69a11
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265597"
---
<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="9b524-101">Dodawanie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="9b524-101">Add a database context class</span></span>

<span data-ttu-id="9b524-102">Dodaj następujący kod `RazorPagesMovieContext` klasy *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="9b524-102">Add the following `RazorPagesMovieContext` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9b524-103">Powyższy kod tworzy `DbSet` właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="9b524-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="9b524-104">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="9b524-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="9b524-105">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="9b524-105">Add a database connection string</span></span>

<span data-ttu-id="9b524-106">Dodaj parametry połączenia, aby *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="9b524-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="9b524-107">Dodawanie wymaganych pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="9b524-107">Add required NuGet packages</span></span>

<span data-ttu-id="9b524-108">Uruchom następujące polecenie interfejsu wiersza polecenia platformy .NET Core, aby dodać bazy danych SQLite i CodeGeneration.Design do projektu:</span><span class="sxs-lookup"><span data-stu-id="9b524-108">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="9b524-109">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakietu jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="9b524-109">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="9b524-110">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="9b524-110">Register the database context</span></span>

<span data-ttu-id="9b524-111">Dodaj następujący kod `using` instrukcji na górze *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b524-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="9b524-112">Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b524-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="9b524-113">Skompiluj projekt, w celu sprawdzenia błędów.</span><span class="sxs-lookup"><span data-stu-id="9b524-113">Build the project as a check for errors.</span></span>
