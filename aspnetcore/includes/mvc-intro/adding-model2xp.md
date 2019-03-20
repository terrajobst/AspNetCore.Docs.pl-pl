---
ms.openlocfilehash: 4a569a3f7f804919b7fd6e481c2ec5a3acdfbbe8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265531"
---
<a name="cli"></a>

## <a name="perform-initial-migration"></a><span data-ttu-id="8096b-101">Przeprowadzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="8096b-101">Perform initial migration</span></span>

<span data-ttu-id="8096b-102">W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="8096b-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="8096b-103">`dotnet ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8096b-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="8096b-104">Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MvcMovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="8096b-104">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="8096b-105">`Initial` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="8096b-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="8096b-106">Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="8096b-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="8096b-107">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8096b-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="8096b-108">`dotnet ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="8096b-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
