<a name="cli"></a>
## <a name="perform-initial-migration"></a>Przeprowadź migrację początkowej

W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`dotnet ef migrations add InitialCreate` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych. Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracji. Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`dotnet ef database update` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _InitialCreate.cs* pliku, który utworzy bazę danych.