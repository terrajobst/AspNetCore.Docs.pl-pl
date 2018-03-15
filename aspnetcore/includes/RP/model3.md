<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="da366-101">Dodawanie szkieletu narzędzi i przeprowadzić migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="da366-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="da366-102">W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="da366-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="da366-103">`add package` Polecenie powoduje zainstalowanie narzędzi wymagane do uruchamiania aparatu szkieletów.</span><span class="sxs-lookup"><span data-stu-id="da366-103">The `add package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="da366-104">`ef migrations add InitialCreate` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="da366-104">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="da366-105">Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="da366-105">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="da366-106">`InitialCreate` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="da366-106">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="da366-107">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="da366-107">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="da366-108">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="da366-108">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="da366-109">`ef database update` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _InitialCreate.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="da366-109">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
