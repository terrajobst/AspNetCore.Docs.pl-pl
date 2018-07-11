<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Dodawanie szkieletu narzędzi i wykonywania początkowej migracji

W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`add package` Polecenie instaluje narzędzia wymagane do uruchamiania aparatu tworzenia szkieletów.

`ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych. Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MovieContext.cs* pliku). `InitialCreate` Argument jest używany do nazywania migracje. Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku, który tworzy bazę danych.
