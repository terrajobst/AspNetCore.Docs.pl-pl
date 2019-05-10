---
title: 'Samouczek: Za pomocą funkcji migracje — ASP.NET MVC z programem EF Core'
description: W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych w aplikacji ASP.NET Core MVC.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: 3ad8ae27c3a7ced2f367919e200aff51fdf03b64
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900025"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a><span data-ttu-id="5042d-103">Samouczek: Za pomocą funkcji migracje — ASP.NET MVC z programem EF Core</span><span class="sxs-lookup"><span data-stu-id="5042d-103">Tutorial: Using the migrations feature - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="5042d-104">W ramach tego samouczka możesz rozpocząć korzystanie z funkcji migracje EF Core dla zarządzania zmianami modelu danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-104">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="5042d-105">W kolejnych samouczkach należy dodać więcej migracji w przypadku zmiany modelu danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-105">In later tutorials, you'll add more migrations as you change the data model.</span></span>

<span data-ttu-id="5042d-106">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="5042d-106">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5042d-107">Dowiedz się więcej o migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-107">Learn about migrations</span></span>
> * <span data-ttu-id="5042d-108">Zmień parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="5042d-108">Change the connection string</span></span>
> * <span data-ttu-id="5042d-109">Tworzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-109">Create an initial migration</span></span>
> * <span data-ttu-id="5042d-110">Sprawdź metod w górę i w dół</span><span class="sxs-lookup"><span data-stu-id="5042d-110">Examine Up and Down methods</span></span>
> * <span data-ttu-id="5042d-111">Dowiedz się więcej o migawki modelu danych</span><span class="sxs-lookup"><span data-stu-id="5042d-111">Learn about the data model snapshot</span></span>
> * <span data-ttu-id="5042d-112">Zastosuj migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-112">Apply the migration</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5042d-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5042d-113">Prerequisites</span></span>

* [<span data-ttu-id="5042d-114">Sortowanie, filtrowanie i stronicowanie</span><span class="sxs-lookup"><span data-stu-id="5042d-114">Sorting, filtering, and paging</span></span>](sort-filter-page.md)

## <a name="about-migrations"></a><span data-ttu-id="5042d-115">Dotyczące migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-115">About migrations</span></span>

<span data-ttu-id="5042d-116">Podczas tworzenia nowej aplikacji swój model danych zmienia często i za każdym razem zmiany modelu, otrzymuje zsynchronizowana z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-116">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="5042d-117">Te samouczki są uruchomione przez konfigurowanie platformy Entity Framework do tworzenia bazy danych, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="5042d-117">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="5042d-118">Następnie zawsze możesz zmienić model danych — dodać, usunąć, lub zmienić klas jednostek lub zmienić klasy DbContext — można usunąć bazy danych i EF utworzony zostaje nowy indeks, który odpowiada modelu i inicjowania inicjuje ją z danymi.</span><span class="sxs-lookup"><span data-stu-id="5042d-118">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="5042d-119">Ta metoda synchronizacja bazy danych z modelem danych działa poprawnie, dopóki nie możesz wdrożyć aplikację do środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="5042d-119">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="5042d-120">Gdy aplikacja jest uruchomiona w środowisku produkcyjnym zazwyczaj zapisuje dane, które mają być przechowywane i nie chcesz utracić wszystko, czego zawsze upewnij się zmiany, takie jak dodawanie nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="5042d-120">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="5042d-121">Funkcja migracji programu EF Core rozwiązuje ten problem, włączając EF do zaktualizowania schematu bazy danych zamiast tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-121">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

