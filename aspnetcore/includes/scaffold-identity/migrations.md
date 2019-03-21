---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320295"
---
<span data-ttu-id="71544-101">Wygenerowany kod tożsamości w bazie danych wymaga [migracje Entity Framework Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="71544-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="71544-102">Utwórz migracji i aktualizują bazę danych.</span><span class="sxs-lookup"><span data-stu-id="71544-102">Create a migration and update the database.</span></span> <span data-ttu-id="71544-103">Na przykład uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="71544-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71544-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71544-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="71544-105">W programie Visual Studio **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="71544-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="71544-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="71544-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="71544-107">Parametr name "CreateIdentitySchema" `Add-Migration` polecenia jest określana.</span><span class="sxs-lookup"><span data-stu-id="71544-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="71544-108">`"CreateIdentitySchema"` w tym artykule opisano migracji.</span><span class="sxs-lookup"><span data-stu-id="71544-108">`"CreateIdentitySchema"` describes the migration.</span></span>
