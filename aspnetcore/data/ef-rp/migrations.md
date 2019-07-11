---
title: Strony razor z programem EF Core w programie ASP.NET Core - Migrations - 4, 8
author: rick-anderson
description: W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 06/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: b887be91a0c8332e7bd5fb7b1acc90eae138d9ed
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813629"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="c0567-103">Strony razor z programem EF Core w programie ASP.NET Core - Migrations - 4, 8</span><span class="sxs-lookup"><span data-stu-id="c0567-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="c0567-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0567-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="c0567-105">W tym samouczku jest używana funkcja migracje EF Core do zarządzania zmianami modelu danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="c0567-106">Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="c0567-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="c0567-107">Gdy nowa aplikacja jest rozwinięte, dane często modelu zmiany.</span><span class="sxs-lookup"><span data-stu-id="c0567-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="c0567-108">Każdorazowo zostanie zmieniony na model model jest niezsynchronizowana z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="c0567-109">W tym samouczku jest uruchamiany przez konfigurowanie platformy Entity Framework do tworzenia bazy danych, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="c0567-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="c0567-110">Każdorazowo podczas zmiany modelu danych:</span><span class="sxs-lookup"><span data-stu-id="c0567-110">Each time the data model changes:</span></span>

* <span data-ttu-id="c0567-111">Baza danych zostanie usunięte.</span><span class="sxs-lookup"><span data-stu-id="c0567-111">The DB is dropped.</span></span>
* <span data-ttu-id="c0567-112">EF utworzony zostaje nowy indeks, który pasuje do modelu.</span><span class="sxs-lookup"><span data-stu-id="c0567-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="c0567-113">Aplikacja inicjowania inicjuje bazy danych z danymi.</span><span class="sxs-lookup"><span data-stu-id="c0567-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="c0567-114">To podejście, synchronizacja bazy danych z modelem danych działa poprawnie, dopóki wdrożyć aplikację do środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="c0567-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="c0567-115">Gdy aplikacja jest uruchomiona w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które musi być zachowana.</span><span class="sxs-lookup"><span data-stu-id="c0567-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="c0567-116">Aplikacja nie może rozpoczynać się od testu DB każdorazowo zostanie wprowadzona zmiana (np. dodanie nowej kolumny).</span><span class="sxs-lookup"><span data-stu-id="c0567-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="c0567-117">Funkcja migracji programu EF Core rozwiązuje ten problem, włączając programu EF Core do zaktualizowania schematu bazy danych zamiast tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="c0567-118">Zamiast porzucenie i ponowne utworzenie bazy danych w przypadku zmiany modelu danych, migracja aktualizuje schemat i zachowuje istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="c0567-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="c0567-119">Upuść bazę danych</span><span class="sxs-lookup"><span data-stu-id="c0567-119">Drop the database</span></span>

<span data-ttu-id="c0567-120">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia:</span><span class="sxs-lookup"><span data-stu-id="c0567-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0567-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0567-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c0567-122">W **Konsola Menedżera pakietów** (PMC), uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c0567-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="c0567-123">Uruchom `Get-Help about_EntityFrameworkCore` z konsoli zarządzania Pakietami, aby uzyskać informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="c0567-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c0567-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c0567-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c0567-125">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="c0567-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="c0567-126">Folder projektu zawiera *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="c0567-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="c0567-127">W oknie wiersza polecenia, należy wprowadzić następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c0567-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

