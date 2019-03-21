---
ms.openlocfilehash: 82c0f3076e846045ca85697e150ff0ab9be75dba
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58214256"
---
<a name="dc"></a>

<span data-ttu-id="66aa3-101">Dodaj następujący kod `MvcMovieContext` klasy *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="66aa3-101">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="66aa3-102">Powyższy kod tworzy `DbSet` właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="66aa3-102">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="66aa3-103">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="66aa3-103">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="66aa3-104">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="66aa3-104">Add a database connection string</span></span>

<span data-ttu-id="66aa3-105">Dodaj parametry połączenia, aby *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="66aa3-105">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="66aa3-106">Dodawanie wymaganych pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="66aa3-106">Add required NuGet packages</span></span>

<span data-ttu-id="66aa3-107">Uruchom następujące polecenie interfejsu wiersza polecenia platformy .NET Core, aby dodać bazy danych SQLite i CodeGeneration.Design do projektu:</span><span class="sxs-lookup"><span data-stu-id="66aa3-107">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="66aa3-108">`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakietu jest wymagany do tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="66aa3-108">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="66aa3-109">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="66aa3-109">Register the database context</span></span>

<span data-ttu-id="66aa3-110">Dodaj następujący kod `using` instrukcji na górze *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66aa3-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="66aa3-111">Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="66aa3-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="66aa3-112">Skompiluj projekt, w celu sprawdzenia błędów.</span><span class="sxs-lookup"><span data-stu-id="66aa3-112">Build the project as a check for errors.</span></span>