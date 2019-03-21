---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320295"
---
Wygenerowany kod tożsamości w bazie danych wymaga [migracje Entity Framework Core](/ef/core/managing-schemas/migrations/). Utwórz migracji i aktualizują bazę danych. Na przykład uruchom następujące polecenia:

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

---

Parametr name "CreateIdentitySchema" `Add-Migration` polecenia jest określana. `"CreateIdentitySchema"` w tym artykule opisano migracji.
