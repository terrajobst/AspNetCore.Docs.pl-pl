---
title: Platformy ASP.NET Core MVC podstawowych EF - Migrations - 4 10
author: tdykstra
description: "W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych w aplikacji platformy ASP.NET Core MVC."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="8c96f-103">Migracje - Core EF z samouczek platformy ASP.NET Core MVC (4 10)</span><span class="sxs-lookup"><span data-stu-id="8c96f-103">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="8c96f-104">Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c96f-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8c96f-105">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c96f-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="8c96f-106">Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).</span><span class="sxs-lookup"><span data-stu-id="8c96f-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="8c96f-107">W tym samouczku możesz uruchomić przy użyciu funkcji migracji EF Core zarządzania zmianami modelu danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="8c96f-108">W kolejnych samouczkach zostanie dodany więcej migracji w przypadku zmiany modelu danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="8c96f-109">Wprowadzenie do migracji</span><span class="sxs-lookup"><span data-stu-id="8c96f-109">Introduction to migrations</span></span>

<span data-ttu-id="8c96f-110">Podczas opracowywania nowej aplikacji modelu danych zmienia często i zawsze zmian modelu, pobiera zsynchronizowane z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="8c96f-111">Te samouczki jest uruchomiona przez skonfigurowanie programu Entity Framework, aby utworzyć bazę danych, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="8c96f-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="8c96f-112">Następnie zawsze można zmienić modelu na model danych — dodać, usunąć, lub zmienić klas jednostek lub zmienić klasy DbContext — można usunąć bazy danych i EF tworzy nową, zgodny z modelem, którą nasiona go z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="8c96f-113">Synchronizacja bazy danych w modelu danych z tej metody działa poprawnie, dopóki możesz wdrożyć aplikację w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="8c96f-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="8c96f-114">Gdy aplikacja działa w środowisku produkcyjnym zwykle jest przechowywana dane, które chcesz zachować, a nie chcesz utracić wszystkie zawsze wprowadzeniu zmiany takie jak dodawanie nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="8c96f-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="8c96f-115">Funkcji EF Core migracje rozwiązuje ten problem, należy włączyć EF do aktualizacji schematu bazy danych zamiast tworzenia nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="8c96f-116">Entity Framework Core NuGet pakietów dla migracji</span><span class="sxs-lookup"><span data-stu-id="8c96f-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="8c96f-117">Aby pracować z migracji, można użyć **Konsola Menedżera pakietów** (PMC) lub interfejsu wiersza polecenia (CLI).</span><span class="sxs-lookup"><span data-stu-id="8c96f-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="8c96f-118">Te samouczki wyjaśniają, jak używać polecenia interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8c96f-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="8c96f-119">Informacje o PMC są obecnie [końcu tego samouczka](#pmc).</span><span class="sxs-lookup"><span data-stu-id="8c96f-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="8c96f-120">Narzędzia EF dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="8c96f-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="8c96f-121">Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* plików, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="8c96f-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="8c96f-122">**Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="8c96f-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="8c96f-123">Można edytować *.csproj* plików przez kliknięcie prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierając **Edytuj ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="8c96f-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="8c96f-124">(Numery wersji w tym przykładzie zostały bieżącej, gdy samouczka został zapisany).</span><span class="sxs-lookup"><span data-stu-id="8c96f-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="8c96f-125">Zmień parametry połączenia</span><span class="sxs-lookup"><span data-stu-id="8c96f-125">Change the connection string</span></span>

<span data-ttu-id="8c96f-126">W *appsettings.json* pliku, Zmień nazwę bazy danych w parametrach połączenia ContosoUniversity2 lub inna nazwa, który nie był używany na komputerze korzystasz.</span><span class="sxs-lookup"><span data-stu-id="8c96f-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="8c96f-127">Ta zmiana skonfigurowanie projektu, aby pierwszy migracji spowoduje utworzenie nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="8c96f-128">To nie jest wymagane wprowadzenie do migracji, ale pojawi się później dlaczego jest dobrym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="8c96f-128">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="8c96f-129">Alternatywą wobec zmiana nazwy bazy danych można usunąć bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="8c96f-130">Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="8c96f-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="8c96f-131">Poniższej sekcji opisano sposób uruchamiania poleceń interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8c96f-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="8c96f-132">Tworzenie początkowej migracji</span><span class="sxs-lookup"><span data-stu-id="8c96f-132">Create an initial migration</span></span>

<span data-ttu-id="8c96f-133">Zapisz zmiany i skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="8c96f-133">Save your changes and build the project.</span></span> <span data-ttu-id="8c96f-134">Następnie otwórz okno polecenia i przejdź do folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="8c96f-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="8c96f-135">Poniżej przedstawiono szybki sposób, w tym:</span><span class="sxs-lookup"><span data-stu-id="8c96f-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="8c96f-136">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Otwórz w Eksploratorze plików** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="8c96f-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Otwórz w Eksploratorze plików elementu menu](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="8c96f-138">Wprowadź "cmd" na pasku adresu i naciśnij klawisz Enter.</span><span class="sxs-lookup"><span data-stu-id="8c96f-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Otwórz okno polecenia](migrations/_static/open-command-window.png)

