<span data-ttu-id="a4f7c-101">Wygenerowany kod tożsamości w bazie danych wymaga [Entity Framework Core migracje](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="a4f7c-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="a4f7c-102">Utwórz migracji i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a4f7c-102">Create a migration and update the database.</span></span> <span data-ttu-id="a4f7c-103">Na przykład uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a4f7c-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4f7c-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4f7c-104">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="a4f7c-105">W programie Visual Studio **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="a4f7c-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4f7c-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a4f7c-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

<span data-ttu-id="a4f7c-107">Parametr name "CreateIdentitySchema" `Add-Migration` jest dowolnego polecenia.</span><span class="sxs-lookup"><span data-stu-id="a4f7c-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="a4f7c-108">""CreateIdentitySchema"opisano migrację.</span><span class="sxs-lookup"><span data-stu-id="a4f7c-108">\`"CreateIdentitySchema" describes the migration.</span></span>