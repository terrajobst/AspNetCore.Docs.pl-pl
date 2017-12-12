---
title: Stron razor podstawowych EF - Migrations - 4, 8
author: rick-anderson
description: "W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC."
keywords: Migracje platformy ASP.NET Core Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 8549fc522bcd05a5755a449cd6d4b655f00d998b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="82d1f-104">Migracje - Core EF z samouczka stron Razor (4 8)</span><span class="sxs-lookup"><span data-stu-id="82d1f-104">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="82d1f-105">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="82d1f-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="82d1f-106">W tym samouczku jest używany EF podstawowych funkcji migracji do zarządzania zmianami modelu danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-106">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="82d1f-107">Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="82d1f-107">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="82d1f-108">Gdy do opracowania nowej aplikacji, danych często modelu zmiany.</span><span class="sxs-lookup"><span data-stu-id="82d1f-108">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="82d1f-109">Zawsze zmiany modelu modelu jest niezsynchronizowana z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-109">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="82d1f-110">W tym samouczku uruchomiona przez skonfigurowanie programu Entity Framework, aby utworzyć bazę danych, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="82d1f-110">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="82d1f-111">Zawsze danych model zmiany:</span><span class="sxs-lookup"><span data-stu-id="82d1f-111">Each time the data model changes:</span></span>

* <span data-ttu-id="82d1f-112">Bazy danych zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="82d1f-112">The DB is dropped.</span></span>
* <span data-ttu-id="82d1f-113">EF tworzy nową, który jest zgodny z modelem.</span><span class="sxs-lookup"><span data-stu-id="82d1f-113">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="82d1f-114">Aplikacja nasion bazę danych z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-114">The app seeds the DB with test data.</span></span>