<span data-ttu-id="8c96f-140">Wprowadź następujące polecenie w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="8c96f-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="8c96f-141">Zostaną wyświetlone dane wyjściowe podobne do następujących w oknie wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="8c96f-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="8c96f-142">Jeśli zostanie wyświetlony komunikat o błędzie *nie pliku wykonywalnego odnaleźć pasującego polecenia "dotnet-ef"*, zobacz [ten wpis w blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) do rozwiązywania problemów w Pomocy.</span><span class="sxs-lookup"><span data-stu-id="8c96f-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="8c96f-143">Jeśli zostanie wyświetlony komunikat o błędzie "*nie może uzyskać dostępu do pliku... ContosoUniversity.dll, ponieważ jest on używany przez inny proces.* ", odnaleźć usług IIS Express ikonę na pasku zadań systemu Windows i kliknij go prawym przyciskiem myszy, a następnie kliknij przycisk **ContosoUniversity > Zatrzymaj witrynę**.</span><span class="sxs-lookup"><span data-stu-id="8c96f-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="8c96f-144">Sprawdź w górę i w dół metody</span><span class="sxs-lookup"><span data-stu-id="8c96f-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="8c96f-145">Kiedy wykonać `migrations add` polecenia EF wygenerowanego kodu, który spowoduje utworzenie bazy danych od podstaw.</span><span class="sxs-lookup"><span data-stu-id="8c96f-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="8c96f-146">Ten kod jest *migracje* folder, w pliku o nazwie  *\<sygnatury czasowej > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="8c96f-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="8c96f-147">`Up` Metody `InitialCreate` klasy tworzy tabele bazy danych, które odpowiadają zestawów jednostek modelu danych, i `Down` metoda usuwa je, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8c96f-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="8c96f-148">Migracje wywołania `Up` metody implementacji zmian modelu danych do migracji.</span><span class="sxs-lookup"><span data-stu-id="8c96f-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="8c96f-149">Po wprowadzeniu polecenia, aby wycofać aktualizacji, migracje wywołania `Down` metody.</span><span class="sxs-lookup"><span data-stu-id="8c96f-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="8c96f-150">Ten kod jest początkowej migracji, który został utworzony po wprowadzeniu `migrations add InitialCreate` polecenia.</span><span class="sxs-lookup"><span data-stu-id="8c96f-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="8c96f-151">Parametr name migracji ("InitialCreate" w przykładzie) jest używany dla nazwy pliku i może być dowolne.</span><span class="sxs-lookup"><span data-stu-id="8c96f-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="8c96f-152">Najlepiej wybrać słowo lub frazę podsumowanie, co jest wykonywana w procesie migracji.</span><span class="sxs-lookup"><span data-stu-id="8c96f-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="8c96f-153">Na przykład możesz nazwać nowsze migracji "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="8c96f-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="8c96f-154">Jeśli utworzono początkowej migracji, gdy baza danych już istnieje, jest generowany kod tworzenia bazy danych, ale nie ma działać, ponieważ w bazie danych jest już zgodny z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="8c96f-155">Gdy wdrażasz aplikację do innego środowiska, w których bazy danych nie istnieje jeszcze tego kodu zostaną uruchomione, aby utworzyć bazę danych, dlatego jest dobrym rozwiązaniem jest przetestowanie go.</span><span class="sxs-lookup"><span data-stu-id="8c96f-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="8c96f-156">Dlatego zmienić nazwę bazy danych w parametrach połączenia wcześniej — tak, aby migracji można utworzyć nową od początku.</span><span class="sxs-lookup"><span data-stu-id="8c96f-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="8c96f-157">Sprawdź migawki modelu danych</span><span class="sxs-lookup"><span data-stu-id="8c96f-157">Examine the data model snapshot</span></span>

<span data-ttu-id="8c96f-158">Tworzy także migracji *migawki* bieżącego schematu bazy danych w *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="8c96f-158">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="8c96f-159">Oto, jak wygląda ten kod:</span><span class="sxs-lookup"><span data-stu-id="8c96f-159">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="8c96f-160">Ponieważ bieżący schemat bazy danych jest reprezentowana w kodzie, EF Core nie ma na interakcję z bazy danych do utworzenia migracji.</span><span class="sxs-lookup"><span data-stu-id="8c96f-160">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="8c96f-161">Po dodaniu migracji EF określa co zmienione przez porównanie modelu danych do pliku migawki.</span><span class="sxs-lookup"><span data-stu-id="8c96f-161">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="8c96f-162">EF współdziała z bazy danych tylko wtedy, gdy musi zaktualizować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-162">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="8c96f-163">Plik migawki musi być zsynchronizowany z migracji, które go utworzyć, dlatego migracji nie można usunąć tylko przez usunięcie pliku o nazwie  *\<sygnatury czasowej > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="8c96f-163">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="8c96f-164">Jeśli usuniesz ten plik, pozostałe migracja będzie zsynchronizowany z pliku migawki bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-164">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="8c96f-165">Aby usunąć ostatniego migracji, który został dodany, użyj [Usuń migracje ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) polecenia.</span><span class="sxs-lookup"><span data-stu-id="8c96f-165">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="8c96f-166">Dotyczą migracji bazy danych</span><span class="sxs-lookup"><span data-stu-id="8c96f-166">Apply the migration to the database</span></span>

