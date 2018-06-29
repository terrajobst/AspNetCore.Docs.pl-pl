---
title: Stron razor podstawowych EF w platformy ASP.NET Core - Migrations - 4, 8
author: rick-anderson
description: W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 15e3bc57e98b249cbefc394bbe1a136a709a03a7
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089961"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Stron razor podstawowych EF w platformy ASP.NET Core - Migrations - 4, 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

W tym samouczku jest używany EF podstawowych funkcji migracji do zarządzania zmianami modelu danych.

Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

Gdy do opracowania nowej aplikacji, danych często modelu zmiany. Zawsze zmiany modelu modelu jest niezsynchronizowana z bazą danych. W tym samouczku uruchomiona przez skonfigurowanie programu Entity Framework, aby utworzyć bazę danych, jeśli nie istnieje. Zawsze danych model zmiany:

* Bazy danych zostało przerwane.
* EF tworzy nową, który jest zgodny z modelem.
* Aplikacja nasion bazę danych z danych testowych.

Takie podejście do przechowywania bazy danych w modelu danych działa poprawnie, dopóki wdrażania aplikacji w środowisku produkcyjnym. Gdy aplikacja działa w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które muszą być kontynuowana. Aplikacja nie może zaczynać się od testu bazy danych zawsze zmianie (na przykład dodawanie nowej kolumny). Funkcji EF Core migracje rozwiązuje ten problem, należy włączyć EF Core zaktualizować schemat bazy danych zamiast tworzenia nowej bazy danych.

Zamiast porzucenie i ponowne utworzenie bazy danych w przypadku zmiany modelu danych, migracje aktualizacje schematu i zachowa istniejące dane.

## <a name="drop-the-database"></a>Porzucenia bazy danych

Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Konsola Menedżera pakietów** (PMC), uruchom następujące polecenie:

```PMC
Drop-Database
```

Uruchom `Get-Help about_EntityFrameworkCore` z PMC, aby uzyskać informacje pomocy.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Otwórz okno polecenia i przejdź do folderu projektu. Folder projektu zawiera *Startup.cs* pliku.

W oknie wiersza polecenia, wprowadź następujące:

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a>Tworzenie początkowej migracji i aktualizowanie bazy danych

Skompiluj projekt i Utwórz pierwszy migracji.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a>Sprawdź w górę i w dół metody

Podstawowe EF `migrations add` polecenia wygenerowany kod w celu utworzenia bazy danych. Ten kod migracji znajduje się w *migracje\<sygnatury czasowej > _InitialCreate.cs* pliku. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych. `Down` Metoda usuwa je, jak pokazano w poniższym przykładzie:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji. Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.

Poprzedni kod jest początkowej migracji. Ten kod został utworzony podczas `migrations add InitialCreate` polecenia. Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku. Nazwa migracji może być dowolną prawidłową nazwę pliku. Najlepiej wybrać słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji. Na przykład migracji, które dodano tabelę działu może mieć nazwę "AddDepartmentTable."

Jeśli zostanie utworzona początkowa migracji i istnieje w bazie danych:

* Zostaje wygenerowany kod tworzenia bazy danych.
* Kod tworzenia bazy danych nie wymaga się uruchomić, ponieważ bazy danych jest już zgodny z modelem danych. Jeśli uruchomiony jest kod tworzenia bazy danych, nie wprowadzać żadnych zmian, ponieważ bazy danych jest już zgodny z modelem danych.

Po wdrożeniu aplikacji do nowego środowiska, aby utworzyć bazę danych należy uruchomić kod tworzenia bazy danych.

Wcześniej bazy danych został porzucony, a nie istnieje, więc migracji tworzy nowe bazy danych.

### <a name="the-data-model-snapshot"></a>Migawki modelu danych

Utwórz migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*. Po dodaniu migracji EF określa co zmienione przez porównanie modelu danych do pliku migawki.

Aby usunąć migracji, użyj następującego polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Usuń migracji

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Aby uzyskać więcej informacji, zobacz [Usuń migracje ef dotnet](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

------

Usuń polecenie migracje usuwa migracji i gwarantuje, że poprawnie zresetowania migawki.

### <a name="remove-ensurecreated-and-test-the-app"></a>Usuń EnsureCreated i testowanie aplikacji

Wczesne rozwoju `EnsureCreated` została użyta. W tym samouczku są używane migracji. `EnsureCreated` ma następujące ograniczenia:

* Pomija migracji i tworzy bazę danych i schematu.
* Nie tworzy tabelę migracji.
* Można *nie* można używać z migracji.
* Jest przeznaczony do testowania lub szybkie tworzenie prototypów gdzie bazy danych jest porzucona i utworzona ponownie często.

Usuń następujący wiersz z `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

Uruchom aplikację i sprawdzić, czy bazy danych jest obsługiwany.

### <a name="inspect-the-database"></a>Inspekcja bazy danych

Użyj **Eksplorator obiektów SQL Server** przeprowadzać inspekcję bazy danych. Zwróć uwagę, dodanie `__EFMigrationsHistory` tabeli. `__EFMigrationsHistory` Tabeli przechowuje informacje o migracji, które zostały zastosowane do bazy danych. Wyświetlanie danych w `__EFMigrationsHistory` tabeli, zawiera jeden wiersz dla pierwszej migracji. Ostatni dziennika w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia zawiera instrukcji INSERT, która tworzy tego wiersza.

Uruchom aplikację i sprawdź, czy wszystko działa.

## <a name="applying-migrations-in-production"></a>Stosowanie migracji w środowisku produkcyjnym

Firma Microsoft zaleca aplikacji w środowisku produkcyjnym należy **nie** wywołać [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) podczas uruchamiania aplikacji. `Migrate` Nie można wywołać z aplikacji w farmie serwerów. Na przykład, jeśli aplikacja została chmury z skalowalnego w poziomie (uruchomionych wiele wystąpień aplikacji).

Migracja bazy danych powinno być wykonywane w ramach wdrożenia, a następnie w kontrolowany sposób. Podejścia do produkcyjnej bazy danych migracji obejmują:

* Przy użyciu migracji można utworzyć skrypty SQL i skrypty SQL we wdrożeniu.
* Uruchomiona `dotnet ef database update` z kontrolą środowiska.

Używa EF Core `__MigrationsHistory` tabeli, aby wyświetlić wszystkie migracje trzeba uruchamiać. Jeśli bazy danych jest aktualne, migracja nie jest uruchamiany.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Aplikacja generuje następujący wyjątek:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Rozwiązanie: Uruchom `dotnet ef database update`

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie .NET core interfejsu wiersza polecenia](/ef/core/miscellaneous/cli/dotnet).
* [Konsola menedżera pakietów (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/sort-filter-page)
> [dalej](xref:data/ef-rp/complex-data-model)