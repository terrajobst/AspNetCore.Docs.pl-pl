Wygenerowany kod bazy danych tożsamości wymaga [migracji Entity Framework Core](/ef/core/managing-schemas/migrations/). Utwórz migracji i aktualizują bazę danych. Na przykład uruchom następujące polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W programie Visual Studio **Konsola Menedżera pakietów**:

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Parametr name "CreateIdentitySchema" dla polecenia `Add-Migration` jest dowolny. `"CreateIdentitySchema"` Opis migracji.
