<a name="cli"></a>

## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="9f423-101">Dodawanie szkieletu narzędzi i wykonywania początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="9f423-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="9f423-102">Dodaj następujące wiersze do *RazorPagesMovie.csproj* pliku, bezpośrednio przed zamykającym `</Project>` tag:</span><span class="sxs-lookup"><span data-stu-id="9f423-102">Add the following lines to the *RazorPagesMovie.csproj* file, just before the closing `</Project>` tag:</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```
  
<span data-ttu-id="9f423-103">W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="9f423-103">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="9f423-104">`DotNetCliToolReference` Elementu i `add package` polecenia instalacji narzędzia wymagane do uruchamiania aparatu tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="9f423-104">The `DotNetCliToolReference` element and the `add package` command install the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="9f423-105">`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f423-105">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9f423-106">Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="9f423-106">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="9f423-107">`InitialCreate` Argument jest używany do nazywania migracje.</span><span class="sxs-lookup"><span data-stu-id="9f423-107">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="9f423-108">Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="9f423-108">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9f423-109">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9f423-109">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9f423-110">`ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku, który tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="9f423-110">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
