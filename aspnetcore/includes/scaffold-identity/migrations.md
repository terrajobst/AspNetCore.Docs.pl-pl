Wygenerowany kod bazy danych tożsamości wymaga [migracji Entity Framework Core](/ef/core/managing-schemas/migrations/). Utwórz migracji i aktualizują bazę danych. Na przykład uruchom następujące polecenia:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

W **konsoli Menedżera pakietów**programu Visual Studio:

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Parametr name "CreateIdentitySchema" dla polecenia `Add-Migration` jest dowolny. `"CreateIdentitySchema"` Opis migracji.
