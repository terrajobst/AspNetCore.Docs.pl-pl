Wygenerowany kod tożsamości w bazie danych wymaga [Entity Framework Core migracje](/ef/core/managing-schemas/migrations/). Utwórz migracji i aktualizacji bazy danych. Na przykład uruchom następujące polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

W programie Visual Studio **Konsola Menedżera pakietów**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

Parametr name "CreateIdentitySchema" `Add-Migration` jest dowolnego polecenia. ""CreateIdentitySchema"opisano migrację.