---

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="c0567-128">Tworzenie początkowej migracji i aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="c0567-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="c0567-129">Skompiluj projekt i Utwórz pierwsze migracji.</span><span class="sxs-lookup"><span data-stu-id="c0567-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0567-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0567-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c0567-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c0567-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="c0567-132">Sprawdź w górę i w dół metody</span><span class="sxs-lookup"><span data-stu-id="c0567-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="c0567-133">EF Core `migrations add` polecenia wygenerowany kod w celu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="c0567-134">Ten kod migracji znajduje się w *migracje\<sygnatura czasowa > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="c0567-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="c0567-135">`Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawy jednostek modelu danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="c0567-136">`Down` Metoda spowoduje usunięcie ich, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c0567-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="c0567-137">Migracje wywołania `Up` metodę, aby wdrożyć model danych zmienia się do migracji.</span><span class="sxs-lookup"><span data-stu-id="c0567-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="c0567-138">Po wprowadzeniu polecenia można wycofać aktualizację, wywołania migracje `Down` metody.</span><span class="sxs-lookup"><span data-stu-id="c0567-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="c0567-139">Powyższy kod jest początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="c0567-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="c0567-140">Ten kod został utworzony, kiedy `migrations add InitialCreate` polecenia.</span><span class="sxs-lookup"><span data-stu-id="c0567-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="c0567-141">Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="c0567-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="c0567-142">Nazwa migracji może być dowolną prawidłową nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="c0567-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="c0567-143">Najlepiej wybrać wyrazu lub frazy, który podsumowuje, co to jest wykonywana w procesie migracji.</span><span class="sxs-lookup"><span data-stu-id="c0567-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="c0567-144">Na przykład migracji, która dodana tabela działu może mieć nazwę "AddDepartmentTable."</span><span class="sxs-lookup"><span data-stu-id="c0567-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="c0567-145">Jeśli początkowej migracji jest utworzony i czy istnieje baza danych:</span><span class="sxs-lookup"><span data-stu-id="c0567-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="c0567-146">Kod tworzenia bazy danych jest generowany.</span><span class="sxs-lookup"><span data-stu-id="c0567-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="c0567-147">Kod tworzenia bazy danych nie muszą zostać uruchomione, ponieważ baza danych już jest zgodna z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="c0567-148">Kod tworzenia bazy danych jest uruchamiana, nie wprowadza żadnych zmian, ponieważ baza danych już jest zgodna z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="c0567-149">Po wdrożeniu aplikacji do nowego środowiska do tworzenia bazy danych należy uruchomić kod tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="c0567-150">Wcześniej baza danych została porzucona i nie istnieje, więc migracji powoduje utworzenie nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="c0567-151">Migawka modelu danych</span><span class="sxs-lookup"><span data-stu-id="c0567-151">The data model snapshot</span></span>

<span data-ttu-id="c0567-152">Utwórz migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="c0567-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="c0567-153">Podczas dodawania migracji EF Określa, co zmieniło się przez porównanie modelu danych do pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="c0567-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="c0567-154">Aby usunąć migracji, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="c0567-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0567-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0567-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c0567-156">Usuń migrację</span><span class="sxs-lookup"><span data-stu-id="c0567-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c0567-157">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c0567-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="c0567-158">Aby uzyskać więcej informacji, zobacz [Usuń migracji ef dotnet](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="c0567-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

---

<span data-ttu-id="c0567-159">Polecenie migracji Usuń Usuwa migracji i gwarantuje, że migawka poprawnie zostanie zresetowana.</span><span class="sxs-lookup"><span data-stu-id="c0567-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="c0567-160">Usuń EnsureCreated i testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c0567-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="c0567-161">Wczesne rozwoju `EnsureCreated` był używany.</span><span class="sxs-lookup"><span data-stu-id="c0567-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="c0567-162">W tym samouczku migracje są używane.</span><span class="sxs-lookup"><span data-stu-id="c0567-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="c0567-163">`EnsureCreated` obowiązują następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="c0567-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="c0567-164">Pomija migrację i tworzy bazy danych i schematu.</span><span class="sxs-lookup"><span data-stu-id="c0567-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="c0567-165">Nie tworzy tabeli migracji.</span><span class="sxs-lookup"><span data-stu-id="c0567-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="c0567-166">Można *nie* można używać z migracji.</span><span class="sxs-lookup"><span data-stu-id="c0567-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="c0567-167">Zaprojektowano na potrzeby testowania lub szybkiego tworzenia prototypów gdzie usunięty i utworzony ponownie często bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="c0567-168">Usuń `EnsureCreated`:</span><span class="sxs-lookup"><span data-stu-id="c0567-168">Remove `EnsureCreated`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="c0567-169">Uruchom aplikację i sprawdź, czy baza danych jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="c0567-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="c0567-170">Inspekcja bazy danych</span><span class="sxs-lookup"><span data-stu-id="c0567-170">Inspect the database</span></span>

<span data-ttu-id="c0567-171">Użyj **Eksplorator obiektów SQL Server** do inspekcji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="c0567-172">Zwróć uwagę, dodanie `__EFMigrationsHistory` tabeli.</span><span class="sxs-lookup"><span data-stu-id="c0567-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="c0567-173">`__EFMigrationsHistory` Tabeli śledzi informacje o migracji, które zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c0567-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="c0567-174">Wyświetlanie danych w `__EFMigrationsHistory` tabeli zawiera jeden wiersz dla pierwszej migracji.</span><span class="sxs-lookup"><span data-stu-id="c0567-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="c0567-175">Ostatni dziennik w powyższym przykładzie danych wyjściowych interfejsu wiersza polecenia zawiera instrukcji INSERT, która tworzy tego wiersza.</span><span class="sxs-lookup"><span data-stu-id="c0567-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="c0567-176">Uruchom aplikację i sprawdzić, czy wszystko działa.</span><span class="sxs-lookup"><span data-stu-id="c0567-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="c0567-177">Stosowanie migracji w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="c0567-177">Applying migrations in production</span></span>

<span data-ttu-id="c0567-178">Firma Microsoft zaleca aplikacji produkcyjnych należy **nie** wywołania [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) przy uruchamianiu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0567-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="c0567-179">`Migrate` Nie można wywołać z aplikacji w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="c0567-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="c0567-180">Na przykład, jeśli aplikacja została chmurę wdrożona za pomocą skalowalnego w poziomie (wykonywania wielu wystąpień aplikacji).</span><span class="sxs-lookup"><span data-stu-id="c0567-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="c0567-181">Migracja bazy danych powinno być wykonywane w ramach wdrożenia, a w sposób kontrolowany.</span><span class="sxs-lookup"><span data-stu-id="c0567-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="c0567-182">Podejścia do produkcji bazy danych migracji obejmują:</span><span class="sxs-lookup"><span data-stu-id="c0567-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="c0567-183">Tworzenie skryptów SQL przy użyciu migracji i za pomocą skryptów SQL we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="c0567-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="c0567-184">Uruchamianie `dotnet ef database update` w kontrolowanym środowisku.</span><span class="sxs-lookup"><span data-stu-id="c0567-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="c0567-185">Używa programu EF Core `__MigrationsHistory` tabelę, aby sprawdzić wszystkie migracje trzeba uruchomić.</span><span class="sxs-lookup"><span data-stu-id="c0567-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="c0567-186">Jeśli baza danych jest aktualne, migracja nie zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="c0567-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c0567-187">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="c0567-187">Troubleshooting</span></span>

<span data-ttu-id="c0567-188">Pobierz [ukończonej aplikacji](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="c0567-188">Download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="c0567-189">Aplikacja wygeneruje następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="c0567-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="c0567-190">Rozwiązanie: Uruchom `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="c0567-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="c0567-191">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c0567-191">Additional resources</span></span>

* [<span data-ttu-id="c0567-192">Wersja usługi YouTube w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="c0567-192">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* <span data-ttu-id="c0567-193">[.NET core interfejsu wiersza polecenia](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="c0567-193">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="c0567-194">Konsola menedżera pakietów (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="c0567-194">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)



> [!div class="step-by-step"]
> <span data-ttu-id="c0567-195">[Poprzednie](xref:data/ef-rp/sort-filter-page)
> [dalej](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="c0567-195">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
