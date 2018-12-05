---
title: Dodaj nowe pole na stronę Razor programu ASP.NET Core
author: rick-anderson
description: Pokazuje, jak dodać nowe pole do strony Razor za pomocą platformy Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: e280bc9553113982a1f1a77eabab32575c905237
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862294"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="ee500-103">Dodaj nowe pole na stronę Razor programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee500-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="ee500-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ee500-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="ee500-105">W tej sekcji [Entity Framework](/ef/core/get-started/aspnetcore/new-db) migracje Code First jest używana do:</span><span class="sxs-lookup"><span data-stu-id="ee500-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="ee500-106">Dodawanie nowego pola do modelu.</span><span class="sxs-lookup"><span data-stu-id="ee500-106">Add a new field to the model.</span></span>
* <span data-ttu-id="ee500-107">Migruj nowej zmiany schematu pola w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ee500-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="ee500-108">Jeśli przy użyciu programu EF Code First automatycznie utworzyć bazę danych, Code First:</span><span class="sxs-lookup"><span data-stu-id="ee500-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="ee500-109">Dodanie tabeli do sprawdzenia, czy schemat bazy danych jest zsynchronizowany z klasy modelu, który został wygenerowany z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee500-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="ee500-110">Jeśli klasy modelu nie są zsynchronizowane z bazy danych, EF zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ee500-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="ee500-111">Automatyczne weryfikacji/model schematu synchronizacji ułatwia znajdowanie problemów z niespójne bazy danych/code.</span><span class="sxs-lookup"><span data-stu-id="ee500-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="ee500-112">Dodawanie właściwości klasyfikacji do modelu Movie</span><span class="sxs-lookup"><span data-stu-id="ee500-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="ee500-113">Otwórz *Models/Movie.cs* pliku i Dodaj `Rating` właściwości:</span><span class="sxs-lookup"><span data-stu-id="ee500-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="ee500-114">Tworzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee500-114">Build the app.</span></span>

<span data-ttu-id="ee500-115">Edytuj *Pages/Movies/Index.cshtml*i Dodaj `Rating` pola:</span><span class="sxs-lookup"><span data-stu-id="ee500-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="ee500-116">Zaktualizuj następujące strony:</span><span class="sxs-lookup"><span data-stu-id="ee500-116">Update the following pages:</span></span>

