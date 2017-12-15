Dodaj następujące właściwości `Movie` klasy:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Dodaj klasę kontekstu bazy danych

Dodaj następujące `DbContext` klasy o nazwie *MovieContext.cs* do *modele* folderu:[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

Poprzedni kod tworzy `DbSet` właściwości dla zestawu jednostek. W terminologii programu Entity Framework zwykle zestawu jednostek odnosi się do tabeli bazy danych, a jednostka odpowiada wiersza w tabeli.
