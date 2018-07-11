<span data-ttu-id="80617-101">Dodaj następujące właściwości do `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="80617-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="80617-102">`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="80617-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="80617-103">Dodawanie klasy kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="80617-103">Add a database context class</span></span>

<span data-ttu-id="80617-104">Dodaj następujący kod *MovieContext.cs* klasy *modeli* folderu:</span><span class="sxs-lookup"><span data-stu-id="80617-104">Add the following *MovieContext.cs* class to the *Models* folder:</span></span>  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="80617-105">Powyższy kod tworzy `DbSet` właściwość zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="80617-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="80617-106">W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="80617-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
