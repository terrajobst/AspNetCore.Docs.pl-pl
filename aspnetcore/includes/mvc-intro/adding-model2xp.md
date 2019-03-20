---
ms.openlocfilehash: 4a569a3f7f804919b7fd6e481c2ec5a3acdfbbe8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265531"
---
<a name="cli"></a>

## <a name="perform-initial-migration"></a>Przeprowadzenie początkowej migracji

W wierszu polecenia Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

`dotnet ef migrations add InitialCreate` Polecenie generuje kod, aby utworzyć schemat początkowej bazy danych. Schemat jest oparta na modelu, określone w `DbContext` (w *Models/MvcMovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracje. Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`dotnet ef database update` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _InitialCreate.cs* pliku, który tworzy bazę danych.
