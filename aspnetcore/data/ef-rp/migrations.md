---
title: Strony razor z programem EF Core w programie ASP.NET Core - Migrations - 4, 8
author: rick-anderson
description: W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: 5848e5e1e45708c3ab5c2a79614111662701aa77
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320163"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core - Migrations - 4, 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Przez [Tom Dykstra](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

W tym samouczku jest używana funkcja migracje EF Core do zarządzania zmianami modelu danych.

Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

Gdy nowa aplikacja jest rozwinięte, dane często modelu zmiany. Każdorazowo zostanie zmieniony na model model jest niezsynchronizowana z bazą danych. W tym samouczku jest uruchamiany przez konfigurowanie platformy Entity Framework do tworzenia bazy danych, jeśli nie istnieje. Każdorazowo podczas zmiany modelu danych:

* Baza danych zostanie usunięte.
* EF utworzony zostaje nowy indeks, który pasuje do modelu.
* Aplikacja inicjowania inicjuje bazy danych z danymi.

To podejście, synchronizacja bazy danych z modelem danych działa poprawnie, dopóki wdrożyć aplikację do środowiska produkcyjnego. Gdy aplikacja jest uruchomiona w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które musi być zachowana. Aplikacja nie może rozpoczynać się od testu DB każdorazowo zostanie wprowadzona zmiana (np. dodanie nowej kolumny). Funkcja migracji programu EF Core rozwiązuje ten problem, włączając programu EF Core do zaktualizowania schematu bazy danych zamiast tworzenia nowej bazy danych.

Zamiast porzucenie i ponowne utworzenie bazy danych w przypadku zmiany modelu danych, migracja aktualizuje schemat i zachowuje istniejące dane.

## <a name="drop-the-database"></a>Upuść bazę danych

Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Konsola Menedżera pakietów** (PMC), uruchom następujące polecenie:

```PMC
Drop-Database
```

Uruchom `Get-Help about_EntityFrameworkCore` z konsoli zarządzania Pakietami, aby uzyskać informacje pomocy.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Otwórz okno polecenia i przejdź do folderu projektu. Folder projektu zawiera *Startup.cs* pliku.

W oknie wiersza polecenia, należy wprowadzić następujące czynności:

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a>Tworzenie początkowej migracji i aktualizowanie bazy danych

Skompiluj projekt i Utwórz pierwsze migracji.

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

---

### <a name="examine-the-up-and-down-methods"></a>Sprawdź w górę i w dół metody

EF Core `migrations add` polecenia wygenerowany kod w celu utworzenia bazy danych. Ten kod migracji znajduje się w *migracje\<sygnatura czasowa > _InitialCreate.cs* pliku. `Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawy jednostek modelu danych. `Down` Metoda spowoduje usunięcie ich, jak pokazano w poniższym przykładzie:

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Migracje wywołania `Up` metodę, aby wdrożyć model danych zmienia się do migracji. Po wprowadzeniu polecenia można wycofać aktualizację, wywołania migracje `Down` metody.

Powyższy kod jest początkowej migracji. Ten kod został utworzony, kiedy `migrations add InitialCreate` polecenia. Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku. Nazwa migracji może być dowolną prawidłową nazwę pliku. Najlepiej wybrać wyrazu lub frazy, który podsumowuje, co to jest wykonywana w procesie migracji. Na przykład migracji, która dodana tabela działu może mieć nazwę "AddDepartmentTable."

Jeśli początkowej migracji jest utworzony i czy istnieje baza danych:

* Kod tworzenia bazy danych jest generowany.
* Kod tworzenia bazy danych nie muszą zostać uruchomione, ponieważ baza danych już jest zgodna z modelem danych. Kod tworzenia bazy danych jest uruchamiana, nie wprowadza żadnych zmian, ponieważ baza danych już jest zgodna z modelem danych.

Po wdrożeniu aplikacji do nowego środowiska do tworzenia bazy danych należy uruchomić kod tworzenia bazy danych.

Wcześniej baza danych została porzucona i nie istnieje, więc migracji powoduje utworzenie nowej bazy danych.

### <a name="the-data-model-snapshot"></a>Migawka modelu danych

Utwórz migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*. Podczas dodawania migracji EF Określa, co zmieniło się przez porównanie modelu danych do pliku migawki.

Aby usunąć migracji, użyj następującego polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Usuń migrację

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Aby uzyskać więcej informacji, zobacz [Usuń migracji ef dotnet](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

---

Polecenie migracji Usuń Usuwa migracji i gwarantuje, że migawka poprawnie zostanie zresetowana.

### <a name="remove-ensurecreated-and-test-the-app"></a>Usuń EnsureCreated i testowanie aplikacji

Wczesne rozwoju `EnsureCreated` był używany. W tym samouczku migracje są używane. `EnsureCreated` obowiązują następujące ograniczenia:

* Pomija migrację i tworzy bazy danych i schematu.
* Nie tworzy tabeli migracji.
* Można *nie* można używać z migracji.
* Zaprojektowano na potrzeby testowania lub szybkiego tworzenia prototypów gdzie usunięty i utworzony ponownie często bazy danych.

Usuń następujący wiersz z `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

Uruchom aplikację i sprawdź, czy baza danych jest obsługiwany.

### <a name="inspect-the-database"></a>Inspekcja bazy danych

Użyj **Eksplorator obiektów SQL Server** do inspekcji bazy danych. Zwróć uwagę, dodanie `__EFMigrationsHistory` tabeli. `__EFMigrationsHistory` Tabeli śledzi informacje o migracji, które zostały zastosowane do bazy danych. Wyświetlanie danych w `__EFMigrationsHistory` tabeli zawiera jeden wiersz dla pierwszej migracji. Ostatni dziennik w powyższym przykładzie danych wyjściowych interfejsu wiersza polecenia zawiera instrukcji INSERT, która tworzy tego wiersza.

Uruchom aplikację i sprawdzić, czy wszystko działa.

## <a name="applying-migrations-in-production"></a>Stosowanie migracji w środowisku produkcyjnym

Firma Microsoft zaleca aplikacji produkcyjnych należy **nie** wywołania [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) przy uruchamianiu aplikacji. `Migrate` Nie można wywołać z aplikacji w farmie serwerów. Na przykład, jeśli aplikacja została chmurę wdrożona za pomocą skalowalnego w poziomie (wykonywania wielu wystąpień aplikacji).

Migracja bazy danych powinno być wykonywane w ramach wdrożenia, a w sposób kontrolowany. Podejścia do produkcji bazy danych migracji obejmują:

* Tworzenie skryptów SQL przy użyciu migracji i za pomocą skryptów SQL we wdrożeniu.
* Uruchamianie `dotnet ef database update` w kontrolowanym środowisku.

Używa programu EF Core `__MigrationsHistory` tabelę, aby sprawdzić wszystkie migracje trzeba uruchomić. Jeśli baza danych jest aktualne, migracja nie zostanie uruchomiony.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Aplikacja wygeneruje następujący wyjątek:

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Rozwiązanie: Uruchom `dotnet ef database update`

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja usługi YouTube w tym samouczku](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* [.NET core interfejsu wiersza polecenia](/ef/core/miscellaneous/cli/dotnet).
* [Konsola menedżera pakietów (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/sort-filter-page)
> [dalej](xref:data/ef-rp/complex-data-model)
