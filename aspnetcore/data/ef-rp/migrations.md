---
title: Stron razor podstawowych EF w platformy ASP.NET Core - Migrations - 4, 8
author: rick-anderson
description: W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 4e9b747a3369bbb608c3b3832c865745a2322142
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="1e361-103">Stron razor podstawowych EF w platformy ASP.NET Core - Migrations - 4, 8</span><span class="sxs-lookup"><span data-stu-id="1e361-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="1e361-104">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1e361-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="1e361-105">W tym samouczku jest używany EF podstawowych funkcji migracji do zarządzania zmianami modelu danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="1e361-106">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="1e361-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="1e361-107">Gdy do opracowania nowej aplikacji, danych często modelu zmiany.</span><span class="sxs-lookup"><span data-stu-id="1e361-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="1e361-108">Zawsze zmiany modelu modelu jest niezsynchronizowana z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="1e361-109">W tym samouczku uruchomiona przez skonfigurowanie programu Entity Framework, aby utworzyć bazę danych, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="1e361-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="1e361-110">Zawsze danych model zmiany:</span><span class="sxs-lookup"><span data-stu-id="1e361-110">Each time the data model changes:</span></span>

* <span data-ttu-id="1e361-111">Bazy danych zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="1e361-111">The DB is dropped.</span></span>
* <span data-ttu-id="1e361-112">EF tworzy nową, który jest zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="1e361-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="1e361-113">Aplikacja nasion bazę danych z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="1e361-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="1e361-114">Takie podejście do przechowywania bazy danych w modelu danych działa poprawnie, dopóki wdrażania aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="1e361-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="1e361-115">Gdy aplikacja działa w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które muszą być kontynuowana.</span><span class="sxs-lookup"><span data-stu-id="1e361-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="1e361-116">Aplikacja nie może zaczynać się od testu bazy danych zawsze zmianie (na przykład dodawanie nowej kolumny).</span><span class="sxs-lookup"><span data-stu-id="1e361-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="1e361-117">Funkcji EF Core migracje rozwiązuje ten problem, należy włączyć EF Core zaktualizować schemat bazy danych zamiast tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="1e361-118">Zamiast porzucenie i ponowne utworzenie bazy danych w przypadku zmiany modelu danych, migracje aktualizacje schematu i zachowa istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="1e361-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="1e361-119">Entity Framework Core NuGet pakietów dla migracji</span><span class="sxs-lookup"><span data-stu-id="1e361-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="1e361-120">Aby pracować z migracji, należy użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI).</span><span class="sxs-lookup"><span data-stu-id="1e361-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="1e361-121">Te samouczki wyjaśniają, jak używać polecenia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="1e361-122">Informacje o PMC są obecnie [końcu tego samouczka](#pmc).</span><span class="sxs-lookup"><span data-stu-id="1e361-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="1e361-123">Narzędzia EF Core dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="1e361-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="1e361-124">Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* plików, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="1e361-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="1e361-125">**Uwaga:** można zainstalować tego pakietu, edytując *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="1e361-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="1e361-126">`install-package` Polecenia lub graficznego interfejsu użytkownika Menedżera pakietów nie może służyć do instalowania tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="1e361-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="1e361-127">Edytuj *.csproj* plików przez kliknięcie prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierając **Edytuj ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="1e361-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="1e361-128">Następujący kod przedstawia zaktualizowanego *.csproj* pliku przy użyciu narzędzi interfejsu wiersza polecenia EF Core wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="1e361-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="1e361-129">Numery wersji w poprzednim przykładzie zostały bieżącej, gdy samouczka został zapisany.</span><span class="sxs-lookup"><span data-stu-id="1e361-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="1e361-130">Użyj tej samej wersji dla narzędzi EF Core interfejsu wiersza polecenia, dostępnych w innych pakietach.</span><span class="sxs-lookup"><span data-stu-id="1e361-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="1e361-131">Zmień parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="1e361-131">Change the connection string</span></span>

<span data-ttu-id="1e361-132">W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="1e361-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="1e361-133">Zmiana nazwy bazy danych w parametrach połączenia powoduje, że pierwszy migracji do utworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="1e361-134">Nowe bazy danych jest tworzony, ponieważ o tej nazwie nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="1e361-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="1e361-135">Zmiana parametrów połączenia nie jest wymagane wprowadzenie do migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="1e361-136">Zmiana nazwy bazy danych zamiast jest usunięcie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="1e361-137">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="1e361-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="1e361-138">Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="1e361-139">Tworzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="1e361-139">Create an initial migration</span></span>

<span data-ttu-id="1e361-140">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="1e361-140">Build the project.</span></span>

<span data-ttu-id="1e361-141">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="1e361-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="1e361-142">Folder projektu zawiera *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="1e361-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="1e361-143">W oknie wiersza polecenia, wprowadź następujące:</span><span class="sxs-lookup"><span data-stu-id="1e361-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="1e361-144">Okno polecenie wyświetla informacje podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="1e361-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="1e361-145">Jeśli migracja nie powiedzie się z komunikatem "*nie może uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* "</span><span class="sxs-lookup"><span data-stu-id="1e361-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="1e361-146">wyświetlane są:</span><span class="sxs-lookup"><span data-stu-id="1e361-146">is displayed:</span></span>

* <span data-ttu-id="1e361-147">Zatrzymaj usługi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1e361-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="1e361-148">Zamknij i uruchom ponownie program Visual Studio, lub</span><span class="sxs-lookup"><span data-stu-id="1e361-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="1e361-149">Znajdowanie usług IIS Express ikonę na pasku zadań systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="1e361-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="1e361-150">Kliknij prawym przyciskiem myszy ikonę programu IIS Express, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.</span><span class="sxs-lookup"><span data-stu-id="1e361-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="1e361-151">Jeśli komunikat o błędzie "kompilacja nie powiodła się."</span><span class="sxs-lookup"><span data-stu-id="1e361-151">If the error message "Build failed."</span></span> <span data-ttu-id="1e361-152">zostanie wyświetlona, ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="1e361-152">is displayed, run the command again.</span></span> <span data-ttu-id="1e361-153">Jeśli ten błąd należy pozostawić notatki w dolnej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="1e361-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="1e361-154">Sprawdź w górę i w dół metody</span><span class="sxs-lookup"><span data-stu-id="1e361-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="1e361-155">Polecenie EF Core `migrations add` wygenerowany kod w celu utworzenia bazy danych z.</span><span class="sxs-lookup"><span data-stu-id="1e361-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="1e361-156">Ten kod migracji znajduje się w *migracje\<sygnatury czasowej > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="1e361-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="1e361-157">`Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="1e361-158">`Down` Metoda usuwa je, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1e361-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="1e361-159">Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="1e361-160">Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.</span><span class="sxs-lookup"><span data-stu-id="1e361-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="1e361-161">Poprzedni kod jest początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="1e361-162">Ten kod został utworzony podczas `migrations add InitialCreate` polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="1e361-163">Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="1e361-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="1e361-164">Nazwa migracji może być dowolną prawidłową nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="1e361-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="1e361-165">Najlepiej wybrać słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="1e361-166">Na przykład migracji, które dodano tabelę działu może mieć nazwę "AddDepartmentTable."</span><span class="sxs-lookup"><span data-stu-id="1e361-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="1e361-167">Jeśli zostanie utworzona początkowa migracji i kończy działanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="1e361-167">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="1e361-168">Zostaje wygenerowany kod tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="1e361-169">Kod tworzenia bazy danych nie wymaga się uruchomić, ponieważ bazy danych jest już zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="1e361-170">Jeśli uruchomiony jest kod tworzenia bazy danych, nie wprowadzać żadnych zmian, ponieważ bazy danych jest już zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="1e361-171">Po wdrożeniu aplikacji do nowego środowiska, aby utworzyć bazę danych należy uruchomić kod tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="1e361-172">Wcześniej parametry połączenia została zmieniona na nową nazwę dla bazy danych użycia.</span><span class="sxs-lookup"><span data-stu-id="1e361-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="1e361-173">Określonej bazy danych nie istnieje, więc migracji tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="1e361-174">Migawki modelu danych</span><span class="sxs-lookup"><span data-stu-id="1e361-174">The data model snapshot</span></span>

<span data-ttu-id="1e361-175">Tworzy migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="1e361-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="1e361-176">Po dodaniu migracji EF określa co zmienione przez porównanie modelu danych do pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="1e361-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="1e361-177">Podczas usuwania migracji, użyj [Usuń migracje ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="1e361-178">`dotnet ef migrations remove` Usuwa migracji i gwarantuje, że poprawnie zresetowania migawki.</span><span class="sxs-lookup"><span data-stu-id="1e361-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="1e361-179">Zobacz [migracje Core EF w środowiskach zespołu](/ef/core/managing-schemas/migrations/teams) Aby uzyskać więcej informacji o sposobie używania pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="1e361-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="1e361-180">Remove EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="1e361-180">Remove EnsureCreated</span></span>

<span data-ttu-id="1e361-181">Wczesne rozwoju `EnsureCreated` użyto polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="1e361-182">W tym samouczku jest używany migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="1e361-183">`EnsureCreated` ma następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="1e361-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="1e361-184">Pomija migracji i tworzy bazę danych i schematu.</span><span class="sxs-lookup"><span data-stu-id="1e361-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="1e361-185">Nie tworzy tabelę migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="1e361-186">Można *nie* można używać z migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="1e361-187">Jest przeznaczony do testowania lub szybkie tworzenie prototypów gdzie bazy danych jest porzucona i utworzona ponownie często.</span><span class="sxs-lookup"><span data-stu-id="1e361-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="1e361-188">Usuń następujący wiersz z `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="1e361-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="1e361-189">Dotyczą migracji bazę danych w rozwoju</span><span class="sxs-lookup"><span data-stu-id="1e361-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="1e361-190">W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele.</span><span class="sxs-lookup"><span data-stu-id="1e361-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="1e361-191">Uwaga: Jeśli `update` polecenie zwraca błąd "Kompilacji nie powiodło się.":</span><span class="sxs-lookup"><span data-stu-id="1e361-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="1e361-192">Ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="1e361-192">Run the command again.</span></span>
* <span data-ttu-id="1e361-193">Jeśli jej nie powiedzie się ponownie, zamknij program Visual Studio, a następnie uruchom `update` polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="1e361-194">Pozostaw wiadomości w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="1e361-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="1e361-195">Dane wyjściowe polecenia jest podobny do `migrations add` danych wyjściowych polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="1e361-196">W poprzednim poleceniu dzienniki dla poleceń SQL, które Konfigurowanie bazy danych są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1e361-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="1e361-197">Większość Dzienniki zostały pominięte w następujących przykładowych danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="1e361-197">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="1e361-198">Aby zmniejszyć poziom szczegółów komunikatów dziennika, zmienić poziom dziennika, w *appsettings. Development.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="1e361-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="1e361-199">Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="1e361-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="1e361-200">Użyj **Eksplorator obiektów SQL Server** przeprowadzać inspekcję bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="1e361-201">Zwróć uwagę, dodanie `__EFMigrationsHistory` tabeli.</span><span class="sxs-lookup"><span data-stu-id="1e361-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="1e361-202">`__EFMigrationsHistory` Tabeli przechowuje informacje o migracji, które zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e361-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="1e361-203">Wyświetlanie danych w `__EFMigrationsHistory` tabeli, zawiera jeden wiersz dla pierwszej migracji.</span><span class="sxs-lookup"><span data-stu-id="1e361-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="1e361-204">Ostatni dziennika w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia zawiera instrukcji INSERT, która tworzy tego wiersza.</span><span class="sxs-lookup"><span data-stu-id="1e361-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="1e361-205">Uruchom aplikację i sprawdź, czy wszystko działa.</span><span class="sxs-lookup"><span data-stu-id="1e361-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="1e361-206">Stosowanie migracji w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="1e361-206">Applying migrations in production</span></span>

<span data-ttu-id="1e361-207">Firma Microsoft zaleca aplikacji w środowisku produkcyjnym należy **nie** wywołać [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1e361-207">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="1e361-208">`Migrate` Nie można wywołać z aplikacji w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="1e361-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="1e361-209">Na przykład, jeśli aplikacja została chmury z skalowalnego w poziomie (uruchomionych wiele wystąpień aplikacji).</span><span class="sxs-lookup"><span data-stu-id="1e361-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="1e361-210">Migracja bazy danych powinno być wykonywane w ramach wdrożenia, a następnie w kontrolowany sposób.</span><span class="sxs-lookup"><span data-stu-id="1e361-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="1e361-211">Podejścia do produkcyjnej bazy danych migracji obejmują:</span><span class="sxs-lookup"><span data-stu-id="1e361-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="1e361-212">Przy użyciu migracji można utworzyć skrypty SQL i skrypty SQL we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="1e361-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="1e361-213">Uruchomiona `dotnet ef database update` z kontrolą środowiska.</span><span class="sxs-lookup"><span data-stu-id="1e361-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="1e361-214">Używa EF Core `__MigrationsHistory` tabeli, aby wyświetlić wszystkie migracje trzeba uruchamiać.</span><span class="sxs-lookup"><span data-stu-id="1e361-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="1e361-215">Jeśli bazy danych jest aktualne, migracja nie jest uruchamiany.</span><span class="sxs-lookup"><span data-stu-id="1e361-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="1e361-216">Vs interfejsu wiersza polecenia (CLI). Konsola Menedżera pakietów (PMC)</span><span class="sxs-lookup"><span data-stu-id="1e361-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="1e361-217">Podstawowe EF narzędzi do zarządzania migracji jest dostępne z:</span><span class="sxs-lookup"><span data-stu-id="1e361-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="1e361-218">.NET core polecenia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="1e361-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="1e361-219">Polecenia cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC).</span><span class="sxs-lookup"><span data-stu-id="1e361-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="1e361-220">Ten samouczek przedstawia sposób użycia interfejsu wiersza polecenia, niektórzy deweloperzy preferowane przy użyciu kryterium.</span><span class="sxs-lookup"><span data-stu-id="1e361-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="1e361-221">Polecenia EF Core PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu.</span><span class="sxs-lookup"><span data-stu-id="1e361-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="1e361-222">Ten pakiet jest uwzględniona w [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, dzięki czemu nie trzeba go zainstalować.</span><span class="sxs-lookup"><span data-stu-id="1e361-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="1e361-223">**Ważne:** nie jest tym samym pakiecie, jak zainstalować dla interfejsu wiersza polecenia, edytując *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="1e361-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="1e361-224">Nazwa tego kończy się `Tools`, w odróżnieniu od nazwy pakietu interfejsu wiersza polecenia, która kończy się `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="1e361-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="1e361-225">Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1e361-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="1e361-226">Aby uzyskać więcej informacji na temat poleceń PMC zobacz [Konsola Menedżera pakietów (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="1e361-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1e361-227">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="1e361-227">Troubleshooting</span></span>

<span data-ttu-id="1e361-228">Pobierz [ukończonej aplikacji dla tego etapu](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="1e361-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="1e361-229">Aplikacja generuje następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="1e361-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="1e361-230">Rozwiązanie: Uruchom `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="1e361-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="1e361-231">Jeśli `update` polecenie zwraca błąd "Kompilacji nie powiodło się.":</span><span class="sxs-lookup"><span data-stu-id="1e361-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="1e361-232">Ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="1e361-232">Run the command again.</span></span>
* <span data-ttu-id="1e361-233">Pozostaw wiadomości w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="1e361-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1e361-234">[Poprzednie](xref:data/ef-rp/sort-filter-page)
> [dalej](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="1e361-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