<span data-ttu-id="82d1f-115">Takie podejście do przechowywania bazy danych w modelu danych działa poprawnie, dopóki wdrażania aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="82d1f-115">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="82d1f-116">Gdy aplikacja działa w środowisku produkcyjnym, jest zazwyczaj przechowywana dane, które muszą być kontynuowana.</span><span class="sxs-lookup"><span data-stu-id="82d1f-116">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="82d1f-117">Aplikacja nie może zaczynać się od testu bazy danych zawsze zmianie (na przykład dodawanie nowej kolumny).</span><span class="sxs-lookup"><span data-stu-id="82d1f-117">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="82d1f-118">Funkcji EF Core migracje rozwiązuje ten problem, należy włączyć EF Core zaktualizować schemat bazy danych zamiast tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-118">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="82d1f-119">Zamiast porzucenie i ponowne utworzenie bazy danych w przypadku zmiany modelu danych, migracje aktualizacje schematu i zachowa istniejące dane.</span><span class="sxs-lookup"><span data-stu-id="82d1f-119">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="82d1f-120">Entity Framework Core NuGet pakietów dla migracji</span><span class="sxs-lookup"><span data-stu-id="82d1f-120">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="82d1f-121">Aby pracować z migracji, należy użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI).</span><span class="sxs-lookup"><span data-stu-id="82d1f-121">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="82d1f-122">Te samouczki wyjaśniają, jak używać polecenia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-122">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="82d1f-123">Informacje o PMC są obecnie [końcu tego samouczka](#pmc).</span><span class="sxs-lookup"><span data-stu-id="82d1f-123">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="82d1f-124">Narzędzia EF Core dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="82d1f-124">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="82d1f-125">Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* plików, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="82d1f-125">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="82d1f-126">**Uwaga:** można zainstalować tego pakietu, edytując *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="82d1f-126">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="82d1f-127">`install-package` Polecenia lub graficznego interfejsu użytkownika Menedżera pakietów nie może służyć do instalowania tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="82d1f-127">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="82d1f-128">Edytuj *.csproj* plików przez kliknięcie prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierając **Edytuj ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="82d1f-128">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="82d1f-129">Następujący kod przedstawia zaktualizowanego *.csproj* pliku przy użyciu narzędzi interfejsu wiersza polecenia EF Core wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="82d1f-129">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="82d1f-130">Numery wersji w poprzednim przykładzie zostały bieżącej, gdy samouczka został zapisany.</span><span class="sxs-lookup"><span data-stu-id="82d1f-130">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="82d1f-131">Użyj tej samej wersji dla narzędzi EF Core interfejsu wiersza polecenia, dostępnych w innych pakietach.</span><span class="sxs-lookup"><span data-stu-id="82d1f-131">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="82d1f-132">Zmień parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="82d1f-132">Change the connection string</span></span>

<span data-ttu-id="82d1f-133">W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="82d1f-133">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="82d1f-134">Zmiana nazwy bazy danych w parametrach połączenia powoduje, że pierwszy migracji do utworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-134">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="82d1f-135">Nowe bazy danych jest utworzony, ponieważ o tej nazwie nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="82d1f-135">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="82d1f-136">Zmiana parametrów połączenia nie jest wymagane wprowadzenie do migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-136">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="82d1f-137">Zmiana nazwy bazy danych zamiast jest usunięcie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-137">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="82d1f-138">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="82d1f-138">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="82d1f-139">Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-139">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="82d1f-140">Tworzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="82d1f-140">Create an initial migration</span></span>

<span data-ttu-id="82d1f-141">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="82d1f-141">Build the project.</span></span>

<span data-ttu-id="82d1f-142">Otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="82d1f-142">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="82d1f-143">Folder projektu zawiera *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="82d1f-143">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="82d1f-144">W oknie wiersza polecenia, wprowadź następujące:</span><span class="sxs-lookup"><span data-stu-id="82d1f-144">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="82d1f-145">Okno polecenie wyświetla informacje podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="82d1f-145">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="82d1f-146">Jeśli migracja nie powiedzie się z komunikatem "*nie może uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* "</span><span class="sxs-lookup"><span data-stu-id="82d1f-146">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="82d1f-147">wyświetlane są:</span><span class="sxs-lookup"><span data-stu-id="82d1f-147">is displayed:</span></span>

* <span data-ttu-id="82d1f-148">Zatrzymaj usługi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="82d1f-148">Stop IIS Express.</span></span>

   * <span data-ttu-id="82d1f-149">Zamknij i uruchom ponownie program Visual Studio, lub</span><span class="sxs-lookup"><span data-stu-id="82d1f-149">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="82d1f-150">Znajdowanie usług IIS Express ikonę na pasku zadań systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="82d1f-150">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="82d1f-151">Kliknij prawym przyciskiem myszy ikonę programu IIS Express, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.</span><span class="sxs-lookup"><span data-stu-id="82d1f-151">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="82d1f-152">Jeśli komunikat o błędzie "kompilacja nie powiodła się."</span><span class="sxs-lookup"><span data-stu-id="82d1f-152">If the error message "Build failed."</span></span> <span data-ttu-id="82d1f-153">zostanie wyświetlona, ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="82d1f-153">is displayed, run the command again.</span></span> <span data-ttu-id="82d1f-154">Jeśli ten błąd należy pozostawić notatki w dolnej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="82d1f-154">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="82d1f-155">Sprawdź w górę i w dół metody</span><span class="sxs-lookup"><span data-stu-id="82d1f-155">Examine the Up and Down methods</span></span>

<span data-ttu-id="82d1f-156">Polecenie EF Core `migrations add` wygenerowany kod w celu utworzenia bazy danych z.</span><span class="sxs-lookup"><span data-stu-id="82d1f-156">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="82d1f-157">Ten kod migracji znajduje się w *migracje\<sygnatury czasowej > _InitialCreate.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="82d1f-157">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="82d1f-158">`Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-158">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="82d1f-159">`Down` Metoda usuwa je, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="82d1f-159">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="82d1f-160">Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-160">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="82d1f-161">Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.</span><span class="sxs-lookup"><span data-stu-id="82d1f-161">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="82d1f-162">Poprzedni kod jest początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-162">The preceding code is for the initial migration.</span></span> <span data-ttu-id="82d1f-163">Ten kod został utworzony podczas `migrations add InitialCreate` polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-163">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="82d1f-164">Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="82d1f-164">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="82d1f-165">Nazwa migracji może być dowolną prawidłową nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="82d1f-165">The migration name can be any valid file name.</span></span> <span data-ttu-id="82d1f-166">Najlepiej wybrać słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-166">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="82d1f-167">Na przykład migracji, które dodano tabelę działu może mieć nazwę "AddDepartmentTable."</span><span class="sxs-lookup"><span data-stu-id="82d1f-167">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="82d1f-168">Jeśli zostanie utworzona początkowa migracji i kończy działanie bazy danych:</span><span class="sxs-lookup"><span data-stu-id="82d1f-168">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="82d1f-169">Zostaje wygenerowany kod tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-169">The DB creation code is generated.</span></span>
* <span data-ttu-id="82d1f-170">Kod tworzenia bazy danych nie wymaga się uruchomić, ponieważ bazy danych jest już zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-170">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="82d1f-171">Jeśli uruchomiony jest kod tworzenia bazy danych, nie wprowadzać żadnych zmian, ponieważ bazy danych jest już zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-171">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="82d1f-172">Po wdrożeniu aplikacji do nowego środowiska, aby utworzyć bazę danych należy uruchomić kod tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-172">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="82d1f-173">Wcześniej parametry połączenia została zmieniona na nową nazwę dla bazy danych użycia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-173">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="82d1f-174">Określonej bazy danych nie istnieje, więc migracji tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-174">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="82d1f-175">Sprawdź migawki modelu danych</span><span class="sxs-lookup"><span data-stu-id="82d1f-175">Examine the data model snapshot</span></span>

<span data-ttu-id="82d1f-176">Tworzy migracje *migawki* bieżącego schematu baz danych w *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="82d1f-176">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="82d1f-177">Ponieważ bieżący schemat bazy danych jest reprezentowana w kodzie, EF Core nie ma na interakcję z bazy danych, aby utworzyć migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-177">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="82d1f-178">Po dodaniu migracji EF Core określa co zmienione przez porównanie modelu danych do pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="82d1f-178">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="82d1f-179">Podstawowe EF współdziała z bazy danych tylko wtedy, gdy musi zaktualizować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-179">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="82d1f-180">Plik migawki musi być zsynchronizowane z migracji, które go utworzył.</span><span class="sxs-lookup"><span data-stu-id="82d1f-180">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="82d1f-181">Nie można usunąć migracji przez usunięcie pliku o nazwie  *\<sygnatury czasowej > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="82d1f-181">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="82d1f-182">Jeśli ten plik zostanie usunięty, pozostałe migracji nie są zsynchronizowane z plikiem migawki bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-182">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="82d1f-183">Aby usunąć ostatniego migracji dodany, użyj [Usuń migracje ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-183">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="82d1f-184">Usuń EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="82d1f-184">Remove EnsureCreated</span></span>

<span data-ttu-id="82d1f-185">Wczesne rozwoju `EnsureCreated` użyto polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-185">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="82d1f-186">W tym samouczku jest używany migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-186">In this tutorial, migrations is used.</span></span> <span data-ttu-id="82d1f-187">`EnsureCreated`ma następujące limatitions:</span><span class="sxs-lookup"><span data-stu-id="82d1f-187">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="82d1f-188">Pomija migracji i tworzy bazę danych i schematu.</span><span class="sxs-lookup"><span data-stu-id="82d1f-188">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="82d1f-189">Nie tworzy tabelę migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-189">Does not create a migrations table.</span></span>
* <span data-ttu-id="82d1f-190">Można *nie* można używać z migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-190">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="82d1f-191">Jest przeznaczony do testowania lub szybkie tworzenie prototypów gdzie bazy danych jest porzucona i utworzona ponownie często.</span><span class="sxs-lookup"><span data-stu-id="82d1f-191">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="82d1f-192">Usuń następujący wiersz z `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="82d1f-192">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="82d1f-193">Dotyczą migracji bazę danych w rozwoju</span><span class="sxs-lookup"><span data-stu-id="82d1f-193">Apply the migration to the DB in development</span></span>

<span data-ttu-id="82d1f-194">W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele.</span><span class="sxs-lookup"><span data-stu-id="82d1f-194">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="82d1f-195">Uwaga: Jeśli `update` polecenie zwraca błąd "Kompilacji nie powiodło się.":</span><span class="sxs-lookup"><span data-stu-id="82d1f-195">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="82d1f-196">Ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="82d1f-196">Run the command again.</span></span>
* <span data-ttu-id="82d1f-197">Jeśli jej nie powiedzie się ponownie, zamknij program Visual Studio, a następnie uruchom `update` polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-197">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="82d1f-198">Pozostaw wiadomości w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="82d1f-198">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="82d1f-199">Dane wyjściowe polecenia jest podobny do `migrations add` danych wyjściowych polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-199">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="82d1f-200">W poprzednim poleceniu dzienniki dla poleceń SQL, które Konfigurowanie bazy danych są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="82d1f-200">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="82d1f-201">Większość Dzienniki zostały pominięte w następujących przykładowych danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="82d1f-201">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="82d1f-202">Aby zmniejszyć poziom szczegółów komunikatów dziennika, można zmienić poziom dziennika w *appsettings. Development.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="82d1f-202">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="82d1f-203">Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="82d1f-203">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="82d1f-204">Użyj **Eksplorator obiektów SQL Server** przeprowadzać inspekcję bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-204">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="82d1f-205">Zwróć uwagę, dodanie `__EFMigrationsHistory` tabeli.</span><span class="sxs-lookup"><span data-stu-id="82d1f-205">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="82d1f-206">`__EFMigrationsHistory` Tabeli przechowuje informacje o migracji, które zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82d1f-206">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="82d1f-207">Wyświetlanie danych w `__EFMigrationsHistory` tabeli, zawiera jeden wiersz dla pierwszej migracji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-207">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="82d1f-208">Ostatni dziennika w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia zawiera instrukcji INSERT, która tworzy tego wiersza.</span><span class="sxs-lookup"><span data-stu-id="82d1f-208">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="82d1f-209">Uruchom aplikację i sprawdź, czy wszystko działa.</span><span class="sxs-lookup"><span data-stu-id="82d1f-209">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="82d1f-210">Appling migracji w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="82d1f-210">Appling migrations in production</span></span>

<span data-ttu-id="82d1f-211">Firma Microsoft zaleca aplikacji w środowisku produkcyjnym należy **nie** wywołać [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82d1f-211">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="82d1f-212">`Migrate`Nie można wywoływać z aplikacji w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="82d1f-212">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="82d1f-213">Na przykład, jeśli aplikacja została chmury z skalowalnego w poziomie (uruchomionych wiele wystąpień aplikacji).</span><span class="sxs-lookup"><span data-stu-id="82d1f-213">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="82d1f-214">Migracja bazy danych powinno być wykonywane w ramach wdrożenia, a następnie w kontrolowany sposób.</span><span class="sxs-lookup"><span data-stu-id="82d1f-214">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="82d1f-215">Podejścia do produkcyjnej bazy danych migracji obejmują:</span><span class="sxs-lookup"><span data-stu-id="82d1f-215">Production database migration approaches include:</span></span>

* <span data-ttu-id="82d1f-216">Przy użyciu migracji można utworzyć skrypty SQL i skrypty SQL we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="82d1f-216">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="82d1f-217">Uruchomiona `dotnet ef database update` z kontrolą środowiska.</span><span class="sxs-lookup"><span data-stu-id="82d1f-217">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="82d1f-218">Używa EF Core `__MigrationsHistory` tabeli, aby wyświetlić wszystkie migracje trzeba uruchamiać.</span><span class="sxs-lookup"><span data-stu-id="82d1f-218">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="82d1f-219">Jeśli bazy danych jest aktualne, migracja nie jest uruchamiany.</span><span class="sxs-lookup"><span data-stu-id="82d1f-219">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="82d1f-220">Vs interfejsu wiersza polecenia (CLI). Konsola Menedżera pakietów (PMC)</span><span class="sxs-lookup"><span data-stu-id="82d1f-220">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="82d1f-221">Podstawowe EF narzędzi do zarządzania migracji jest dostępne z:</span><span class="sxs-lookup"><span data-stu-id="82d1f-221">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="82d1f-222">.NET core polecenia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="82d1f-222">.NET Core CLI commands.</span></span>
* <span data-ttu-id="82d1f-223">Polecenia cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC).</span><span class="sxs-lookup"><span data-stu-id="82d1f-223">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="82d1f-224">Ten samouczek przedstawia sposób użycia interfejsu wiersza polecenia, niektórzy deweloperzy preferowane przy użyciu kryterium.</span><span class="sxs-lookup"><span data-stu-id="82d1f-224">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="82d1f-225">Polecenia EF Core PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu.</span><span class="sxs-lookup"><span data-stu-id="82d1f-225">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="82d1f-226">Ten pakiet jest uwzględniona w [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, dzięki czemu nie trzeba go zainstalować.</span><span class="sxs-lookup"><span data-stu-id="82d1f-226">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="82d1f-227">**Ważne:** nie jest tym samym pakiecie, jak zainstalować dla interfejsu wiersza polecenia, edytując *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="82d1f-227">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="82d1f-228">Nazwa tego kończy się `Tools`, w odróżnieniu od nazwy pakietu interfejsu wiersza polecenia, która kończy się `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="82d1f-228">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="82d1f-229">Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="82d1f-229">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="82d1f-230">Aby uzyskać więcej informacji na temat poleceń PMC zobacz [Konsola Menedżera pakietów (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="82d1f-230">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="82d1f-231">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="82d1f-231">Troubleshooting</span></span>

<span data-ttu-id="82d1f-232">Pobierz [ukończonej aplikacji dla tego etapu](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="82d1f-232">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="82d1f-233">Aplikacja generuje następujący wyjątek:</span><span class="sxs-lookup"><span data-stu-id="82d1f-233">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="82d1f-234">Rozwiązanie: Uruchom`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="82d1f-234">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="82d1f-235">Jeśli `update` polecenie zwraca błąd "Kompilacji nie powiodło się.":</span><span class="sxs-lookup"><span data-stu-id="82d1f-235">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="82d1f-236">Ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="82d1f-236">Run the command again.</span></span>
* <span data-ttu-id="82d1f-237">Pozostaw wiadomości w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="82d1f-237">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="82d1f-238">[Poprzednie](xref:data/ef-rp/sort-filter-page)
[dalej](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="82d1f-238">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
