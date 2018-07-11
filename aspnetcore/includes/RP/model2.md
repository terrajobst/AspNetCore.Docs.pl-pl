Dodaj następujące właściwości do `Movie` klasy:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Dodawanie klasy kontekstu bazy danych

Dodaj następujący kod *MovieContext.cs* klasy *modeli* folderu:  

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

Powyższy kod tworzy `DbSet` właściwość zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.