<span data-ttu-id="8c96f-167">W oknie wiersza polecenia wprowadź następujące polecenie, aby utworzyć bazę danych i tabele w nim.</span><span class="sxs-lookup"><span data-stu-id="8c96f-167">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="8c96f-168">Dane wyjściowe polecenia jest podobny do `migrations add` poleceń, z wyjątkiem tego, zobacz dzienniki dla poleceń SQL, który konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-168">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="8c96f-169">Większość Dzienniki zostały pominięte w następujących przykładowych danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-169">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="8c96f-170">Jeśli wolisz nie wyświetlić ten poziom szczegółów komunikatów dziennika, można zmienić poziom dziennika w *appsettings. Development.JSON* pliku.</span><span class="sxs-lookup"><span data-stu-id="8c96f-170">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="8c96f-171">Aby uzyskać więcej informacji, zobacz [wprowadzenie do rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="8c96f-171">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="8c96f-172">Użyj **Eksplorator obiektów SQL Server** przeprowadzać inspekcję bazy danych, tak jak w pierwszym samouczku.</span><span class="sxs-lookup"><span data-stu-id="8c96f-172">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="8c96f-173">Można zauważyć dodanie __EFMigrationsHistory tabeli, która przechowuje informacje o migracji, które zostały zastosowane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-173">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="8c96f-174">Wyświetlanie danych w tej tabeli, i zobaczysz jednego wiersza dla pierwszego migracji.</span><span class="sxs-lookup"><span data-stu-id="8c96f-174">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="8c96f-175">(Ostatniego logowania w poprzednim przykładzie danych wyjściowych interfejsu wiersza polecenia pokazuje instrukcji INSERT, która tworzy ten wiersz).</span><span class="sxs-lookup"><span data-stu-id="8c96f-175">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="8c96f-176">Uruchom aplikację, aby sprawdzić, czy wszystkie elementy nadal działa tak samo jak przed.</span><span class="sxs-lookup"><span data-stu-id="8c96f-176">Run the application to verify that everything still works the same as before.</span></span>

![Strona indeksu uczniów lub studentów](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="8c96f-178">Vs interfejsu wiersza polecenia (CLI). Konsola Menedżera pakietów (PMC)</span><span class="sxs-lookup"><span data-stu-id="8c96f-178">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="8c96f-179">EF narzędzi do zarządzania migracji jest niedostępna, z platformy .NET Core polecenia interfejsu wiersza polecenia lub poleceń cmdlet programu PowerShell w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC).</span><span class="sxs-lookup"><span data-stu-id="8c96f-179">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="8c96f-180">Ten samouczek przedstawia sposób użycia interfejsu wiersza polecenia, ale jeśli wolisz użyć PMC.</span><span class="sxs-lookup"><span data-stu-id="8c96f-180">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="8c96f-181">Polecenia EF poleceń PMC znajdują się w [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) pakietu.</span><span class="sxs-lookup"><span data-stu-id="8c96f-181">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="8c96f-182">Ten pakiet jest już uwzględniony w [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, dzięki czemu nie trzeba go zainstalować.</span><span class="sxs-lookup"><span data-stu-id="8c96f-182">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="8c96f-183">**Ważne:** nie jest tym samym pakiecie, jak zainstalować dla interfejsu wiersza polecenia, edytując *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="8c96f-183">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="8c96f-184">Nazwa tego kończy się `Tools`, w odróżnieniu od nazwy pakietu interfejsu wiersza polecenia, która kończy się `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="8c96f-184">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="8c96f-185">Aby uzyskać więcej informacji na temat poleceń interfejsu wiersza polecenia, zobacz [interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="8c96f-185">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="8c96f-186">Aby uzyskać więcej informacji na temat poleceń PMC zobacz [Konsola Menedżera pakietów (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="8c96f-186">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="8c96f-187">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="8c96f-187">Summary</span></span>

<span data-ttu-id="8c96f-188">W tym samouczku przedstawiono sposób tworzenia i stosowania pierwszy migracji.</span><span class="sxs-lookup"><span data-stu-id="8c96f-188">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="8c96f-189">W następnym samouczku zostaną patrzeć bardziej zaawansowanych tematów, rozwijając modelu danych.</span><span class="sxs-lookup"><span data-stu-id="8c96f-189">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="8c96f-190">Wzdłuż sposób możesz utworzyć i zastosować dodatkowe migracji.</span><span class="sxs-lookup"><span data-stu-id="8c96f-190">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8c96f-191">[Poprzednie](sort-filter-page.md)
[dalej](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="8c96f-191">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
