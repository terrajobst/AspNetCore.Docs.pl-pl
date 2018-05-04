<span data-ttu-id="db6e8-101">Dodaj następujące właściwości `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="db6e8-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="db6e8-102">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="db6e8-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="db6e8-103">Dodaj klasę kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="db6e8-103">Add a database context class</span></span>

<span data-ttu-id="db6e8-104">Dodaj następujące *MovieContext.cs* klasy do *modele* folderu: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="db6e8-104">Add the following  *MovieContext.cs* class to the *Models* folder: [!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="db6e8-105">Poprzedni kod tworzy `DbSet` właściwości dla zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="db6e8-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="db6e8-106">W terminologii programu Entity Framework zwykle zestawu jednostek odnosi się do tabeli bazy danych, a jednostka odpowiada wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="db6e8-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