<span data-ttu-id="5042d-122">Aby pracować z migracji, można użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI).</span><span class="sxs-lookup"><span data-stu-id="5042d-122">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="5042d-123">Tych samouczkach przedstawiono sposób użycia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="5042d-123">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="5042d-124">Informacje o konsoli zarządzania Pakietami wynosi [po ukończeniu tego samouczka](#pmc).</span><span class="sxs-lookup"><span data-stu-id="5042d-124">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="5042d-125">Zmień parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="5042d-125">Change the connection string</span></span>

<span data-ttu-id="5042d-126">W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2 lub innej nazwy, które nie były używane na komputerze używasz.</span><span class="sxs-lookup"><span data-stu-id="5042d-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="5042d-127">Ta zmiana konfiguruje projekt tak, aby pierwszej migracji utworzy nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="5042d-128">Nie jest to wymagane, aby rozpocząć pracę z migracji, ale zostanie wyświetlony później dlaczego jest dobrym pomysłem.</span><span class="sxs-lookup"><span data-stu-id="5042d-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="5042d-129">Alternatywnie można zmienić nazwy bazy danych można usunąć bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="5042d-130">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="5042d-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
>
> ```console
> dotnet ef database drop
> ```
>
> <span data-ttu-id="5042d-131">Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia platformy.</span><span class="sxs-lookup"><span data-stu-id="5042d-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="5042d-132">Tworzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-132">Create an initial migration</span></span>

<span data-ttu-id="5042d-133">Zapisz zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="5042d-133">Save your changes and build the project.</span></span> <span data-ttu-id="5042d-134">Następnie otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="5042d-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="5042d-135">Poniżej przedstawiono szybki sposób, aby to zrobić:</span><span class="sxs-lookup"><span data-stu-id="5042d-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="5042d-136">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Otwórz Folder w Eksploratorze plików** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="5042d-136">In **Solution Explorer**, right-click the project and choose **Open Folder in File Explorer** from the context menu.</span></span>

  ![Otwórz w elemencie menu Eksploratora plików](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="5042d-138">Wprowadź "cmd" na pasku adresu i naciśnij klawisz Enter.</span><span class="sxs-lookup"><span data-stu-id="5042d-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Otwórz okno polecenia](migrations/_static/open-command-window.png)

<span data-ttu-id="5042d-140">Wprowadź następujące polecenie w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="5042d-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="5042d-141">Zostaną wyświetlone dane wyjściowe podobne do następujących w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="5042d-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="5042d-142">Jeśli zostanie wyświetlony komunikat o błędzie *nie plik wykonywalny znaleziono pasującego polecenia "dotnet-ef"*, zobacz [ten wpis w blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) do rozwiązywania problemów w Pomocy.</span><span class="sxs-lookup"><span data-stu-id="5042d-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="5042d-143">Jeśli zostanie wyświetlony komunikat o błędzie "*uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* ", Znajdź ikonę usług IIS Express w zasobniku systemu Windows i kliknij go prawym przyciskiem myszy, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.</span><span class="sxs-lookup"><span data-stu-id="5042d-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-up-and-down-methods"></a><span data-ttu-id="5042d-144">Sprawdź metod w górę i w dół</span><span class="sxs-lookup"><span data-stu-id="5042d-144">Examine Up and Down methods</span></span>

<span data-ttu-id="5042d-145">Kiedy wykonać `migrations add` polecenia EF wygenerowany kod, który spowoduje utworzenie bazy danych od podstaw.</span><span class="sxs-lookup"><span data-stu-id="5042d-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="5042d-146">Ten kod znajduje się w *migracje* folderu, w pliku o nazwie  *\<sygnatura czasowa > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="5042d-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="5042d-147">`Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawy jednostek modelu danych i `Down` metoda spowoduje usunięcie ich, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="5042d-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="5042d-148">Migracje wywołania `Up` metodę, aby wdrożyć model danych zmienia się do migracji.</span><span class="sxs-lookup"><span data-stu-id="5042d-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="5042d-149">Po wprowadzeniu polecenia można wycofać aktualizację, wywołania migracje `Down` metody.</span><span class="sxs-lookup"><span data-stu-id="5042d-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="5042d-150">Ten kod jest początkowej migracji, który został utworzony po wprowadzeniu `migrations add InitialCreate` polecenia.</span><span class="sxs-lookup"><span data-stu-id="5042d-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="5042d-151">Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku i może być dowolnie.</span><span class="sxs-lookup"><span data-stu-id="5042d-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="5042d-152">Najlepiej wybrać wyrazu lub frazy, który podsumowuje, co to jest wykonywana w procesie migracji.</span><span class="sxs-lookup"><span data-stu-id="5042d-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="5042d-153">Na przykład można nazwać późniejszej migracji "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="5042d-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="5042d-154">Jeśli utworzono początkowej migracji bazy danych już istnieje, kod tworzenia bazy danych jest generowany, ale nie ma działać, ponieważ baza danych już jest zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="5042d-155">Podczas wdrażania aplikacji do innego środowiska, w którym baza danych nie istnieje jeszcze uruchamiane ten kod, aby utworzyć bazę danych, więc to dobry pomysł, aby przetestować go najpierw.</span><span class="sxs-lookup"><span data-stu-id="5042d-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="5042d-156">Dlatego właśnie zmieniono nazwę bazy danych w parametrach połączenia wcześniej — tak aby migracji można utworzyć nową od podstaw.</span><span class="sxs-lookup"><span data-stu-id="5042d-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="5042d-157">Migawka modelu danych</span><span class="sxs-lookup"><span data-stu-id="5042d-157">The data model snapshot</span></span>

<span data-ttu-id="5042d-158">Tworzy migracje *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="5042d-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="5042d-159">Podczas dodawania migracji EF Określa, co zmieniło się przez porównanie modelu danych do pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="5042d-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="5042d-160">Podczas usuwania migracji, należy użyć [Usuń migracji ef dotnet](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia.</span><span class="sxs-lookup"><span data-stu-id="5042d-160">When deleting a migration, use the [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="5042d-161">`dotnet ef migrations remove` Usuwa migracji i gwarantuje, że migawka poprawnie zostanie zresetowana.</span><span class="sxs-lookup"><span data-stu-id="5042d-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="5042d-162">Zobacz [EF Core migracji w środowiskach zespołu](/ef/core/managing-schemas/migrations/teams) Aby uzyskać więcej informacji o sposobie korzystania z pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="5042d-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration"></a><span data-ttu-id="5042d-163">Zastosuj migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-163">Apply the migration</span></span>

<span data-ttu-id="5042d-164">W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele w nim.</span><span class="sxs-lookup"><span data-stu-id="5042d-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="5042d-165">Dane wyjściowe polecenia są podobne do `migrations add` poleceń, z tą różnicą, że Zobacz dzienniki dla polecenia SQL, który konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="5042d-166">Większość dzienników zostały pominięte w następujących przykładowych danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="5042d-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="5042d-167">Jeśli wolisz nie wyświetlić ten poziom szczegółów komunikatów dziennika można zmienić poziom dziennika w *appsettings. Development.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="5042d-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="5042d-168">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="5042d-168">For more information, see <xref:fundamentals/logging/index>.</span></span>

```text
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 2.2.0-rtm-35687 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (274ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (60ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      IF SERVERPROPERTY('EngineEdition') <> 5
      BEGIN
          ALTER DATABASE [ContosoUniversity2] SET READ_COMMITTED_SNAPSHOT ON;
      END;
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (15ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20190327172701_InitialCreate', N'2.2.0-rtm-35687');
Done.
```

<span data-ttu-id="5042d-169">Użyj **Eksplorator obiektów SQL Server** do inspekcji bazy danych, tak jak w pierwszym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5042d-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="5042d-170">Można zauważyć dodanie \_ \_EFMigrationsHistory tabeli, która przechowuje informacje o migracji, które zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-170">You'll notice the addition of an \_\_EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="5042d-171">Wyświetlanie danych w tej tabeli, a następnie zostanie wyświetlony jeden wiersz dla pierwszej migracji.</span><span class="sxs-lookup"><span data-stu-id="5042d-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="5042d-172">(Ostatni dziennik w powyższym przykładzie danych wyjściowych interfejsu wiersza polecenia wyświetla instrukcji INSERT, która tworzy ten wiersz).</span><span class="sxs-lookup"><span data-stu-id="5042d-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="5042d-173">Uruchom aplikację, aby zweryfikować, że wszystko nadal działa tak jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="5042d-173">Run the application to verify that everything still works the same as before.</span></span>

![Strona indeksu uczniów](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a><span data-ttu-id="5042d-175">Porównanie interfejsu wiersza polecenia i konsolę zarządzania Pakietami</span><span class="sxs-lookup"><span data-stu-id="5042d-175">Compare CLI and PMC</span></span>

<span data-ttu-id="5042d-176">EF narzędzi do zarządzania migracji jest niedostępna, z poleceń interfejsu wiersza polecenia platformy .NET Core lub poleceń cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC).</span><span class="sxs-lookup"><span data-stu-id="5042d-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="5042d-177">W tym samouczku pokazano, jak używać interfejsu wiersza polecenia, ale można użyć konsoli zarządzania Pakietami, jeśli użytkownik sobie tego życzy.</span><span class="sxs-lookup"><span data-stu-id="5042d-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="5042d-178">Polecenia EF poleceń PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu.</span><span class="sxs-lookup"><span data-stu-id="5042d-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="5042d-179">Ten pakiet jest objęta [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), więc nie musisz dodać odwołanie do pakietu, jeśli aplikacja ma odwołania do pakietu dla `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="5042d-179">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to add a package reference if your app has a package reference for `Microsoft.AspNetCore.App`.</span></span>

<span data-ttu-id="5042d-180">**Ważne:** Nie jest to ten sam pakiet, instalowanie interfejsu wiersza polecenia, edytując *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="5042d-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="5042d-181">Nazwa tego z nich kończy się na `Tools`, w odróżnieniu od nazwy pakiet interfejsu wiersza polecenia, którego nazwa kończy się na `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="5042d-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="5042d-182">Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="5042d-182">For more information about the CLI commands, see [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="5042d-183">Aby uzyskać więcej informacji na temat poleceń konsoli zarządzania Pakietami, zobacz [Konsola Menedżera pakietów (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="5042d-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="5042d-184">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="5042d-184">Get the code</span></span>

[<span data-ttu-id="5042d-185">Pobieranie i wyświetlanie ukończonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5042d-185">Download or view the completed application.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a><span data-ttu-id="5042d-186">Następny krok</span><span class="sxs-lookup"><span data-stu-id="5042d-186">Next step</span></span>

<span data-ttu-id="5042d-187">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="5042d-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5042d-188">Uzyskaliśmy informacje dotyczące migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-188">Learned about migrations</span></span>
> * <span data-ttu-id="5042d-189">Przedstawia informacje na temat pakietów migracji NuGet</span><span class="sxs-lookup"><span data-stu-id="5042d-189">Learned about NuGet migration packages</span></span>
> * <span data-ttu-id="5042d-190">Zmienić parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="5042d-190">Changed the connection string</span></span>
> * <span data-ttu-id="5042d-191">Utworzone początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-191">Created an initial migration</span></span>
> * <span data-ttu-id="5042d-192">Zbadanie metod w górę i w dół</span><span class="sxs-lookup"><span data-stu-id="5042d-192">Examined Up and Down methods</span></span>
> * <span data-ttu-id="5042d-193">Przedstawia informacje na temat migawek modelu danych</span><span class="sxs-lookup"><span data-stu-id="5042d-193">Learned about the data model snapshot</span></span>
> * <span data-ttu-id="5042d-194">Stosowane migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-194">Applied the migration</span></span>

<span data-ttu-id="5042d-195">Przejdź do następnego samouczka, aby rozpocząć spojrzenie na bardziej zaawansowanych tematów dotyczących Rozszerzanie modelu danych.</span><span class="sxs-lookup"><span data-stu-id="5042d-195">Advance to the next tutorial to begin looking at more advanced topics about expanding the data model.</span></span> <span data-ttu-id="5042d-196">Po drodze możesz utworzyć i zastosować potrzeby dodatkowych migracji.</span><span class="sxs-lookup"><span data-stu-id="5042d-196">Along the way you'll create and apply additional migrations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5042d-197">Tworzenie i stosowanie potrzeby dodatkowych migracji</span><span class="sxs-lookup"><span data-stu-id="5042d-197">Create and apply additional migrations</span></span>](complex-data-model.md)