* <span data-ttu-id="ee500-117">Dodaj `Rating` pola strony Delete i szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="ee500-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="ee500-118">Aktualizacja [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="ee500-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="ee500-119">Dodaj `Rating` pole edytowanie strony.</span><span class="sxs-lookup"><span data-stu-id="ee500-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="ee500-120">Aplikacja nie będzie działać, dopóki baza danych została zaktualizowana do nowego pola.</span><span class="sxs-lookup"><span data-stu-id="ee500-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="ee500-121">Jeśli teraz uruchomić zgłasza aplikacji `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="ee500-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="ee500-122">Ten błąd jest spowodowany przez zaktualizowane klasy modelu film jest inny niż schemat tabeli filmu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee500-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="ee500-123">(Brak nie `Rating` kolumny w tabeli bazy danych.)</span><span class="sxs-lookup"><span data-stu-id="ee500-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="ee500-124">Istnieje kilka sposobów rozwiązania problemu:</span><span class="sxs-lookup"><span data-stu-id="ee500-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="ee500-125">Ma automatycznie Porzuć i ponownie utworzyć bazę danych przy użyciu nowego schematu klasy modelu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ee500-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="ee500-126">To podejście jest wygodne na wczesnym etapie cyklu tworzenia oprogramowania; Umożliwia szybkie razem rozwijania schematu za jego modelu i bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ee500-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="ee500-127">Minusem jest to utraty istniejących danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ee500-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="ee500-128">Nie używaj tego podejścia w produkcyjnej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="ee500-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="ee500-129">Usunięcie bazy danych na zmiany schematu i automatycznie inicjowanie bazy danych z danymi za pomocą inicjatora jest często produktywny sposób do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ee500-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="ee500-130">Jawnie zmodyfikować schemat istniejącej bazy danych, aby odpowiadały one klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="ee500-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="ee500-131">Zaletą tego podejścia jest, aby zachować dane.</span><span class="sxs-lookup"><span data-stu-id="ee500-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="ee500-132">Można to zrobić to ręcznie lub przez tworzenie bazy danych zmiana skryptu.</span><span class="sxs-lookup"><span data-stu-id="ee500-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="ee500-133">Aby zaktualizować schemat bazy danych, należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="ee500-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="ee500-134">W tym samouczku należy użyć migracje Code First.</span><span class="sxs-lookup"><span data-stu-id="ee500-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="ee500-135">Aktualizacja `SeedData` klasy tak, aby go oferuje wartości dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="ee500-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="ee500-136">Poniżej przedstawiono przykładowe zmiany, ale należy to zrobić dla każdego `new Movie` bloku.</span><span class="sxs-lookup"><span data-stu-id="ee500-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="ee500-137">Zobacz [ukończone pliku SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="ee500-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="ee500-138">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="ee500-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ee500-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee500-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="ee500-140">Dodaj migrację dla pola klasyfikacji</span><span class="sxs-lookup"><span data-stu-id="ee500-140">Add a migration for the rating field</span></span>

<span data-ttu-id="ee500-141">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ee500-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="ee500-142">W konsoli zarządzania Pakietami wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ee500-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="ee500-143">`Add-Migration` Polecenie informuje platformę, by:</span><span class="sxs-lookup"><span data-stu-id="ee500-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="ee500-144">Porównaj `Movie` modelu przy użyciu `Movie` schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee500-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="ee500-145">Utwórz kod, aby migrować schemat bazy danych do nowego modelu.</span><span class="sxs-lookup"><span data-stu-id="ee500-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="ee500-146">Nazwa "Ocena" dowolnej i jest używany do nazywania plików migracji.</span><span class="sxs-lookup"><span data-stu-id="ee500-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="ee500-147">Warto użyć znaczącą nazwę pliku migracji.</span><span class="sxs-lookup"><span data-stu-id="ee500-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a>

<span data-ttu-id="ee500-148">Jeśli usuniesz wszystkie rekordy w bazie danych, inicjatora będzie obsługiwał bazy danych i obejmują `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="ee500-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="ee500-149">Można to zrobić za pomocą łącza delete w przeglądarce, albo z [Eksplorator obiektów Sql Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="ee500-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="ee500-150">Aby usunąć bazy danych z SSOX:</span><span class="sxs-lookup"><span data-stu-id="ee500-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="ee500-151">Wybierz bazę danych w SSOX.</span><span class="sxs-lookup"><span data-stu-id="ee500-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="ee500-152">Kliknij prawym przyciskiem myszy w bazie danych, a następnie wybierz pozycję *Usuń*.</span><span class="sxs-lookup"><span data-stu-id="ee500-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="ee500-153">Sprawdź **Zamknij istniejące połączenia**.</span><span class="sxs-lookup"><span data-stu-id="ee500-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="ee500-154">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="ee500-154">Select **OK**.</span></span>
* <span data-ttu-id="ee500-155">W [PMC](xref:tutorials/razor-pages/new-field#pmc), aktualizują bazę danych:</span><span class="sxs-lookup"><span data-stu-id="ee500-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ee500-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ee500-156">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ee500-157"><!-- copy/paste this tab to the next. Not worth an include  --> Bazy danych SQLite nie obsługuje migracji.</span><span class="sxs-lookup"><span data-stu-id="ee500-157"><!-- copy/paste this tab to the next. Not worth an include  --> SQLite doesn't support migrations.</span></span>

* <span data-ttu-id="ee500-158">Usuń bazę danych lub zmień nazwę bazy danych w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee500-158">Delete the database or change the database name in the *appsettings.json* file.</span></span>
* <span data-ttu-id="ee500-159">Usuń *migracje* folder (i wszystkie pliki w folderze).</span><span class="sxs-lookup"><span data-stu-id="ee500-159">Delete the *Migrations* folder (and all the files in the folder).</span></span>

<span data-ttu-id="ee500-160">Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ee500-160">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ee500-161">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="ee500-161">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ee500-162">Bazy danych SQLite nie obsługuje migracji.</span><span class="sxs-lookup"><span data-stu-id="ee500-162">SQLite doesn't support migrations.</span></span>

* <span data-ttu-id="ee500-163">Usuń bazę danych lub zmień nazwę bazy danych w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee500-163">Delete the database or change the database name in the *appsettings.json* file.</span></span>
* <span data-ttu-id="ee500-164">Usuń *migracje* folder (i wszystkie pliki w folderze).</span><span class="sxs-lookup"><span data-stu-id="ee500-164">Delete the *Migrations* folder (and all the files in the folder).</span></span>

<span data-ttu-id="ee500-165">Uruchom następujące polecenia interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ee500-165">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="ee500-166">Uruchom aplikację i sprawdź, można tworzenia/edycji/wyświetlania filmów z `Rating` pola.</span><span class="sxs-lookup"><span data-stu-id="ee500-166">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="ee500-167">Jeśli baza danych nie jest obsługiwany, należy ustawić punkt przerwania w `SeedData.Initialize` metody.</span><span class="sxs-lookup"><span data-stu-id="ee500-167">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee500-168">[Poprzedni: Dodawanie wyszukiwania](xref:tutorials/razor-pages/search)
> [dalej: Dodawanie walidacji](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="ee500-168">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
