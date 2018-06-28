---
title: Stron razor podstawowych EF w platformy ASP.NET Core - Migrations - 4, 8
author: rick-anderson
description: W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: f1776506ef15c75beb9f1a2579b0073f927b013a
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033254"
---
[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="f0839-103">Stron razor podstawowych EF w platformy ASP.NET Core - Migrations - 4, 8</span><span class="sxs-lookup"><span data-stu-id="f0839-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="f0839-104">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f0839-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="f0839-105">W tym samouczku jest używany EF podstawowych funkcji migracji do zarządzania zmianami modelu danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="f0839-106">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="f0839-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="f0839-107">Gdy do opracowania nowej aplikacji, danych często modelu zmiany.</span><span class="sxs-lookup"><span data-stu-id="f0839-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="f0839-108">Zawsze zmiany modelu modelu jest niezsynchronizowana z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="f0839-109">W tym samouczku uruchomiona przez skonfigurowanie programu Entity Framework, aby utworzyć bazę danych, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="f0839-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="f0839-110">Zawsze danych model zmiany:</span><span class="sxs-lookup"><span data-stu-id="f0839-110">Each time the data model changes:</span></span>

* <span data-ttu-id="f0839-111">Bazy danych zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="f0839-111">The DB is dropped.</span></span>
* <span data-ttu-id="f0839-112">EF tworzy nową, który jest zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="f0839-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="f0839-113">Aplikacja nasion bazę danych z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="f0839-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="f0839-114">Takie podejście do przechowywania bazy danych w modelu danych działa poprawnie, dopóki wdrażania aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f0839-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="f0839-115">Gdy aplikacja działa w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które muszą być kontynuowana.</span><span class="sxs-lookup"><span data-stu-id="f0839-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="f0839-116">Aplikacja nie może zaczynać się od testu bazy danych zawsze zmianie (na przykład dodawanie nowej kolumny).</span><span class="sxs-lookup"><span data-stu-id="f0839-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="f0839-117">Funkcji EF Core migracje rozwiązuje ten problem, należy włączyć EF Core zaktualizować schemat bazy danych zamiast tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="f0839-118">Zamiast porzucenie i ponowne utworzenie bazy danych w przypadku zmiany modelu danych, migracje aktualizacje schematu i zachowa istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="f0839-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="f0839-119">Porzucenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="f0839-119">Drop the database</span></span>

<span data-ttu-id="f0839-120">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia:</span><span class="sxs-lookup"><span data-stu-id="f0839-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0839-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0839-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f0839-122">W **Konsola Menedżera pakietów** (PMC), uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f0839-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="f0839-123">Uruchom `Get-Help about_EntityFrameworkCore` z PMC, aby uzyskać informacje pomocy.</span><span class="sxs-lookup"><span data-stu-id="f0839-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f0839-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f0839-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f0839-125">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="f0839-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="f0839-126">Folder projektu zawiera *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="f0839-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="f0839-127">W oknie wiersza polecenia, wprowadź następujące:</span><span class="sxs-lookup"><span data-stu-id="f0839-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="f0839-128">Tworzenie początkowej migracji i aktualizowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="f0839-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="f0839-129">Skompiluj projekt i Utwórz pierwszy migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0839-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0839-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f0839-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f0839-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="f0839-132">Sprawdź w górę i w dół metody</span><span class="sxs-lookup"><span data-stu-id="f0839-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="f0839-133">Podstawowe EF `migrations add` polecenia wygenerowany kod w celu utworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="f0839-134">Ten kod migracji znajduje się w *migracje\<sygnatury czasowej > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="f0839-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="f0839-135">`Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="f0839-136">`Down` Metoda usuwa je, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f0839-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="f0839-137">Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="f0839-138">Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.</span><span class="sxs-lookup"><span data-stu-id="f0839-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="f0839-139">Poprzedni kod jest początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="f0839-140">Ten kod został utworzony podczas `migrations add InitialCreate` polecenia.</span><span class="sxs-lookup"><span data-stu-id="f0839-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="f0839-141">Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="f0839-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="f0839-142">Nazwa migracji może być dowolną prawidłową nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="f0839-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="f0839-143">Najlepiej wybrać słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="f0839-144">Na przykład migracji, które dodano tabelę działu może mieć nazwę "AddDepartmentTable."</span><span class="sxs-lookup"><span data-stu-id="f0839-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="f0839-145">Jeśli zostanie utworzona początkowa migracji i istnieje w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="f0839-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="f0839-146">Zostaje wygenerowany kod tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="f0839-147">Kod tworzenia bazy danych nie wymaga się uruchomić, ponieważ bazy danych jest już zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="f0839-148">Jeśli uruchomiony jest kod tworzenia bazy danych, nie wprowadzać żadnych zmian, ponieważ bazy danych jest już zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="f0839-149">Po wdrożeniu aplikacji do nowego środowiska, aby utworzyć bazę danych należy uruchomić kod tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="f0839-150">Wcześniej bazy danych został porzucony, a nie istnieje, więc migracji tworzy nowe bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="f0839-151">Migawki modelu danych</span><span class="sxs-lookup"><span data-stu-id="f0839-151">The data model snapshot</span></span>

<span data-ttu-id="f0839-152">Utwórz migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="f0839-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="f0839-153">Po dodaniu migracji EF określa co zmienione przez porównanie modelu danych do pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="f0839-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="f0839-154">Aby usunąć migracji, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="f0839-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0839-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0839-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f0839-156">Usuń migracji</span><span class="sxs-lookup"><span data-stu-id="f0839-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f0839-157">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f0839-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="f0839-158">Aby uzyskać więcej informacji, zobacz [Usuń migracje ef dotnet](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="f0839-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

------

<span data-ttu-id="f0839-159">Usuń polecenie migracje usuwa migracji i gwarantuje, że poprawnie zresetowania migawki.</span><span class="sxs-lookup"><span data-stu-id="f0839-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="f0839-160">Usuń EnsureCreated i testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="f0839-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="f0839-161">Wczesne rozwoju `EnsureCreated` została użyta.</span><span class="sxs-lookup"><span data-stu-id="f0839-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="f0839-162">W tym samouczku są używane migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="f0839-163">`EnsureCreated` ma następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="f0839-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="f0839-164">Pomija migracji i tworzy bazę danych i schematu.</span><span class="sxs-lookup"><span data-stu-id="f0839-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="f0839-165">Nie tworzy tabelę migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="f0839-166">Można *nie* można używać z migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="f0839-167">Jest przeznaczony do testowania lub szybkie tworzenie prototypów gdzie bazy danych jest porzucona i utworzona ponownie często.</span><span class="sxs-lookup"><span data-stu-id="f0839-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="f0839-168">Usuń następujący wiersz z `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="f0839-168">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="f0839-169">Uruchom aplikację i sprawdzić, czy bazy danych jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="f0839-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="f0839-170">Inspekcja bazy danych</span><span class="sxs-lookup"><span data-stu-id="f0839-170">Inspect the database</span></span>

<span data-ttu-id="f0839-171">Użyj **Eksplorator obiektów SQL Server** przeprowadzać inspekcję bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="f0839-172">Zwróć uwagę, dodanie `__EFMigrationsHistory` tabeli.</span><span class="sxs-lookup"><span data-stu-id="f0839-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="f0839-173">`__EFMigrationsHistory` Tabeli przechowuje informacje o migracji, które zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f0839-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="f0839-174">Wyświetlanie danych w `__EFMigrationsHistory` tabeli, zawiera jeden wiersz dla pierwszej migracji.</span><span class="sxs-lookup"><span data-stu-id="f0839-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="f0839-175">Ostatni dziennika w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia zawiera instrukcji INSERT, która tworzy tego wiersza.</span><span class="sxs-lookup"><span data-stu-id="f0839-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="f0839-176">Uruchom aplikację i sprawdź, czy wszystko działa.</span><span class="sxs-lookup"><span data-stu-id="f0839-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="f0839-177">Stosowanie migracji w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="f0839-177">Applying migrations in production</span></span>

<span data-ttu-id="f0839-178">Firma Microsoft zaleca aplikacji w środowisku produkcyjnym należy **nie** wywołać [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f0839-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="f0839-179">`Migrate` Nie można wywołać z aplikacji w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="f0839-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="f0839-180">Na przykład, jeśli aplikacja została chmury z skalowalnego w poziomie (uruchomionych wiele wystąpień aplikacji).</span><span class="sxs-lookup"><span data-stu-id="f0839-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="f0839-181">Migracja bazy danych powinno być wykonywane w ramach wdrożenia, a następnie w kontrolowany sposób.</span><span class="sxs-lookup"><span data-stu-id="f0839-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="f0839-182">Podejścia do produkcyjnej bazy danych migracji obejmują:</span><span class="sxs-lookup"><span data-stu-id="f0839-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="f0839-183">Przy użyciu migracji można utworzyć skrypty SQL i skrypty SQL we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="f0839-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="f0839-184">Uruchomiona `dotnet ef database update` z kontrolą środowiska.</span><span class="sxs-lookup"><span data-stu-id="f0839-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="f0839-185">Używa EF Core `__MigrationsHistory` tabeli, aby wyświetlić wszystkie migracje trzeba uruchamiać.</span><span class="sxs-lookup"><span data-stu-id="f0839-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="f0839-186">Jeśli bazy danych jest aktualne, migracja nie jest uruchamiany.</span><span class="sxs-lookup"><span data-stu-id="f0839-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f0839-187">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="f0839-187">Troubleshooting</span></span>

<span data-ttu-id="f0839-188">Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="f0839-188">Download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="f0839-189">Aplikacja generuje następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="f0839-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="f0839-190">Rozwiązanie: Uruchom `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="f0839-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="f0839-191">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f0839-191">Additional resources</span></span>

* <span data-ttu-id="f0839-192">[Oprogramowanie .NET core interfejsu wiersza polecenia](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="f0839-192">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="f0839-193">Konsola menedżera pakietów (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="f0839-193">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="f0839-194">[Poprzednie](xref:data/ef-rp/sort-filter-page)
> [dalej](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="f0839-194">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>